# Apply Fleet Management Pipeline: phils-demo  

## Script  

```bash  
curl -s -w "\n%{http_code}" --request POST --url "https://fleet-management-prod-014.grafana.net/pipeline.v1.PipelineService/UpsertPipeline" --user "<USER>:${API_KEY}" --header "Content-Type: application/json" --data @pipeline.json
```


## Notes  

- `UpsertPipeline` creates the pipeline if it doesn't exist, or updates it if it does.  
- Safe to run repeatedly (idempotent).  
- Requires `curl` and `jq` to be installed.  
