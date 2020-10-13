---
title: Nuttx
date: 2017-09-08 14:28:00 +0800
categories: [PX4]
tags: [nuttx, px4]
seo:
  date_modified: 2020-03-25 15:19:00 +0800
---
## Nuttx Documentation

* [Nuttx Doc](https://cwiki.apache.org/confluence/display/NUTTX/Documentation)

## PX4 add custom modules(uavcan, ecl, etc..) as NSH "Built-In" Applications
About NSH "Built-In" Applications :

> Overview. In addition to these commands that are a part of NSH, external programs can also be executed as NSH commands. These external programs are called "Built-In" Applications for historic reasons. That terminology is somewhat confusing because the actual NSH commands as described above are truly "built-into" NSH whereas these applications are really external to NuttX. These applications are built-into NSH in the sense that they can be executed by simply typing the name of the application at the NSH prompt. Built-in application support is enabled with these configuration option:  
CONFIG_BUILTIN: Enable NuttX support for builtin applications.  
CONFIG_NSH_BUILTIN_APPS: Enable NSH support for builtin applications.  
When these configuration options are set, you will also be able to see the built-in applications if you enter "nsh> help". They will appear at the bottom of the list of NSH commands under:  
Builtin Apps:

## NSH default commands executes priority
> nice'd Background Commands NSH executes at the mid-priority (128). Backgrounded commands can be made to execute at higher or lower     priorities using nice:  
```
[nice [-d <niceness>>]] <cmd> [> <file>|>> <file>] [&]
```
Where <niceness> is any value between -20 and 19 where lower (more negative values) correspond to higher priorities. The default niceness is  10.

## Register custom commands
> Logic for the context target in apps/examples/hello/Makefile registers the hello_main() application in the builtin's builtin_proto.hand builtin_list.h files. That logic that does that in apps/examples/hello/Makefile is abstracted below:  
First, the Makefile includes apps/Make.defs:
`include $(APPDIR)/Make.defs`  
This defines a macro called REGISTER that adds data to the builtin header files:
```
define REGISTER
    @echo "Register: $1"
    @echo "{ \"$1\", $2, $3, $4 }," >> "$(APPDIR)/builtin/builtin_list.h"
    @echo "EXTERN int $4(int argc, char *argv[]);" >> "$(APPDIR)/builtin/builtin_proto.h"
endef
```
When this macro runs, you will see the output in the build "Register: hello", that is a sure sign that the registration was successful.  
The make file then defines the application name (hello), the task priority (default), and the stack size that will be allocated in the task runs (2K).
```
APPNAME         = hello
PRIORITY        = SCHED_PRIORITY_DEFAULT
STACKSIZE       = 2048
```
And finally, the Makefile invokes the REGISTER macro to added the hello_main() builtin application. Then, when the system build completes, the hello command can be executed from the NSH command line. When the hello command is executed, it will start the task with entry point hello_main() with the default priority and with a stack size of 2K.  
context:  
`$(call REGISTER,$(APPNAME),$(PRIORITY),$(STACKSIZE),$(APPNAME)_main)`


## Realtime Programming
![nuttx-realtime]({{ "/assets/img/sample/nuttx-realtime.png" | relative_url }})
