# Apply Fleet Management Pipeline: phils-demo  

## Script  

```bash  
#!/usr/bin/env bash  
# apply-pipeline.sh — Upsert a Fleet Management pipeline named phils-demo  
set -euo pipefail  

# ── Config (set via env or edit here) ────────────────────────────────────────  
BASE_URL="${FM_BASE_URL:?Set FM_BASE_URL to your Fleet Management base URL}"  
# e.g. https://fleet-management-prod-123.grafana.net  
API_KEY="${GRAFANA_API_KEY:?Set GRAFANA_API_KEY to a Grafana Cloud service account token}"  
PIPELINE_NAME="phils-demo"  
CONFIG_FILE="${1:-config.alloy}"   # pass path to your .alloy file as $1  

# ── Build JSON payload ────────────────────────────────────────────────────────  
if [[ ! -f "$CONFIG_FILE" ]]; then  
  echo "ERROR: config file '$CONFIG_FILE' not found" >&2  
  exit 1  
fi  

PAYLOAD=$(jq -n \  
  --arg name "$PIPELINE_NAME" \  
  --rawfile contents "$CONFIG_FILE" \  
  '{  
    pipeline: {  
      name: $name,  
      contents: $contents,  
      matchers: []  
    }  
  }')  

# ── Upsert pipeline ───────────────────────────────────────────────────────────  
echo "Upserting pipeline '$PIPELINE_NAME' ..."  
RESPONSE=$(curl -sf \  
  --request POST \  
  --url "${BASE_URL}/pipeline.v1.PipelineService/UpsertPipeline" \  
  --header "Authorization: Bearer ${API_KEY}" \  
  --header "Content-Type: application/json" \  
  --data "$PAYLOAD")  

echo "Done:"  
echo "$RESPONSE" | jq .  
```


# ── Upsert pipeline ───────────────────────────────────────────────────────────  
```
echo "Upserting pipeline '$`PIPELINE_NAME`' ..."  
RESPONSE=$(curl -sf \  
  --request POST \  
  --url "${`BASE_URL`}/pipeline.v1.PipelineService/UpsertPipeline" \  
  --header "Authorization: Bearer ${`API_KEY`}" \  
  --header "Content-Type: application/json" \  
  --data "$PAYLOAD")  

echo "Done:"  
echo "$RESPONSE" | jq .  
```

## Usage  

```  
export `FM_BASE_URL`="https://fleet-management-prod-123.grafana.net"  
export `GRAFANA_API_KEY`="glsa_..."  
bash apply-pipeline.sh ./phils-demo.alloy

```
​  

## Where to find your values  

- **FM_BASE_URL** — Fleet Management UI → API tab  
- **GRAFANA_API_KEY** — Grafana Cloud → Administration → Service Accounts (Editor role required)  
- **matchers** — add key/value pairs to target specific collectors, e.g. `[{"name":"env","value":"prod"}]`  

## Notes  

- `UpsertPipeline` creates the pipeline if it doesn't exist, or updates it if it does.  
- Safe to run repeatedly (idempotent).  
- Requires `curl` and `jq` to be installed.  
