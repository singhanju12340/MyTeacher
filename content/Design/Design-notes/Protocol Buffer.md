To make remote procedure calls, we need a way to transform objects (payload) in memory into bytes such that they can be transmitted to the other systems as shown below. There are many names for it: serilization, marshaling, encoding, etc
![[Screenshot 2024-08-08 at 1.18.27 PM.png]]

> The common serialised format in use is JSON. But JSON is slower and bigger in size. mostly when we use it with big data or when we need to communicate within micro services.
> JSON also has forward and backward compatibility issues. 


Protocol buffers are Google’s language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster, and simpler. You define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages.





Protocol buffers support generated code in C++, C#, Dart, Go, Java, Kotlin, Objective-C, Python, and Ruby. With proto3, you can also work with PHP.

## Example Implementation
 Person John Java data is converted to output byte info and can be read and parsed in C code as well.

**Figure 1.** A proto definition.

```proto
message Person {
  optional string name = 1;
  optional int32 id = 2;
  optional string email = 3;
}
```



**Figure 2.** Using a generated class to persist data.
```java
// Java code
Person john = Person.newBuilder()
    .setId(1234)
    .setName("John Doe")
    .setEmail("jdoe@example.com")
    .build();
output = new FileOutputStream(args[0]);
john.writeTo(output);
```

**Figure 3.** Using a generated class to parse persisted data.

```cpp
// C++ code
Person john;
fstream input(argv[1],
    ios::in | ios::binary);
john.ParseFromIstream(&input);
id = john.id();
name = john.name();
email = john.email();
```


