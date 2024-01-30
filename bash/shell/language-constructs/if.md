# `if` construct

##### Syntax

`if` test-commands `; then`
  consequent-commands `;`
[`elif` more-test-commands `; then`
  more-consequents `;`]
[`else` alternate-consequents `;`]
`fi`

##### Description

The test-commands list is executed, and if its return status is zero, the consequent-commands list is executed. If test-commands returns a non-zero status, each `elif` list is executed in turn, and if its exit status is zero, the corresponding more-consequents is executed and the command completes. If ‘else alternate-consequents’ is present, and the final command in the final `if` or `elif` clause has a non-zero exit status, then alternate-consequents is executed. The return status is the exit status of the last command executed, or zero if no condition tested true.

##### References

- https://www.gnu.org/software/bash/manual/bash.html#index-if