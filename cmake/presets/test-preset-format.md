# Test Preset Format

###### `name`

- type: [`string`]()
- required: yes

Unique name among test presets.

###### `hidden`

- type: [`bool`]()
- required: no

Specifies whether or not a preset should be hidden.

See [[preset-hiding]].

###### `inherits`

- type: [`string`]() or [`array`]() of [`string`]()

Name or names of presets to inherit from.

See [[preset-inheritance]].

###### `condition`

- type: [`object`]() of [[preset-condition]]
- required: no

Specifies if the preset should be enabled.

###### `vendor`

- type: [`object`]() of [[preset-vendor-map]]
- required: no

###### `displayName`

- type: [`string`]
- required: no

Human-friendly name of the preset.

###### `description`

- type: [`string`]
- required: no

Human-friendly description of the preset.

###### `environment`

- type: [`object`]()
- required: no

An map of environment variables.

###### `configurePreset`

- type: [`string`]()
- required: no

The name of a configure preset to associate with this test preset.

If `configurePreset` is not specified, it must be inherited from the inherits preset unless this preset is hidden.

###### `inheritConfigureEnvironment`

- type: [`bool`]()
- required: no
- default: `true`

If true, the environment variables from the associated configure preset are inherited after all inherited test preset environments, but before environment variables explicitly specified in this test preset.

###### `configuration`

- type: [`string`]()
- required: no

Equivalent to passing [`--build-config`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-C) on the command line.

###### `overwriteConfigurationFile`

- type: [`array`] of [`string`]
- required: no

Configuration options to overwrite options specified in the [ctest](programming/tools/ctest/ctest) configuration file.

Equivalent to passing [`--overwrite`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-overwrite) for each value in the array.  

The array values support macro expansion.

###### `output`

- type: [`object`]
- required: no

Output options.

The object may contain the following fields.

- `shortProgress`
  
  - type: `bool`
  - required: no
  
  If true, equivalent to passing  [`--progress`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-progress) on the command line.
  
- `verbosity`
  
  - type: [`string`]()
  - required: no
  
  Verbosity level. 
  
  Must be one of the following:
  
  - `default`
    
    Equivalent to passing no verbosity flags on the command line.
    
  - `verbose`
    
    Equivalent to passing [`--verbose`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-V) on the command line.
    
  - `extra`
    
    Equivalent to passing [`--extra-verbose`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-VV) on the command line.
    
- `debug`
  
  - type: [`bool`]()
  - required: no
    
    If true, equivalent to passing [`--debug`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-debug) on the command line.
    
- `outputOnFailure`
  - type: [`bool`]()
  - required: no
  
  If true, equivalent to passing [`--output-on-failure`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-output-on-failure) on the command line.

- `quiet`
  - type: [`bool`]()
  - required: no
  
  If true, equivalent to passing [`--quiet`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-Q) on the command line.

- `outputLogFile`
  
  - type: [`string`]()
  - required: no
  
  Path to a log file.
  
  Equivalent to passing [`--output-log`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-O) on the command line.  
  
  This field supports macro expansion.

- `outputJUnitFile`
  
  - type: [`string`]()
  - required: no
  - since: version 6
  
  Path to a JUnit file.
  
  Equivalent to passing [`--output-junit`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-output-junit) on the command line.
  
  This field supports macro expansion.

- `labelSummary`
    
  - type: [`bool`]()
  - required: no
  
  If false, equivalent to passing [`--no-label-summary`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-no-label-summary) on the command line.

- `subprojectSummary`
  
  - type: [`bool`]()
  - required: no
  
  If false, equivalent to passing [`--no-subproject-summary`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-no-subproject-summary) on the command line.
  
- `maxPassedTestOutputSize`
  
  - type: [`int`]()
  - required: no
  
  Specifies the maximum output for passed tests in bytes.
  
  Equivalent to passing [`--test-output-size-passed`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-test-output-size-passed) on the command line.
  
- `maxFailedTestOutputSize`
  
  - type: [`int`]()
  - required: no
  
  Specifyies the maximum output for failed tests in bytes.
  
  Equivalent to passing [`--test-output-size-failed`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-test-output-size-failed) on the command line.
  
- `testOutputTruncation`
  
  - type: [`string`]()
  - required: no
  - since: version 5
  
  Specifyies the test output truncation mode.
  
  Equivalent to passing [`--test-output-truncation`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-test-output-truncation) on the command line.
  
- `maxTestNameWidth`
  
  - type: [`int`]()
  - required: no
  
  Specifyies the maximum width of a test name to output.
  
  Equivalent to passing [`--max-width`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-max-width) on the command line.
  
###### `filter`

- type: [`object`]()
- required: no

Specifyies how to filter the tests to run.

The object may contain the following fields.

- `include`
  
  An optional object specifying which tests to include.
  
  The object may contain the following fields.

- `name`
  
  An optional string specifying a regex for test names.
  
  Equivalent to passing [`--tests-regex`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-R) on the command line.
  
  This field supports macro expansion.
  
  CMake regex syntax is described under [string(REGEX)](https://cmake.org/cmake/help/latest/command/string.html#regex-specification).
  
- `label`
  
  An optional string specifying a regex for test labels.
  
  Equivalent to passing [`--label-regex`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-L) on the command line.
  
  This field supports macro expansion.

- `useUnion`
  
  An optional bool.
  
  Equivalent to passing [`--union`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-U) on the command line.

- `index`
  
  An optional object specifying tests to include by test index.
  
  The object may contain the following fields. Can also be an optional string specifying a file with the command line syntax for [`--tests-information`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-I). If specified as a string, this field supports macro expansion.

- `start`
  
  An optional integer specifying a test index to start testing at.

- `end`
  
  An optional integer specifying a test index to stop testing at.

- `stride`
  
  An optional integer specifying the increment.
  
- `specificTests`
  
  An optional array of integers specifying specific test indices to run.

- `exclude`
  
  An optional object specifying which tests to exclude. The object may contain the following fields.
  
- `name`
  
  An optional string specifying a regex for test names. Equivalent to passing [`--exclude-regex`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-E) on the command line.
  
  This field supports macro expansion.

- `label`
  
  An optional string specifying a regex for test labels.
  
  Equivalent to passing [`--label-exclude`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-LE) on the command line.
  
  This field supports macro expansion.

- `fixtures`
  
  An optional object specifying which fixtures to exclude from adding tests.
  
  The object may contain the following fields.

- `any`
  
  An optional string specifying a regex for text fixtures to exclude from adding any tests. Equivalent to [`--fixture-exclude-any`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-FA) on the command line. This field supports macro expansion.
1. `setup`An optional string specifying a regex for text fixtures to exclude from adding setup tests. Equivalent to [`--fixture-exclude-setup`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-FS) on the command line. This field supports macro expansion.
2. `cleanup`An optional string specifying a regex for text fixtures to exclude from adding cleanup tests. Equivalent to [`--fixture-exclude-cleanup`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-FC) on the command line. This field supports macro expansion.
3. `execution`An optional object specifying options for test execution. The object may contain the following fields.
4. `stopOnFailure`An optional bool. If true, equivalent to passing [`--stop-on-failure`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-stop-on-failure) on the command line.
5. `enableFailover`An optional bool. If true, equivalent to passing [`-F`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-F) on the command line.`jobs`An optional integer. Equivalent to passing[`--parallel`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-j) on the command line.
6. `resourceSpecFile`An optional string. Equivalent to passing [`--resource-spec-file`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-resource-spec-file) on  
    the command line. This field supports macro expansion.
43. `testLoad`An optional integer. Equivalent to passing [`--test-load`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-test-load) on the command line.
44. `showOnly`An optional string. Equivalent to passing [`--show-only`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-N) on the  
    command line. The string must be one of the following values:
45. `human`
46. `json-v1repeat`An optional object specifying how to repeat tests. Equivalent to  
    passing [`--repeat`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-repeat) on the command line.  
    The object must have the following fields.
47. `mode`A required string. Must be one of the following values:  
    `until-fail`  
    `until-pass`
48. `after-timeoutcount`A required integer.`interactiveDebugging`An optional bool. If true, equivalent to passing [`--interactive-debug-mode 1`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-interactive-debug-mode) on the command line. If false, equivalent to passing [`--interactive-debug-mode 0`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-interactive-debug-mode) on the command line.
49. `scheduleRandom`An optional bool. If true, equivalent to passing [`--schedule-random`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-schedule-random) on the command line.
50. `timeout`An optional integer. Equivalent to passing [`--timeout`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-timeout) on the command line.
51. `noTestsAction`An optional string specifying the behavior if no tests are found. Must  
    be one of the following values:
    1. `default`Equivalent to not passing any value on the command line.
    2. `error`Equivalent to passing [`--no-tests=error`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-no-tests) on the command line.
    3. `ignore`Equivalent to passing [`--no-tests=ignore`](https://cmake.org/cmake/help/latest/manual/ctest.1.html#cmdoption-ctest-no-tests) on the command line.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id8
