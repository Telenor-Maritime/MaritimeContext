# MQTT Interface


## Topic Definition

The topic used for publishing of data is the same as the naming of the specific datapoint.

- For MC formatted payload, the topic must follow the MC namingrule. This namingrule is not very defined yet, so the naming of the datapoints is somewhat open.
- ISO formatted payload must follow either follwo the VIS or JSMEA namingscheme, this are well defined naming convencions. 


## Payload

UHS supports two differnet payload formats on the MQTT bus, Maritime Context and ISO 19848. It is required to use one of these.

