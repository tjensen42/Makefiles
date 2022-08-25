# Makefile templates

## Variables
```make
NAME        := # Executable name

CC          := # Program for compiling C programs; default ‘cc’
CFLAGS      := # Extra flags to give to the C compiler

CXX         := # Program for compiling C++ programs
CXXFLAGS    := # Extra flags to give to the C++ compiler

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

### Overview:

```make
# Set the name of the target in the generated dependency file.
-MT $@

# Generate dependency information as a side-effect of compilation, not instead of compilation. This version omits system headers from the generated dependencies: if you prefer to preserve system headers as prerequisites, use -MD.
-MMD

# Adds a target for each prerequisite in the list, to avoid errors when deleting files.
-MP

# Write the generated dependency file $(DEPDIR)/$*.d.
-MF $(DEPDIR)/$*.d

# Generate list of dependency files
DEPS := $(SRCS:%.c=$(DEPDIR)/%.d)

# Declare the generated dependency file as a prerequisite of the target, so that if it’s missing the target will be rebuilt. 
... $(DEPDIR)/%.d

# Declare the dependency directory as an order-only prerequisite of the target, so that it will be created when needed.
... | $(DEPDIR)

# Declare a rule for creating the dependency directory if it doesn’t exist:
$(DEPDIR): ; @mkdir -p $@

# Mention each dependency file as a target, so that make won’t fail if the file doesn’t exist.
$(DEPFILES):

# Include the dependency files that exist
-include $(DEPS)
```

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
