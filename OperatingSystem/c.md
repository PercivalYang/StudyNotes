# Pointer
## void pointer `void *`
- can hold address of any type
- can be typecasted to any type
- `void *` can't be dereferenced(we can dereference after typecast to other type)

## Double pointer
- first pointer用来存储variable的地址，second pointer用来存储first pointer的地址
- 当初始化first pointer初始化后，例如

# Allocate and free dynamic memory
- `#include <stdlib.h>`
- `void *malloc(size_t size)`: 
    - 分配`size` bytes的内存并返回一个指针指向该内存开头;
- `void free(void *ptr);`
    - `ptr` 必须是a return by previous call to `malloc()`, `calloc()`, `realloc()`
- `void *calloc(size_t nmemb, size_t size);`
    - 为包含`nmemb`个`size` 字节大小元素的array分配内存
- `void *realloc(void *ptr, size_t size);`
    - 将`ptr`指向的memory block更改为`size` bytes
- `void *reallocarray(void *ptr, size_t nmemb, size_t size);`
    - 将`ptr`指向的memory block更改为足够大容纳下`nmemb`个元素，且每个元素大小为`size` bytes的array.

# dirent.h
## DIR结构
```c
struct __dirstream
 {
   int fd;                     /* File descriptor.  */

   __libc_lock_define (, lock) /* Mutex lock for this structure.  */

   size_t allocation;          /* Space allocated for the block.  */
   size_t size;                /* Total valid data in the block.  */
   size_t offset;              /* Current offset into the block.  */

   off_t filepos;              /* Position of next entry to read.  */

   /* Directory block.  */
   char data[0] __attribute__ ((aligned (__alignof__ (void*))));
 };
```
# File operation
## `fopen`
- FILE *fopen(const char *pathname, const char *mode);
    - 返回`FILE *`的pointer，`pathname`是文件路径, `mode`代表文件打开的方式，有如下几种：
        - `r`: Open file for reading, at the beginning of;
        - `r+`: 在`r`基础上, file可以writing or reading;
        - `w`: Truncate file to zero lenght or create file for writing, at the beginning;
        - `w+`: 在`w`基础上, file可以writing or reading;
        - `a`: Open file for appending, create file if not exist;
        - `a+`: zai`a`基础上，file可以reading or appending. 
    - `fopen() mode`与`open() flag`的对应:  
        | fopen() mode | open() flag |
        | :----------: | :---------: |
        | r | O_RDONLY |
        | w | O_WRONLY \| O_CREAT \| O_TRUNC |
        | a | O_WRONLY \| O_CREAT \| O_APPEND|
        | r+ | O_RDWR |
        | w+ | O_RDWR \| O_CREAT \| O_TRUNC |
        | a+ | O_RDWR \| O_CREAT \| O_APPEND|
## `fscanf`
- `int fscanf(FILE *stream, const char *format, ...);`
