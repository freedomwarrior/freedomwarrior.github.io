## How to clear prometheus database?
 
First, you need to enable admin API by passing the `--web.enable-admin-api` command line flag to Prometheus, then you can provide selectors for the time series you wish to remove:
```
curl -X POST -g 'http://localhost:9090/api/v1/admin/tsdb/delete_series?match[]=a_bad_metric&match[]={region="mistake"}'
```

Once the request returns, the data will no longer be accessible. Storage space on disk will be freed up at the next compaction. You can hurry this along by using the clean tombstones API, but this is not something you should be doing on a regular basis.
