[![Build Status](https://travis-ci.org/lizmat/FINALIZER.svg?branch=master)](https://travis-ci.org/lizmat/FINALIZER)

NAME
====

FINALIZER - dynamic finalizing for objects that need finalizing

SYNOPSIS
========

    {
        use FINALIZER;   # enable finalizing for this scope
        my $foo = Foo.new(...);
        # do stuff with $foo
    }
    # $foo has been finalized by exiting the above scope

    # different file / module
    class Foo {
        use FINALIZER;
        method new(|c) {
            my $object = self.bless(|c);
            FINALIZER.register: { $object.finalize }
            $object
        }
        method finalize() {
            # do whatever we need to finalize, e.g. close db connection
        }
    }

DESCRIPTION
===========

FINALIZER allows one to register finalization of objects in the scope that you want, rather than in the scope where objects where created (like one would otherwise do with `LEAVE` blocks or the `is leave` trait).

AS A MODULE DEVELOPER
=====================

If you are a module developer, you need to use the FINALIZE module in your code. In any logic that returns an object (typically the `new` method) that you want finalized at the moment the client decides, you register a code block to be executed when the object should be finalized. Typically that looks something like:

    method new(|c) {
        my $object = self.bless(|c);
        FINALIZER.register: { $object.finalize }
        $object
    }

    FINALIZER.register: { ... }   # code that will do finalization

AS A PROGRAM DEVELOPER
======================

AUTHOR
======

Elizabeth Mattijsen <liz@wenzperl.nl>

Source can be located at: https://github.com/lizmat/FINALIZER . Comments and Pull Requests are welcome.

COPYRIGHT AND LICENSE
=====================

Copyright 2018 Elizabeth Mattijsen

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.
