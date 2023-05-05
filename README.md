# rip (Rm ImProved)
[![crates.io](https://img.shields.io/crates/v/rm-improved.svg)](https://crates.io/crates/rm-improved)

#### Goals of Fork:

`rip` is a command-line deletion tool focused on safety, ergonomics, and performance.  It favors a simple interface, and does /not/ implement the `xdg-trash` spec or attempt to achieve the same goals.

Deleted files get sent to the graveyard (`$XDG_DATA_HOME/graveyard` if set, else `/tmp/graveyard-$USER` by default, see [notes](https://github.com/nivekuil/rip#-notes) on changing this) under their absolute path, giving you a chance to recover them.  No data is overwritten.  If files that share the same path are deleted, they will be renamed as numbered backups.

`rip` is made for lazy people.  If any part of the interface could be more intuitive, please open an issue or pull request.

## ⚰ Installation
#### Installing this fork
```sh
cargo install --git https://github.com/jeiang/rip/

# or clone and install
git clone https://github.com/jeiang/rip
cd rip
cargo build --release && mv target/release/rip ~/bin
```

## ⚰ Usage

TODO: update usage

## ⚰ Notes
- You probably shouldn't alias `rm` to `rip`.
    - Unlearning muscle memory is hard, but it's harder to ensure that every `rm` you make (as different users, from different machines and application environments) is the aliased one.
    - If you're using `zsh`, it is possible to create a `zsh` function like the following and add it to your `fpath`. This allows the user to use `rm` as `rip`, but if you type `sudo`, then it will use the actual `rm` command.
```zsh
[[ $EUID -ne 0 ]] && rip "${@}" || command rm -I -v "${@}"
```

- If you have `$XDG_DATA_HOME` environment variable set, `rip` will use `$XDG_DATA_HOME/graveyard` instead of the `/tmp/graveyard-$USER`.
- If you want to put the graveyard somewhere else (like `~/.local/share/Trash`), you have two options, in order of precedence:
```zsh
# 1) Aliasing rip
alias rip="rip --graveyard $HOME/.local/share/Trash"

# 2) Set environment variable
export GRAVEYARD="$HOME/.local/share/Trash"
```
 This can be a good idea because if the `graveyard` is mounted on an in-memory filesystem (as `/tmp` is in Arch Linux), deleting large files can quickly fill up your RAM.  It's also much slower to move files across file-systems, although the delay should be minimal with an SSD.

- In general, a deletion followed by a `--unbury` should be idempotent.
- The deletion log is kept in `.record`, found in the top level of the graveyard.

## ⚰ TODO

- Update Usage
- Run tests on github actions
- Remove system dependent stuff (so that it could run on windows)
- Clean up the funky changes that I made
- Add error checking for justfile (e.g when stuff needs to be installed)
- Add nix stuff to justfile
