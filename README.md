# pi-mqtt
A node.js module to standardize communication with the mqtt server

## MQTT topic structures:

### devices

### devices/\<deviceID>
Each device conneced has a unique ID assigned in code/firmware

### devices/\<deviceID>/name
Device name.
* Should be set by device on first connection, if not already set
* Should be retained

### devices/\<deviceID>/status
Current device status. Usually "online" or "offline".
* Should be retained
* Should be set to "offline" by MQTT Last Will

### devices/\<deviceID>/config/\<configTopics>
Device-specific configuration topics
* If any required configuration is not set, the device should use internal defaults, but should not update the missing topic.
* Should be retained
* Should be used to set desired values (use the sensors to report actual values)

### devices/\<deviceID>/\<sensorSet>
A device may have multiple sensors of the same type. Thus they can be organized into logical sets.
* Each device should have at least one sensor set.
* Each sensorSet topic should be unique to the device
* Devices may publish a human-friendly name to the sensorSet topic itself, which should be retained
 
### devices/\<deviceID>/\<sensorSet>/\<sensorName>
The actual sensor data. Depending on how this is organized, there may be subtopics. Some examples follow.
* Will generally be retained, although one should be careful about stale data
* These should use standard names across all devices and sensor sets
* Use to report actual values. A properly authenticated system will only allow the sensor to write to these. Use config topics to set desired values

`devices/<deviceID>/<sensorSet>/temperature`
`devices/<deviceID>/<sensorSet>/presence/daniel`
`devices/<deviceID>/<sensorSet>/presence/sarah`
