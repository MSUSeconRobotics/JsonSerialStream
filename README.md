# JsonSerialStream
A library for Arduinos to output data in JSON format through the board's serial streams.

## JSON Format
**J**ava**S**cript **O**bject **N**otation is standardize format of conveying large amounts of data and objects as serialized bytes. There exist libraries in almost any langauge toconvert JSON data back into an object. This essentially makes an easy way to send data as string bytes between devices, while maintaining a human readable format.  
Note that JSON data is sent without whitespace, but it is often added for readability.

#### Examples:
Simple message
```JSON  
{"item1":"An Example"}
```

---
Nested objects
```JSON
{
  "1": 1,
  "2": true,
  "3": {
    "bool as string": "true",
    "num as string": "1"
  }
}
```
  
---
Arrays
```JSON
{
  "Streams": [
    {"1": {"Data": "ipsom lorem"} },
    {"2": {}}
  ]
}
```

## How to use
1) Attach the JSON stream object to a serial stream with `JsonSerialStream(serial)` where the serial stream may be: `Serial`, `Serial1`, `Serial2`, or `Serial3`
2) Open a JSON message to send with `.openMessage()` which will send an open brace indicating a new JSON object
3) Add message contents:
   1) Simple properties. Formed as `"Name":value` with `.addProperty(name, value)`
   2) Nested objects. Which may contain sets of properties (_or more nested objects_) within themselves. To open one use `.addNestedObject(name)`, add the interior values, and close with `.closeNestedObject()`
   3) No properties. An empty JSON message is well formed as long as the message contains `{}\n` 
4) Close the message with `.closeMessage()`

#### Examples

Simple message
```c#
JsonSerialStream output = JsonSerialStream(Serial);

output.openMessage();
output.addProperty("item1","An Example");
output.closeMessage();
```
```JSON  
{"item1":"An Example"}
```

---
Nested objects
```c#
JsonSerialStream output = JsonSerialStream(Serial);

output.openMessage();
output.addProperty("1",1);
output.addProperty("2",false);

output.addNestedObject("3");
output.addProperty("bool as string","true");
output.addPropertyAsString("int as string",1);
output.closeNestedObject();

output.closeMessage();
```
```JSON
{
  "1": 1,
  "2": true,
  "3": {
    "bool as string": "true",
    "num as string": "1"
  }
}
```

---
Arrays
**! Currently unsupported !**