# JsonSerialStream
A library for Arduinos to output data in JSON format through the board's serial streams.

<details>

<summary> JSON Format Explained </summary>

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
</details>

### Things to note
* The serial stream must be initilized prior to the library being used.
* The library has no malformed output protection. All opened messages and nested objects must be closed.
* The data is output immediately as methods are called; the data is never stored. This improves performance.

### To include:
The recommended way of including this library into your sketch is to:
1) Creating a `src` folder in your sketch's directory
2) Cloning this repository into that `src` folder. 
3) In your sketch, add `#include "src/JsonSerialStream/JsonSerialStream.h"`

This results in a structure that looks like this:
+ ExampleSketch
  + ExampleSketch.ino
  + src
    + JsonSerialStream
      + JsonSerialStream.h
      + JsonSerialStream.cpp
      + LICENSE
      + README.md
      + .gitignore

You may also clone this repository into your Arduino libaries directory. Or you can extract the `.h` and `.cpp` and put them into your sketch directory.
These are not advised as they can get cluttered, are hard to update, or may result in "works on my machine" as a result of different installed libraires.

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