# Git SCM as a Snap

Git for Snap-supporting environments like Ubuntu Desktop, Ubuntu Core and Ubuntu Touch

## TL;DR: How to install

First ensure your system has the `snapd` package installed.

To install this package, run:

```
sudo snap install git-scm
sudo snap alias git-scm git
```

## General info

This is a package of Git following upstream releases as closely as possible, using the
default build configuration for Git.

It features:

- `git-scm` command: The regular Git command. If you prefer to shorten it as with other
  regular incarnations of Git, make sure you run the alias command from the installation
  section.
- Nano (default) and VIM for when Git requests text input using an editor. The selected
  editor can be change via exporting the `EDITOR=vim` environment variable.
- OpenSSH for proper transfer of objects using SSH
- GnuPG for signing commits, integrated with the host's gpg-agent
- `git-upload-pack` & `git-receive-pack` commands for use on a Git server. More below.

## Hardening of your Git server

Using this Snap package it is possible to harden servers that host Git repositories,
as long as that server supports the full range of security features from snapd, like
AppArmor, SecComp, etc.

The crucial parts are `git-upload-pack` & `git-receive-pack` that need to be enabled
system-wide:

```
sudo snap alias git-scm.git-upload-pack git-upload-pack
sudo snap alias git-scm.git-receive-pack git-receive-pack
```

and ensure `/snap/bin` is in a system-wide `PATH` for SSH to pick it up (default on Ubuntu).

On your server, whenever you push or pull with your development machine it will cause
the handlers for interacting with the hosted repository to be run using this Snap's
strict confinement rules, which are limited to:

- visible files in the remote user's home directory
- removable media accessible by that remote user
- a special directory `$HOME/.local/src` as an analogue to `/usr/local/src`.

To ensure proper security, make sure you don't store secrets in those paths.

## License

GPL v2
