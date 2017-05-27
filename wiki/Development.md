# Development
This page describes CopyQ source code and processes for developers.

## Making Changes

Pull requests are welcome at [github project page](https://github.com/hluk/CopyQ).

Try to keep the code style consistent with the existing code.

## Tests

You can run automated tests if the application is built either in debug mode, with CMake flag `-DWITH_TESTS=ON` or QMake flag `CONFIG+=tests` (releases are usually build with tests).

Run the tests with following command.

```bash
copyq tests
```

While running tests there must be **no keyboard and mouse interaction**.
Preferably you can execute the tests in separate virtual environment.
On Linux you can run the tests on virtual X11 server with `xvfb-run`.

```bash
xvfb-run sh -c 'openbox & sleep 1; copyq tests'
```

Test invocation examples:
- Run specific tests: `copyq tests commandHelp commandVersion`
- Run specific tests for a plugin: `copyq tests 'PLUGINS:pinned' isPinned`
- Run tests only for specific plugins: `copyq tests 'PLUGINS:pinned|tags'`
- List tests: `copyq tests -functions`
- List tests for a plugin: `copyq tests PLUGINS:tags -functions`
- Less verbose tests: `copyq tests -silent`
- Slower GUI tests: `COPYQ_TESTS_KEYS_WAIT=1000 COPYQ_TESTS_KEY_DELAY=50 copyq tests editItems`

## Applications, Frameworks and Libraries

The application is written in C++11 and uses Qt framework.

Source code can be build either with CMake (preferred) or QMake.

```bash
mkdir build
cd build
cmake \
    -DCMAKE_BUILD_TYPE=Debug \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DWITH_QT5=ON \
    -DWITH_TESTS=ON \
    ..
```

Most icons in the application are taken from theme by default (which currently works only on Linux)
with fallback to built-in icons provided by [FontAwesome](http://fontawesome.io/).

Application logo was created in [Blender](https://www.blender.org/)
(scene source is [here](https://github.com/hluk/CopyQ/blob/master/src/images/logo.blend)).

The logo is used for bigger application icon.
Smaller icons were created in [Inkscape](https://inkscape.org/)
(icon source is [here](https://github.com/hluk/CopyQ/blob/master/src/images/icon.svg)).

## Application Processes

There are these system processes:

- main GUI application,
- clipboard monitor (started from main application),
- multiple clients (run scripts in main application).

### Main GUI Application

The main GUI application (or server) can be executed by running `copyq` binary without attributes
(session name can be optionally specified on command line).

It creates local server allowing communication with clipboard monitor process and other client processes.

Each user can run multiple main application processes each with unique session name (default name is empty).

### Clipboard Monitor

Clipboard monitoring happens in separate process because otherwise it would block GUI
(in Qt clipboard needs to be accessed in main GUI thread).
The process is allowed to crash or loop indefinitely due to bugs on some platforms.

Setting and retrieving clipboard can still happen in GUI thread
(copying and pasting in various GUI widgets)
but it's preferred to send and receive clipboard data using monitor process.

The monitor process is launched as soon as GUI application starts
and is restarted whenever it doesn't respond to keep-alive requests.

### Clients and Scripting

Scripting language is [Qt Script](https://doc.qt.io/qt-5/qtscript-index.html)
(mostly same syntax and functions as JavaScript).

API is described in
[src/scriptable/README.md](https://github.com/hluk/CopyQ/blob/master/src/scriptable/README.md).

A script can be started by passing arguments to `copyq`.
This tells the server (main GUI application) to run the script.

After script finishes, the server sends back output of last command and exit code
(non-zero if script crashes).

```bash
copyq eval 'read(0,1,2)' # prints first three items in list
copyq eval 'fail()' # exit code will be non-zero
```

While script is running, it can send print requests to client.

```bash
copyq eval 'print("Hello, "); print("World!\n")'
```

Scripts can ask for stdin from client.

```bash
copyq eval 'var client_stdin = input()'
```

The script run in current directory of client process.

```bash
copyq eval 'Dir().absolutePath()'
copyq eval 'execute("ls", "-l").stdout'
```

Single function call where all arguments are numbers or strings can be executed
by passing function name and function arguments on command line.
Following commands are equal.

```bash
copyq eval 'copy("Hello, World!")'
copyq copy "Hello, World!"
```

Getting application version or help mustn't require the server to be running.

```bash
copyq help
copyq version
```

Scripts run in separate thread and communicate with main thread by calling methods on an object of `ScriptableProxy` class.
If called from non-main thread, these methods invoke a slot on an `QObject` in main thread and pass it a function object which simply calls the method again.

```c++
bool ScriptableProxy::loadTab(const QString &tabName)
{
    // This section is wrapped in an macro so to remove duplicate code.
    if (!m_inMainThread) {
        // Callable object just wraps the lambda so it's possible to send it to a slot.
        auto callable = createCallable([&]{ return loadTab(tabName); });

        m_inMainThread = true;
        QMetaObject::invokeMethod(m_wnd, "invoke", Qt::BlockingQueuedConnection, Q_ARG(Callable*, &callable));
        m_inMainThread = false;

        return callable.result();
    }

    // Now it's possible to call method on an object in main thread.
    return m_wnd->loadTab(tabName);
}
```

## Platform-dependent Code

Code for various platforms is stored in [src/platform](https://github.com/hluk/CopyQ/tree/master/src/platform).

This leverages amount of `#if`s and similar preprocessor directives in common code.

Each supported platform implements [PlatformNativeInterface](https://github.com/hluk/CopyQ/blob/master/src/platform/platformnativeinterface.h)
and `createPlatformNativeInterface()`.

The implementations can contain:

- creating Qt application objects,
- clipboard handling (for clipboard monitor),
- focusing window and getting window titles,
- getting system paths,
- setting "autostart" option,
- handling global shortcuts (**note:** this part is in [qxt/](https://github.com/hluk/CopyQ/tree/master/qxt)).

For unsupported platforms there is [simple implementation](https://github.com/hluk/CopyQ/tree/master/src/platform/dummy)
to get started.

## Plugins

Plugins are built as dynamic libraries which are loaded from runtime plugin directory (platform-dependent) after application start.

Code is stored in [plugins](https://github.com/hluk/CopyQ/tree/master/plugins).

Plugins implement interfaces from [src/item/itemwidget.h](https://github.com/hluk/CopyQ/tree/master/src/item/itemwidget.h).

To create new plugin just duplicate and rewrite an existing plugin.
You can build the plugin with `make {PLUGIN_NAME}`.

## Continuous Integration (CI)

The application binaries and packages are built and tested on multiple CI servers.

- [Travis CI](https://travis-ci.org/hluk/CopyQ)
  - Builds packages for OS X.
  - Builds and runs tests for Linux binaries with Qt 4.

- [GitLab CI](https://gitlab.com/CopyQ/CopyQ/builds)
  - Builds and runs tests for Ubuntu 16.04 binaries with Qt 5.
  - Screenshots are taken while GUI tests are running. These are available if a test fails.

- [AppVeyor](https://ci.appveyor.com/project/hluk/copyq)
  - Builds installers and portable packages for Windows with Qt 5.
  - Provides downloads for recent commits.
  - Release build are based on gcc-compiled binaries (Visual Studio builds are also available).

- [OBS Linux Packages](https://build.opensuse.org/project/show/home:lukho:copyq)
  - Builds release packages for various Linux distributions.

- [Beta OBS Linux Packages](https://build.opensuse.org/project/show/home:lukho:copyq-beta)
  - Builds beta and unstable packages for various Linux distributions.

- [Coveralls](https://coveralls.io/github/hluk/CopyQ)
  - Contains coverage report from tests run with Travis CI.

## Translations

Translations can be done either via [Weblate](https://hosted.weblate.org/projects/copyq/) (preferred)
or by using Qt utilities.

All GUI strings should be translatable.
This is indicated in code with `tr("Some GUI text", "Hints for translators")`.

### Adding New Language

To add new language for the application follow these steps.

1. Edit `copyq.pro` and add file name for new language (`translations/copyq_<LANGUAGE>.ts`) to `TRANSLATIONS` variable.
2. Create new language file with `lupdate copyq.pro`.
3. Add new language file to Git repository.
4. Translate with Weblate service or locally with `linguist translations/copyq_<LANGUAGE>.ts`.
