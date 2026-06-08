### List of requirements:
GDB debugger
Access to terminal
Program to debug

## Example of a code with error:
```
$ cd gnu/m4
$ ./m4
define(foo,0000)

foo
0000
define(bar,defn(‘foo’))

bar
0000
changequote(<QUOTE>,<UNQUOTE>)

define(baz,defn(<QUOTE>foo<UNQUOTE>))
baz
Ctrl-d

m4: End of input: 0: fatal error: EOF in string
```

#### List of commands used:
[[Commands]]

1. To see how the *changequote* works it is recommended to set a breakpoint at the relevant subroutine.
	```
	(gdb) break m4_changequote
	```

2. By calling *changequote* GDB suspends execution of m4 and displays information about *breakpoint*
	```
	changequote(<QUOTE>,<UNQUOTE>)
	```

3. 