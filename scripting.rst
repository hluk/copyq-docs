Scripting
=========

If you need to process items in some non-trivial way you can take
advantage of the scripting interface the application provides. This is
accessible on command line as ``copyq eval SCRIPT`` or
``copyq -e SCRIPT`` where ``SCRIPT`` is string containing commands
written in Javascript-similar scripting language (Qt Script with is
ECMAScript scripting language).

Every command line option is available as function in the scripting
interface. Command ``copyq help tab`` can be written as
``copyq eval 'print(help("tab"))'`` (note: ``print`` is needed to print
the return value of ``help("tab")`` function call).

Searching Items
---------------

You can print each item with ``copyq read N`` where N is item number
from 0 to ``copyq size`` (i.e. number of items in the first tab) and put
item to clipboard with ``copyq select N``. With these commands it's
possible to search items and copy the right one with a script. E.g.
having file ``script.js`` containing

::

    var match = "MATCH-THIS";
    var i = 0;
    while (i < size() && str(read(i)).indexOf(match) === -1)
        ++i;
    select(i);

and passing it to CopyQ using ``cat script.js | copyq eval -`` will put
first item containing "MATCH-THIS" string to clipboard.

Working with Tabs
-----------------

By default commands and functions work with items in the first tab.
Calling ``read(0, 1, 2)`` will read first three items from the first
tab. To access items in other tab you need to switch the current tab
with ``tab("TAB_NAME")`` (or ``copyq tab TAB_NAME`` on command line)
where ``TAB_NAME`` is name of the tab.

For example to search for an item as in the previous script but in all
tabs you'll have to run:

::

    var match = "MATCH-THIS";
    var tabs = tab();
    for (var i in tabs) {
        tab(tabs[i]);
        var j = 0;
        while (j < size() && str(read(j)).indexOf(match) === -1)
            ++j;
        if (j < size())
            print("Match in tab \"" + tabs[i] + "\" item number " + j + ".\n");
    }

Scripting Functions
-------------------

As mentioned above, all command line options are also available for
scripting e.g.: ``show()``, ``hide()``, ``toggle()``, ``copy()``,
``paste()``.

Reference for available scripting functions can be found
`here <https://github.com/hluk/CopyQ/blob/master/src/scriptable/README.md>`__.

Other supported functions can be found at `ECMAScript
Reference <http://doc.qt.io/qt-5/ecmascript.html>`__.

Example Commands with Scripting
-------------------------------

-  | UPPERCASE/lowercase
   | https://gist.github.com/m4r71n/6726a03d89fb23aee8c8
   | https://gist.github.com/m4r71n/2ab66d6592e36d767236

-  | Date conversion
   | https://gist.github.com/m4r71n/6fd7452cba0151436639
   | https://gist.github.com/m4r71n/b988322cece3dfb483ee

-  | Strip whitespaces and linebreaks
   | https://gist.github.com/m4r71n/d93fe7fdca3f72e64a52
