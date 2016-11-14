# Watch

*Monitor API endpoint* and print http status code every second

```
watch -n 1 curl -s -o /dev/null -w "%{http_code}" "https://host/api/health"
```
