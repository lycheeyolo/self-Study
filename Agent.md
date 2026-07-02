from deepagents import create_deep_agent
from deepagents.backends import FilesystemBackend
from langgraph.checkpoint.memory import MemorySaver
set_alarm_context(alarm)
log = get_current()
measdef = _infer_measdef_from_alarm(alarm)
intent = intent_override or _derive_intent(alarm)
chart_type = _select_chart(intent, alarm)
known = _extract_spectral_context(alarm, measdef_type=measdef, chart_type=chart_type)
task = build_analysis_task(alarm, intent_override=intent_override, known=known)
if log:
    log.info(f"[VLM 频谱预提取] {known}")
    log.info(f"[VLM 任务] {task}")
backend = FilesystemBackend(root_dir=_log_dir())
from deepagents.middleware._tool_exclusion import _ToolExclusionMiddleware
agent = create_deep_agent(
    model=self.llm, tools=self.tools,
    system_prompt=self.system_prompt
    + "\n限制：每次只调用1个工具（禁止并行调用多个工具），最多调用10次，之后必须输出结论JSON。"
    + "\n注意：conclusion和fault_type必须一致。若conclusion=指标误算或证据不足，fault_type必须为'无'。",
    checkpointer=MemorySaver(), backend=backend,
    middleware=[_ToolExclusionMiddleware(excluded=["write_todos", "task"])])
last = ""
all_msgs = []
prev = 0
try:
    for state in agent.stream(
            {"messages": [{"role": "user", "content": task}]},
            config={"configurable": {"thread_id": alarm.alarm_uuid or "default"}},
            stream_mode="values"):
        msgs = state.get("messages", [])
        # 只记新增的消息（实时日志）
        for m in msgs[prev:]:
            _log_msg(m, log)
        prev = len(msgs)
        all_msgs = msgs
    # 从最后状态提取 AI 最终输出
    for m in reversed(all_msgs):
        c = getattr(m, "content", None)
        if isinstance(c, str) and c.strip():
            last = c
            break
except Exception as e:
    if log:
        log.warn(f"[VLM-deep-agent] 异常: {e!r} → 强制收尾")
