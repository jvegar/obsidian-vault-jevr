#stow #symlink
# Using the `stash` Command in Ubuntu

The `stash` command (also known as `stow`) is a **symlink** manager that helps organize and manage software installations in Unix-like systems. Here's how to use it in Ubuntu:

## Installation

First, install `stow` if you don't have it already:

```bash
sudo apt update
sudo apt install stow
```

## Basic Usage

### Creating a Package

1. Create a directory structure for your package (following standard Unix conventions):

```bash
mkdir -p ~/stow/package_name/bin
mkdir -p ~/stow/package_name/share/man/man1
```

2. Place your files in the appropriate directories:

```bash
cp my_program ~/stow/package_name/bin/
cp my_program.1 ~/stow/package_name/share/man/man1/
```

### Installing (Stowing) a Package

```bash
cd ~/stow
stow package_name
```

This creates symlinks from your package directory to the target directory (usually `/usr/local` by default).

### Uninstalling (Unstowing) a Package

```bash
cd ~/stow
stow -D package_name
```

## Common Options

- `-n` or `--no`: Dry run (show what would be done without actually doing it)
- `-v` or `--verbose`: Show more output
- `-d DIR` or `--dir=DIR`: Specify the stow directory (default is current directory)
- `-t DIR` or `target=DIR`: Specify the target directory (default is parent of stow directory)
- `--ignore=PATTERN`: Ignore files matching the pattern

## Example: Managing Dotfiles

A common use case is managing dotfiles:

```bash
# Create a stow directory for dotfiles
mkdir -p ~/dotfiles

# Move your dotfiles into the stow directory
mv ~/.bashrc ~/dotfiles/
mv ~/.vimrc ~/dotfiles/

# Stow them back to home directory
cd ~/dotfiles
stow -t ~ .
```

This creates symlinks from your home directory to the files in your dotfiles directory.

Would you like me to explain any specific aspect of `stow` in more detail?