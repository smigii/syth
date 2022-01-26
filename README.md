# Syth

***IN PROGRESS***

Syth is pretty much just a search and replace program, tailored for managing
system-wide themes in linux environments. This is accomplished using a specific
directory structure and a simple scripting language, syth script.

## Usage

```text
usage: syth theme

positional arguments:
  theme       JSON theme file, relative to ~/.config/syth/themes.

optional arguments:
  -h, --help  show this help message and exit
```

## Directory Structure

```text
Basic example directory structure

ROOT
│
├─ modules
│  ├── bashrc
│  │   ├─ template
│  │   └─ themes
│  │      ├─ bashrc1.syth
│  │      └─ bashrc2.syth    
│  └── vimrc
│      ├─ template.vim
│      └─ themes
│         ├─ vimrc_gruvbox.syth
│         └─ vimrc_monokai.syth
│
└─ sthemes
   ├─ theme1.json
   └─ theme2.json
```

The systheme root directory contains all the files necessary to build your
config files according to some rules defined in the systheme you specify.
This directory should go in `~/.config/syth`

### Sthemes Directory

This directory is simple, it holds the system theme files (in json format)
that define a theme.

```json
{
  "some_module": "some_theme.syth",
  "some_other_module": "another_theme.syth"
}
```

It's just a bunch of key-value pairs, matching a theme to a module, which
is explained below.

Using the directory structure above, we could create `sthemes/gruvbox.json`
as follows

```json
{
  "bashrc": "bashrc1.json",
  "vimrc": "vimrc_gruvbox.json"
}
```

### Modules Directory

Each sub-directory in `modules` represents a config file for some
program (or part of a program). For example, let's look at how we could
manage bash config files.

We start by creating the `bashrc` (or whatever name you like) directory
within `modules/`. Next, we need to create a template, and the parts we
want to swap out for each theme, which will go in `themes/`.

The template file defines the parts of the config that don't change. Note that
this template file can also have any file extension, so you can keep some syntax
highlighting when editing. Let's write a template file for bashrc

```text
# This is a comment
path = ~/.bashrc
[% __END_HEADER__ %]
alias test1=$(echo constant)
alias test=[% cmd %]
```

The first 3 lines are part of the header, and are used to define the destination
path, brace symbols, pre- and post- script locations, and more (in the future).

* `#` is used as a comment, everything after will be ignored.
* `path = path/to/dst` defines the destination for the file
* `[% __END_HEADER__ %]` marks the end of the header, and the output file
  will begin on the next line

Nothing in the header will be written to the output file.

The line `[% cmd %]` is telling systheme to replace that piece of text (from 
start brace to end brace inclusive) with the value defined in a `.syth` file.
These files are located in the `themes/` directory of a module.

Now we can add some files to the `bashrc/themes/` directory, which will define
the modular parts of our config. Let's create two files here,

themes/bashrc1.syth

```text
string cmd = "'echo hello'"
```

themes/bashrc2.syth

```text
string cmd = "'echo world'"
```

So, using the `systhemes/gruvbox.json` file defined above,
when we run `systheme gruvbox.json`, systheme will first look at the
`systhemes/gruvbox.json` file. It will then write to `~/.bashrc`, as
directed in the template file, then it will replace `[% cmd %]` with
`'echo hello'` as directed in the `themes/bashrc1.syth` file.
