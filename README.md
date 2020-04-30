[![PyPI version](https://badge.fury.io/py/agda-pkg.svg)](https://badge.fury.io/py/agda-pkg) [![Build Status](https://travis-ci.org/agda/agda-pkg.svg?branch=master)](https://travis-ci.org/agda/agda-pkg)

**Agda-pkg** is a tool to manage [Agda](http://github.com/agda/agda) libraries with extra features like
installing libraries from different kind of sources. This tool does
not modify `Agda` at all, it just manages systematically the directory
`.agda` and the files: `.agda/defaults` and `.agda/libraries` used by
Agda to locate libraries. For more information about how Agda package
system works, please read the official documentation
[here](https://agda.readthedocs.io/en/v2.6.0/tools/package-system.html).

Common usages:

A library from the package-index. See list above.

```bash
$ apkg install standard-library
```

A Github repository with a specific version release:

```bash
$ apkg install --github agda/agda-stdlib --version v1.3
```

A Github repository with a specific branch with a specific library name:

```bash
$ apkg install --github plfa/plfa.github.io --branch dev --name plfa
```

For your library: 

```bash
$ apkg install --editable .
```


After running `apkg init`, you will be able to install libraries from the index
[agda/package-index](http://github.com/agda/package-index), below you'll see a list, possible outdated.


**Library name**         | **Latest version** | **URL**
-----|-----|-----
agda-base            | v0.2            		 | https://github.com/pcapriotti/agda-base.git
agda-categories      | v0.1            		 | https://github.com/agda/agda-categories.git
agda-metis           | v0.2.1          		 | https://github.com/jonaprieto/agda-metis.git
agda-prelude         | df679cf         		 | https://github.com/UlfNorell/agda-prelude.git
agda-prop            | v0.1.2          		 | https://github.com/jonaprieto/agda-prop.git
agda-real            | e1558b62        		 | https://gitlab.com/pbruin/agda-real.git
agda-ring-solver     | d1ed21c         		 | https://github.com/oisdk/agda-ring-solver.git
agdarsec             | v0.3.0          		 | https://github.com/gallais/agdarsec.git
alga-theory          | 0fdb96c         		 | https://github.com/algebraic-graphs/agda.git
ataca                | a9a7c06         		 | https://github.com/jespercockx/ataca.git
cat                  | v1.6.0          		 | https://github.com/fredefox/cat.git
cubical              | v0.1            		 | https://github.com/agda/cubical.git
FiniteSets           | c8c2600         		 | https://github.com/L-TChen/FiniteSets.git
fotc                 | apia-1.0.2      		 | https://github.com/asr/fotc.git
generic              | f448ab3         		 | https://github.com/effectfully/Generic.git
hott-core            | 1037d82         		 | https://github.com/HoTT/HoTT-Agda.git
hott-theorems        | 1037d82         		 | https://github.com/HoTT/HoTT-Agda.git
HoTT-UF-Agda         | 9d0f38e         		 | https://github.com/martinescardo/HoTT-UF-Agda-Lecture-Notes.git
ial                  | v1.5.0          		 | https://github.com/cedille/ial.git
lightweight-prelude  | b2d440a         		 | https://github.com/L-TChen/agda-lightweight-prelude.git
mini-hott            | d9b4a7b         		 | https://github.com/jonaprieto/mini-hott.git
MtacAR               | 5417230         		 | https://github.com/L-TChen/MtacAR.git
plfa                 | stable-web-2019.09        | https://github.com/plfa/plfa.github.io.git
routing-library      | thesis          		 | https://github.com/MatthewDaggitt/agda-routing.git
standard-library     | v1.3            		 | https://github.com/agda/agda-stdlib.git

# Quick Start

To install `agda-pkg`, you must have installed `Python 3.6+` or a latter version
on your machine. In addition, the python package manager `pip3 18.0+` (for python 3).

We have tested `agda-pkg` with `Agda v2.5.4+`.

To install this tool run the following command:

```bash
$ pip install agda-pkg
```

Now, we can run the package manager using the command `agda-pkg` or even
shorter just `apkg`.

## Using with Nix or NixOS


A `nix-shell` environment that loads `agda-pkg` as well as `agda` and
`agda-mode` for emacs is available. To use this, `apkg` can put the necessary
files in your project folder by running the following command:

```
$ apkg nixos
```

Then, you will have new files for you to run the nix shell.

```
$ nix-shell
```

To launch Emacs with `agda-mode` enabled, run `mymacs` in the newly launched
shell; `mymacs` will also load your `~/.emacs` file if it exists. If you are
using [Spacemacs](https://www.spacemacs.org), you will need to edit `shell.nix`
to use `~/.spacemacs` instead.

The files provided by `apkg` are also available in this repository
(`apkg/support/nix`) and in a third-party [example
repository](https://github.com/bbarker/LearningAgda) to give an idea of exactly
which files need to be copied to your project.

**Example**:

```agda
$ cat hello-world.agda
module hello-world where
open import IO
main = run (putStrLn "Hello, World!")
```

Run `mymacs hello-world.agda` then type `C-c C-x C-c` in emacs to compile the loaded hello world file.

### Configuration

After running `apkg nixos`, you will see the following files:

```
./hello-world.agda
./agda_requirements.txt
./shell.nix
./deps.nix
./emacs.nix
```

Edit any of the `nix` expressions as needed. In particular:

0. To add Agda dependencies via `agda-pkg`, edit `agda_requirements.txt`
1. To add more 4Haskell or other system dependencies or other
  target-language dependencies, edit `deps.nix`.
2. To add or alter the editor used, change the `myEmacs`
  references in `shell.nix` or add similar derivations.
3. Optionally, create `.emacs_user_config` in the repository root directory and
  add any additional config, such as `(setq agda2-backend "GHC")` to use GHC by
  default when compiling Agda files from emacs.


# Usage

## Initialisation of the package index

The easiest way to install libraries is by using [the package index].
`agda-pkg` uses a local database to maintain a register of all
libraries available in your system. To initialize the index and the
database please run the following command:

```bash
$ apkg init
Indexing libraries from https://github.com/agda/package-index.git
```

**Note**. To use a different location for your agda files `defaults`
and `libraries`, you can set up the environment variable `AGDA_DIR`
before run `apkg` as follows:

```bash
$ export AGDA_DIR=$HOME/.agda
```

Other way is to create a directory `.agda` in your directory and run
`agda-pkg` from that directory. `agda-pkg` will prioritize the `.agda`
directory in the current directory.

## Help command

Check all the options of a command or subcommand by using the flag `--help`.

```bash
$ apkg --help
$ apkg install --help
```

## Upgrade the package index

Recall updating the index every once in a while using `upgrade`.

```bash
$ apkg upgrade
Updating Agda-Pkg from https://github.com/agda/package-index.git
```

If you want to index your library go to [the package index] and make [PR].

## Environmental variables

If there is an issue with your installation or you suspect something
is going wrong. You might want to see the environmental variables used by apkg.

```bash
$ apkg environment
```


## List all the packages available

To see all the packages available run the following command:

```bash
$ apkg list
```

This command also has the flag `--full` to display a version of the
this list with more details.


## Installation of packages

Install a library is now easy. We have multiple ways to install a package.

-   from the [package-index](http://github.com/agda/package-index)

```
$ apkg install standard-library
```

-   from a [local directory]

```bash
$ apkg install .
```

or even much simpler:

```bash
$ apkg install
```

Installing a library creates a copy for agda in the directory assigned
by agda-pkg. If you want your current directory to be taken into
account for any changes use the `--editable` option.  as shown below.


```bash
$ apkg install --editable .
```

-   from a github repository

```bash
$ apkg install --github agda/agda-stdlib --version v1.1
```

-   from a git repository

```bash
$ apkg install http://github.com/jonaprieto/agda-prop.git
```

To specify the version of a library, we use the flag `--version`

```bash
$ apkg install standard-library --version v1.0
```

Or simpler by using `@` or `==` as it follows.

```bash
$ apkg install standard-library@v1.0
$ apkg install standard-library==v1.0
```

### Multiple packages at once

To install multiple libraries at once, we have two options:

1. Using the inline method

```bash
$ apkg install standard-library agda-base
```

Use `@` or `==` if you need a specific version, see above
examples.

2. Using a requirement file:

Generate a requirement file using `apkg freeze`:

```bash
$ apkg freeze > requirements.txt
$ cat requirements.txt
standard-library==v1.1
```

Now, use the flag `-r` to install all the listed libraries
in this file:

```bash
$ apkg install -r requirements.txt
```

Check all the options of this command with the help information:

```bash
$ apkg install --help
```

## Uninstalling a package

Uninstalling a package will remove the library from the visible libraries for Agda.

- using the name of the library

```bash
$ apkg uninstall standard-library
```

- infering the library name from the current directory

```bash
$ apkg uninstall .
```

And if we want to remove the library completely (the sources and
everything), we use the flag `--remove-cache`.

```bash
$ apkg uninstall standard-library --remove-cache
```

## Update a package to latest version

We can get the latest version of a package from
the versions registered in the package-index.

- Update all the installed libraries:

```bash
$ apkg update
```

- Update a specific list of libraries. If some
library is not installed, this command will installed
the latest version of it.

```bash
$ apkg update standard-library agdarsec
```

## See packages installed


```bash
$ apkg freeze
standard-library==v1.1
```

This command is useful to keep in a file the versions used for your project
to install them later.


```bash
$ apkg freeze > requirements.txt
```

To install from this requirement file run this command.


```bash
$ apkg install < requirements.txt
```

## Approximate search of packages

We make a search (approximate) by using keywords and title of the
packages from the index. To perform such a search, see the following
example:


```bash
$ apkg search metis
1 result in 0.0012656739999998834seg
cubical
url: https://github.com/agda/cubical.git
installed: False
```

## Get all the information of a package


```bash
$ apkg info cubical
```

# Creating a library for Agda-Pkg

In this section, we describe how to build a library.

To build a project using `agda-pkg`, we just run the following command:

```bash
$ apkg create
```

Some questions are going to be prompted in order to create
the necessary files for Agda and for Agda-Pkg.

The output is a folder like the following showed below.

## Directory structure of an agda library

A common Agda library has the following structure:

```
$ tree -L 1 mylibrary/
mylibrary/
├── LICENSE
├── README.md
├── mylibrary.agda-lib
├── mylibrary.agda-pkg
├── src
└── test

2 directories, 3 files
```

## .agda-lib library file

```yaml
$ cat mylibrary.agda-lib
name: mylibrary  -- Comment
depend: LIB1 LIB2
  LIB3
  LIB4
include: PATH1
  PATH2
  PATH3
```

## .agda-pkg library file

This file only works for `agda-pkg`. The idea of
this file is to provide more information about the
package, pretty similar to the cabal files in Haskell.
This file has priority over its version `.agda-lib`.

```yaml
$ cat mylibrary.agda-pkg
name:              mylibrary
version:           v0.0.1
author:            
    - AuthorName1
    - AuthorName2
category:          cat1, cat2, cat3
homepage:          http://github.com/user/mylibrary
license:           MIT
license-file:      LICENSE.md
source-repository: http://github.com/user/mylibrary.git
tested-with:       2.6.0
description:       Put here a description.

include:
    - PATH1
    - PATH2
    - PATH3
depend:
    - LIB1
    - LIB2
    - LIB3
    - LIB4
```

# About

This is a proof of concept of an Agda Package Manager.
Contributions are always welcomed.



[the package index]: https://github.com/agda/package-index
[local directory]: https://agda.readthedocs.io/en/v2.5.4/tools/package-system.html
[PR]: https://github.com/agda/package-index/pull/new/master
