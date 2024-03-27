# tmux-fzf-open

## Contents

<!--toc:start-->

- [Introduction](#introduction)
- [Installation](#installation)
  - [Install using TPM](#install-using-tpm)
  - [Install manually](#install-manually)
- [Usage](#usage)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

<!--toc:end-->

## Introduction

`tmux-fzf-open` is a tmux plugin for opening files/links from a tmux pane. It
started as a fork of [wfxr/tmux-fzf-url](https://github.com/wfxr/tmux-fzf-url)
with a preview window added to it. Later, breaking changes were made to make it
make configuration more flexible.

`tmux-fzf-open` has the following features:

- [x] open files/links in a tmux pane with fzf.
- [x] choose any program to open the item.
- [x] add your own regexes to capture more items.
- [x] set a custom sort command.
- [x] enable a preview window that shows the content of the tmux buffer.

![demo](https://github.com/BartSte/tmux-fzf-url/raw/master/demo.gif)

## Installation

Dependencies:

- [`fzf`](https://github.com/junegunn/fzf)

### Install using TPM

Add this line to your tmux config file:

```tmux
set -g @plugin 'BartSte/tmux-fzf-open'
```

### Install manually

Clone this repo and source the `setup.tmux` script in your tmux config file:

```tmux
run "source /path/to/tmux-fzf-open/setup.tmux"
```

## Usage

As can be seen in the [demo](#introduction), `tmux-fzf-open` is activated by
pressing the prefix key (usually `Ctrl+b`), followed by `u`. This will open a
fzf window with the items that are found in the tmux pane. The items are found
by using a regex that is defined in the `@fzf-url-regex` option. By default,
this regex is set to only match URLs and IP addresses. After you selected one or
more items, they will be opened in the program that is defined in the
`@fzf-url-open-cmd` option. By default, this is set to `$BROWSER`, but you
should set it to another program if you also want to items that are not URLs or
IP addresses.

## Configuration

The following configuration is the default configuration:

```tmux
# set -g @fzf-open-regex "<some long regex that matches URLs and IP addresses>"
set -g @fzf-open-extra-regexes "" # empty by default
set -g @fzf-open-open-limit "screen" # only the visible part of the screen is searched
set -g @fzf-open-open-cmd "$BROWSER" # the program that is used to open the items
set -g @fzf-open-sort-cmd "sort -u -t: -k2" # sort the items before passing them to fzf
set -g @fzf-open-fzf-opts "-w 100% -h 50% --multi -0" # fzf options
set -g @fzf-open-fzf-preview true # enable the preview window
```

Below, the options are explained in more detail:

- **regex**: the regex that is used to find items in the tmux pane. By default,
  it is set to only match URLs and IP addresses. The default regex can be found
  in the `fzf-open` script.

- **extra regexes**: if you want to extend the default regex, you can add your
  own regexes to the `@fzf-open-extra-regexes` option.

- **open limit**: by default, only the visible part of the screen is searched
  for items. This can be changed by setting the `@fzf-open-open-limit` option to
  a number. This number is the number of lines that are searched. For example,
  if you set it to `5000`, the first 5000 lines of the tmux buffer are searched
  for items.

- **open cmd**: the program that is used to open the items. By default, it
  is set to `$BROWSER`, but you should set it to another program if you also
  want to items that are not URLs or IP addresses. For example, you can set it
  to `xdg-open` or `open` (if they are available on your system), which will
  open the items in the default program that is associated with the item.

- **sort command**: after searching the tmux buffer, the results are presented
  in the following format: `line_number:match`. Here `line_number` is the line
  number in the tmux buffer, and `match` is the match. Before passing them to
  fzf, any duplicates are removed by using the `sort -u` command. Next, they are
  fed to a command that can be specified by the user.

  When the default command is used, only the `match` part is used for sorting.
  This will remove any duplicates, even if the `line_number` part is different.
  This will limit the number of lines that are shown in the fzf window, and will
  make it easier to find the desired line. However, when using the preview
  window, it may be desirable to display all the lines, such that you can scroll
  through them. In this case, the following may be more suitable:

  ```tmux
  set -g @fzf-url-sort-cmd "sort -n"
  ```

  This will only sort the `line_number` part, and will not remove any
  duplicated matches on different lines.

- **fzf options**: the options that are passed to fzf. It is also possible to
  set the `$FZF_OPEN_OPTS` environment variable instead, as this conforms to the
  fzf way of configuring options. The tmux settings will take precedence over
  the environment variable.

- **preview window**: a preview window can be enabled that shows the content of
  the tmux buffer. The selected fzf line will be highlighted in the preview
  window.

## Troubleshooting

If you encounter any issues, please report them on the issue tracker at:
[tmux-fzf-open issues](https://github.com/BartSte/tmux-fzf-open/issues)

## Contributing

Contributions are welcome! Please see [CONTRIBUTING](./CONTRIBUTING.md) for
more information.

## License

Distributed under the [MIT License](./LICENCE).
