# Neo4j-docekr镜像
- spg-registry.us-west-1.cr.aliyuncs.com/spg/openspg-neo4j:latest
- docker-compose.yaml
```bash
version: "3.7"
services:
  neo4j:
      restart: always
      image: spg-registry.us-west-1.cr.aliyuncs.com/spg/openspg-neo4j:latest
      container_name: release-openspg-neo4j
      ports:
        - "7474:7474"
        - "7687:7687"
      environment:
        - TZ=Asia/Shanghai
        - NEO4J_AUTH=neo4j/neo4j@openspg
        - NEO4J_PLUGINS=["apoc"]
        - NEO4J_server_memory_heap_initial__size=1G
        - NEO4J_server_memory_heap_max__size=4G
        - NEO4J_server_memory_pagecache_size=1G
        - NEO4J_apoc_export_file_enabled=true
        - NEO4J_apoc_import_file_enabled=true
        - NEO4J_dbms_security_procedures_unrestricted=*
        - NEO4J_dbms_security_procedures_allowlist=*
      volumes:
        - /home/lychee/kg/neo4j_database/data:/data
        - /home/lychee/kg/neo4j_database/logs:/logs
```
- 
