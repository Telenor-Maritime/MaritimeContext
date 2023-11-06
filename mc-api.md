# mc-api

Api for fecthing data on MC format

<!--- If we have only one group/collection, then no need for the "ungrouped" heading -->
1. [getMCTimeseriesData](#1-getmctimeseriesdata)



## Endpoints


--------



### 1. getMCTimeseriesData - GPS example



***Endpoint:***

```bash
Method: GET
Type: 
URL: 10.246.0.10:8083/api/timeseriesdata/MC/V1/mbb-modem/GPS.fix.data
```

***Authorization - Bearer token***  -- Let us know when you are ready to test it out, and we will provide you with the token

***Query params:***

| Key | DataType | Default |
| --- | ------|-------------|
| timefrom | ISO 8601 | 2020-01-01T00:00:00Z |
| timeto | ISO 8601 | current time |
| orderby | string 'desc'/'asc' | descending |
| limit | Integer | 1 |


***Response example***

```json
[
    {
        "value": {
            "Position": {
                "lat": "-69.421166666667",
                "long": "14.986"
            },
            "Heading": 173.8,
            "Speed": 21.1
        },
        "timeStamp": "2022-09-11T12:59:58.000Z"
    },
    {
        "value": {
            "Position": {
                "lat": "-75.420333333333",
                "long": "19.785333333333"
            },
            "Heading": 173.8,
            "Speed": 21.1
        },
        "timeStamp": "2022-09-11T12:59:50.000Z"
    }
]
```
---
