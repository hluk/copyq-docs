Basic Usage
===========

This page describes some of the basic functionality of CopyQ clipboard
manager.

Start the App
-------------

To start the CopyQ double-click the program icon or run command
``copyq``. This starts graphical interface which can be accessed from
tray. Click on the tray icon to show application window (alternatively
right-click on tray icon and select "Show/Hide" or run ``copyq show``
command).

Main element in the window is **list with clipboard history**. By
default the application **stores any new clipboard content** in the
list. This can be temporarily disabled from "File" menu or tray menu or
permanently **disabled in configuration** (clear "Tab for storing
clipboard" in "History" config tab).

If you copy some text it will immediately show at the top of the list.
Try copying few times to see how this works. You should be able to copy
parts of web pages and images if you installed CopyQ with image and web
support.

To assign **system shortcut to quickly open and close application
window**: - open Command dialog from "File" menu "Command/Global
Shortcuts...", - click "Add" button, - select "Show/hide main window"
from list and click OK, - change "Global Shortcut" option.

Basic Item Manipulation
-----------------------

You can **edit text of selected items** in the list by pressing F2.
After editing **save the text** with F2.

Create **new item** with Ctrl+N, type some text and press F2.

**Copy the selected items** back to clipboard with Enter or Ctrl+C.

**Move items** around with Ctrl+Down and Ctrl+Up.

You can move important or special items to new tabs (see
:ref:`advanced-usage-tabs` for more info).

Search
------

In the list you can simply **search for text by typing some text**.

For example typing "Example" will hide items that don't contain
"Example" text. Press Enter to copy the first found item.

Tray
----

To quickly copy item to clipboard you can select the item from tray
menu. To display the menu either right-click on tray icon, run command
``copyq menu`` or use a custom system shortcut.

After selecting an item in tray menu and pressing enter (pressing a
number key works as well) the item is copied to the clipboard.

Command Line
------------

Tabs, items, clipboard and configuration can be changed through command
line interface. Run command ``copyq help`` to see complete list of
commands and their description.

To add new item to tab with name "notes" run:

::

    copyq tab notes add "This is the first note."

To print the item:

::

    copyq tab notes read 0

Add other item:

::

    copyq tab notes add "This is second note."

and print all items in the tab:

::

    copyq eval -- "tab('notes'); for(i=size(); i>0; --i) print(str(read(i-1)) + '\n');"

This will print:

::

    This is the first note.
    This is second note.

Advanced Usage
--------------

Among other things that are possible with CopyQ are: \* open video
player if text copied in clipboard is URL with multimedia, \* store text
copied from a code editor in "code" tab, \* store URLs in different tab,
\* save screenshots (print-screen), \* load all files from directory to
items (create image gallery), \* replace a text in all matching items,
\* run item as a Python script.
