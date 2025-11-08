+++
date = '2025-06-19T20:44:09+02:00'
draft = false
title = 'MacOS Truely Hibernate'
theme = 'PaperMod'
+++

![Mac Sleeping](./macos-hibernate.jpg)

## Configure

You only need to set this in the terminal
```sh
sudo pmset -a hibernatemode 25
```

Resetting back to default:

```sh
sudo pmset -a hibernatemode 3
```

## Why do I need it?

I like to truely shutdown my computer and turn it off, so there is no power being wasted.  At the same time I want to continue working right where I left my computer.



pmset -g assertions