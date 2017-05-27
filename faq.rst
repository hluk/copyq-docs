FAQ - Frequently Asked Questions
================================

How to preserve the order of copied items on copy or pasting multiple items?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. reverse order of selected items with ``Ctrl+Shift+R`` and copy them
   or
2. select items in reverse order and copy

See
`#165 <https://github.com/hluk/CopyQ/issues/165#issuecomment-34745058>`__

How does pasting single/multiple items internally work?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``Return`` key copies the whole item (with all formats) to the clipboard
and -- if the "Paste to current window" option is enabled -- it sends
``Shift+Insert`` to previous window. So the target application decides
what format to paste on ``Shift+Insert``.

If you select more items and press ``Return``, just the concatenated
text of selected items is put into clipboard. Thought it could do more
in future, like join HTML, images or other formats.

See
`#165 <https://github.com/hluk/CopyQ/issues/165#issuecomment-34957089>`__

How to open the menu or context menu with only the keyboard?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. use ``Alt+I`` to open the item menu
2. use the ``Menu`` key on your keyboard to open the context menu of an
   item

Is it possible to hide menu bar to have even cleaner main window?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Menu bar can be hidden by modifying style sheet of current theme. 1.
Open Preferences dialog (Ctrl+P), 2. go to Appearance, 3. enable
checkbox "Set colors for tabs, tool bar and menus", 4. click "Edit
Theme" button, 5. find ``menu_bar_css`` option and add ``height: 0`` so
it will be something like:

::

    menu_bar_css="
        ;height: 0
        ;background: ${bg}
        ;color: ${fg}"

How to enable logging [STRIKEOUT:under Windows]?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  From CopyQ 2.1.0 there are two new environment variables
   ``COPYQ_LOG_LEVEL`` and ``COPYQ_LOG_FILE`` you can set. See
   `#195 <https://github.com/hluk/CopyQ/issues/195#issuecomment-38729873>`__
-  Before CopyQ 2.1.0: [STRIKEOUT:If want to see console logs you have
   to redirect stdout and stderr with appending ``1> logfile 2>&1`` e.g.
   ``copyq.exe 1> server.log 2>&1``]

How to back up commands?
~~~~~~~~~~~~~~~~~~~~~~~~

-  From CopyQ 2.1.0 there are two buttons under the ``Commands`` tab in
   the preferences. ``Save Selected Commands...`` and ``Load Commands``.
   Additionally you can copy and paste commands from the ``Commands``
   tab in the preferences.

See
`#125 <https://github.com/hluk/CopyQ/issues/125#issuecomment-33514437>`__
and
`#157 <https://github.com/hluk/CopyQ/issues/157#issuecomment-39028552>`__

Where are the notifications?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The default values for ``Interval in seconds to display notifications``
and ``Number of lines for clipboard notification`` are ``0``. After
increasing these values the notifications will be shown.

How to reuse file paths copied from a file manager?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default only the text is stored in item list when you copy of cut
files from a file manager. Other data are usually needed to be able to
copy/paste files from CopyQ.

You have to add new data formats (MIME) to format list in "Data" item
under "Item" configuration tab. Commonly used format in many file
managers is ``text/uri-list``. Other special formats include
``x-special/gnome-copied-files`` for Nautilus,
``application/x-kde-cutselection`` for Dolphin. These formats are used
to specify type of action (copy or cut).

How to paste as plain text?
~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  In the ``Commands/Global Shortcuts dialog [F6]`` you can ``Add`` the
   command ``Paste clipboard as plain text`` and set the desired
   ``Global Shortcut``.
-  In the ``Commands/Global Shortcuts dialog [F6]`` you can ``Add`` the
   command ``Paste as Plain Text``. You can now to paste an item as
   plain text from the item list with ``Shift+Return`` or with the help
   of the context menu.
-  You can also disallow rich text storing (so Shift+Enter pastes plain
   text out-of-the-box) -- go to preferences, "Items" tab and uncheck
   "Web" checkbox under "Text" uncheck "HTML" checkbox. See
   `#308 <https://github.com/hluk/CopyQ/issues/308#issuecomment-69925390>`__

Where to find saved items and configuration?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can find configuration and saved items in: - Windows folder
``%APPDATA%\copyq`` for installed version of the app or ``config``
folder in unzipped portable version, - Linux directory
``~/.config/copyq``.

Run ``copyq info config`` to get absolute path to the configuration file
(parent directory contains saved items).

**NOTE:** Main configuration for installed version of the app on Windows
is stored in registry.

Why are items and configuration not saved?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check access rights to configuration directory and files.

How to load shared commands and share them?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can stumble upon code that looks like this.

.. code-block:: ini

    [Command]
    Name=Show/hide main window
    Command=copyq: toggle()
    Icon=\xf022
    GlobalShortcut=ctrl+shift+1

This code represents a command that can used in CopyQ (specifically it
opens main window on Ctrl+Shift+1). To use the command in CopyQ:

1. copy the code above,
2. open Command dialog F6 in CopyQ,
3. click "Paste Commands" button at the bottom of the dialog,
4. click OK button.

(Now you should be able to open main window with Ctrl+Shift+1.)

To share your commands, you can select the commands from command list in
Command dialog and press "Copy Selected" button (or just hit Ctrl+C).

How to open application window or tray menu using shortcut?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add new command to open window or menu with global shortcut:

1. open Command dialog F6 in CopyQ,
2. click "Add" button in the dialog,
3. select "Show/hide main window" or "Show the tray menu" from the list
   and click "OK" button,
4. click the button next to "Global Shortcut" label and set the
   shortcut,
5. click "OK" button to save the changes.

For more information about commands see :ref:`writing-commands`.

How to omit storing text copied from specific windows like a password manager?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add and modify automatic command to ignore text copied from the window:

1. open Command dialog F6 in CopyQ,
2. click "Add" button in the dialog,
3. select "Ignore *Password* window" from the list and click "OK"
   button,
4. select "Show Advanced"
5. change Window text box to match the title (or part of it) of the
   window to ignore (e.g. ``KeePass``),
6. click "OK" button to save the changes.

**Note:** This new command should be at top of the command list because
automatic commands are executed in order they appear in the list and we
don't want to process sensitive data in any way.

How to paste double-clicked item from application window?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open Preferences (Ctrl+P),
2. go to History tab,
3. enable "Paste to current window" option.

Next time you open main window and activate an item it should be pasted.
