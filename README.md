# README

## Contents

## Introduction

`tmux-fzf-open` is a tmux plugin for opening files/links from a tmux pane. It
started as a fork of [wfxr/tmux-fzf-url](https://github.com/wfxr/tmux-fzf-url)
with a preview window added to it. Later, breaking changes were made to make the
repo fit more to my personal preferences.

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

### Install using [TPM](https://github.com/tmux-plugins/tpm)

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

** TODO continue here **

The following features have been added to this fork:

- **preview window**: a preview window can be enabled that shows the content of
  the tmux buffer. The selected fzf line will be highlighted in the preview
  window. The preview window is disabled by default, and can be enabled by
  setting:

  ```tmux
  set -g @fzf-url-fzf-preview true
  ```

  Note that it may be desirable to set a custom sort command, as is explained
  below.

- **sort command**: after searching the tmux buffer, the results are presented
  in the following format: `line_number:match`. Here `line_number` is the line
  number in the tmux buffer, and `match` is the match. Before passing them to
  fzf, any duplicates are removed by using the `sort -u` command. Next, they are
  fed to a command that can be specified by the user. By default, the command is

  ```tmux
  set -g @fzf-url-sort-cmd "sort -u -t: -k2"
  ```

  which means that only the `match` part is used for sorting this time, instead
  of the entire line. It will remove any duplicates, even if the `line_number`
  part is different. This will limit the number of lines that are shown in the
  fzf window, and will make it easier to find the desired line. However, when
  using the preview window, it may be desirable to display all the lines, such
  that you can scroll through them. In this case, the following may be more
  suitable:

  ```tmux
  set -g @fzf-url-sort-cmd "sort -n"
  ```

  This will only sort the `line_number` part, and will not remove any
  duplicates.

