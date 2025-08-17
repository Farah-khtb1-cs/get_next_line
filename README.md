get_next_line - Reading a line from a file descriptor is far too tedious
📖 About
get_next_line (GNL) is a 42 School project that teaches you to implement a function that reads and returns one line at a time from a file descriptor. This project introduces you to static variables in C and efficient file reading techniques. The function is designed to work with any file descriptor, including files, standard input, or network sockets.
🎯 Project Objectives

Static Variables: Master the use of static variables to maintain state between function calls
File I/O: Learn efficient file reading with the read() system call
Buffer Management: Implement smart buffering strategies for optimal performance
Memory Management: Handle dynamic memory allocation for variable-length lines
Edge Case Handling: Manage different line endings, buffer sizes, and file types

📋 Requirements
Technical Specifications

Language: C
Compilation: Must compile with flags -Wall -Wextra -Werror
Compiler: cc
Buffer Size: Defined via -D BUFFER_SIZE=n compilation flag
Prototype: char *get_next_line(int fd);
Return Value: Next line from file descriptor, or NULL when finished/error

Allowed External Functions

read, malloc, free

Forbidden

libft usage: Cannot use your libft in this project
lseek(): Seeking in files is not allowed
Global variables: Must use static variables instead

File Structure
get_next_line/
├── get_next_line.h
├── get_next_line.c
├── get_next_line_utils.c
└── (bonus files)
    ├── get_next_line_bonus.h
    ├── get_next_line_bonus.c
    └── get_next_line_utils_bonus.c
🚀 Function Behavior
Core Functionality

Line Reading: Returns one complete line per call, including \n character
Sequential Access: Maintains reading position between calls using static variables
EOF Handling: Returns NULL when reaching end of file
Error Handling: Returns NULL on read errors or invalid file descriptors

Line Termination Rules

Lines include the terminating \n character when present
The last line may not include \n if the file doesn't end with newline
Empty lines return "\n"

💻 Usage Examples
Basic File Reading
c#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int fd;
    char *line;
    
    fd = open("test.txt", O_RDONLY);
    if (fd == -1)
        return (1);
        
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("Line: %s", line);
        free(line);
    }
    
    close(fd);
    return (0);
}
Reading from Standard Input
c#include "get_next_line.h"
#include <stdio.h>

int main(void)
{
    char *line;
    
    printf("Enter lines (Ctrl+D to quit):\n");
    while ((line = get_next_line(0)) != NULL)  // fd 0 = stdin
    {
        printf("You entered: %s", line);
        free(line);
    }
    
    return (0);
}
Compilation Examples
bash# With specific buffer size
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c main.c -o gnl

# With different buffer sizes for testing
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=1 *.c -o gnl_small
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=10000 *.c -o gnl_large

# Without defining BUFFER_SIZE (should use your default)
gcc -Wall -Wextra -Werror *.c -o gnl_default
🔧 Implementation Strategy
Static Variable Usage
cchar *get_next_line(int fd)
{
    static char *buffer;  // Persists between function calls
    char *line;
    
    // Use buffer to store leftover data from previous reads
    // ...
    
    return (line);
}
Buffer Management Approach

Read Strategy: Read BUFFER_SIZE bytes at a time
Line Assembly: Concatenate chunks until newline found
Leftover Handling: Store remaining data in static buffer
Memory Efficiency: Only allocate what's needed for each line

Key Implementation Considerations

Buffer Size Flexibility: Must work with any buffer size (1, 42, 10000000)
Minimal Reading: Don't read entire file at once
Line Boundary Detection: Stop reading when \n is encountered
Memory Management: Free all allocated memory, prevent leaks

🎁 Bonus Features
Multiple File Descriptor Support
The bonus version can handle multiple file descriptors simultaneously:
cint main(void)
{
    int fd1 = open("file1.txt", O_RDONLY);
    int fd2 = open("file2.txt", O_RDONLY);
    char *line;
    
    // Alternate between files
    line = get_next_line(fd1);  // Read from file1
    printf("File1: %s", line);
    free(line);
    
    line = get_next_line(fd2);  // Read from file2
    printf("File2: %s", line);
    free(line);
    
    line = get_next_line(fd1);  // Back to file1
    printf("File1: %s", line);
    free(line);
    
    close(fd1);
    close(fd2);
    return (0);
}
Bonus Requirements

Single Static Variable: Implement using only one static variable
State Management: Track reading state for multiple file descriptors
No Cross-Contamination: Lines from different FDs must not mix

🧪 Testing Strategy
Test Cases to Cover
Basic Functionality
bash# Test with different buffer sizes
echo -e "Line 1\nLine 2\nLine 3" | ./gnl_test

# Test files without final newline
echo -n "No newline at end" > test_no_newline.txt

# Test empty files
touch empty.txt

# Test very long lines
python -c "print('A' * 10000)" > long_line.txt
Edge Cases

Empty files
Files with only newlines
Files without final newline
Very long lines (longer than buffer size)
Binary files (undefined behavior, but shouldn't crash)
Invalid file descriptors
Files with mixed line lengths

Buffer Size Testing
bash# Test extreme buffer sizes
gcc -D BUFFER_SIZE=1 *.c -o test_1
gcc -D BUFFER_SIZE=999999 *.c -o test_large

# Memory leak testing
valgrind --leak-check=full ./your_program
Performance Considerations

Buffer Size 1: Should still work (though slowly)
Large Buffer: Should handle efficiently without memory waste
Mixed Sizes: Lines shorter and longer than buffer size

⚠️ Important Implementation Notes
Static Variable Best Practices
cstatic char *saved_buffer = NULL;  // Persists between calls
Memory Management

Always free the returned line after use
Handle allocation failures gracefully
Clean up static variables when appropriate

Error Handling

Invalid file descriptors (negative values, etc.)
Read errors (permissions, closed files, etc.)
Memory allocation failures

Common Pitfalls

Buffer Overflow: Ensure proper bounds checking
Memory Leaks: Free all allocated memory
Static Variable Cleanup: Handle end-of-file cleanup
Line Assembly: Correctly joining buffer chunks

🔄 Utility Functions Suggestions
Recommended Helper Functions (get_next_line_utils.c)
csize_t  ft_strlen(const char *s);
char    *ft_strchr(const char *s, int c);
char    *ft_strjoin(char const *s1, char const *s2);
char    *ft_substr(char const *s, unsigned int start, size_t len);
Function Organization

get_next_line.c: Main function and core logic
get_next_line_utils.c: Helper functions for string manipulation
get_next_line.h: Function prototypes and necessary includes

🎯 Success Tips

Start Simple: Implement basic functionality first
Test Early: Test with different buffer sizes from the beginning
Debug Tools: Use printf debugging and valgrind for memory leaks
Edge Cases: Test thoroughly with unusual inputs
Code Organization: Keep functions small and focused
Static Variables: Understand how they maintain state between calls

📚 Learning Outcomes

Static Variables: Master persistent state in C functions
File I/O: Efficient reading strategies with system calls
Buffer Management: Smart buffering for performance optimization
Memory Management: Dynamic allocation for variable-length data
String Manipulation: Building strings incrementally
Error Handling: Robust error detection and recovery

🔄 Integration with Future Projects
After completion, get_next_line becomes a valuable addition to your toolkit:

Reading configuration files
Processing large datasets line by line
Interactive command-line programs
Log file analysis
Any application requiring sequential file processing
