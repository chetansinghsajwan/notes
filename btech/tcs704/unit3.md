# Unit 3

## Pipelining

Pipelining is the technique of decomposing a sequential process into sub-operations, with every sub-operation being executed concurrently in a dedicated segments.

In general, the pipeline organization is applicable for two areas of computer design:

1. Arithmetic Pipeline
2. Instruction Pipeline

### Arithmetic Pipeline

### Instruction Pipeline

## Performance Issues in Pipelining

## Pipeline Hazards

Types of pipelilne hazards:

1. Structural Hazard
2. Data Hazard
3. Control Hazard

### Structural Hazard

It is the situation when more than one instruction tries to access the same resource in the same cycle.

This resources could be ALU, memory or register.

### Data Hazard

It is the situation when the execution of instruction depends on the result of another instruction that is still being processed in the pipeline.

### Control Hazard

Also known as branch hazards.

It is the situation when the pipeline makes incorrect branch prediction decisions, resulting in instructions entering the pipeline that must be discarded.

#### Solutions

##### Stall
##### Prediction
##### Dynamic Branch Prediction
##### Reordering Instructions