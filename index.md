# yandlines

yet another newline delimited (json spec)

With a heavy nod to https://github.com/wardi/jsonlines and a wink at https://github.com/ndjson/ndjson-spec, because who is crazy enough to write JSON without the object notation '{}' or '[]'

I was inspired to extend and as relax these other two standards because I really want comments, for my use case it was a deal breaker. My yandlines file isn't being streamed or written to by a logging system, and yet 

This page describes the ndjson format, also called Newline delimited JSON. NDJSON is a convenient format for storing or streaming structured data that may be processed one record at a time. It works well with unix-style text processing tools and shell pipelines. It's a great format for log files. It's also a flexible format for passing messages between cooperating processes.

1. Last character of each line must be '\n' (so '\r\n' will obviously work too).
2. Each line is valid JSON where the "object" must start with either "{" or "["
3. Ignoring whitespace, any line that doesn't start with "{" or "[" is skipped

Notes: 
1. Ideally use UTF-8 for the encoding

```
{ "msg":"this is a valid yandlines line", "valid":true }
     { "msg":"this is also valid, note the whitespace" }
[{"id":"A"},{"id":"B"}]
```

How easy is the file to extract from?

```
POWERSHELL
$(Get-Content "index.md") -split '\n' | Where-Object {$_.trim() -like "['{','[']*"} | Foreach-Object {$_ | ConvertFrom-Json} 

```

