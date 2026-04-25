# Apply Fleet Management Pipeline: phils-demo  

## Script  

```bash  
`  
#!/usr/bin/env bash  
set -euo pipefail  

BASE_URL="${FM_BASE_URL:?Set FM_BASE_URL}"  
API_KEY="${GRAFANA_API_KEY:?Set GRAFANA_API_KEY}"  

curl -sf \  
  --request POST \  
  --url "${BASE_URL}/pipeline.v1.PipelineService/UpsertPipeline" \  
  --header "Authorization: Bearer ${API_KEY}" \  
  --header "Content-Type: application/json" \  
  --data "$(yq -o=json pipeline.yaml)" \  
  | jq .  
```

## Usage  

```  
export FM_BASE_URL="https://fleet-management-prod-123.grafana.net"  
export GRAFANA_API_KEY="glsa_..."  
bash apply-pipeline.sh ./phils-demo.alloy  

```
​  

## Where to find your values  

- **FM_BASE_URL** — Fleet Management UI → API tab  
- **GRAFANA_API_KEY** — Grafana Cloud → Administration → Service Accounts (Editor role required)  

## Notes  

- `UpsertPipeline` creates the pipeline if it doesn't exist, or updates it if it does.  
- Safe to run repeatedly (idempotent).  
- Requires `curl` and `jq` to be installed.  
