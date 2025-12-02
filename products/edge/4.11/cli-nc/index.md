# nc


Listens on a port for traffic, and outputs stats and data. (Netcat-like utility.)

**Usage:**

```text
./cribl nc -p 4200
```

**Sample response:**

```text
2021-08-20T22:44:30.457Z - starting server on 0.0.0.0:9999
2021-08-20T22:44:30.462Z - server listening 0.0.0.0:9999
2021-08-20T22:44:31.461Z - messages: 0, socks: 0, thruput: 0MBps
2021-08-20T22:44:32.466Z - messages: 0, socks: 0, thruput: 0MBps
...
2021-08-20T22:44:39.212Z - got connection: 127.0.0.1:37190
2021-08-20T22:44:39.213Z - got connection: 127.0.0.1:37192
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-p <port>` | Port to listen on. |
| `-f <family>` | If this is `6` then nc will listen on `::1`. Otherwise, it listens on `0.0.0.0`. |
| `-s <statsInterval>` | Stats output interval (ms), use `0` to disable. |
| `-u` | Listen on UDP port instead. |
| `-o` | Output received data to stdout. |
| `-t <throttle>` | Throttle rate in (unit)/sec, where units can be KB, MB, GB, and TB. |