# get_next_line

**get_next_line** is a C function that reads and returns one line at a time from any file descriptor. This 42 School project introduces static variables and efficient file I/O techniques, teaching you to handle variable-length lines with optimal memory management and buffer strategies.

## 🚀 Features

- **Line-by-Line Reading**: Returns one complete line per function call
- **Static Variable Management**: Maintains reading state between calls
- **Universal File Descriptor Support**: Works with files, stdin, network sockets
- **Configurable Buffer Size**: Compile-time buffer size configuration
- **Memory Efficient**: Dynamic allocation for variable-length lines
- **Multiple FD Support**: Handle multiple file descriptors simultaneously (bonus)

## 📋 Requirements

- **Language**: C
- **Compilation**: `-Wall -Wextra -Werror`
- **Buffer Size**: Defined via `-D BUFFER_SIZE=n` flag
- **Prototype**: `char *get_next_line(int fd);`
- **Allowed Functions**: `read`, `malloc`, `free`
- **Forbidden**: libft usage, `lseek()`, global variables

## 📁 Project Structure

```
get_next_line/
├── get_next_line.h
├── get_next_line.c
├── get_next_line_utils.c
└── bonus/
    ├── get_next_line_bonus.h
    ├── get_next_line_bonus.c
    └── get_next_line_utils_bonus.c
```

## 🎯 Function Behavior

| Behavior | Description |
|----------|-------------|
| **Return Value** | Next line including `\n`, or `NULL` when finished |
| **Line Endings** | Includes `\n` when present, last line may not have `\n` |
| **Sequential Reading** | Maintains position using static variables |
| **Error Handling** | Returns `NULL` on read errors or invalid FDs |

## 🔧 Usage

### Basic File Reading
```c
#include "get_next_line.h"
#include <fcntl.h>

int main(void)
{
    int fd = open("file.txt", O_RDONLY);
    char *line;
    
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    
    close(fd);
    return (0);
}
```

### Reading from Standard Input
```c
int main(void)
{
    char *line;
    
    while ((line = get_next_line(0)) != NULL)  // stdin
    {
        printf("Input: %s", line);
        free(line);
    }
    return (0);
}
```

### Compilation
```bash
# With specific buffer size
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 *.c -o gnl

# Test different buffer sizes
gcc -D BUFFER_SIZE=1 *.c -o gnl_small
gcc -D BUFFER_SIZE=10000 *.c -o gnl_large
```

## 🎁 Bonus Features

### Multiple File Descriptor Support
```c
int main(void)
{
    int fd1 = open("file1.txt", O_RDONLY);
    int fd2 = open("file2.txt", O_RDONLY);
    
    char *line1 = get_next_line(fd1);  // Read from file1
    char *line2 = get_next_line(fd2);  // Read from file2
    char *line3 = get_next_line(fd1);  // Back to file1
    
    // Process lines...
    free(line1); free(line2); free(line3);
    close(fd1); close(fd2);
    return (0);
}
```

**Bonus Requirements:**
- Single static variable implementation
- No cross-contamination between file descriptors
- Simultaneous state management for multiple FDs

## 🔧 Implementation Strategy

### Core Architecture
```c
char *get_next_line(int fd)
{
    static char *buffer;  // Persists between calls
    char *line;
    
    // Read and assemble line logic
    // Handle leftover data in buffer
    
    return (line);
}
```

### Key Components
- **Buffer Management**: Read `BUFFER_SIZE` chunks efficiently
- **Line Assembly**: Concatenate until `\n` found
- **Static Storage**: Preserve leftover data between calls
- **Memory Safety**: Proper allocation and deallocation

## 🧪 Testing Strategy

### Essential Test Cases
```bash
# Different buffer sizes
gcc -D BUFFER_SIZE=1 *.c && ./a.out
gcc -D BUFFER_SIZE=9999 *.c && ./a.out

# Edge cases
echo -n "No newline" > test.txt     # File without final newline
touch empty.txt                     # Empty file
echo -e "\n\n\n" > newlines.txt     # Only newlines
```

### Test Categories
- ✅ Various buffer sizes (1, 42, 10000)
- ✅ Files with/without final newline
- ✅ Empty files and very long lines
- ✅ Invalid file descriptors
- ✅ Memory leak testing with `valgrind`

## 📊 Helper Functions

Typical utility functions needed:
```c
size_t  ft_strlen(const char *s);
char    *ft_strchr(const char *s, int c);
char    *ft_strjoin(char const *s1, char const *s2);
char    *ft_substr(char const *s, unsigned int start, size_t len);
```

## ⚠️ Implementation Notes

### Critical Considerations
- **Buffer Flexibility**: Must work with any `BUFFER_SIZE` (1 to millions)
- **Memory Management**: Always free returned lines after use
- **Error Handling**: Handle read errors and invalid file descriptors
- **Static Variable Cleanup**: Proper cleanup at end of file

### Common Pitfalls
- Buffer overflow with small buffer sizes
- Memory leaks from improper cleanup
- Incorrect line boundary detection
- Static variable contamination between files

## 🎯 Learning Outcomes

- **Static Variables**: Master persistent state in C functions
- **File I/O**: Efficient reading with system calls
- **Buffer Management**: Smart buffering for performance
- **Memory Management**: Dynamic allocation strategies
- **String Processing**: Incremental string building
- **Error Handling**: Robust file operation error management

## 🔄 Future Applications

After completion, `get_next_line` becomes essential for:
- Configuration file parsing
- Log file processing
- Interactive command-line tools
- Large dataset processing
- Any line-by-line file operations

---

**get_next_line** teaches fundamental file I/O concepts essential for systems programming and forms the backbone of many file processing applications.
