# Setup Tools

## Git

## Sencha CMD

## Source Code Editor

Any text editor can get the job done, but an important capability to look for is JavaScript inline syntax checking.

Visual Studio Code is a great free and open-source option with built-in git integration and support for `.jshintrc`-powered JavaScript quality checking with its JSHint Extension.

## Web Server

Sencha CMD comes with a simple (but relatively slow) web server you can spin up in any directory with one command: `sencha web start`

Using this server in the long term across multiple projects brings some frustrations though. You have to keep track of when/where you have them started. Your browser's autocomplete is totally jumbled with each work session getting a server started on a different port and possibly at a different path. Reloading pages with hundreds of assets is slow.

Instead, consider installing a permanent, high-performance web server on your development machine to expose your entire workspace of projects at http://localhost/. Apache is a great simple option here, and if you're using a Mac it's already installed and ready to configure+launch on your system. Otherwise, search for "install apache on [your operating system name + version]"

```bash
# TODO: quickstart guides for Mac / Ubuntu to activate apache on ~/Repositories
```
