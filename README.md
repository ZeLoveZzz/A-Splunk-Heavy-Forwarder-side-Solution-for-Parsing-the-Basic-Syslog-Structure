# A-Splunk-Heavy-Forwarder-side-Solution-for-Parsing-the-Basic-Syslog-Structure
The basic Syslog structure is as follows: &lt;PRI> TS HOST APP[PID]: { JSON }. This repository provides props.conf and transforms.conf files to parse this format.

## Example

### Raw Syslog Input
```log
<134> 2026-01-07T17:19:57+08:00 syslog-source-01 app-name[1234]: {
  "event_type": "generic_event",
  "event_time": "2026-01-07T17:19:52+08:00",
  "ingest_time": "2026-01-07T17:19:57+08:00",
  "source": {
    "id": "<REDACTED_ID>",
    "ip": "203.0.113.10",
    "agent_version": "vX.Y.Z",
    "endpoint": {
      "endpoint_id": "<REDACTED_ID>",
      "endpoint_name": "endpoint-xxxx",
      "user": "user_x",
      "os": "Generic OS",
      "tags": ["tag_a", "tag_b"],
      "local_ips": ["192.168.x.x"]
    }
  },
  "details": {
    "category": "file_activity",
    "action": "modify",
    "process": {
      "pid": 4592,
      "name": "process.exe",
      "path": "C:\\Path\\To\\process.exe"
    },
    "target": {
      "file_name": "example.ini",
      "file_path": "C:\\Path\\To\\example.ini",
      "hash": "<REDACTED_HASH>"
    }
  }
}
```

---

### After Parsing on Splunk Heavy Forwarder

After applying `props.conf` and `transforms.conf`, the event received by indexers will be a valid JSON event with injected Syslog header fields:

```json
{
  "syslog_time": "2026-01-07T17:19:57+0800",
  "syslog_host": "syslog-source-01",
  "event_type": "generic_event",
  "event_time": "2026-01-07T17:19:52+08:00",
  "ingest_time": "2026-01-07T17:19:57+08:00",
  "source": { "...": "..." },
  "details": { "...": "..." }
}
```

* `_time` is derived from `syslog_time`
* `host` metadata is set to `syslog_host`
* All JSON fields are automatically extracted (`KV_MODE=json`)

* GitHub public repositories
* Internal architecture / onboarding documentation
* Splunk TA demos
* SOC parsing strategy discussions

