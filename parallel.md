# Parallel

Install and silence parallel

```shell
brew install parallel
parallel --bibtex
# enter 'will cite'
```

Run 4 jobs in parallel

```shell
cat ids.txt | time parallel --jobs 4 'curl -X "GET" -s -w "{} %{time_total}\n" -o /dev/null -u "johndoe:secretpass" "https://host/api/things/{}"' > things_parallel.txt
```
