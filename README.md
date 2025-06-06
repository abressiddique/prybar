# ![Prybar](logo.svg)


Prybar is a universal interpreter front-end. Same interface, same
REPL, different languages.

## Why

At [Repl.it](https://repl.it), we're in the business of running REPLs.
As it happens to be, every language implements them differently. We
wanted them to all behave the same: run code and drop into a REPL!

## How it works

Prybar, written in [Go](https://golang.org/), maintains a common
command-line interface that calls into a select language backend. When
possible, the language backends are implemented using cgo and the
language's C-bindings. Otherwise, they make use of a small script
written in the host language which starts a Prybar-compatible REPL.

## Usage

    Usage: ./prybar-LANG [FLAGS] [FILENAME]...
      -I	interactive (use readline repl)
      -c string
        	execute without printing result
      -e string
        	evaluate and print result
      -i	interactive (use language repl)
      -ps1 string
        	repl prompt (default "--> ")
      -ps2 string
        	repl continuation prompt (default "... ")
      -q	don't print language version

## Language Support

| language                  | eval | eval expression | eval file | repl | repl like eval | set prompt |
| ------------------------- | ---- | --------------- | --------- | ---- | -------------- | ---------- |
| Clojure                   | ✔    | ✔               | ✔         | ✔    | ✔              | -          |
| Emacs Lisp                | ✔    | ✔               | ✔         | ✔    | ✗              | ✔          |
| Javascript (nodejs)       | ✔    | ✔               | ✔         | ✔    | ✔              | ✔          |
| Javascript (spidermonkey) | ✔    | ✗               | ✗         | ✗    | ✗              | -          |
| Julia                     | ✔    | ✗               | ✔         | ✔    | ✗              | ✔          |
| Lua 5.1                   | ✔    | ✗               | ✔         | ✔    | ✗              | ✔          |
| OCaml                     | ✔    | ✔               | ✔         | ✔    | ✗              | ✔          |
| Python 2.7                | ✔    | ✔               | ✔         | ✔    | ✔              | ✔          |
| Python 3.x                | ✔    | ✔               | ✔         | ✔    | ✔              | ✔          |
| R                         | ✔    | ✗               | ✗         | ✔    | ✗              | ✗          |
| Ruby 2.5                  | ✔    | ✔               | ✔         | ✔    | ✗              | ✗          |
| SQLite                    | ✔    | ✔               | ✔         | ✔    | ✗              | ✔          |
| Tcl                       | ✔    | ✔               | ✔         | ✗    | ✗              | -          |

## Start to Develop with Nix

Look in `packages` in `flake.nix`. For each package present, you can do

```
nix build .#<package name>
```

to build it. The result will be a directory named `result`.

Example:

```
$ nix build .#prybar-python311
$ ls result/bin
prybar-python311
```

Alternately, you can use the nix shell. Enter

```
nix develop
```

to drop into a shell that has all dependencies installed an ready to go.

If you don't have nix yet, install that: https://nixos.org/

## Build and run

    % make help
    usage:
      make all          Build all Prybar binaries
      make prybar-LANG  Build the Prybar binary for LANG
      make docker       Run a shell with Prybar inside Docker 
                        (don't do this in the nix shell, although this is
                                                not needed if you use nix)
      make image        Build a Docker image with Prybar for distribution
      make test         Run integration tests
      make test-image   Test Docker image for distribution
      make clean        Remove build artifacts
      make help         Show this message

### Distribution

Run `make image` to create a Docker image containing not only Prybar's
dependencies and source code but also its compiled binaries, which can
be embedded inside other Docker images by means of `COPY
--from=basicer/prybar`.

This image is automatically built and deployed to [Docker
Hub](https://hub.docker.com/) every time a commit is merged to
`master`.

## License

Copyright (C) 2004-2018 Neoreason, Inc. et al.

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Suite 500, Boston, MA
02110-1335, USA.

See the COPYING file for more information regarding the GNU General
Public License.
