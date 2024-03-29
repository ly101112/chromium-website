---
breadcrumbs:
- - /developers
  - For Developers
- - /developers/how-tos
  - How-Tos
page_name: visualstudio-tricks
title: VisualStudio Tricks
---

Here is an incomplete stack of tricks to help you work with Chromium in Visual
Studio.

### Faster Solution Loading / IntelliSense

Loading a huge solution (like all.sln as generated by gn) makes Visual
Studio very slow, and certain operations like IntelliSense can be somewhat
unusable. Some tips for speeding it up:

*   Use Visual Studio 2022, which is significantly more responsive when dealing
    with large solutions, and less likely to crash with out of memory errors.
*   Use the "filters" argument to gn to exclude parts of Chromium
    you're not interested in, e.g. `gn gen --ide=vs
    --filters=//components/omnibox/\* --sln=omnibox out/Debug` will only
    create projects for `components/omnibox/` and its dependencies.
    However, be aware that Code Search (see below) will not work on code
    that's been filtered out!
*   A very useful extension which helps manage this is
    [VsFunnel](https://marketplace.visualstudio.com/items?itemName=DimitriDering.Funnel).
    This extension allows you to select which projects in a solution
    will be loaded at solution load time, and which will remain in the
    "unloaded" state. Unloaded projects are not indexed by VS, so this
    extension can drastically speed up VS responsiveness. The unloaded
    projects will remain searchable via VsChromium (see below).

After opening the solution, give Visual Studio about 10-15 minutes to finish
parsing files etc; it'll become more responsive after that.

### Code Search

The [VsChromium](http://chromium.github.io/vs-chromium/) extension provides
fast code search, among other useful features. Code from unloaded solutions (via
VsFunnel, see above) is still searchable!

### Column Limit

You can set up column guidelines at 80 columns or wherever you would like by
installing [this Visual Studio
extension](https://visualstudiogallery.msdn.microsoft.com/da227a0b-0e31-4a11-8f6b-3a149cf2e459)
and using the context menu options.

Once you've installed the extension, you can automatically add guidelines at the
style-guide-specified line length limits for all Chromium languages as well as
commit messages by downloading [this .editorconfig
file](https://chromium-review.googlesource.com/c/chromium/src/+/4868635/3/.editorconfig)
and placing it in the parent of your `src/` checkout -- that is, the directory
with your `.gclient` file in it. This will also set indenting size/style and
other similar style defaults for Chromium. For more information on EditorConfig
files, see
[here](https://learn.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options?view=vs-2022).

### cpplint.py integration

cpplint.py integration makes it easy to check that a source file conforms to the
style guide. To do this, just go to Tools &gt; External Tools &gt; Add. Specify:

*   Title: cpplint.py
*   Command: c:\\src\\depot_tools\\cpplint.bat
*   Arguments: --output=vs7 $(ItemPath)
*   Initial directory: $(ItemDir)
*   Check Use Output window

To create a keyboard shortcut:

1.  Go to Tools &gt; Options &gt; Environment &gt; Keyboard.
2.  Select Tools.ExternalCommand1. (This assumes cpplint.py is your
    first external command in your Tools menu.)
3.  Press a shortcut key (let's say Alt+L) and Assign it.
4.  Press OK.

Or, make the title something like c&pplint.py, and invoke it with Alt+t,p.

### Text Editor (No tabs, indentation, line numbers)

The style guide requires no tabs and 2 char indentation. If you have copied in
the `.editorconfig` file mentioned above, you should get this automatically
(but only for files inside `src/`). If not, or to set this globally, go to
Tools &gt; Options. On the Text Editor/All languages/Tabs page, set

*   _Indenting_ radio to _Smart_
*   _Tab size_ and _Indent size_ to _2_
*   Check _Insert spaces_

### Debugging visualization, macros, more

See
<http://www.chromium.org/developers/how-tos/how-to-set-up-visual-studio-debugger-visualizers>.
