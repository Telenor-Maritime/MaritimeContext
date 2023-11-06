# MC Howto 

## 1. Define topic in Client Managment 
	Example topic: 'MC/V1/first_level/second_level/third_level/end_level'

## 2. At startup of app publish metadata 
	Example topic metadata (corspond to the topic above): 'MC/V1/**meta/**first_level/second_level/third_level/end_level'. 
	The metadata should be verified towards the json schema 'metadata_json_schema.json'. Note that most fields are optional.
	If data shall be synced to shore the 'sync2Shore' field must be present. Please see 'MaritimeContext.md' for more information. 
	If metadata needs to be changed, just republish it. The UHS will keep controll over which data that are connected to which version of metadata. 
	If metadata is republished unchanged, no change will be done.

## 3. Publish payload on topic 
	Then it is time to publish data. The payload must follow the format spesified in 'MaritimeContext.md'. 
	The values can be any of string, json, integer or float. Note that it must match what is spesified in metadata.
