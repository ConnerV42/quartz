---
title: "Vim"
tags:
- software-engineering
disableToc: false
---

## Why Vim?
Learning to use Vim is worth it for several reasons. First, Vim (or vi) is almost guaranteed to be installed on any remote server you may need to ssh into. Second, although Vim was released in 1991, it's widely available as a plugin on modern IDEs, like Intellij, VS Code, Eclipse, etc. Third, once you're comfortable with the controls, Vim allows you to quickly navigate to any place on the page with precision. Combining that with the ability to record chains of commands and replay them, it allows for programmatic editing of files on the fly. Once you're comfortable with Vim, you have a text editor at your service that you can on virtually any server.

## Vim Commands
- [Vim Cheat Sheet](https://vim.rtorr.com/)

### Sort Commands
The following command sorts all lines and removes duplicate lines:
```
:sort u
```

### Search Commands
Search for lines containing this pattern `- **`: `?` + `- \*\*`

Find and Replace: `%s/foo/bar/g`

The traditional approach to find lines not matching a pattern is using the :v command:
  - `:v/Warning/p`
  - `:v/Warning/d` (NOTE: this deletes every line that does NOT match this pattern)

### Navigation Commands
- `)` or `shift + 0` to drop to the next newline
- `u` is undo and `ctrl + r` to redo

### Record Macro Commands
- Press `q` then any letter to start recording in a register (ex: `qq`)
- Perform command(s)
- Press `q` to stop recording
- Press `N@q` to run the macro stored in the q register N times

### Window Panes in Vim
- Add this to the .vimrc in your home user's directory to use the mouse in vim: `set mouse=a`
- Use `:Vex` to open a filesystem navigation window in vim
- Use `:ter` or `:vert ter` to open up a shell window
- The mouse access allows you to resize windows and click through a filesystem menu

### Regex for Number Searches
Regex syntax is a little crazy in Vim, but being able to record macros that search for certain patterns is very useful for extracting data.

Search numbers of fixed length say 5 (matches `12345`, `123456`). Numbers more than 5 digits contain substring of 5 digits, and are also found by this regex.
```
/\d\{5\}
```

word boundary start:
```
\<
```

word boundary end:
```
\>
```

Then use below to search numbers of exact 5 digits (match 12345 but not 123456):
```
/\<\d\{5\}\>
```

Use below to search numbers of 5 or more digits:
```
/\<\d\{5,\}\>
```

Use below to search numbers of 5 to 8 digits:
```
/\<\d\{5,8\}\>
```

Use below to search numbers of 8 or less digits:
```
/\<\d\{,8\}\>
```

Shortcut numbers of 1 or more digits

```
/\d\+
```

## .vimrc file
```
set nu
set relativenumber
set scrolloff=8
set mouse=a
set ttymouse=sgr
set clipboard=unnamed
```