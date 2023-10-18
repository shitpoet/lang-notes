# C for non-C programmers


## Types

`char` can be signed or unsigned.

`long` is guaranteed to be at least 32 bits.

`long long` is guaranteed to be at least 64 bits.

`short` is a shorthand for `short int`.

`long` is a shorthand for `long int`.

### `stdbool.h` type

    bool (true / false)

### `stdint.h` types:

    int8_t uint8_t
    int16_t uint16_t
    int32_t uint32_t
    int64_t uint64_t

TOCHEK: New C standard also has complex numbers AFAIK.

### Multi-byte strings

String is just `char*` or `char[]`.

There's `wchar_t` for multi-byte strings. AFAIK its size is not specified.

    L"hello" (TOCHECK)


## Literals

    123
    123.5
    1e15
    1e-15
    123L
    0xae15
    0b1010
    'a'
    '\n'
    '\0'
    '\x61' ('a')
    "hello"  // creates extra '\0'


## POSIX IO and stdio IO

POSIX IO is more low-level and thread-safe. Works with file-like objects (sockets, pipes, signal streams).

    open close read write
    remove(name)
    rename(name, name)
    todo: seek, tell

File descriptor is just a `int` number.

There are STDIN_FILENO and STDOUT_FILENO constants.

POSIX functions also can be called through `syscall` assembly instruction.

`stdio.h` functions have buffering by default and aren't thread-safe:

    fopen(name, char* flags)
    fclose(f)
    fprintf(f, ...)
    fscanf(f, ...)
    getchar()  // stdin
    putchar(c)  // stdout
    fgetc
    fputc
    fgets(s, bufsize, f)
    fputc(s, f)
    getline(&buffer, &size, f)
    feof // checks for EOF **after** an op
    fflush(f)
    ...
    todo: seek, tell, buffering?

File descriptor is `FILE*`.

There are `stdin` and `stdout` constants.

On end-of-stream `EOF` is returned.
