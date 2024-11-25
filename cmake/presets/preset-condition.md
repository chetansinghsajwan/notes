# Preset Condition

The `condition` field of a preset, allowed in preset files specifying version `3` or above, is used to determine whether or not the preset is enabled.

For example, this can be used to disable a preset on platforms other than Windows.

`condition` may be either a boolean, `null`, or an object.

If it is a boolean, the boolean specifies the condition indicates whether the preset is enabled or disabled.

If it is `null`, the preset is enabled, but the `null` condition is not inherited by any presets that may inherit from the preset.

Sub-conditions (for example in a `not`, `anyOf`, or `allOf` condition) may not be `null`.

If it is an object, it can have the following fields:

###### `type`

- type: [`string`]()
- required: yes

Valid values are:

- `"const"`
  
  Indicates that the condition is constant. This is equivalent to using a boolean in place of the object.
  
  The condition object will have the following additional fields:

  - `value`
      
    - type: [`bool`]()
    - required: yes
    
    Constant value for the condition's evaluation.

- `"equals"` or `"notEquals"`
  
  Indicates that the condition compares two strings to see if they are equal or not equal.
  
  The condition object will have the following additional fields:
  
  - `lhs`
    
    - type: [`string`]()
    - required: yes
    
    First string to compare.
    
    This field supports macro expansion.

  - `rhs`
    
    - type: [`string`]()
    - required: yes
    
    Second string to compare.
    
    This field supports macro expansion.

- `"inList"` or `"notInList"`
  
  Indicates that the condition searches for a string in a list of strings.
  
  The condition object will have the following additional fields:
  
  - `string`
    
    - type: [`string`]()
    - required: yes
    
    String to search for.
    
    This field supports macro expansion.
    
  - `list`
    
    - type: [`array`]() of [`string`]()
    - required: yes
    
    An array of strings to search.
    
    This field supports macro expansion, and uses short-circuit evaluation.

- `"matches"` or `"notMatches"`
  
  Indicates that the condition searches for a regular expression in a string.
  
  The condition object will have the following additional fields:
  
  - `string`
    
    - type: [`string`]()
    - required: yes
    
    A string to search.
    
    This field supports macro expansion.
    
  - `regex`
    
    - type: [`string`]()
    - required: yes
    
    A required regular expression to search for.
    
    This field supports macro expansion.
    
- `"anyOf"` or `"allOf"`

    Indicates that the condition is an aggregation of zero or more nested conditions.
    
    The condition object will have the following additional fields:
    
    - `conditions`
    
      - type: [`array`]() or [[preset-condition]]
      - required: yes
      
      An array of condition objects.
      
      These conditions use short-circuit evaluation.
      
  - `"not"`
    
    Indicates that the condition is an inversion of another condition.
    
    The condition object will have the following additional fields:
    
    - `condition`
      
      - type: [`object`]() of [`condition`]()
      - required: yes
      
      A condition.

## References

- https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#id11