# p5-fork-hook
fork::hook отпределяет 2 функции, которые будут вызваны после форка в дочерних процессах.


### sub AFTER_FORK
AFTER_FORK_OBJ - будет вызываться в каждом пакете где определена эта функция.

#### Пример:
```perl
#!perl

package TEST;

my $old_pid = $$;

sub AFTER_FORK {
	warn "package TEST: parent pid: $old_pid, my pid: $$";
}

package TEST2;

my $old_pid = $$;

sub AFTER_FORK {
	warn "package TEST2: parent pid: $old_pid, my pid: $$";
}

package main;

use fork::hook;

fork;
```
#### Вывод:
	package TEST2: parent pid: 27428, my pid: 27429 at t/fork-hook.t line 40.
	package TEST: parent pid: 27428, my pid: 27429 at t/fork-hook.t line 31.


### sub AFTER_FORK_OBJ
AFTER_FORK_OBJ - будет вызываться как метод объекта, для каждого пакета где определена эта функция.

#### Пример:
```perl
#!perl

package TEST;

my $old_pid = $$;

sub AFTER_FORK_OBJ {
	warn "package TEST, " . $_[0]->{name} . " object : parent pid: $old_pid, my pid: $$";
}

package main;

use fork::hook;

my $object1 = bless { name => 'first' }, 'TEST';
my $object2 = bless { name => 'second' }, 'TEST';
fork;
```
#### Вывод:
	package TEST, first object : parent pid: 27815, my pid: 27816 at t/fork-hook.t line 31.
	package TEST, second object : parent pid: 27815, my pid: 27816 at t/fork-hook.t line 31.
