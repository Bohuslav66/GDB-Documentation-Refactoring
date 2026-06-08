``` 1.
$ cd gnu/m4
$ ./m4
define(foo,0000);
define(bar,defn(<QUOTE>,<UNQUOTE>))
```

``` 2.
set width 70
```

``` 3. 
break m4_changequote
```

``` 4.
run
define(foo,0000)
```

``` 5
changequote(<QUOTE>,<UNQUOTE>)
```

``` 6
p lquote
p rquote
```

``` 7
p len_lquote=strlen(lquote)
p len_rquote=strlen(rquote)
```