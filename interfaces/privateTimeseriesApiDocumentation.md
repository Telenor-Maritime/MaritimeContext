# Private Timeseriesdata API

This API is used to get your private data that is being stored in the UHS.

### API URL

Ip and port will be the same on every site.
```
10.246.0.10:8081/api/timeseriesdata/private/<companyId>/<measurement-topic>
```

## Authentication
The authentication method used is Bearer token, the bearer token is a combination of the credentials used for connecting to the MQTT buss. 
`Bearer <clientId>:<username>:<password>`


## Query parameters

Following are the supported parameters than can be used by the client to customize the API response. All the parameters has a default value, so they are optional to use. If no parameters are given, the default response is the latest value for the matching measurement point.  
  
| **Parameter** | **DataType**          | **Default value**    |
|---------------|-----------------------|----------------------|
| limit         | Integer               | 1                    |
| timefrom      | ISO 8601              | 2020-01-01T00:00:00Z |
| timeto        | ISO 8601              | current time         |
| orderby       | string <'asc'/'desc'> | descending           |

### Limit
Restricts the number of values returned in the response.

### Timefrom
Returns values that have been recorded after the given timestamp.

### Timeto
Returns values that have been recorded before the given timestamp.

### Orderby
Used to specify the order of the values in the response, ascending or descending based on the timestamp of the value.

## Response payload

The response payload differs depending on usage of wildcard or not.
### Request for one datachannel
```json
[
	{
		"timestamp" : ""
		"value" : ""
	},
	{
		"timestamp" : ""
		"value" : ""
	},
]
```

### Request with wildcard, multiple measurements

```json
{
	"the/measurment/topic/1" :
	[
		{
			"timestamp" : ""
			"value" : ""
		}
	],
	"the/measurment/topic/2" :
	[
		{
			"timestamp" : ""
			"value" : ""
		}
	],
}
```

## Wildcard
The API supports the use of wildcard at the end of the URL. With this wildcard, the client is able to query on multiple datachannels that matches the given prefix given in the request URL.
The symbol used as a wildcard is `**`.

**Example**
```console
curl --location --request GET '<ip>:<port>/api/timeseriesdata/private/<companyId>/<topic-prefix>/**?limit=1&orderby=desc' --header 'Authorization: Bearer <clientId>:<username>:<password>'
```









