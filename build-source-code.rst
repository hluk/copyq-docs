Build from Source Code
======================

This page describes how to build the application from source code.

Get the Source Code
-------------------

Download the source code from git repository

::

    git clone https://github.com/hluk/CopyQ.git

or download the latest source code archive from:

- `latest release <https://github.com/hluk/CopyQ/releases>`__
- `master branch in zip <https://github.com/hluk/CopyQ/archive/master.zip>`__
- `master branch in tar.gz <https://github.com/hluk/CopyQ/archive/master.tar.gz>`__


Build and Install
-----------------

Build the source code with CMake and make.

::

    cd CopyQ    # set root source code directory as current
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local .
    make
    make install

