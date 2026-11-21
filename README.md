# opener: `o`

Find a file by name and open it. One small shell script for macOS, Linux and a Raspberry Pi.

```
o decides         # one match: opens it
o creative        # several: lists them by number, newest first; type a number
o review guide    # every word must be in the file name
o -p notes        # print the matching paths only (also automatic in a pipe: vim "$(o -p notes | head -1)")
o -a old plan     # also search archive folders
o -w notes        # also search git worktrees
```

It uses [fd](https://github.com/sharkdp/fd) if it's installed (fast, and it skips files
your `.gitignore` ignores). Otherwise it falls back to `find`, which is slower but needs nothing.

**Why it skips git worktrees.** (`-w` searches them too.) If you run several AI coding agents, each often works in its
own worktree, which is another full copy of the repo. Without this, every file shows up once per
copy. (Written after one search returned 132 results from 20 copies of the same repo.) A worktree
is a folder whose `.git` is a file pointing into another repo's `.git/worktrees/`. Submodules
also have a `.git` file and are not skipped.

## Install

```
mkdir -p ~/.local/bin
curl -fsSL https://raw.githubusercontent.com/rdubar/opener/main/o -o ~/.local/bin/o
chmod +x ~/.local/bin/o
```

On Debian and Raspberry Pi OS, `~/.local/bin` is on your `PATH` after your next login. On macOS
it usually isn't: use a folder that is (`echo $PATH`), or add `export PATH="$HOME/.local/bin:$PATH"`
to `~/.zshrc`. Optional: install fd
(`brew install fd`, or `sudo apt install fd-find` on Debian and Raspberry Pi OS, where
it's called `fdfind`; `o` finds it either way).

Already use `o` for something else? Save the script under another name (`fo`, say);
it doesn't care what it's called.

## Settings

All optional, as environment variables:

| Variable | What it does | Default |
|---|---|---|
| `O_ROOTS` | folders to search, separated by `:` (so a folder name can't contain `:`) | `$HOME` |
| `O_OPEN` | command that opens the file. Split on spaces, so simple arguments work (`code -g`); quotes don't. Use a small script for anything fancier | `$VISUAL`, then `$EDITOR`, then `open` / `xdg-open`, else print the path |
| `O_PINS` | a text file, e.g. your to-do list; a worktree whose folder appears in it (alone, as `~/...`, or inside a longer path) is searched anyway | none |
| `O_MAX` | how many matches to list | 25 |

Example, in `~/.zshrc` or `~/.bashrc`:

```
export O_ROOTS="$HOME/dev:$HOME/writing"
export O_OPEN="zed"
```

## Limits

- It matches file names, not contents.
- Filenames containing a newline aren't supported.
- Without fd, it doesn't know about `.gitignore`, it skips hidden folders but not hidden files,
  and searching a whole home folder takes seconds.
- Worktrees are looked for up to 6 folders deep with fd, 4 without.
- On macOS it skips `~/Library`.

## Licence

MIT.
