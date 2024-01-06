# Unit 4

## Branch Prediction

Branch prediction is an approach to computer architecture that attempts to mitigate the costs of branching.

###### Need

- The gain produced by pipelining can be reduced by the presence of program branch instructions eg JMP, CALL, RET etc

- They change the sequence causing all the instructions that entered the pipeline after program transfer instructions invalid

- Thus no work is done as the pipeline stages are reloaded.

###### Logic

In this scheme, a prediction is made for the branch instruction currently in the pipeline.

If the prediction is true then the pipeline will not be flushed and no clock cycles will be lost.

If the prediction is false then the pipeline is flushed and starts over with the current instruction.

