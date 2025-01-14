# ZSH Terminal Commands Reference Guide

## Basic Navigation and File Operations

### Directory Navigation
```bash
pwd                     # Print working directory
cd directory           # Change directory
cd ..                  # Go up one directory
cd ~                   # Go to home directory
cd -                   # Go to previous directory
pushd directory        # Change directory and save previous
popd                   # Return to pushd saved directory
```

### File Operations
```bash
ls                     # List files and directories
ls -la                 # List detailed including hidden files
ls -lh                 # List with human-readable sizes
cp file1 file2         # Copy file
mv file1 file2         # Move/rename file
rm file               # Remove file
rm -r directory       # Remove directory and contents
mkdir directory       # Create directory
mkdir -p dir1/dir2    # Create nested directories
touch file           # Create empty file or update timestamp
```

## File Viewing and Editing

### File Content
```bash
cat file              # Display file content
less file             # View file content with pagination
head file             # Show first 10 lines
tail file             # Show last 10 lines
tail -f file          # Follow file content in real-time
nano file             # Edit file with nano
vim file             # Edit file with vim
```

## File Permissions and Ownership

### Permission Management
```bash
chmod permissions file  # Change file permissions
chmod +x file          # Make file executable
chown user:group file  # Change file ownership
```

## Process Management

### Process Control
```bash
ps                     # Show current processes
ps aux                # Show all processes
top                   # Interactive process viewer
htop                  # Enhanced interactive process viewer
kill PID              # Kill process by ID
killall process_name  # Kill process by name
jobs                  # List background jobs
fg                    # Bring job to foreground
bg                    # Send job to background
```

## System Information

### System Commands
```bash
uname -a              # System information
df -h                 # Disk usage (human readable)
du -sh directory      # Directory size
free -h               # Memory usage
whoami               # Current user
date                 # Current date/time
uptime               # System uptime
```

## ZSH Specific Features

### History Commands
```bash
history              # Show command history
!!                   # Repeat last command
!string              # Run last command starting with string
!$                   # Last argument of previous command
!*                   # All arguments of previous command
ctrl+r               # Search command history
```

### Directory Shortcuts
```bash
..                   # cd ..
...                  # cd ../..
....                 # cd ../../..
~                    # Home directory
/                    # Root directory
```

### Glob Patterns
```bash
*.txt                # Match all .txt files
file?(s)             # Match 'file' or 'files'
file(1|2)            # Match 'file1' or 'file2'
**/pattern           # Recursive match
```

## Advanced ZSH Features

### Aliases and Functions
```bash
alias                # List all aliases
alias name='command' # Create alias
unalias name         # Remove alias
functions            # List all functions
```

### Command Completion
```bash
tab                  # Auto-complete command
tab tab              # Show all completion options
ctrl+space          # Complete word from history
```

### Directory Stack
```bash
dirs                 # Show directory stack
pushd dir           # Push directory to stack
popd                # Pop directory from stack
```

## Network Commands

### Network Operations
```bash
ping host            # Test network connectivity
wget url             # Download file
curl url             # Transfer data from/to server
ssh user@host        # SSH connection
scp file user@host:  # Secure copy file
netstat             # Network statistics
```

## Package Management

### Package Commands (varies by system)
```bash
apt update           # Update package list (Debian/Ubuntu)
apt install package  # Install package
brew install package # Install package (macOS with Homebrew)
brew update         # Update Homebrew
```

## ZSH Customization

### Configuration
```bash
$ZDOTDIR/.zshrc     # ZSH configuration file
$ZDOTDIR/.zprofile  # Login shell configuration
$ZDOTDIR/.zshenv    # Environment variables
source ~/.zshrc     # Reload ZSH configuration
```

## Useful Tips and Tricks

### Command Line Shortcuts
```bash
ctrl+a              # Move to beginning of line
ctrl+e              # Move to end of line
ctrl+u              # Clear line before cursor
ctrl+k              # Clear line after cursor
ctrl+w              # Delete word before cursor
ctrl+l              # Clear screen
```

### Advanced Features
```bash
command | grep pattern  # Filter output
command > file         # Redirect output to file
command >> file        # Append output to file
command1 && command2   # Run command2 if command1 succeeds
command1 || command2   # Run command2 if command1 fails
$(command)            # Command substitution
```

## Best Practices

1. Use tab completion to avoid typing errors
2. Create aliases for frequently used commands
3. Use ctrl+r for history search
4. Utilize directory stack for quick navigation
5. Learn and use glob patterns for file operations
6. Regularly update your .zshrc with useful configurations
7. Back up your ZSH configuration files

## Common Issues and Solutions

1. Command not found
   - Check PATH variable
   - Verify command installation
   - Source .zshrc after changes

2. Permission denied
   - Check file permissions
   - Use sudo when necessary
   - Verify ownership

3. Slow terminal startup
   - Review .zshrc for slow commands
   - Clean up plugins and configurations
   - Use async loading when possible
