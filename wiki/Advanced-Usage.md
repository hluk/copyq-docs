Advanced Usage
==============

Tabs
----

Tab layout used by default is similar to modern web browser. Tabs has a simple name and are usually placed at the top of main window.

You can define key hint or accelerator key for new tab by prefixing a letter with ampersand (`&`). For instance tab name "No&tes" will be opened with **Alt+t** shortcut. Similarly "&1" will be opened with **Alt+1**.

To employ more hierarchical structure for tab select **Tree** for "Tab Layout" option in configuration dialog. This enables to use slash (`/`) as path separator in tree of tabs. E.g. creating tabs with name "group/subgroup/tab1" and "group/subgroup/tab2" will make the tree look like this:

    + group
        + subgroup
            - tab1
            - tab2

After tabs are placed in tree they can be moved to different groups by renaming or simply dragging them with mouse.

Sessions
--------

You can run multiple instances of the application given that they have different session names.

To start new instance with `test1` name, run:

    copyq --session=test1

This instance uses configuration, tabs and items unique to given session name.

You can still start default session (with empty session name) with just:

    copyq

In the same manner you can manipulate the session. E.g. to add an item to first tab in `test1` session, run:

    copyq --session=test1 add "Some text"

Default session has empty name but it can be overridden by setting `COPYQ_SESSION_NAME` environment variable.

Writing Raw Data
----------------

Application allows you to save any kind of data using *drag and drop* or scripting interface.

To add an image to `Images` tab you can run:

    cat image1.png | copyq tab Images write image/png -

This works for any other MIME data type (though unknown formats won't be displayed properly).