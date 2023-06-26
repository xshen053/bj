# Bug 1

## A) How is your program acting differently than you expect it to?

Error when compiling

```
clang_example.c:21:66: error: no matching function for call to 'clang_getCursorKindSpelling'
    printf("%s (%s)\n", clang_getCString(name), clang_getCString(clang_getCursorKindSpelling(kind)));
                                                                 ^~~~~~~~~~~~~~~~~~~~~~~~~~~
/usr/lib/llvm-14/include/clang-c/Index.h:5121:25: note: candidate function not viable: no known conversion from 'int' to 'enum CXCursorKind' for 1st argument
CINDEX_LINKAGE CXString clang_getCursorKindSpelling(enum CXCursorKind Kind);
```

## B) Brainstorm a few possible causes of the bug
Line 5121 in Index.h

`CINDEX_LINKAGE CXString clang_getCursorKindSpelling(enum CXCursorKind Kind);`

The type of parameter is enum CXCursorKind rather than int

Probably because compiler I used cannot implicitly converse from 'int' to 'enum CXCursorKind'

## C) How you fixed the bug and why the fix was necessary
Changed from

`int kind = clang_getCursorKind(cursor);`

to 

`enum CXCursorKind kind = clang_getCursorKind(cursor);`


# Bug 2

## A) How is your program acting differently than you expect it to?
Error when compiling

```
clang++ -std=c++17 -Wall -Wextra -I /usr/lib/llvm-14/include/  -o clang_example clang_example.o
/usr/bin/ld: clang_example.o: in function `printAST(CXCursor, CXCursor, void*)':
clang_example.c:(.text+0x64): undefined reference to `clang_getCursorLocation'
/usr/bin/ld: clang_example.c:(.text+0xb5): undefined reference to `clang_getCursorSpelling'
/usr/bin/ld: clang_example.c:(.text+0x11c): undefined reference to `clang_getCursorKind'
/usr/bin/ld: clang_example.c:(.text+0x191): undefined reference to `clang_getCString'
/usr/bin/ld: clang_example.c:(.text+0x1a0): undefined reference to `clang_getCursorKindSpelling'
/usr/bin/ld: clang_example.c:(.text+0x1bf): undefined reference to `clang_getCString'
/usr/bin/ld: clang_example.c:(.text+0x1ff): undefined reference to `clang_disposeString'
/usr/bin/ld: clang_example.c:(.text+0x27c): undefined reference to `clang_visitChildren'
/usr/bin/ld: clang_example.o: in function `main':
clang_example.c:(.text+0x2b2): undefined reference to `clang_createIndex'
/usr/bin/ld: clang_example.c:(.text+0x2d8): undefined reference to `clang_parseTranslationUnit'
/usr/bin/ld: clang_example.c:(.text+0x30e): undefined reference to `clang_getTranslationUnitCursor'
/usr/bin/ld: clang_example.c:(.text+0x36b): undefined reference to `clang_visitChildren'
/usr/bin/ld: clang_example.c:(.text+0x374): undefined reference to `clang_disposeTranslationUnit'
/usr/bin/ld: clang_example.c:(.text+0x37d): undefined reference to `clang_disposeIndex'
```
## B) Brainstorm a few possible causes of the bug

The linker error we encountered indicates that the linker cannot find the definitions for the functions clang_getCursorLocation, clang_getCursorSpelling, and clang_getCursorKind. These functions are part of the Clang API and should be provided by the Clang library.


If we take a look at `Index.h`, we will see 
`CINDEX_LINKAGE CXString clang_getCursorSpelling(CXCursor);`
the `CINDEX_LINKAGE` macro suggests that the functions with `clang_ prefix`, such as `clang_getCursorLocation`, `clang_getCursorSpelling`, and `clang_getCursorKind`, are part of the Clang API.
## C) How you fixed the bug and why the fix was necessary


So we need to add `-lclang` flag in our Makefile to link to Clang library

`CFLAGS=-std=c++17 -Wall -Wextra -I /usr/lib/llvm-14/include/ -lclang
`




# Bug 1

## A) How is your program acting differently than you expect it to?

## B) Brainstorm a few possible causes of the bug


## C) How you fixed the bug and why the fix was necessary

# Bug 1

## A) How is your program acting differently than you expect it to?

## B) Brainstorm a few possible causes of the bug


## C) How you fixed the bug and why the fix was necessary

# Bug 1

## A) How is your program acting differently than you expect it to?

## B) Brainstorm a few possible causes of the bug


## C) How you fixed the bug and why the fix was necessary

# Bug 1

## A) How is your program acting differently than you expect it to?

## B) Brainstorm a few possible causes of the bug


## C) How you fixed the bug and why the fix was necessary