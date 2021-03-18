---
title: Command line
---


# Prompting — the shell expects something from you

- When executing interactively, bash displays the primary prompt string `PS1`
  when it is ready to read a command, and the secondary prompt `PS2` when it
  needs more input to complete a command.
- Bash allows these prompt strings (environment variables) to be customized. A
  number of backslash-escaped special characters can be used to display
  various information.

```bash
$ PS1="(\!) \h:\W\$ "
```

  | `\\!` | history number of this command                |
  | `\\h` | hostname up to the first `.`                  |
  | `\\W` | the basename of current working directory     |
  | `\\$` | if effective UID is 0, a `#`, otherwise a `$` |

Look at the prompt string in your machine. Can you figure out how it is built?

Can you figure out what this shell functin does (hint, run it...in a subshell)?

```bash
tutorial_prompt () {
    FONT_RESET=$(tput sgr0)
    FONT_BOLD=$(tput bold)
    PS1="\[${FONT_RESET}\]\w \$ \[${FONT_BOLD}\]"
    trap 'echo -ne "${FONT_RESET}" > $(tty)' DEBUG
}
```

