---
Creation Time: Friday, September 20th 2024
Modified Time: Friday, September 20th 2024
---
Cap’n Proto is an insanely fast data interchange format and capability-based RPC system. This interchange data format is designed to be fast by avoiding need for parsing or serialising data.
- **No parsing**: Cap'n Proto directly maps the serialized data to memory structures without needing to parse or serialize.
- **Fast**: It's very efficient for both read and write operations due to its zero-copy design.
- **Compact**: Data is stored compactly without needing additional metadata during serialization.

commonly used in systems that require high-performance data exchange, such as RPC systems, logging, or low-latency services.

Example:
```capnp
struct Person {
	name @0 :Text;
	age @1 :Int32; 
	friends @2 :List(Person); 
}
```


We can use capnp compiler to compile this message to any language.
