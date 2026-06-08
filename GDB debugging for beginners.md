### List of requirements:
GDB debugger
Access to terminal
Program to debug

## Example of a code with error:
[Code](https://github.com/Bohuslav66/GDB-Documentation-Refactoring/blob/main/Code.md)

#### List of commands used:
[Commands](https://github.com/Bohuslav66/GDB-Documentation-Refactoring/blob/main/Commands.md)

## Step by Step of fixing the example code
1. To see how the *changequote* works, set a breakpoint at the relevant subroutine.
	```
 	(gdb) break m4_changequote
 	Breakpoint 1 at 0x62f4: file builtin.c, line 879.
	```

2. Command *run* is used to start running the code under GDB control.
   ```
   (gdb) run
 	Startin program: /wor/Editorial/gdb/gnu/m4/m4
 	define(foo, 0000)

 	foo
 	0000
   ```

3. Use *changequote* to suspend execution of m4 and display information about breakpoint.
	```
	changequote(<QUOTE>,<UNQUOTE>)

 	Breakpoint 1, m4_changequote (argc=3, argv=0x33c70)
 		at builtin.c:879
 	879 		if (bad_argc(TOKEN_DATA_TEXT(argv[0]),argc,1,3))
 	```

4. Use the command *next* to advance execution to the next line of current function.
   ```
   (gdb) n
   882 		set_quotes((argc >= 2) ? TOKEN_DATA_TEXT(argv[1])\
   	: nil,
   ```

5. Use command *step* to advance to the next subroutine.
   ```
   (gdb) s
   set_quotes (lq=0x34c78 "<QUOTE>", rq=0x34c88 "<UNQUOTE>")
    at input.c:530
	530         if (lquote != def_lquote)
   ```

6. Use *backtrack* to show a stack frame for each subroutine.
   ```
   (gdb) bt
   #0  set_quotes (lq=0x34c78 "<QUOTE>", rq=0x34c88 "<UNQUOTE>")
    at input.c:530
	#1  0x6344 in m4_changequote (argc=3, argv=0x33c70)
	    at builtin.c:882
	#2  0x8174 in expand_macro (sym=0x33320) at macro.c:242
	#3  0x7a88 in expand_token (obs=0x0, t=209696, td=0xf7fffa30)
	    at macro.c:71
	#4  0x79dc in expand_input () at macro.c:40
	#5  0x2930 in main (argc=0, argv=0xf7fffb20) at m4.c:195
   ```

7. The command *step* can be used twice before falling into *xstrdup* subroutine.
   ```
	(gdb) s
	0x3b5c  532         if (rquote != def_rquote)
	(gdb) s
	0x3b80  535         lquote = (lq == nil || *lq == '\0') ?  \
	def_lquote : xstrdup(lq);
   ```
   
8. Use the command *next* to advance through more lines in current subroutine.
	```
	(gdb) n
	536         rquote = (rq == nil || *rq == '\0') ? def_rquote\
	 : xstrdup(rq);
	(gdb) n
	538         len_lquote = strlen(rquote);
 	```

9. Examine the variables lquote and rquote with command *print*.
    ```
    (gdb) p lquote
	$1 = 0x35d40 "<QUOTE>"
	(gdb) p rquote
	$2 = 0x35d50 "<UNQUOTE>"
    ```
10. For context in code use command *list*.
    ```
    (gdb) l
	533             xfree(rquote);
	534
	535         lquote = (lq == nil || *lq == '\0') ? def_lquote\
	 : xstrdup (lq);
	536         rquote = (rq == nil || *rq == '\0') ? def_rquote\
	 : xstrdup (rq);
	537
	538         len_lquote = strlen(rquote);
	539         len_rquote = strlen(lquote);
	540     }
	541
	542     void
    ```

11. Use command *next* to advance the code to the position of *len_lquote* and *len_rquote*.
    ```
    (gdb) n
	539         len_rquote = strlen(lquote);
	(gdb) n
	540     }
	(gdb) p len_lquote
	$3 = 9
	(gdb) p len_rquote
	$4 = 7
    ```

12. Use the command *print* to display and change the values of *len_lquote* and *len_rquote*.
    ```
    (gdb) p len_lquote=strlen(lquote)
	$5 = 7
	(gdb) p len_rquote=strlen(rquote)
	$6 = 9
    ```

13. Use command *continue* to run code past breakpoint
    ```
    (gdb) c
	Continuing.
	
	define(baz,defn(<QUOTE>foo<UNQUOTE>))
	
	baz
	0000
    ```

14. If the issue is fixxed use command *quit* to exit GDB
	```
 	(gdb) quit
	```
