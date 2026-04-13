# Makefile

## Installation

### Windows

- `make -v`
- `choco install make`
- `choco upgrade make`

`Makefile` is a file that lets developers specify a set of operations that are necessary to build a program and the order in which the steps must be performed. 
You may not be familiar with make, but it's been used to build programs on Unix systems since 1976.

Create a file called `Makefile` with the following contents:

```
.DEFAULT_GOAL := build
.PHONY: fmt vet build

fmt:
	go fmt ./...
vet: 
	go vet ./...
build: fmt vet 
	go build
```

- `.DEFAULT_GOAL` - allows to specify default target that will be executed on `make`
- `.PHONY` - allows to avoid naming conflicts. By default, `make` assumes every target is a file it needs to create.
  If a file with the exact same name as your target already exists in the directory, `make` will compare its timestamp against its dependencies.
  If the file is newer than its dependencies, `make` will assume it is "up to date" and skip the recipe.
  If you have a target named `clean` and someone creates a file named `clean` in your folder, running `make clean` might do nothing because `make` thinks the "file" clean is already finished.
  The Solution: Declaring `.PHONY: clean` tells make to ignore any actual file named `clean` and run the recipe every time you call it.

If we run `make` it will run commands in this order:

```bash
go fmt ./...
go vet ./...
go build
```
