+++
title = "The Beginner's Guide to Hex Dumps"
date = 2024-06-08T12:31:07-05:00
+++

# A Beginner's Guide to Hex Dumps

## What is Hex?

Hex, short for hexadecimal, is a base-16 number system. Unlike our usual base-10 system (which counts 0-9), hex counts from 0-9 and then uses A-F to represent 10-15.

## Converting Between String, Hex, and Binary

Let's see how this works with a simple example. Take the string "Hi":

1. **String to Hex**:
   - "H" is 0x48
   - "i" is 0x69
   - So, "Hi" in hex is 0x4869.

2. **Hex to Binary**:
   - 0x48 in binary is 0100 1000
   - 0x69 in binary is 0110 1001
   - So, "Hi" in binary is 0100 1000 0110 1001.

In summary:
- **String**: "Hi"
- **Hex**: 0x4869
- **Binary**: 01001000 01101001

Hex dumps use this hex format to display binary data in a way that's easier for humans to read.

# Creating a Hex Dump with xxd

Next, let's create a hex dump using the `xxd` tool.  This tool can be used on Linux, Mac, or Windows Subsystem for Linux (WSL). 

1. **Create a Text File**:
   Open a terminal window and create a simple ASCII text file named `example.txt` with the content "Hello, Hex Dump!" using the `echo` command:
   ```bash
   echo "Hello, Hex Dump!" > example.txt
   ```
   
2. Generate the Hex Dump:
Use the xxd command to generate a hex dump of the example.txt file. Here's the command:
```bash
xxd example.txt
```

Running this command will display the hex dump of the example.txt file, showing both the hexadecimal representation and the ASCII text side by side.

Here's what the output might look like:

```
00000000: 4865 6c6c 6f2c 2048 6578 2044 756d 7021  Hello, Hex Dump!
00000010: 0a                                       .
```

In this output:

- 00000000 is the offset in hexadecimal.
- 4865 6c6c 6f2c 2048 6578 2044 756d 7021 represents the hexadecimal bytes of the ASCII text.
- Hello, Hex Dump!\n is the ASCII representation.

# Using Hex Dumps with Protobuf

## What is Protobuf?

Protobuf, short for Protocol Buffers, is a method for serializing structured data. It uses a binary format to efficiently store and exchange data between different systems. For a detailed understanding of Protobuf, you can refer to the [Protobuf documentation](https://developers.google.com/protocol-buffers).

## Viewing a Protobuf Object with Hex Dump

Let's consider an example where we have a Protobuf object defined as follows(in a`example.proto` file:

```protobuf
syntax = "proto3";

message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
```

We can use Python to serialize and instance to binary and then view the hex dump.

1. Compile the Protobuf Definition:\
Compile the example.proto file to generate the necessary Protobuf code (e.g., example_pb2.py for Python). Use the Protobuf compiler (protoc) for this step.

```bash
protoc --proto_path=. --python_out=. example.proto
```

Serialize the Protobuf Object:\
Use the generated Protobuf code to serialize the Person object to binary format. Here's an example using Python:

```python
from example_pb2 import Person

# Create a Person object and populate it with data
person = Person()
person.name = "John"
person.id = 1234
person.email = "john@example.com"

# Serialize the object to binary format
serialized_data = person.SerializeToString()

# Write the serialized data to a file (e.g., person_data.bin)
with open("person_data.bin", "wb") as f:
    f.write(serialized_data)
```

3. View the Hex Dump:\
After serializing the Person object to binary (person_data.bin), you can use the xxd command to view its hex dump:

```bash
xxd person_data.bin

00000000: 0a 04 4a 6f 68 6e 12 05 31 32 33 34 15 08 6a 6f  ..John..1234..jo
00000010: 68 6e 40 65 78 61 6d 70 6c 65 2e 63 6f 6d        hn@example.com
```

Each line in the output represents a chunk of binary data, where:

0a is the field tag for the name field (string type). (A field tag is a protobuf specific concept that combines the field number defined in the proto object with its associated data type)\
04 indicates the length of the following string ("John" in ASCII).\
12 is the field tag for the id field (int32 type).\
05 indicates the length of the following varint (1234 in varint encoding).\
15 is the field tag for the email field (string type).\
The rest are the bytes representing the ASCII characters of the email address ("john@example.com").\
This hex dump helps us visualize how Protobuf encodes and stores data in its binary format.\

The reason you don't see the field tags in the ASCII output, is because `0a` does not have an [associated ASCII character](https://www.freecodecamp.org/news/ascii-table-hex-to-ascii-value-character-code-chart-2/).  Anytime a hex dump has a non ASCII character you will see `.` instead.

# Hex Dumps for Non-ASCII Files

Hex dumps are not limited to ASCII text files; they are incredibly useful for analyzing and understanding the structure of binary files as well. When dealing with non-ASCII files, such as images, executables, or compressed archives, hex dumps provide a way to peek into their raw data.

## Magic Numbers

In the realm of binary files, "magic numbers" play a crucial role. These are specific byte sequences at the beginning of a file that identify its format or type. Hex dumps help in identifying these magic numbers, which are like signatures that reveal the file's content or purpose.

For a comprehensive list of magic numbers and their associated file formats, you can refer to the [Magic Number (programming)](https://en.wikipedia.org/wiki/Magic_number_(programming)) Wikipedia page.

## Demo: Analyzing an Image File

Let's dive into an interesting demo where we use a hex dump to analyze an image file. We'll take a simple PNG image and inspect its hex dump to understand its structure.

1. **Download an Image**:
   Start by downloading a sample PNG image. You can use any image you have or download one from the internet.

2. **Generate the Hex Dump**:
   Once you have the image file (let's say it's named `example.png`), open a terminal and run the following command to generate its hex dump:
   ```bash
   xxd example.png
   ```

   The hex dump will display the raw binary data of the PNG file, including the magic number and various chunks that make up the image.

3. Interpret the Hex Dump:
Look for the PNG magic number "89 50 4e 47 0d 0a 1a 0a" at the beginning of the hex dump. This sequence identifies the file as a PNG image.

    Explore other parts of the hex dump to see how different chunks such as headers, metadata, and image data are represented in binary format.

Using hex dumps in this way gives you insights into the internal structure of binary files, making them a valuable tool for file analysis and reverse engineering.

# Conclusion: Exploring Hex Dumps
Throughout this guide, we've delved into the functionality of hex dumps in deciphering binary data. From foundational hex knowledge to practical Protobuf analysis, we've gained a comprehensive understanding of digital encoding.

Hex dumps are essential for examining non-ASCII files, identifying unique file signatures, and comprehending binary file structures. They're indispensable tools for programmers, analysts, and enthusiasts.

With this knowledge, you're equipped to dive into the world of hex dumps and unlock a deeper understanding of binary data!