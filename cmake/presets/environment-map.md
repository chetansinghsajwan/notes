# Preset Environment Map

A map of environment variables.

The key is the variable name, and the value is either `null` or a string representing the value of the variable.

Environment variables in this map may reference each other, and may be listed in any order, as long as such references do not cause a cycle (for example, if `ENV_1` is `$env{ENV_2}`, `ENV_2` may not be `$env{ENV_1}`.)

This field supports [[macro-expansion]].

Environment variables are inherited through the `inherits` field, and the preset's environment will be the union of its own `environment` and the `environment` from all its parents. If multiple presets in this union define the same variable, the standard rules of `inherits` are applied.

Setting a variable to `null` causes it to not be set, even if a value was inherited from another preset.