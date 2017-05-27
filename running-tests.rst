Tests
=====

This page describes how to run automatic test cases to see if the
application works properly.

The application needs to be build with tests enabled or in debug mode.

To build with tests add ``-DWITH_TESTS=TRUE`` to ``cmake`` build
command.

After application is compiled user can run all tests with:

::

    copyq tests

This command will execute all test cases in new special CopyQ session so
that user configuration, tabs and items are not modified. It's better to
close any other CopyQ session before running tests since they can affect
test results.

To print additional help:

::

    copyq tests --help

to list tests:

::

    copyq tests -functions

or run individual test:

::

    copyq tests "moveAndDeleteItem"
