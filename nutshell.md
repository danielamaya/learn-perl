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
intuitive to always access arrays with @, and hashes with %, (e.g., `@foo[0]`),
but that is incorrect.

Because you are accessing a single element, you use the scalar sigil. Remember
a scalar is a single element, thus the context of a single element is scalar.
The sigil you use will also determine the context of your variables.

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


