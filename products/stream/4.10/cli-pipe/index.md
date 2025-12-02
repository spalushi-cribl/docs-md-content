# pipe


Feeds standard input (stdin) to a Pipeline.

**Usage:**

```text
cat sample.log |  ./cribl pipe -p pipelineName
cat sample.log |  ./cribl pipe -p pipelineName 2>/dev/null
```

**Sample response:**

```text
...
{"time":"2021-08-20T20:37:00.017Z","cid":"api","channel":"commands","level":"info","message":"creating new pipeline","id":"main","conf":{"asyncFuncTimeout":1000,"functions":[{"id":"eval","disabled":false,"filter":"true","conf":{"add":[{"name":"cribl","value":"'yes'"}],"remove":[]}}]}}
{"time":"2021-08-20T20:37:00.019Z","cid":"api","channel":"pipe:main","level":"info","message":"start loading and initializing functions","count":1}
{"time":"2021-08-20T20:37:00.021Z","cid":"api","channel":"pipe:main","level":"info","message":"finished loading and initializing functions","count":1}
{"time":"2021-08-20T20:37:00.022Z","cid":"api","channel":"commands","level":"info","message":"START pushing stdin events","id":"main"}
{"time":"2021-08-20T20:37:00.028Z","cid":"api","channel":"GrokMgr","level":"info","message":"loaded grok patterns","count":152}
...
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-p <pipeline>` | Pipeline to feed data through. |
| `-d` | Include dropped events. |
| `-c <cpuProfile>` | Perform CPU profiling. |
| `-t` | Perform pipeline tracing. |
| `-a <pack>` | Optional Cribl Pack context. Mutually exclusive with -b. |
| `-b <project>` | Optional Cribl Project context. |
| `-i` | Runs the pipe command without sandboxing javascript expressions from potential attackers. |
