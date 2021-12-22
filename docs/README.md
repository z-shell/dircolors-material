<h2 align="center">
  <a href="https://github.com/z-shell/zi">
    <img src="https://github.com/z-shell/zi/raw/main/docs/images/logo.svg" alt="Logo" width="80" height="80">
  </a>
❮ ZI ❯ Package - Any Gem
</h2>

<h2 align="center">

| **Package source:** | Tarball | Binary |             Git              | Node | Gem |
| :-----------------: | :-----: | :----: | :--------------------------: | :--: | :-: |
|     **Status:**     |   :x:   |  :x:   | :heavy_check_mark: (default) | :x:  | :x: |

</h2>

> This repository compatible with [ZI](https://github.com/z-shell-zi)

[ZI](https://github.com/z-shell/zi) can use a `package.json`
(similar in construct to the one used in `npm` packages) to automatically:

- get the plugin's Git repository OR release-package URL,
- get the list of the recommended ices for the plugin,
  - there can be multiple lists of ices,
  - the ice lists are stored in _profiles_; there's at least one profile, _default_,
  - the ices can be selectively overridden.

The package provides the [zpm-zsh/dircolors-material](https://github.com/zpm-zsh/dircolors-material) definitions for GNU `ls`, `ogham/exa` and also setups zsh-completion system to use the definitions.

## Install

### Available `pack''` invocations

`zpm-zsh/dircolors-material` from Git repository in the most optimized way.

```zsh
# Download the default profile
zi pack for dircolors-material

# Download the no-zsh-completion profile
zi pack"no-zsh-completion" for dircolors-material

# Download the no-color-swaps profile
zi pack"no-color-swaps" for dircolors-material

# Download the minimal profile
zi pack"minimal" for dircolors-material
```

### Default Profile

Provides the dircolors-material definitions for GNU `ls`, `ogham/exa` and also:

- sets up the zsh-completion system to use the definitions,
- automatically (i.e.: transparently for the updates of the plugin) swaps the
  color for the directory to a more bright blue (of the index 63 in the standard
  256 color palette),
- uses a workaround for some old `dircolors` commands that improperly test
  `$TERM` variable and produce empty output.

The ZI command executed will be equivalent to:

```zsh
zi lucid \
 atclone'(( !${+commands[dircolors]} )) && local P=g; \
    ${P}sed -i '/DIR/c\\DIR 38;5;63;1' material.dircolors; \
    TERM=ansi ${P}dircolors -b material.dircolors >! colors.zsh' \
 atpull'%atclone' pick"colors.zsh" nocompile'!' reset \
 atload'zstyle ":completion:*:default" list-colors "${(s.:.)LS_COLORS}";' for \
    zpm-zsh/dircolors-material
```

### `No-Color-Swaps` Profile

Provides the dircolors-material definitions for GNU `ls`, `ogham/exa` but
doesn't swap the directory color, i.e.: it doesn't customize the original theme.
It still initializes the zsh-completion system with the theme's colors.

The ZI command executed will be equivalent to:

```zsh
zi lucid \
 atclone'(( !${+commands[dircolors]} )) && local P=g; \
    TERM=ansi ${P}dircolors -b dircolors-material >! colors.zsh' \
 atpull'%atclone' pick"colors.zsh" nocompile'!' \
 atload'zstyle ":completion:*:default" list-colors "${(s.:.)LS_COLORS}";' for \
    zpm-zsh/dircolors-material
```

### `No-Zsh-Completion` Profile

Provides the dircolors-material definitions for GNU `ls`, `ogham/exa` but
doesn't set up the zsh-completion system to use them. It still swaps the
directory color.

The ZI command executed will be equivalent to:

```zsh
zi lucid \
 atclone'(( !${+commands[dircolors]} )) && local P=g; \
    ${P}sed -i '/DIR/c\\DIR 38;5;63;1' material.dircolors; \
    TERM=ansi ${P}dircolors -b material.dircolors >! colors.zsh' \
 atpull'%atclone' pick"colors.zsh" nocompile'!' for \
    zpm-zsh/dircolors-material
```

### `Minimal` Profile

Provides the dircolors-material definitions for GNU `ls`, `ogham/exa` with both
extra functions/features disabled, i.e.: without altering the original theme and
without setting the zsh-completion system. Thus, this is a barebones profile.

The ZI command executed will be equivalent to:

```zsh
zi lucid \
 atclone'(( !${+commands[dircolors]} )) && local P=g; \
    TERM=ansi ${P}dircolors -b dircolors-material >! colors.zsh' \
 atpull'%atclone' pick"colors.zsh" nocompile'!' for \
    zpm-zsh/dircolors-material
```
