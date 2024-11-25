# Unit 4

## Branch Prediction

Branch prediction is an approach to computer architecture that attempts to mitigate the costs of branching.

It has to tackle two problems:

1. Direction predicting.
2. Calculating target address.

###### Need

- The gain produced by pipelining can be reduced by the presence of program branch instructions eg JMP, CALL, RET etc

- They change the sequence causing all the instructions that entered the pipeline after program transfer instructions invalid

- Thus no work is done as the pipeline stages are reloaded.

##### Logic

In this scheme, a prediction is made for the branch instruction currently in the pipeline.

If the prediction is true then the pipeline will not be flushed and no clock cycles will be lost.

If the prediction is false then the pipeline is flushed and starts over with the current instruction.

It is implemented using 4 way set associated cache with 256 entries. This is called **Branch Target Buffer (BTB)**. The directory entry for each line consists of:

-   **Valid bit:** Indicates whether the entry is valid or not.
-   **History bit:** Track how often bit has been taken.

##### Working

1. BTB is a lookaside cache that sits to the side of Decode Instruction(DI) stage of 2 pipelines and monitors for branch instructions.

2. The first time that a branch instruction enters the pipeline, the BTB uses its source memory to perform a lookup in the cache.

3. Since the instruction was never seen before, it is BTB miss. It predicts that the branch will not be taken even though it is unconditional jump instruction.

4. When the instruction reaches the EU(execution unit), the branch will either be taken or not taken. If taken, the next instruction to be executed will be fetched from the branch target address. If not taken, there will be a sequential fetch of instructions.

5. When a branch is taken for the first time, the execution unit provides feedback to the branch prediction. The branch target address is sent back which is recorded in BTB.

6. A directory entry is made containing the source memory address and history bit is set as strongly taken.

#### Types

There are two types of prediction techniques:

1. Static Branch Prediction
2. Dynamic Branch Prediction

##### Static Branch Prediction

Also known as Compile Time Branch Prediction.

The prediction decisions are made before execution and doesn't change during the execution.

It's implemented at software level.

It's generally implemented using compilers.

It's cheap and easy to implement than dynamic branch prediction, but have lower accuracy.

###### Schemes

- **Always not taken**
	
	The branch is never taken.
	
	Simple to implement: no need for BTB, no direction prediction
	
	Low accuracy: ~30-40%
	
	Compiler can layout code such that the likely path is the “not taken” path

- **Always taken**
	
	Same as 'Always no taken' except, the branch is always taken.

- **BTFN**
	
	Stands for Backward taken, forward not taken.
	
	Predict backward (loop) branches as taken, others not-taken

- **Profile based**
	
	Compiler determines likely direction for each branch using profile run.
	
	Encodes that direction as a hint bit in the branch instruction format.

##### Dynamic Branch Prediction

Also known as Runtime Branch Prediction.

Prediction decisions are made at runtime, while executing instructions.

It can adapt to dynamic changes in branch behavior

It's implemented at hardware level.

It's costly than static branch prediction, but the accuracy is high.

#### Direction Predictor

A direction predictor is a component of branch prediction in computer architecture that specifically focuses on predicting the outcome or direction of a branch instruction.

The goal of a direction predictor is to anticipate whether a branch will be taken (resulting in a change in program flow) or not taken (allowing the program to continue along its current path).

#### Heirarchical Predictor


### RAW Hazard

RAW stands for Read After Write.

It is the situation when one instruction depends on the result of a previous instruction that has not yet produced its result.

**Example**: If Instruction A writes a value to a register, and Instruction B reads from the same register, B is dependent on the result produced by A. If B is scheduled to execute before A has completed, a RAW hazard occurs.

### WAW Hazard

WAW stands for Write After Write.

It is the situation when two or more instructions attempt to write to the same destination register, and their executions overlap.

**Example**: If Instruction A and Instruction B both write to the same register, and they are scheduled to execute concurrently, a WAW hazard arises. The hazard results from potential inconsistencies in the final value stored in the register, depending on the order of execution.

## Duplicating Register Values

RAW dependency is considered true dependency, because this is how the program actually executes.

WAR and WAW are considered false dependencies because in reality there is no actual dependency, the so seemed dependency can be eliminated just by using different registers.

1.  **Duplicating Register Values**
    
    - **Technique:** In this approach, when a register is written to, multiple versions of the register are maintained, each associated with a specific write operation. Each version represents the state of the register after a particular write operation.
	
	- **Purpose:** The goal is to handle dependencies by allowing subsequent instructions to reference the correct version of the register based on the order of write operations.
	
	- **Example:** If an instruction writes to register R4 twice, two versions of R4 are maintained. Subsequent read operations will reference the appropriate version based on the chronological order of writes.

1.  **Renaming Register Values:**
    
    - **Technique:** Register renaming involves assigning a new, unique name (or identifier) to a register in each write operation. Instead of maintaining multiple versions of the physical register, a table (referred to as a rename table or register alias table) is used to keep track of the mapping between logical registers and their corresponding physical register instances.
	
	- **Purpose:** The primary goal is to eliminate data hazards by allowing multiple instructions to write to the same logical register without causing conflicts.
	
	- **Example:** If an instruction writes to register R4 twice, the register renaming mechanism assigns different physical registers to each write operation and updates the rename table accordingly. Subsequent read operations reference the logical register name, and the table ensures the correct physical register is used.
