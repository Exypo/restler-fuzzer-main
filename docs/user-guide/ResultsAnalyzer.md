# Analyzing RESTler network logs

After testing has finished, a log analyzer takes the network logs generated by RESTler and generates a
run summary, which bucketizes the responses by error code and message.  The results analyzer may also be run separately as follows:

```C:\restler_bin\ResultsAnalyzer\Restler.ResultsAnalyzer.exe --logs_path <path to network log file> --output_dir .```

At this time, this tool processes requests individually, and does not extract
sequence information.  Adding data on sequences is future work.


Upon completion, the following files will be generated:

## runSummary.json

This file contains statistics on responses found.

For example:

```json
{
    "requestsCount": 5529,
    "sequencesCount": 0,
    "bugCount": 77,
    "codeCounts": {
        "0": 572,
        "200": 1183,
        "201": 1501,
        "202": 129,
        "409": 1,
        "500": 77
    },
    "errorBuckets": {
        "(0, 6e4eded0-c117-4664-8fb2-d7856750e03d)": 286,
        "(404, 9e563b03-3646-4806-8150-b239cc9c8c48)": 122,
        "(409, 41466154-8377-4022-917f-714d4a14ff89)": 1,
        "(500, ca61c5c9-50fd-47df-896a-ba6e15c7fe8f)": 77
    }
}
```

Note: in some cases, the results analyzer is unabled to extract the response code from the
response (this may happen due to an unsupported format or a bug in the RESTler engine).  Such responses are assigned a "0" response code.

## errorBuckets.json

This file contains detailed information on the error buckets.  It has all of the
responses in the network log, grouped by error code and bucket. For example:

```json
{
    "0": {
        "c2925694-5591-4ee3-829e-78372d44b42a": [
            {
                "request": {
                    "RequestData": {
                        "method": "GET",
                        "path": "/zones/zone.name.com38d7f7fa3e/SOA/{\\n",
                        "query": "",
                        "body": ""
                    }
                },
                "response": {
                    "ResponseText": "{\"error\":{\"code\":\"InvalidHostName\",\"message\":\"The provided host name \\'10.0.0.30\\' is not whitelisted. \"}}"
                }
            }
        ]
    }
}
```

