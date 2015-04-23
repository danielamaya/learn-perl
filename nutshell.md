# Perl, in a nutshell

## Variables

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


