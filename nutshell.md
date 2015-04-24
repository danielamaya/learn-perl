# Perl, in a nutshell

## Variables

### Names
Variables are named (by you) representations of data. They contain a value. The
name is there so that you can access that value.

__Valid Names__

You can name your variables whatever you want to name them, but they must
conform to the following rules:

1. They must start with a letter or underscore.
2. They must contain only letters, numbers, and underscores.

As well, there are some best practices to follow:

1. Use underscores, not camelCase (e.g., `$this_is_good`, `$thisIsBad`)
2. Make them descriptive of what they do (e.g., $x is not descriptive,
   $first_name is descriptive).

### Sigils

All variables begin with a sigil that denotes what it holds.

    $foo; # scalar (covered below)
    @bar; # array
    %baz; # hash

Sigils can change depending on how you're using the variable, but we will cover
that after we cover the variable types, and context.

### Scalars

Contain a single item:

    $foo = 'Hello World';
    $bar = $foo;

### Arrays

Containers for a list (multiple items), indexed by a number:

    @foo = ('item1', 'item2', 'item3');

Are accessed by an index number, starting with 0:

    print $foo[0]; # prints item1
    print $foo[2]; # prints item3

Can contain any number of elements. The last index is represented by $#array:

    print $foo[$#foo]; # $#foo = 2, prints item3

### Hashes

Container for a list of key/value pairs, indexed by the key:

    %foo = ( 'foo', 'fool', 'bar', 'barge', 'baz', 'bazzle' );

Use "fat comma" to make it more readable:

    %foo = ( foo => 'fool', bar => 'barge', baz => 'bazzle' );

Are accessed by their key:

    print $foo{bar}; # prints barge


## Context

Context is determined by what is on the left side of an assignment. It
specifies how many items you want.

### Void Context

When you call a subroutine but do not use its return value, you are using void
context:

    function();


### Scalar Context

When you call a subroutine and assign the return value to a scalar is scalar
context:

    my $foo =  function();

### List Context

Calling a subroutine and assigning return value to an array or list is list
context:

    my @foo = function();
    my ($foo,$bar,$baz) = function();
    my ($foo) = function();
    my ($foo,@the_rest) = function();

__Remember, context is determined by the left side of an assignment!__

### Sigils & Context

Notice above that when I accessed a single element of an array, I used the
scalar sigil (`$foo[0]`), and same for the hash (`$foo{bar}`). It may seem
intuitive to always access arrays with @, and hashes with %, (e.g., `@foo[0]`,
`%foo{bar}`), but that is incorrect.

Because you are accessing a single element, you use the scalar sigil. Remember
a scalar is a single element, thus a single element of an array or hash is
a scalar. The sigil you use determines the context in which you're using your
variable.

### A Final Word on Context

Understanding context is fundamental to understanding Perl. With a strong
understanding of context, you will see the difference between the following
code:

    print @foo;
    print "@foo\n";

    my $bar = @foo;
    my ($bar) = @foo;

Don't worry if context doesn't come natural at first. Many Perl programmers
have a difficult time with it. The more code you write, the more context will
become natural for you.

### Scope

You should always scope your variables to where they are needed. How does one
do this? Let's cover our first real piece of code:

    #!/usr/bin/env perl
    use strict;
    use warnings;

    my $global = 'Hello!';

    {
      my $scoped = 'World';
      print "$global $scoped\n";
    }

    print "$global $scoped\n";

The program above will fail to run. More on that in a second. 

First, because this is our first program, let's discuss the first three lines.

1. `#!/usr/bin/env perl` - Perl is an interpreted language. Put simply, this
   means that it is not compiled into an executable; rather, it stays in the
   nice readable format you write it in. In order for the system to run it, it
   must be run through an interpreter, the perl executable. This first line is
   called a shebang line, and it tells the system what interpreter we're using
   (in this case perl), and where it's at. The system will then run our
   instructions through the interpreter we've instructed it to.

2. use strict; - This is called a pragma. A pragma is a module (library of
   functions you're including in your code) that impacts the way your script
   behaves. In the case of strict, it will generate an error if you try to use
   a variable that has not been declared. It does a few other things, but we're
   not ready to cover those just yet.

3. use warnings; - Another pragma. This one tells Perl to let us know if
   something wonky is happening in our code.

__A good rule of thumb is to always include those first three lines in every
script you write.__

OK, now on to our program. The first variable in our program ($global) is
a global variable. That means the variable can be accessed from anywhere in our
code, it is not scoped. The second variable ($scoped) is declared within
a block ({ }), and is scoped to that block. That means $scoped cannot be access
outside of the block. The code above will fail because we are trying to use
$scoped outside of the block, and outside of the block $scoped does not exist.

### Why Use Strict?

To save you a massive headache. Take the following code where strict is not
used:

    #!/usr/bin/env perl

    $foo = 'bar';
    print "$fo\n";

Notice the typo in that program? Well, it will run just fine. Now imagine the
headache it will cause when you are trying to find some silly typo in thousands
of lines of code. Using strict saves you that headache:

    #!/usr/bin/env perl

    my $foo = 'bar';
    print "$fo\n";

That will not run, because $fo has not been declared. You cannot use a variable
that has not been declared.

The following example was taken from this very insightful Perlmonks thread:
http://www.perlmonks.org/?node_id=87956

    $count = 10;
    while ($count > 0) {
      print "$count ";
      $count--;
      liftoff() if $count == 0;
    }

    sub liftoff {
      $count++; # count liftoffs
      print "This is liftoff $count\n";
    }


