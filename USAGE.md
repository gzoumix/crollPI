
# Croll-PI Interpreter Uage

## Getting Started.

The interpreter is written in Maude [1]. To run it, you need maude
core installed on your computer. Then just execute maude, and type
"load croll-interpreter.maude": this loads all the definitions of
the syntax and semantics of croll-pi in the maude runtime.

In order to run the interpreter you must provide a configuration to
run. This can be done with the following steps.
 
 - Create a maude source file that will contain the configuration to
   simulate
 - In the first line, write `in croll-interpreter`. This will
   automatically load the interpreter when loading the configuration
   in maude.
 - Create a module including the interpreter for your configuration, e.g.
    ```
    mod MY-CONFIGURATION is protecting CROLL-INTERPRETER . [...] endm
    ```
 - Fill it with your configuration, e.g.
    ```
    op myConf : -> Configuration.
    eq myConf = [...] .
    ```
 - Load your file in maude and type "rew myConf" to simulate it.


## Syntax of the language

The interpreter is based on 7 different sorts:
 - MyInt is the sort of integers (with classic operations)
 - Channel is the sort of channel names
 - Variable is the sort of process variables
 - Thread is the sort of keys
 - Kev is the sort of key variables
 - Process is the sort of processes
 - Configuration is the sort of configurations.

Basically, all the names that are used in a croll-pi process
(i.e. channels, key variables and variables) are Qid, i.e. any string
starting with a quote '. Keys are integers. Then, we have the
following syntax:

```
Process ::=
   Variables | MyInt                                   *** Variables and integers
 | Process ; Process | fst( Process ) | snd( Process ) *** Pairs and operations on pairs
 | Process | Process                                   *** Parallel composition
 | Channel . Process                                   *** Channel binder
 | Channel < Process >: Process                        *** Message sending (any process can be used as a compensation)
 | Channel ( Variable ): Roll -> Process               *** Trigger
 | roll( Kev )                                         *** Roll command
 | if Process then Process else Process fi             *** Conditional

Configuration ::=
   cnil                           *** The empty configuration
 | Configuration | Configuration  *** Parallel composition of configurations
 | Thread : Process               *** A tagged process
 | m(Configuration ; Thread)      *** A Memory
 | Thread (Thread, Thread)        *** A connector
 *** For fresh Names
 | t#( Int )                      *** Counter used to generate fresh threads
 | c#( Int )                      *** Counter used to generate fresh channels
 *** Rollback, low level semantics
 | rlc( Thread )                  *** Rollback command
 | rm( Configuration ; Thread )   *** Tagged memory
 | [ Configuration ]              *** Frozen configuration
```

It is not recommended to write a configuration from scratch, since the
notion of correct configuration is complex, and one must carefully use
the constructors `t#` and `c#`.  croll-interpreter.maude thus provides some
operators to help create names and valid configurations:
 - `main` takes a process as argument and creates a valid configuration from it;
 - `fresh` takes in parameter a Qid an integer, and generates a Qid being the concatenation of the two. For instance `cn('fail, 3) = 'fail3`
 - `cat` takes in parameter two Qid and returns the contatenation of both.

Finally, we provide some classic shortcut for the syntax:
 - "`'a < P >`" sends the process P with an empty compensation (equivalent to "`'a < P >: 0`")
 - "`'a <>`" sends an empty message (equivalent to "`'a < 0 >: 0`")
 - "`'a('X) -> Q`" is a reception without declaring a key variable
 - "`'a -> Q`" is a reception that throw away the received message
 - "`'a !('X): 'gamma -> Q`" is the guarded replication, with the possibility to rollback to `'gamma`
The full list of shortcuts are presented lines 189-199 of the croll-interpreter.maude file. 



[1] http://maude.cs.uiuc.edu/


