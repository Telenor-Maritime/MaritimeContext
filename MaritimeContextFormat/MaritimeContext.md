#  Maritime Context

Date: 2022-06-29, Version: V1

##  About this document

There is a need for a simpler format that the ISO19847/ISO19848.

It has been requested from several UHS users, but the specification work seems

to go slow.

This is therefore an attempt to at least make a minimum specification for the first implementations.

All comments are welcome and hopefully most of the specification will be base for version 2 of MC (Maritime Context).

#  Payload data MC

##  Topic

Shared data: `MC/<version>/...`

Private data: `private/<OEM>/<OEM to fully decide>`

##  Value

Shared data:

```

{

"timestamp": <ISO 8601>,

"value": <string> | <integer> | <float>

}

```

Private stored data:

```

{

"timestamp": <ISO 8601>,

"value": <string>

}

```

Private not stored data: NA, UHS only do admission control.

#  Metadata MC

Somewhat 'based on'/'inspired from' ISO19848 and SignalK

##  Publishing metadata MC

The topic for metadata is obtained by adding the word `meta` after MC version. Example MBB GPS position:

- IoT data topic: `MC/V1/mbb-modem/GPS.fix.data`

- IoT metadata topic: `MC/V1/meta/mbb-modem/GPS.fix.data`

Publishing of metadata need to be done at least once before publishing IoT data and when changes to metadata occur.

Periodically publishing of metadata is possible, but not required.

Use QoS 2 (or maybe 1), but not 0 when publishing metadata.

Also set the retain flag.

##  Format of metadata MC

```cpp

{

"dataChannelType": <dataChannelType>,

"format": <format>,

"unit": <unit>,

// <optional fields>,

// <own fields>

}

```

###  `dataChannelType`

```cpp

"dataChannelType": {

"type": <string>,

"updateCycle": <in ms>, // Optional

"calculationPeriod": <in ms> // Optional

}

```

`type` must be one of:

```cpp

[
  "inst",
  "average",
  "max",
  "min",
  "median",
  "mode",
  "standardDeviation",
  "calculated",
  "setPoint",
  "controlOutput",
  "alert",
  "status",
  "manualInput"
]

```

###  `format`

```cpp

"format": {

"type" : "decimal" | "integer" | "boolean" | "string" | "dateTime",

"restriction": <> // Optional

}

```

###  `unit`

```cpp

"unit": {

"unitSymbol": <>,

"standard": <>, // Optional

"quantityName": <> // Optional

}

```

###  optional fields

```cpp

"range":{
  "low":"<>",
  "high":"<>"
},
"custom":"<string>",
"name":"<string>",
"sync2Shore":{
  "syncRules":[
    {
      "bearer":"satellite""|""mbb""|""wifi",
      "priority":"<integer>"
    }
  ],
  "pubOnMQTT":true
}

```

If sync to shore shall be done, one or more sync rules must be added.

A sync rule must contain a list of bearers that is allow to be used for sync.

And a priority to be used within each bearer.

Top priority is 1 and lowest priority is 10.

Optional sync parameters are `pubOnMQTT` which if true the topic will be published a.s.a.p on the

shore MQTT broker available for customers.

```cpp

{
  "remarks":"<string>",
  "displayName":"<string>",
  "longName":"<string>",
  "shortName":"<string>",
  "description":"<string>",
  "timeout":"<number>",
  "displayScale":{
    "lower":"<number>",
    "upper":"<number>",
    "type":"linear""|""logarithmic""|""squareroot""|""power"
  },
  "alertMethod":"sound""|""visual",
  "warnMethod":"sound""|""visual",
  "alarmMethod":"sound""|""visual",
  "emergencyMethod":"sound""|""visual",
  "zones":[
    {
      "lower":"<number>",
      "upper":"<number>",
      "state":"nominal""|""normal""|""alert""|""warn""|""alarm""|""emergency",
      "message":"<string>"
    }
  ],
  "source":"<string> // application publishing to MQTT"
}

```

###  own fields

Own fields can be added but must not interfere with the above definitions.

##  Example

The MBB GPS using **absolute minimum specification:**

```json
{
  "dataChannelType":{
    "type":"inst"
  },
  "format":{
    "type":"string"
  },
  "unit":{
    "unitSymbol":"$GPGGA"
  }
}

```

The MBB GPS **with a more extended (and better) specification:**

```json

{
  "dataChannelType":{
    "type":"inst",
    "updateCycle":10000
  },
  "format":{
    "type":"string"
  },
  "unit":{
    "unitSymbol":"$GPGGA",
    "standard":"NMEA"
  },
  "sync2Shore":{
    "syncRules":[
      {
        "bearer":"satellite",
        "priority":7
      },
      {
        "bearer":"mbb",
        "priority":3
      },
      {
        "bearer":"wifi",
        "priority":1
      }
    ],
    "pubOnMQTT":true
  },
  "displayName":"MBB-GPS",
  "longName":"MBB modem GPS",
  "shortName":"MBB-GPS",
  "description":"The GPS position read from the MBB modem",
  "timeout":10
}

```

#  Internal (not shared) IoT-data

The UHS can provide the possibility for internal data, that is not

possible to share with other application in UHS. These internal data

types exists:

- on ship only, not stored data

- on ship, stored data

- synced to shore, stored data

##  Topic of Internal (not shared) IoT-data

private/&lt;OEM>/&lt;OEM to fully decide>


If no metadata is provided, the data is only OEM internal on ship and UHS only handle permission control.

If metadata is provided, data is stored on ship for 30 days,

and available to OEM for that period using REST API.

##  Metadata

Metadata is mandatory for stored and data synced to shore

Topic of metadata is:

private/&lt;companyId>/meta/&lt;company to fully decide>


###  Format of metadata

As for MC.

If the IoT-data shall be available on shore the optional key `sync2Shore` need to be specified.

##  Example

Internal communication between micro services, no data stored:

private/&lt;customerUUID>/mbb-modem/cpu-load

Internal communication between micro services, data stored for 30 days

on board:

- metadata: `private/meta/<customerUUID>/meta/mbb-modem/memory-usage`

- IoT data: `private/<customerUUID>/mbb-modem/memory-usage`

#  Alarm/Alert

Alarms value should contain this json structure:

```cpp

{
  "id":"<integer>",
  "header":"<string>",
  "address":"<string>",
  "status":"<boolean>",
  "severity":"<integer>",
  "acknowledged":{
    "status":"<boolean>",
    "time":<ISO 8601>,
    "byWhom":"<string> // only when status = true"
  },
  "description":"string",
  
}

```

This applies only for non private alarms/data.

Note that some of these keywords can be (or should be) part of metadata

if there is a

one to one relation between id or header and topic.

##  Alarm severity

| Value | Severity | Description |

| --- | --- | --- |

| 1 | Critical | |

| 2 | Major | |

| 3 | Minor | |

| 4 | Warning | |

| 5 | Indeterminate| Indicates that the severity of an alarm cannot be determined. |
