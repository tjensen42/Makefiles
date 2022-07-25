# Makefile templates

## Variables
```bash
NAME        := # Executable name

CC          := cc
CFLAGS      := -Wall -Wextra -Werror

CPPFLAGS    := -I./libft/
DEPFLAGS     = -MT $@ -MMD -MP -MF $(DDIR)/$*.d

LDFLAGS     :=
LDLIBS      := libft/libft.a

VPATH       := ./ src/
SRCS        := main.c

ODIR        := obj
OBJS        := $(SRCS:%.c=$(ODIR)/%.o)

DDIR        := $(ODIR)/.deps
DEPS        := $(SRCS:%.c=$(DDIR)/%.d)

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
	CFLAGS += -D LINUX -Wno-unused-result
endif
```


