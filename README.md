Gofluff is a fork of the Golint tool, adding command line flags for suppressing specific warnings.

[![Build Status](https://travis-ci.org/btubbs/gofluff.svg?branch=master)](https://travis-ci.org/btubbs/gofluff)

## Installation

Gofluff requires Go 1.6 or later.

    go get -u github.com/btubbs/gofluff/gofluff

## Usage

Invoke `gofluff` with one or more filenames, directories, or packages named
by its import path. Gofluff uses the same
[import path syntax](https://golang.org/cmd/go/#hdr-Import_path_syntax) as
the `go` command and therefore
also supports relative import paths like `./...`. Additionally the `...`
wildcard can be used as suffix on relative and absolute file paths to recurse
into them.

To ignore a specific kind of error, use the `-ignore` flag and give it the same
code shown in parentheses when the error is printed out.  If you see an error
like this:

    testdata/4.go:27:5: (comments.exportedval) exported var W should have comment or be unexported

You can ignore it like this:

    gofluff -ignore comments.exportedval testdata/4.go

You can ignore a whole class of errors by providing just the parent category:

    gofluff -ignore comments testdata/4.go

You can pass multiple things to ignore by joining them with a comma:

    gofluff -ignore naming.leadingk,naming.allcaps testdata/names.go

The output of this tool is a list of suggestions in Vim quickfix format,
which is accepted by lots of different editors.

## Purpose

Gofluff differs from gofmt. Gofmt reformats Go source code, whereas gofluff prints out style
mistakes.

Gofluff differs from govet. Govet is concerned with correctness, whereas gofluff is concerned with
coding style.

Gofluff differs from golint.  Golint has a hard-coded set of warnings matching Google's internal
style rules.  Gofluff allows teams to define and enforce their own subset of these rules.

If you find an established style that is frequently violated, and which
you think gofluff could statically check,
[file an issue](https://github.com/btubbs/gofluff/issues) or even better, [submit a pull
request](https://github.com/btubbs/gofluff/pulls).

## Vim

Add this to your ~/.vimrc:

    set rtp+=$GOPATH/src/github.com/btubbs/gofluff/misc/vim

If you have multiple entries in your GOPATH, replace `$GOPATH` with the right value.

Running `:Lint` will run gofluff on the current file and populate the quickfix list.

Optionally, add this to your `~/.vimrc` to automatically run `gofluff` on `:w`

    autocmd BufWritePost,FileWritePost *.go execute 'Lint' | cwindow


## Emacs

Add this to your `.emacs` file:

    (add-to-list 'load-path (concat (getenv "GOPATH")  "/src/github.com/btubbs/gofluff/misc/emacs"))
    (require 'golint)

If you have multiple entries in your GOPATH, replace `$GOPATH` with the right value.

Running M-x gofluff will run gofluff on the current file.

For more usage, see [Compilation-Mode](http://www.gnu.org/software/emacs/manual/html_node/emacs/Compilation-Mode.html).
