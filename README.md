# Makefile templates

## Variables
```bash
NAME        := # Executable name

CC          := # Program for compiling C programs; default ‘cc’
CFLAGS      := # Extra flags to give to the C compiler

CPPFLAGS    := # Extra flags to give to the C preprocessor and programs that use it
DEPFLAGS     = # Specific flags which convince the compiler to generate the dependency file

LDFLAGS     := # Linker flags, path where to search for library: -L./libft
LDLIBS      := # Library flags or names: -lm -lft

VPATH       := # A list of directories to be searched for source files: ./src/ ./src/parser
SRCS        := # specify all source files (*.c)

ODIR        := # Dir for .o files (object files)
OBJS        := # $(SRCS:%.c=$(ODIR)/%.o) get object files from src files

DDIR        := # Dir for .d files (dependency files)
DEPS        := # $(SRCS:%.c=$(DDIR)/%.d) get dep files from src files

```

## Auto dependency generation 
* https://make.mad-scientist.net/papers/advanced-auto-dependency-generation/<br>(dependency flags explained at the bottom of the page)

## Linux warn_unused_result fix
<img width="1218" alt="Screenshot 2022-07-25 at 20 44 35" src="https://user-images.githubusercontent.com/56789534/180851762-8bc60ebe-39ec-44f9-babd-fa2a123e637c.png">

```Makefile
# **************************************************************************** #
#   SYSTEM SPECIFIC SETTINGS                                                   #
# **************************************************************************** #

ifeq ($(shell uname -s), Linux)
	CFLAGS += -Wno-unused-result
endif
```

## Parallel compilation with make -j
```Makefile
# It is recommended to use number of CPU cores for the num_of_processes
make -j<num_of_processes>
```
```Makefile
# Makefile builtin approach
UNAME	:= $(shell uname -s)
NUMPROC	:= 8

ifeq ($(UNAME), Linux)
	NUMPROC := $(shell grep -c ^processor /proc/cpuinfo)
else ifeq ($(UNAME), Darwin)
	NUMPROC := $(shell sysctl -n hw.ncpu)
endif

all:
	@$(MAKE) $(NAME) -j$(NUMPROC)
```



