# Rust support for Visual Studio Code

Adds language support for Rust to Visual Studio Code. Supports:

* code completion
* jump to definition, peek definition, find all references, symbol search
* types and docs on hover
* code formatting
* refactoring (rename, deglob)
* error squiggles and apply suggestions from errors
* snippets
* build tasks

This extension is powered by the [Rust Language Server](https://github.com/rust-lang-nursery/rls)
(RLS) and is the reference client implementation. If you don't have it installed,
the extension will install the RLS for you.

This extension is maintained by the [Rust Developer Tools team](https://www.rust-lang.org/en-US/team.html#Dev-tools-team).
For support, please file an [issue on the repo](https://github.com/rust-lang-nursery/rls-vscode/issues/new)
or talk to us in #rust-dev-tools on IRC ([Mozilla servers](https://wiki.mozilla.org/IRC)).

Contributing code, tests, documentation, and bug reports is very welcome! For
more details on building and debugging, etc., see [contributing.md](contributing.md).


## Quick start

* Install this extension from the marketplace.
* Install [rustup](https://www.rustup.rs/) (Rust toolchain manager).
* The extension will start when you open a Rust file. You'll be prompted to
  install the RLS. Once installed, the RLS should start building you project.
  The first build can take a while.


## Configuration

This extension provides some options into VSCode's configuration settings. These
options have names which start with `rust.`. You can find the settings under
`File > Preferences > Settings`, they all have Intellisense help.

Some highlights:

* `rust.show_warnings` - set to false to silence warnings in the editor.
* `rust.build_lib` - if you have both a binary and library in your crate, set to
  true to build the library.
* `rust.build_bin` - if you have multiple binaries, you can specify which to build
  using this option.
* `rust.workspace_mode` - experimental cargo workspace support. Note that using
  this feature will slow down builds significantly.


## Requirements

* [Rustup](https://www.rustup.rs/),
* A nightly Rust toolchain (the extension will configure this for you, with permission),
* RLS, rust-src, and rust-analysis components (the extension will install these for you, with permission).


## Features

### Commands

Commands can be found in the command palette (ctrl + shift + p). We provide the
following commands:

* `deglob` - replace a glob import with an explicit import. E.g., replace
  `use foo::*;` with `use foo::{bar, baz};`. Select only the `*` when running
  the command.


### Snippets

Snippets are code templates which expand into common boilerplate. Intellisense
includes snippet names as options when you type; select one by pressing 'enter'.
You can move to the next 'hole' in the template by pressing 'tab'. We provide
the following snippets:

* `for` - a for loop
* `unimplemented`
* `unreachable`
* `println`
* `macro_rules` - declare a macro
* `if let Option` - an `if let` statement for executing code only in the `Some` case.
* `spawn` - spawn a thread


### Tasks

The plugin provides tasks for building, running, and testing using the relevant
cargo commands. You can build using `ctrl + shift + b`. Access other tasks via
`Run tasks` in the command palette.

The plugin writes these into `tasks.json`. The plugin will not overwrite
existing tasks, so you can customise these tasks. To refresh back to the
defaults, delete `tasks.json` and restart VSCode.


## Format on save

To enable formatting on save, you need to set the `editor.formatOnSave` setting
to `true`. Find it under `File > Preferences > Settings`.


## Implementation

This extension almost exclusively uses the RLS for its feature support (syntax
highlighting, snippets, and build tasks are provided client-side). The RLS uses
the Rust compiler (rustc) to get data about Rust programs. It uses Cargo to
manage building. Both Cargo and rustc are run in-process by the RLS. Formatting
and code completion are provided by rustfmt and Racer, again both of these are
run in-process by the RLS.
