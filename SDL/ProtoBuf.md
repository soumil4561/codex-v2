---
Created: 2025-03-15
tags:
---
Protocol Buffers are a language-neutral, platform-neutral extensible mechanism for serializing structured data. 
It’s like JSON, except it’s smaller and faster, and it generates native language bindings. You define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages.

### Advantages of Protocol Buffers

#### 1. Serialization format for typed, structured data
Protobuf allows you to define the structure of your data using a `.proto` file.

- You specify **fields** with specific types (like `int32`, `string`, `bool`, etc.).
- The `protoc` compiler generates source code that lets you easily serialize (encode) and deserialize (decode) the data in an efficient binary format.
#### 2. Packets of data up to a few megabytes in size
Protobuf is designed to handle small to medium-sized payloads efficiently (usually up to a few MB). For example, it's ideal for network messages, API responses, or inter-service communication.

#### 3. Suitable for both ephemeral network traffic and long-term storage
- **Ephemeral traffic** → Protobuf is widely used in gRPC and other RPC systems for sending messages between services.
- **Long-term storage** → You can store serialized protobuf data in a database or file and read it back later.
- Since protobuf is a binary format, it’s compact and faster to encode/decode than something like JSON or XML.
#### 4. Extendable without breaking compatibility
Protobuf is **forward and backward compatible**:
- You can add new fields to an existing `.proto` definition without breaking old code.
- Old data (that doesn't have the new fields) can still be read by newer code.
- Unknown fields are ignored when parsing, so older code can handle newer data without failing.

>[!What is Serialization?]
>Serialization is the process of **converting an object or structured data into a format** that can be: Stored (in a file, database, etc.),Transmitted (over a network), Reconstructed (deserialized) later
>Imagine you have a Python object like :

```python
data = {
    'id': 1,
    'name': 'Alice',
    'active': True
}
```

>[!Continue]
>If you want to send this data over a network or store it in a file, you can’t just transmit Python objects directly — they need to be converted into a format that can be easily transferred or stored.
>Options:
>1. **JSON:** Converts the object to a string
>2. **Protobuf:** Converts the object into a **compact binary format** (more on that next).


Protocol buffers are ideal for any situation in which you need to serialize structured, record-like, typed data in a language-neutral, platform-neutral, extensible manner. They are most often used for defining communications protocols (together with gRPC) and for data storage.

### When are Protobuf not a good fit
Protocol buffers do not fit all data. In particular:

- Protocol buffers tend to assume that entire messages can be loaded into memory at once and are not larger than an object graph. For data that exceeds a few megabytes, consider a different solution; when working with larger data, you may effectively end up with several copies of the data due to serialized copies, which can cause surprising spikes in memory usage. Protobuf follows a **copy-on-write** style when processing data:

	- When you serialize a message, it creates a copy of the original object.
	- When you deserialize a message, it creates another copy to parse it into an object.

- When protocol buffers are serialized, the same data can have many different binary serializations. You cannot compare two messages for equality without fully parsing them. Fields can be written out in **any order** and still be valid because the field numbers are encoded in the message.
	**How to parse protobuf messages**
	To check for equality between two Protobuf messages, you need to:
	1. **Parse both messages** into objects.
	2. Compare the parsed objects using the built-in equality check.
	3. Protobuf implementations usually override `equals()` (in Java) or `==` (in Python/Go) to handle this.
- Messages are not compressed. While messages can be zipped or gzipped like any other file, special-purpose compression algorithms like the ones used by JPEG and PNG will produce much smaller files for data of the appropriate type.

### How do Protocol Buffers work?
![[ProtoBuf-1742033516353.webp]]

`.pb` files are **Protocol Buffer (Protobuf) serialized files** — they store data that has been serialized using the Protobuf format.
- A `.pb` file contains **binary data** encoded in the Protocol Buffers format.
- It is the result of **serializing a Protobuf message** into a compact, efficient binary format.
- `.pb` files are:
    - **Compact** → Smaller than JSON or XML
    - **Fast to read/write** → Optimized binary format
    - **Cross-platform** → Can be read/written across different languages (Java, Python, Go, etc.)


### Proto Language Definitions

#### Cardinality
You can define three types of cardinality in `.proto` files:

|Cardinality|Description|Example|
|---|---|---|
|**singular**|Field holds **at most one value**. Default value applies if unset.|`string name = 1;`|
|**optional**|Field holds **at most one value** but tracks explicit presence. Allows checking if the value was set or not.|`optional string name = 1;`|
|**repeated**|Field holds **zero or more values** (like a list/array).|`repeated string tags = 3;`|

#### Implicit/Explicit Presence
In **proto3**, fields without `optional` have **implicit presence** — Protobuf cannot distinguish between:
- A field being **unset** vs.
- A field being set to its **default value**

#### **Proto2 vs Proto3 Differences**
| Feature                    | Proto2                        | Proto3                                      |
| -------------------------- | ----------------------------- | ------------------------------------------- |
| **Default cardinality**    | `optional` is default         | `singular` is default                       |
| **Explicit `optional`**    | Always supported              | Introduced in proto3 in 2020                |
| **Default value tracking** | `hasField()` always available | `hasField()` only available with `optional` |
| **Extension fields**       | Supported                     | Removed (use `Any` instead)                 |

After setting the cardinality of a field, you specify the data type. Protocol buffers support the usual primitive data types, such as integers, booleans, and floats. See the [types](https://protobuf.dev/programming-guides/proto3/#scalar)

A field can also be of:

- A `message` type, so that you can nest parts of the definition, such as for repeating sets of data.
- An `enum` type, so you can specify a set of values to choose from.
- A `oneof` type, which you can use when a message has many optional fields and at most one field will be set at the same time.
- A `map` type, to add key-value pairs to your definition.

**Message**

```proto
syntax = "proto3";

message Address {
  string city = 1;
  string country = 2;
}
```

**Enum**

```proto
syntax = "proto3";

enum Status {
  UNKNOWN = 0;
  ACTIVE = 1;
  INACTIVE = 2;
}

message User {
  int32 id = 1;
  Status status = 2;
}
```

**OneOf**
```proto
syntax = "proto3";

message User {
  int32 id = 1;
  oneof contact_info {
    string email = 2;
    string phone = 3;
  }
}
```

**Map**
```proto
syntax = "proto3";

message User {
  int32 id = 1;
  map<string, string> metadata = 2;
}
```

- Keys **must be unique**.
- Keys are always stored in **sorted order** when serialized.
- Keys can only be **integers** or **strings** — no complex types.

Messages → Hierarchical data  
Enums → Fixed set of constants  
Oneof → Mutually exclusive fields  
Maps → Key-value pairs

