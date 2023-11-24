# Software Design Approaches
### Top Down Approach
This design technique is entirely focused on first subdividing the system into subsystems and components. The top-down approach conceptualizes the entire system first and then divides it into multiple subsystems. These subsystems are then designed and separated into smaller subsystems and sets of components that meet the larger system's requirements.
### Bottom-Up Approach
This system design technique prioritises the design of subsystems and the lowest-level components (even sub-components). Higher-level subsystems and larger components can be produced more readily and efficiently if these components are designed beforehand. This reduces the amount of time spent on recon and troubleshooting. The process of assembling lower-level components into larger sets is repeated until the entire system is composed of a single component. This design technique also makes generic solutions and low-level implementations more reusable.
# System Design Strategies
### Structured Design
Structured design is primarily about breaking problems down into several well-organised components.
Structured design is primarily based on the **divide and conquer** technique, in which a large problem is divided into smaller ones, each of which is tackled independently until the larger problem is solved.
Solution modules are used to address the individual problems.
### Function Oriented Design
Function-oriented design is related to **structured design** in that it splits the entire system into subsystems known as functions.
Uses Top-Down approach.
### Object Oriented Design
Treats each entity as an object.
Each object has its own functionality and encapsulation.
Objects communicate with each other by sending messages or commands, like functions.
The object-oriented design technique is considered superior to the function-oriented design approach because real-world entities may be easily incorporated in the computer world.This method also allows for the implementation of several very basic object behaviors like as polymorphism, inheritance, abstraction, and encapsulation.
# Coupling and Cohesion
|Coupling|Cohesion|
|---|---|
|Coupling is also called Inter-Module Binding.|Cohesion is also called Intra-Module Binding.|
|Coupling shows the relationships between modules.|Cohesion shows the relationship within the module.|
|Coupling should be low.|Cohesion should be high.|
### Advantages of low coupling:
- Maintainable, does not affect others on change.
- Isolation testing and development
- Scalable
### Advantages of high cohesion:
- Resolves circular dependencies.
- Improves code reusability.
- Single task.
### Disadvantages of high coupling:
- Increased complexity
- Reduced flexibility
- Decreased modularity
### Disadvantages of low cohesion:
- Increased code duplication
- Reduced functionality
# Software Measurement
A software metric is a measure of software characteristics which are measurable or countable. Software metrics are valuable for many reasons, including measuring software performance, planning work items, measuring productivity, and many other uses.
### Attribute of Software metrics
1. Size
2. Quantity
3. Time
4. Complexity
5. Effort
### Classification of Software Metrics
1. **Product Metrics:** Product metrics are used to evaluate the state of the product, tracing risks and undercover prospective problem areas. The ability of the team to control quality is evaluated.
    - Lines of code
    - Cyclomatic complexity
    - Performance
    - Reliability
    - Functionality
    - Design
2. **Process Metrics:** Process metrics pay particular attention to enhancing the long-term process of the team or organization.
    - Effort required
    - Time to complete
    - Tools and Technology
    - No. of defects
3. **Project Metrics:** The project matrix describes the project characteristic and execution process.
    - Number of software developer
    - Cost
    - Time schedule
    - Productivity
### Advantages of Software Metrics
1. Reduction in cost or budget.
2. It helps to identify the particular area for improvising.
3. It helps to increase the product quality.
4. Managing the workloads and teams.
5. Reduction in overall time to produce the product,.
6. It helps in providing effective planning, controlling and managing of the entire product.
### Disadvantages of Software Metrics
1. It is expensive and difficult to implement the metrics in some cases.
2. Performance of the entire team or an individual from the team can’t be determined. Only the performance of the product is determined.
3. It leads to measure the unwanted data which is wastage of time.
4. Measuring the incorrect data leads to make wrong decision making.
## Code Flow Graph
Represents the flow of logic in the program.
Same as flowchart.
## Cyclometic Complexity
Cyclometic Complexity = Num of Edges in CFG - Num of Nodes in CFG + 2
It should be lower.