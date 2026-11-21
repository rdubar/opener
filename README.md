# opener: `o`

Find a file by name and open it. One small shell script for macOS, Linux and a Raspberry Pi.

```
o decides         # one match: opens it
o creative        # several: lists them by number, newest first; type a number
o review guide    # every word must be in the file name
o -p notes        # print the matches only (for scripts)
o -a old plan     # also search archive folders
o -w notes        # also search git worktrees
```

It uses [fd](https://github.com/sharkdp/fd) if it's installed (fast, and it skips files
your `.gitignore` ignores). Otherwise it falls back to `find`, which is slower but needs nothing.

**Why it skips git worktrees.** If you run several AI coding agents, each often works in its
own worktree, which is a full copy of the repo. Without this, every file shows up once per
copy. (Written after one search returned 132 results, 20 copies of each file.)

## Install

```
curl -fsSL https://raw.githubusercontent.com/rdubar/opener/main/o -o ~/.local/bin/o
chmod +x ~/.local/bin/o
```

Any folder on your `PATH` works instead of `~/.local/bin`. Optional: install fd
(`brew install fd`, or `sudo apt install fd-find` on Debian and Raspberry Pi OS, where
the command is `fdfind`; link it as `fd`).

## Settings

All optional, as environment variables:

| Variable | What it does | Default |
|---|---|---|
| `O_ROOTS` | folders to search, separated by `:` | `$HOME` |
| `O_OPEN` | command that opens the file | `$VISUAL`, then `$EDITOR`, then `open` / `xdg-open`, else print the path |
| `O_PINS` | a file; worktrees whose paths appear in it are searched anyway | none |
| `O_MAX` | how many matches to list | 25 |

Example, in `~/.zshrc` or `~/.bashrc`:

```
export O_ROOTS="$HOME/dev:$HOME/writing"
export O_OPEN="zed"
```

## Licence

MIT.
