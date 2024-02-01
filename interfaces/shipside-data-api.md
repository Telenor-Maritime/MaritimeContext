

## API authorization

The authentication method used is Bearer token, the bearer token is a combination of the credentials used for connecting to the MQTT buss.

`Bearer <clientId>:<username>:<password>`

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


***Default response***
```json
[
    {
        "value":"",
        "timeStamp":""
    }
]
```

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


### 2. getISOTimeseriesData - GPS example

***Endpoint:***

```bash
Method: GET
Type: 
URL: 10.246.0.10:8081/api/timeseriesdata/dnvgl-v2/vis-2-3/mbb-modem/GPS.fix.data
```

***Authorization - Bearer token***  -- Let us know when you are ready to test it out, and we will provide you with the token

***Query params:***


| **Key** | **DataType**     | **Default**          |
|---------|------------------|----------------------|
| offset  | ISO 8601         | 2020-01-01T00:00:00Z |
| orderby | string(desc/asc) | desc                 |
| before  | Boolean          | False                |
| header  | Boolean          | False                |
| limit   | Integer          | 1                    |



***Response example***

```json
{
  "TimeSeriesData":[
    {
      "TabularData":{
        "DataChannelID":[
          "dnvgl-v2/vis-2-3/mbb-modem/GPS.fix.data"
        ],
        "DataSet":[
          {
            "TimeStamp":"2022-10-18T01:00:05.000Z",
            "Value":[
              "{\"Position\": {\"lat\": \"-10.053333333333\", \"long\": \"39.486666666667\"}, \"Heading\": 173.8, \"Speed\": 21.1}"
            ]
          },
          {
            "TimeStamp":"2022-10-18T01:00:15.000Z",
            "Value":[
              "{\"Position\": {\"lat\": \"-10.061666666667\", \"long\": \"39.487333333333\"}, \"Heading\": 173.8, \"Speed\": 21.1}"
            ]
          }
        ]
      }
    }
  ]
}
```
---


### 3. getGPS

***Endpoint:***

```bash
Method: GET
Type: 
URL: 10.246.0.10:8081/api/GPS
```

***No Authorization

***Query params:***


| **Key** | **DataType**     | **Default**          |
|---------|------------------|----------------------|
| timefrom | ISO 8601 | 2020-01-01T00:00:00Z |
| timeto | ISO 8601 | current time |
| limit | Integer | 1 |


The UHS will provide GPS and GPS releated information from several available sources. 1 priority source is brigde.


***Response example***

```json
[
    {
        "course": 323.8,
        "heading": 2,
        "source": "bridge",
        "latitude": 59.126066666666674,
        "longitude": 10.227300000000001,
        "HDOP": "1.3",
        "timestamp": "2024-02-01 08:45:15+00:00Z",
        "speedKnots": 0.1,
        "speedKph": 0.3
    }
]
```
---
source: GPS source


### 4. getTraveledDistance

***Endpoint:***

```bash
Method: GET
Type: 
URL: 10.246.0.10:8081/api/traveleddistance
```

***No Authorization

***Query params:***


| **Key** | **DataType**     | **Default**          |
|---------|------------------|----------------------|
| timefrom | ISO 8601 | 2020-01-01T00:00:00Z |
| timeto | ISO 8601 | current time |



***Response example***

```json
{
    "start": "2024-01-31T06:50:09.542Z",
    "end": "2024-01-31T10:29:51.866Z",
    "averageSpeed": "19.77 knots",
    "timeTraveled": "03:39:42",
    "distanceTraveled": "72.38 nautical miles",
    "samples": "1169",
    "precision": "88.69 %"
}
```
---
samples: The number of samples that is the base for calculation  
precision: The percent of samples compare to the theoretical number of samples


### 5.  Private Timeseriesdata API

This API is used to get your private data that is being stored in the UHS.

###  API URL

Ip and port will be the same on every site.

```

10.246.0.10:<private_api_port>/api/timeseriesdata/private/<companyId>/<measurement-topic>

```


###  Query parameters

Following are the supported parameters than can be used by the client to customize the API response. All the parameters has a default value, so they are optional to use. If no parameters are given, the default response is the latest value for the matching measurement point.


| **Parameter** | **DataType** | **Default value** |
|---------------|-----------------------|----------------------|
| limit | Integer | 1 |
| timefrom | ISO 8601 | 2020-01-01T00:00:00Z |
| timeto | ISO 8601 | current time |
| orderby | string <'asc'/'desc'> | descending |



###  Limit

Restricts the number of values returned in the response.

###  Timefrom

Returns values that have been recorded after the given timestamp.

###  Timeto

Returns values that have been recorded before the given timestamp.

###  Orderby

Used to specify the order of the values in the response, ascending or descending based on the timestamp of the value.

##  Response payload

The response payload differs depending on usage of wildcard or not.

###  Request for one datachannel

```json

[
  {
    "timestamp":"",
    "value":""
  },
  {
    "timestamp":"",
    "value":""
  }
]

```

###  Request with wildcard, multiple measurements

```json

{
  "the/measurment/topic/1":[
    {
      "timestamp":"",
      "value":""
    }
  ],
  "the/measurment/topic/2":[
    {
      "timestamp":"",
      "value":""
    }
  ]
}

```

##  Wildcard

The API supports the use of wildcard at the end of the URL. With this wildcard, the client is able to query on multiple datachannels that matches the given prefix given in the request URL.

The symbol used as a wildcard is `**`.

**Example**

```console

curl --location --request GET '<ip>:<port>/api/timeseriesdata/private/<companyId>/<topic-prefix>/**?limit=1&orderby=desc' --header 'Authorization: Bearer <clientId>:<username>:<password>'

```


