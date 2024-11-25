# Optimization Techniques

#### Premature optimization is the root of all evil**

Donald Knuth wrote, "Programmers waste enormous amounts of time thinking about, or worrying about, the speed of noncritical parts of their programs, and these attempts at efficiency actually have a strong negative impact when debugging and maintenance are considered. We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.”

In general prioritize correctness and readability over code performance in most cases. For a small fraction of your code, you may have to sacrifice readability to improve performance. Such optimizations should be carried out when the project is nearing completion. You have a better idea of the performance critical code when you have a working system.
That said, it is important to recognize that many optimization techniques are just sound programming practices as they improve performance as well as code readability. Such techniques should be applied right from the project start.

#### Adjust structure sizes to power of two

When arrays of structures are involved, the compiler performs a multiply by the structure size to perform the array indexing. If the structure size is a power of 2, an expensive multiply operation will be replaced by an inexpensive shift operation. Thus keeping structure sizes aligned to a power of 2 will improve performance in array indexing.

#### Place case labels in narrow range

If the case labels are in a narrow range(, the compiler does not generate an if-else-if cascade for the switch statement. Instead, it generates a jump table of case labels along with manipulating the value of the switch to index the table. This code generated is faster than if-else-if cascade code that is generated in cases where the case labels are far apart. Also, performance of a jump table based switch statement is independent of the number of case entries in switch statement.

## References

> [[https://www.eventhelix.com/embedded/optimizing-c-and-cpp-code/]]