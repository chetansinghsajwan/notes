# Unit 1

## Moore's Law

Moore's law is a term used to refer to the observation made by Gordon Moore in 1965 that the number of transistors in a dense integrated circuit (IC) doubles about every two years.

## Parallelism

There are basically 4 types of parallelism:

1. Bit-level parallelism
2. Instruction-level parallelism
3. Task Parallelism
4. Data-level parallelism

### Instruction Level Parallelism

###### Advantages

- Improved Performance: By allowing multiple instructions to be executed simultaneously or out-of-order.

- Efficient Resource Utilization: Reduces resource wastage (like cpu idle) and increase efficiency.

- Reduced Instruction Dependency: Reduces the number of instruction dependencies, which can limit the amount of instruction-level parallelism that can be used.

- Increased Throughput: Increases the overall throughput of processors by allowing multiple instructions to be executed simultaneously.

###### Disadvantages

- Increased Complexity: Implementing ILP can be complex and requires additional hardware resources, which can increase the complexity and cost of processors.

- Instruction Overhead: ILP can introduce additional instruction overhead, which can slow down the execution of some instructions and reduce performance.

- Data Dependency: Data dependency can limit the amount of instruction-level parallelism that can be exploited.

- Reduced Energy Efficiency: Can increase power consumption and result in higher energy costs.

