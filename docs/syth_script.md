# Syth Script

A really simple script language for creating symbol tables used in templates.
These scripts are interpreted line by line. Each line must be of one of the following
forms.

* `# This is a comment`
* `include module::theme as module_name`
* `string id = "value"`
* `string id = module_name.id`
* `file f = path/to/file` (not yet implemented)

## Comments

Comments can be placed anywhere, at the start of a line or at the end of
any other command

```text
# This is a comment
include alacritty::gruvbox as alg   # Another comment
```

## Includes

Includes the symbols from another module and theme. This can be very useful if
you want to use a color defined in a terminal config in some other program config.

`module::theme` refers to a module in the `modules/` directory, and `theme` is a
syth script theme in the `themes/` directory of that module.

The module name you use must be alphanumeric, and can only be used once per syth script. 

```text
include alacritty::gruvbox as alg
include bashrc::gruvbox as bg
```

## String

## File
