# fzfuncs

Some [fzf](https://github.com/junegunn/fzf/) functions which might be useful for your bashrc.

They provide "fuzzy" variants of popular commands such as cd, ls, vim, cat, firefox, chromium, grep, head, tail, evince, vlc, ...

If you consider the bash completion provided by [fzf](https://github.com/junegunn/fzf/) a bit clumsy, this is for you!

## Features

- support for Favorites
- user-based or global customization
- extensible
- implementation via bash aliases
- automatic dependency checks
- reasonable defaults
- small & fast (thanks to [fzf](https://github.com/junegunn/fzf/)!)

## Table of contents

- [Installation](#installation)
- [Usage](#usage)
- [Customization](#customization)
- [Copyright](#copyright)

## Installation

1. Move the `bashrc_fzf` file to a directory of your liking, let's say `/etc/`.
2. Import the `bashrc_fzf` content into your local or global `bashrc`, e.g. via `source "/etc/bashrc_fzf"`.
3. Optional: Create your own configuration at `~/.bashrc_fzf` (user-based) or inside the aforementioned directory, e.g. `/etc/bashrc_fzf.conf` (global). See the [customization](#customization) section for further details.

### Dependencies

[GNU coreutils](https://www.gnu.org/software/coreutils/) and of course [fzf](https://github.com/junegunn/fzf/). They are packaged by most Linux distributions.

Installed aliases may introduce [further dependencies](#installed aliases).

## Usage

### Installed aliases

By default, the aliases have the same name as the original command, followed by a single `f` (for "fuzzy").

Exceptions are: `ls` --> `lf`, `mpc` --> `f`, `$EDITOR` --> `e`, `firefox` --> `fox` or `firefoxf`

All names can be changed (cf. [customization](#customization)).

The aliases will _not_ be enabled, if any of their dependencies are not installed. So if you e.g. don't have `mpc` installed, `f` won't exist. In particular most default aliases depend on `(m)locate`. So either install it, or [customize your configuration](#customization) to make `bashrc_fzf` use something else.

### Examples

```
cdf courage
```
Switch to a directory with `courage` in its file path. If a file is matched, go to the parent directory of that file.

```
e
```
Let the user decide from all files in his home directory which one to open with his `$EDITOR`. If (s)he presses `Ctrl-C`, just open the editor.

```
evincef bash cool
```
Attempt to find a `pdf` file with both the terms `bash` and `cool` in your home directory. If a match is found, open it. If no match is found, pass the parameters to `evince` as is (will likely cause some errors).

```
shuff love songs | tail -n1 | f
```
Find a text file listing love songs in your home directory, extract a random one and play it with `mpc`.

```
grepf queen favs.txt | shuf | tail -n1 | f
```
Find all Queen songs in your list of favorite songs, pick a random one and play it (`f queen` would let you select from all of your Queen songs). Alternatively, you could [customize](#customization) the `f` command to know your favorite songs.

```
fox git
```
Visit github.com with firefox.

```
chromiumf "https://github.com/3hhh/"
```
Same as with `chromium`: Visit my github profile.

```
killf chromium
```
Send a termination signal to an open chromium instance. If there are multiple, select it from a list.

## Customization

All customizations should be done in either `~/.bashrc_fzf` or in the installation directory of `bashrc_fzf` in a file called `bashrc_fzf.conf`.

Customizations are implemented as standard bash code and usually involve redefining certain associative arrays declared inside `bashrc_fzf`.

### Examples

The command `libreofficef` is too long for your liking. If so, add the following redefinition to your config file:
```bash
FZF_CMDS["lo"]="id:libreoffice"
```
This makes `libreofficef` available as `lo` as well.

If you'd like to redefine all commands, just redeclare the associative array `FZF_CMDS`, e.g. as such:
```bash
#I just want libreofficef, cdf and grepf
declare -gA FZF_CMDS=(
	["cdf"]="id:cd"
	["grepf"]="id:grep"
	["libreofficef"]="id:libreoffice"
	)
```
See the source code of `bashrc_fzf` for the list of available IDs/commands.

### Configuration Variables

#### FZF\_CMDS
The aforementioned associative array used to define all bash aliases.

#### FZF\_FAVS\_DIRS
Associative array to define your favorite directories. The matching happens against the keys, but the values are passed to the final command. If you don't want to use that feature, just define identical keys and values.

#### FZF\_FAVS\_FILES
Associative array to define your favorite files.

#### FZF\_FAVS\_URLS
Associative array to define your favorite URLs.

#### FZF\_FAVS
Associative array defining which favorites are applied for which command. New types of Favorites could be defined here as well.

#### FZF\_BASE\_MAIN
The fzf command to execute to resolve fuzzy arguments, if the Favorites did not apply.

#### FZF\_BASE\_FAVS
The fzf command to execute to resolve fuzzy arguments with the Favorites.

#### FZF\_FIND
Command to employ to find directories and files on your file system. By default, `(m)locate` is used. If you prefer `find` or `fd` or something else, change that here.

#### FZF\_PROC
Command to employ to find processes on your system. By default, `ps aux` is used. If you don't like the output, change that here. You also might have to redefine `FZF_PROC_PID`.

#### FZF\_IMPL & FZF\_DEPS
Required to implement new commands. `FZF_DEPS` holds the dependencies, `FZF_IMPL` the actual implementation.

## Copyright

© 2019 David Hobach
GPLv3

See `LICENSE` for details.
