# Data Structure Alignment

- **Data structure alignment** is the way data is arranged and accessed in [computer memory](https://en.wikipedia.org/wiki/Computer_memory). It consists of three separate but related issues: **data alignment**, **data structure padding**, and **packing**.

- The [CPU](https://en.wikipedia.org/wiki/Central_processing_unit) in modern computer hardware performs reads and writes to memory most efficiently when the data is _naturally aligned_, which generally means that the data's memory address is a multiple of the data size. For instance, in a 32-bit architecture, the data may be aligned if the data is stored in four consecutive bytes and the first byte lies on a 4-byte boundary.

- Although data structure alignment is a fundamental issue for all modern computers, many computer languages and computer language implementations handle data alignment automatically. [Fortran](https://en.wikipedia.org/wiki/Fortran), [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)),[[v.9]](https://en.wikipedia.org/wiki/Data_structure_alignment\#cite_note-1)[[2]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-2) [PL/I](https://en.wikipedia.org/wiki/PL/I),[[3]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-3) [Pascal](https://en.wikipedia.org/wiki/Pascal_(programming_language)),[[4]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-4) certain [C](https://en.wikipedia.org/wiki/C_(programming_language)) and [C++](https://en.wikipedia.org/wiki/C%2B%2B) implementations, [D](https://en.wikipedia.org/wiki/D_(programming_language)),[[5]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-5) [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language)),[[6]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-6) [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)),[[7]](https://en.wikipedia.org/wiki/Data_structure_alignment#cite_note-7) and [assembly language](https://en.wikipedia.org/wiki/Assembly_language) allow at least partial control of data structure padding, which may be useful in certain special circumstances.

- A memory address _a_ is said to be _n-_[_byte_](https://en.wikipedia.org/wiki/Byte) _aligned_ when _a_ is a multiple of _n_ (where _n_ is a power of 2). In this context, a byte is the smallest unit of memory access, i.e. each memory address specifies a different byte. An _n_-byte aligned address would have a minimum of log2(_n_) least-significant zeros when expressed in [binary](https://en.wikipedia.org/wiki/Binary_numeral_system).

- The alternate wording _b-bit aligned_ designates a _b/8 byte aligned_ address (ex. [64-bit](https://en.wikipedia.org/wiki/64-bit) aligned is 8 bytes aligned).

- Note that by definition byte memory accesses are always aligned.

- Problems:
    - If the highest and lowest bytes in a datum are not within the same  
        memory word the computer must split the datum access into multiple  
        memory accesses.
    - When a single memory word is accessed the operation is atomic, i.e. the whole memory word is read or written at once and other devices must wait until the read or write operation completes before they can access it. This may not be true for unaligned accesses to multiple memory words, e.g. the first word might be read by one device, both words written by another device and then the second word read by the first device so that the value read is neither the original value nor the updated value. Although such failures are rare, they can be very difficult to identify.

- Natural alignment refers to the size of the variable, not the size of the processor register and/or data path. A floating point `double` is 8 bytes, and so its natural alignment is 8 bytes. To be more precise, the natural alignment is the smallest power of 2 that is large enough to hold the variable.

## For Tommorow

- https://developer.ibm.com/articles/pa-dalign

- https://stackoverflow.com/questions/381244/purpose-of-memory-alignment

## References

- https://en.wikipedia.org/wiki/Data_structure_alignment

- https://developer.ibm.com/articles/pa-dalign

- https://hackaday.com/2022/05/10/data-alignment-across-architectures-the-good-the-bad-and-the-ugly

- https://stackoverflow.com/questions/38875369/what-is-data-alignment-why-and-when-should-i-be-worried-when-typecasting-pointe

- https://stackoverflow.com/questions/2846914/what-is-meant-by-memory-is-8-bytes-aligned

## References to write from

- https://www.quora.com/What-is-natural-alignment-Why-should-a-generic-pointer-be-aligned/answer/Robert-Love-1?ch=10&oid=1525840&share=24546538&srid=hg6k4m&target_type=answer

- https://stackoverflow.com/questions/4306186/structure-padding-and-packing

- https://leimao.github.io/blog/CPP-Data-Alignment

- https://stackoverflow.com/questions/17091382/memory-alignment-how-to-use-alignof-alignas
