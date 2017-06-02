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

Install Dependencies
--------------------

The build requires:

- `CMake <https://cmake.org/download/>`__
- `Qt <https://download.qt.io/archive/qt/>`__

On **Ubuntu** you can install all build dependencies with:

::

    sudo apt install cmake qtbase5-private-dev qtscript5-dev qttools5-dev qttools5-dev-tools libqt5svg5-dev

Following is optional but recommended:

::

    sudo apt install libxfixes-dev libxtst-dev

Build and Install
-----------------

Build the source code with CMake and make.

::

    cd CopyQ    # set root source code directory as current
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local .
    make
    make install

