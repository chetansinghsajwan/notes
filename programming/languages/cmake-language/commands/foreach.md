# `foreach()` command

Evaluate a group of commands for each value in a list.

The commands [`break()`](https://cmake.org/cmake/help/latest/command/break.html#command:break "break") and [`continue()`](https://cmake.org/cmake/help/latest/command/continue.html#command:continue "continue") provide means to escape from the normal control flow.

### `foreach(<loop_var> <items>)`

#### Parameters

##### `<loop_var>`

Value holding current iteration value.

##### `<items>`

Whitespace or semicolon separated list of items.

### `foreach(<loop_var> RANGE <stop>)`

Iterates over the numbers 0, 1, ... up to (and including) the nonnegative integer `<stop>`.

### `foreach(<loop_var> RANGE <start> <stop> [<step>])`

iterates over the numbers from `<start>` up to at most `<stop>` in steps of `<step>`.

#### Parameters

##### `<loop_var>`

Variable holding current iteration value.

##### `<start>`

The  start of the range.

This should be a non negative value.

##### `<stop>`

The end value of the range.

This should be a non negative value.

##### `<step>` #optional 

The amount of step between iterations.

By default it is 1.

This should be a non negative value.

### `foreach(<loop_var> IN [LISTS [<lists>]] [ITEMS [<items>]])`

#### Parameters

##### `<loop_var>`

Value holding current iteration value.

##### `<lists>` #optional 

Whitespace or semicolon separated list of list-valued variables.

##### `<items>` #optional 

Whitespace of semicolon separated list of values.

#### Example

```cmake

	set(A 0;1)
	set(B 2 3)
	set(C "4 5")
	set(D 6;7 8)
	set(E "")
	foreach(X IN LISTS A B C D E)
	    message(STATUS "X: ${X}")
	endforeach()

	# Prints

	# X: 0
	# X: 1
	# X: 2
	# X: 3
	# X: 4 5
	# X: 6
	# X: 7
	# X: 8

```

### `foreach(<loop_var>... IN ZIP_LISTS <lists>)`

The `foreach` command iterates over each list simultaneously setting the iteration variables.

#### Parameters

##### `<loop_var>...`

Variable holding current iteration value.

If single variable is specified, then the current value for each list is addressed by `loop_var_N`, where `N` represents lists index.

If multiple variables are specified, then their count should match the number of `<lists>`. Each variable will hold the current iteration value for their corresponding list.

##### `<lists>`

Whitespace or semicolon separated list of list-valued variables.

if any of the lists are shorter, the corresponding iteration variable is not defined for the current iteration.

#### Example

```cmake

	list(APPEND English one two three four)
	list(APPEND Bahasa satu dua tiga)
	
	foreach(num IN ZIP_LISTS English Bahasa)
	    message(STATUS "num_0=${num_0}, num_1=${num_1}")
	endforeach()
	
	foreach(en ba IN ZIP_LISTS English Bahasa)
	    message(STATUS "en=${en}, ba=${ba}")
	endforeach()

	# Prints
	
	# num_0=one, num_1=satu
	# num_0=two, num_1=dua
	# num_0=three, num_1=tiga
	# num_0=four, num_1=
	# en=one, ba=satu
	# en=two, ba=dua
	# en=three, ba=tiga
	# en=four, ba=

```