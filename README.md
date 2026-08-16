<!--[![Build Status](https://travis-ci.org/akalongman/sublimetext-codeformatter.svg?branch=master)](https://travis-ci.org/akalongman/sublimetext-codeformatter)-->

💡 This is a fork of the [akalongman/sublimetext-codeformatter](https://github.com/akalongman/sublimetext-codeformatter) repo.

---

# CodeFormatter

CodeFormatter is a Sublime Text 2/3/4 plugin that formats (beautifies) source code.

CodeFormatter has support for the following languages:

* PHP - By [phpF](https://github.com/subins2000/phpF)
* JavaScript/JSON - By JSBeautifier
* HTML - By [Custom fork of BeautifulSoup](https://github.com/akalongman/python-beautifulsoup)
* CSS,LESS,SASS - By JSBeautifier
* Python - By PythonTidy (only ST2)
* Go - By [gofmt](https://golang.org/cmd/gofmt/)
* Visual Basic/VBScript
* Coldfusion/Railo/Lucee


## Installing

<!--
**With the Package Control plugin:** The easiest way to install CodeFormatter is through Package Control, which can be found at this site: http://wbond.net/sublime_packages/package_control

Once you install Package Control, restart Sublime Text and bring up the Command Palette (`Command+Shift+P` on OS X, `Control+Shift+P` on Linux/Windows). Select "Package Control: Install Package", wait while Package Control fetches the latest package list, then select CodeFormatter when the list appears. The advantage of using this method is that Package Control will automatically keep CodeFormatter up to date with the latest version.

**Without Git:** Download the latest source from [GitHub](https://github.com/akalongman/sublimetext-codeformatter) and copy the CodeFormatter folder to your Sublime Text "Packages" directory.-->

**With Git:** Clone the repository in your Sublime Text "Packages" directory:

    git clone https://github.com/kyle0r/sublimetext-codeformatter.git CodeFormatter


### Where are package files located?

You can determine these locations in a platform-independent way via the Sublime console:

```
import sublime; print("Packages dir:", sublime.packages_path()); print("Installed Packages dir:", sublime.installed_packages_path())
```

Sublime loads packages from two package roots: the unpacked `Packages/` directory and the `Installed Packages/` directory which typically contains zip archives with the `.sublime-package` extension.

💡 Files placed under `Packages/<PackageName>/` override resources with the same path inside `Installed Packages/<PackageName>.sublime-package`.

## The shared Python environment & Python compatibility

As of 2026-Aug, CodeFormatter contains a number of bundled Python libraries, some of which
date back to the original Sublime Text 2/3 implementation.

This fork has namespaced the bundled formatter dependencies to avoid collisions
with packages installed into Sublime Text's shared Python environment.  

Recently, after installing the MarkdownLivePreview plugin, which installs the `BeautifulSoup` dependency into the shared Python environment,
CodeFormatter stopped working, with the following error:

```
error: CodeFormatter
Format error:
prettify() got an unexpected keyword argument 'indent_size'
```

The root cause was an **import conflict** between the shared `BeautifulSoup` package and CodeFormatter's bundled custom fork.

This fork was created primarily to solve this 👆 issue.

### Current status as of 2026-Aug

This fork's test suite currently passes under Python 3.9:

    145 passed

Sublime Text 4 is transitioning away from its legacy Python 3.3 plugin host.
Build 4200 provides a Python 3.8 plugin host, while newer development builds
have moved the modern plugin host to Python 3.14.

**CodeFormatter** has not yet been fully audited for Python 3.14 compatibility.
In particular, some of its older bundled dependencies use Python APIs that
have since been deprecated or removed.

### Future compatibility work

Before explicitly opting **CodeFormatter** into Sublime Text's modern plugin
host, the following must be considered:

- Test the complete formatter suite under the modern Python versions supported by
  Sublime Text's plugin hosts.
- Audit warnings and deprecated APIs in bundled dependencies.
- Replace removed APIs with compatibility code that continues to support
  older Sublime Text plugin hosts where practical.
- Exercise each formatter inside Sublime's actual plugin host in addition
  to running the standalone test suite.
- Add a `.python-version` only after modern-host compatibility has been
  verified.

This should help to preserve compatibility with existing installations while
making the transition to newer Sublime Text Python runtimes explicit and
testable.

## Configuration

To change the default configurations you have to update the **CodeFormatter - User Preferences** file. You can find this file in the Sublime Text menu bar under: `Sublime Text > Package Settings > CodeFormatter > Settings - User`. 

Make sure that you wrap all the configurations into a single root object.

```js
{
   "codeformatter_php_options": {...},
   "codeformatter_js_options": {...},
   ..
}
```

## Formatter-specific notes
Following are notes specific to individual formatters that you should be aware of:
### PHP
PHP - Used [phpF](https://github.com/subins2000/phpF) by [@subins2000](https://github.com/subins2000)

Getting and installing PHP - http://www.php.net/manual/en/install.general.php

You must install 5.6 or above (https://github.com/subins2000/phpF#requirements)

On Linux/OSx after installation of package, you must set chmod +x to file fmt.phar in folder %PACKAGESDIR%/CodeFormatter/codeformatter/lib/phpbeautifier

You can list all available transformations from Command Palette: CodeFormatter: Show PHP Transformations
Examples of many transformations can be found here: [PHP Transformation Examples](https://github.com/kyle0r/sublimetext-codeformatter/blob/master/PHP-Transformations.md)

Language specific options:
```js
   "codeformatter_php_options":
    {
        "syntaxes": "php", // Syntax names which must process PHP formatter
        "php_path": "", // Path for PHP executable, e.g. "/usr/lib/php" or "C:/Program Files/PHP/php.exe". If empty, uses command "php" from system environments
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "psr1": false, // Activate PSR1 style
        "psr1_naming": false, // Activate PSR1 style - Section 3 and 4.3 - Class and method names case
        "psr2": true, // Activate PSR2 style
        "indent_with_space": 4, // Use spaces instead of tabs for indentation
        "enable_auto_align": true, // Enable auto align of = and =>
        "visibility_order": true, // Fixes visibility order for method in classes - PSR-2 4.2
        "smart_linebreak_after_curly": true, // Convert multistatement blocks into multiline blocks

        // Enable specific transformations. Example: ["ConvertOpenTagWithEcho", "PrettyPrintDocBlocks"]
        // You can list all available transformations from command palette: CodeFormatter: Show PHP Transformations
        // You can also see examples of many transformations at https://github.com/kyle0r/sublimetext-codeformatter/blob/master/PHP-Transformations.md
        "passes": [],

        // Disable specific transformations
        "excludes": []
    }
```



### Javascript/JSON
Javascript/JSON - used [JSBeautifier] (http://jsbeautifier.org/) by Einar Lielmanis

Language specific options:
```js
    "codeformatter_js_options":
    {
        "syntaxes": "javascript,json", // Syntax names which must process JS formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 4, // indentation size
        "indent_char": " ", // Indent character
        "indent_with_tabs": false, // Indent with one tab (overrides indent_size and indent_char options)
        "eol": "\n", // EOL symbol
        "preserve_newlines": false, // whether existing line breaks should be preserved,
        "max_preserve_newlines": 10, // maximum number of line breaks to be preserved in one chunk
        "space_in_paren": false, // Add padding spaces within paren, ie. f( a, b )
        "space_in_empty_paren": false, // Add padding spaces within paren if parent empty, ie. f(  )
        "e4x": false, // Pass E4X xml literals through untouched
        "jslint_happy": false, // if true, then jslint-stricter mode is enforced. Example function () vs function()
        "brace_style": "collapse", // "collapse" | "expand" | "end-expand". put braces on the same line as control statements (default), or put braces on own line (Allman / ANSI style), or just put end braces on own line.
        "keep_array_indentation": false, // keep array indentation.
        "keep_function_indentation": false, // keep function indentation.
        "eval_code": false, // eval code
        "unescape_strings": false, // Decode printable characters encoded in xNN notation
        "wrap_line_length": 0, // Wrap lines at next opportunity after N characters
        "break_chained_methods": false, // Break chained method calls across subsequent lines
        "end_with_newline": false, // Add new line at end of file
        "comma_first": false // Add comma first
    }
```

### HTML
HTML - used custom python port, please use it with caution, feature in early beta

Language specific options:
```js
    "codeformatter_html_options":
    {
        "syntaxes": "html,asp,xml", // Syntax names which must process HTML formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 4, // indentation size
        "indent_char": " ", // Indentation character
        "indent_with_tabs": false, // Indent with one tab (overrides indent_size and indent_char options)
        "exception_on_tag_mismatch": false, // If the last closing tag is not at the same indentation level as the first opening tag, there's probably a tag mismatch in the file
        "expand_javascript": false, // Expand JavaScript inside of <script> tags (also affects CSS purely by coincidence)
        "expand_tags": false, // Expand tag attributes onto new lines
        "minimum_attribute_count": 2, // Minimum number of attributes needed before tag attributes are expanded to new lines
        "first_attribute_on_new_line": false, // Put all attributes on separate lines from the tag (only uses 1 indentation unit as opposed to lining all attributes up with the first)
        "reduce_empty_tags": false, // Put closing tags on same line as opening tag if there is no content between them
        "reduce_whole_word_tags": false, // Put closing tags on same line as opening tag if there is whole word between them
        "custom_singletons": "" // Custom singleton tags for various template languages outside of the HTML5 spec
    }
```

### CSS
CSS - used [JSBeautifier] (http://jsbeautifier.org/) by Einar Lielmanis and Style Css by Harutyun Amirjanyan

Language specific options:
```js
    "codeformatter_css_options":
    {
        "syntaxes": "css,less", // Syntax names which must process CSS formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 4, // Indentation size
        "indent_char": " ", // Indentation character
        "indent_with_tabs": false, // Indent with one tab (overrides indent_size and indent_char options)
        "selector_separator_newline": false, // Add new lines after selector separators
        "end_with_newline": false, // Add new line of end in file
        "newline_between_rules": false, // Add new line between rules
        "eol": "\n" // EOL symbol
    }
```
### SCSS
SCSS - Simply modified the CSS formatter module as per the response from scss-lint (Gives way to modify further)

Language specific options:
```js
    "codeformatter_scss_options":
    {
        "syntaxes": "scss", // Indentation size
        "indent_size": 2, // Indentation size
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_char": " ", // Indentation character
        "indent_with_tabs": false, // Indent with one tab (overrides indent_size and indent_char options)
        "selector_separator_newline": true, // Add new lines after selector separators
        "newline_between_rules": true, // Add new line between rules
        "end_with_newline": true // Add new line of end in file
    }
```

### Python
Python - used [PythonTidy] (https://pypi.python.org/pypi/PythonTidy/) by Chuck Rhode

Language specific options:
```js
    "codeformatter_python_options":
    {
        "syntaxes": "python", // Syntax names which must process Python formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 1, // indentation size
        "indent_with_tabs": true, // Indent with tabs or spaces
        "max_char": 80, // Width of output lines in characters.
        "assignment": " = ", // This is how the assignment operator is to appear.
        "function_param_assignment": "=", // This is how function-parameter assignment should appear.
        "function_param_sep": ", ", // This is how function parameters are separated.
        "list_sep": ", ", // This is how list items are separated.
        "subscript_sep": "=", // This is how subscripts are separated.
        "dict_colon": ": ", // This separates dictionary keys from values.
        "slice_colon": ":", // this separates the start:end indices of slices.
        "comment_prefix": "# ", // This is the sentinel that marks the beginning of a commentary string.
        "shebang": "#!/usr/bin/env python", // Hashbang, a line-one comment naming the Python interpreter to Unix shells.
        "boilerplate": "", // Standard code block (if any). This is inserted after the module doc string on output.
        "blank_line": "", // This is how a blank line is to appear (up to the newline character).
        "keep_blank_lines": true, // If true, preserve one blank where blank(s) are encountered.
        "add_blank_lines_around_comments": true, // If true, set off comment blocks with blanks.
        "add_blank_line_after_doc_string": true, // If true, add blank line after doc strings.
        "max_seps_func_def": 3, // Split lines containing longer function definitions.
        "max_seps_func_ref": 5, // Split lines containing longer function calls.
        "max_seps_series": 5, // Split lines containing longer lists or tuples.
        "max_seps_dict": 3, // Split lines containing longer dictionary definitions.
        "max_lines_before_split_lit": 2, // Split string literals containing more newline characters.
        "left_margin": "", // This is how the left margin is to appear.
        "normalize_doc_strings": false, // If true, normalize white space in doc strings.
        "leftjust_doc_strings": false, // If true, left justify doc strings.
        "wrap_doc_strings": false, // If true, wrap doc strings to max_char.
        "leftjust_comments": false, // If true, left justify comments.
        "wrap_comments": false, // If true, wrap comments to max_char.
        "double_quoted_strings": false, // If true, use quotes instead of apostrophes for string literals.
        "single_quoted_strings": false, // If true, use apostrophes instead of quotes for string literals.
        "can_split_strings": false, // If true, longer strings are split at the max_char.
        "doc_tab_replacement": "....", // This literal replaces tab characters in doc strings and comments.

        // Optionally preserve unassigned constants so that code to be tidied
        // may contain blocks of commented-out lines that have been no-op'ed
        // with leading and trailing triple quotes.  Python scripts may declare
        // constants without assigning them to a variables, but CodeFormatter
        // considers this wasteful and normally elides them.
        "keep_unassigned_constants": false,

        // Optionally omit parentheses around tuples, which are superfluous
        // after all.  Normal CodeFormatter behavior will be still to include them
        // as a sort of tuple display analogous to list displays, dict
        // displays, and yet-to-come set displays.
        "parenthesize_tuple_display": true,

        // When CodeFormatter splits longer lines because max_seps
        // are exceeded, the statement normally is closed before the margin is
        // restored.  The closing bracket, brace, or parenthesis is placed at the
        // current indent level.  This looks ugly to "C" programmers.  When
        // java_style_list_dedent is True, the closing bracket, brace, or
        // parenthesis is brought back left to the indent level of the enclosing
        // statement.
        "java_style_list_dedent": false
    }
```

### Go
Go - used [gofmt](https://golang.org/src/cmd/gofmt/gofmt.go)

Currently no options are supported:
```js
    "codeformatter_go_options":
    {
        "syntaxes": "go",
        "format_on_save": false // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
    }
```

### Visual Basic/VBScript
Visual Basic/VBScript - used custom approach using the HTML beautifier as a guide

Language specific options:
```js
    "codeformatter_vbscript_options":
    {
        "syntaxes": "vbscript", // Syntax names which must process VBScript formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 1, // indentation size
        "indent_char": "\t", // Indentation character
        "indent_with_tabs": true, // Indent with one tab (overrides indent_size and indent_char options)
        "preserve_newlines": true, // Preserve existing line-breaks
        "max_preserve_newlines": 10, // Maximum number of line-breaks to be preserved in one chunk
        "opening_tags": "^(Function .*|Sub .*|If .* Then|For .*|Do While .*|Select Case.*)", // List of keywords which open a new block
        "middle_tags": "^(Else|ElseIf .* Then|Case .*)$", // List of keywords which divide a block, but neither open or close the block
        "closing_tags": "(End Function|End Sub|End If|Next|Loop|End Select)$" // List of keywords which close an open block
    }
```

### Coldfusion Markup Language

Language specific options:
```js
    "codeformatter_coldfusion_options":
    {
        "syntaxes": "coldfusion,cfm,cfml", // Syntax names which must process Coldfusion Markup Language formatter
        "format_on_save": false, // Format on save. Either a boolean (true/false) or a string regexp tested on filename. Example : "^((?!.min.|vendor).)*$"
        "indent_size": 2, // indentation size
        "indent_char": " ", // Indentation character
        "indent_with_tabs": false, // Indent with one tab (overrides indent_size and indent_char options)
        "exception_on_tag_mismatch": false, // If the last closing tag is not at the same indentation level as the first opening tag, there's probably a tag mismatch in the file
        "expand_javascript": false, // Expand JavaScript inside of <script> tags (also affects CSS purely by coincidence)
        "expand_tags": false, // Expand tag attributes onto new lines
        "minimum_attribute_count": 2, // Minimum number of attributes needed before tag attributes are expanded to new lines
        "first_attribute_on_new_line": false // Put all attributes on separate lines from the tag (only uses 1 indentation unit as opposed to lining all attributes up with the first)
        "reduce_empty_tags": false, // Put closing tags on same line as opening tag if there is no content between them
        "reduce_whole_word_tags": false, // Put closing tags on same line as opening tag if there is whole word between them
        "custom_singletons": "" // Custom singleton tags for various template languages outside of the HTML5 spec
    }
```

Usage
-----
Tools -> Command Palette (`Cmd+Shift+P` or `Ctrl+Shift+P`) and type `Format Code`.

You can set up your own key combo for this, by going to Preferences -> Key Bindings - User, and adding a command in that huge array: `{ "keys": ["ctrl+alt+f"], "command": "code_formatter" },`. Default keybinding is `ctrl+alt+f`. You can use any other key you want, though most of them are already taken.

<!--
## TODO

Add other languages support:
* Python (for ST3)
* Perl
* Ruby

Add tests
-->

## Troubleshooting

<!--If you like living on the edge, please report any bugs you find on the [CodeFormatter issues](https://github.com/kyle0r/sublimetext-codeformatter/issues) page.-->
TBD

## Contributing

<!--Pull requests are welcome.
See [CONTRIBUTING.md](CONTRIBUTING.md) for information.-->
TBD

## License

Please see the [LICENSE](LICENSE.md) included in this repository for a full copy of the MIT license,
which this project is licensed under.

## Beautiful Soup fork provenance

📆 In 2026-Aug I wanted to answer:

> From which scm revision was **CodeFormatter's** `BeautifulSoup` forked?

The outcome of the research follows.

CodeFormatter's bundled HTML formatter is based on a custom fork of
**Beautiful Soup 4.4.1** per the fork `PKG-INFO` [here](https://github.com/akalongman/python-beautifulsoup/blob/master/PKG-INFO).

ℹ Prior to Git, the Beautiful Soup project used [Bazaar](https://en.wikipedia.org/wiki/GNU_Bazaar) for source control.

The upstream source has been traced to the Beautiful Soup Bazaar
development branch immediately before the 4.4.1 release:

``` text
Repository:  lp:beautifulsoup
SCM:         Bazaar
Revision:    396
Revision ID: leonardr@segfault.org-20150929000141-8lwjfsxp2z2b4803
Date:        2015-09-28 20:01:41 -0400
```

Revision 396 has the commit message:

``` text
Add a __license__ statement to all source files.
```

The published `beautifulsoup4-4.4.1.tar.gz` source distribution is
derived from this revision. The source files match revision 396 byte-for-byte apart
from the release version being changed from `4.4.0` to `4.4.1`. The
source distribution additionally contains generated packaging metadata
and excludes several repository-only files.

#### Python 3 conversion used by CodeFormatter

An important additional step exists between the fork repository and the copy
bundled by CodeFormatter. Beautiful Soup 4.4.1 was developed from a canonical
Python 2 source tree and included a `convert-py3k` helper which generated its
Python 3 form by copying `bs4/` and running `2to3` over it:

```sh
cp -r bs4/ py3k/
2to3 -w py3k
```

The provenance research establishes the ordering of these steps:

1. `akalongman/python-beautifulsoup` commit `16d60f0` imports the official
   Beautiful Soup 4.4.1 Python 2 source.
2. All **36 files shared** between that commit and the official 4.4.1 source
   distribution are **byte-for-byte identical**. The sdist additionally has
   five generated `beautifulsoup4.egg-info` files, while the fork repository
   has `.gitignore` and `LICENSE`.
3. Commit `95e760c` applies Longman's small custom modifications, including
   the `indent_size` behavior, to the **Python 2 source**.
4. The modified source is subsequently converted with the upstream
   `convert-py3k` / `2to3` workflow.
5. A fresh `2to3` conversion of the `95e760c` Python 2 fork was compared
   against CodeFormatter's vendored `bs4/` tree. After normalizing line
   endings, **8 of the 9 production files are byte-for-byte identical**.
6. The sole production source divergence is `element.py`. In four places,
   CodeFormatter's vendored copy replaces the `2to3` output:

   ```python
   callable(formatter)
   ```

   with:

   ```python
   isinstance(formatter, collections.Callable)
   ```

   There are also insignificant comment/whitespace differences. The upstream
   `bs4/tests/` directory was not vendored into CodeFormatter.

This establishes that CodeFormatter *vendors* the Python 3-converted custom
fork with a very small set of post-conversion edits. This distinction matters
when comparing CodeFormatter's vendored `bs4/` tree with the 4.4.1 source
distribution: a direct recursive diff will include the mechanical Python
2-to-3 transformations, the fork-specific changes, and those later
post-conversion edits.

The resulting provenance is therefore:

```text
Beautiful Soup Bazaar repository
    lp:beautifulsoup
    revno 396
    leonardr@segfault.org-20150929000141-8lwjfsxp2z2b4803
        |
        | release preparation
        | version 4.4.0 -> 4.4.1
        v
beautifulsoup4-4.4.1.tar.gz
        |
        | byte-for-byte import of all 36 shared files
        v
akalongman/python-beautifulsoup @ 16d60f0
    canonical Python 2 source
        |
        | custom modifications @ 95e760c
        | including indent_size
        v
Modified Python 2 fork
        |
        | upstream convert-py3k / 2to3 workflow
        v
Python 3-converted custom fork
        |
        | small post-conversion edits in element.py
        | callable(...) -> isinstance(..., collections.Callable)
        v
CodeFormatter custom Beautiful Soup fork
```

### Provenance methodology

The provenance was established by comparing the historical upstream
source, the official 4.4.1 source distribution, and the source bundled
with CodeFormatter.

1.  The bundled source was identified as a fork of Beautiful Soup 4.4.1
    from its source layout, metadata, and the history of
    `akalongman/python-beautifulsoup`.

2.  The official release archive used for comparison was:

    ``` text
    beautifulsoup4-4.4.1.tar.gz
    ```

3.  The original Bazaar development branch was retrieved from Launchpad:

    ``` sh
    bzr branch lp:beautifulsoup
    ```

4.  The release period was located in the Bazaar history using revision
    IDs and timestamps. The final upstream revision immediately
    preceding the release was:

    ``` text
    revno 396
    leonardr@segfault.org-20150929000141-8lwjfsxp2z2b4803
    ```

    The following mainline revision, 397, was not committed until
    November 2015, placing revision 396 at the head of the branch when
    4.4.1 was released.

5.  Revision 396 was exported from Bazaar:

    ``` sh
    bzr export -r 396 ../bs4-r396
    ```

6.  The exported revision was compared recursively against the official
    4.4.1 source distribution:

    ``` sh
    diff -rq beautifulsoup4-4.4.1 bs4-r396
    ```

    The differences were limited to release preparation and packaging:

    ``` text
    beautifulsoup4.egg-info/       release-generated metadata
    PKG-INFO                       release-generated metadata
    setup.cfg                      release packaging
    prepare-release.sh             repository-only file
    doc.zh/source/index.zh.html    repository-only file
    setup.py                       version difference
    bs4/__init__.py                version difference
    ```

7.  The two differing source files were inspected individually. In both
    cases the substantive difference was only the release version:

    ``` text
    4.4.0 -> 4.4.1
    ```

8.  A clone of `akalongman/python-beautifulsoup` was checked out at
    commit `16d60f0` and compared with the official 4.4.1 source
    distribution. All 36 files common to both trees were byte-for-byte
    identical. The only differences were five generated
    `beautifulsoup4.egg-info` files present only in the sdist and `.gitignore`
    plus `LICENSE` present only in the fork repository.

9.  Commit `95e760c` was verified to apply the custom changes to the Python 2
    source. The Python 3 form vendored by CodeFormatter therefore comes
    *after* the custom modifications, via Beautiful Soup's own
    `convert-py3k` / `2to3` workflow:

    ``` sh
    cp -r bs4/ py3k/
    2to3 -w py3k
    ```

10. The `95e760c` source was freshly converted with that workflow and compared
    against CodeFormatter's vendored `bs4/` tree. After normalizing line
    endings, 8 of the 9 production files were byte-for-byte identical. The
    only differing production file was `element.py`.

11. The substantive post-conversion difference in `element.py` consists of
    four substitutions of `callable(formatter)` with
    `isinstance(formatter, collections.Callable)`, plus insignificant
    comment/whitespace differences. CodeFormatter also omits the upstream
    `bs4/tests/` directory.

This establishes **Bazaar revision 396** as the upstream source revision from
which Beautiful Soup 4.4.1 was prepared, and establishes the complete
CodeFormatter lineage as: **4.4.1 Python 2 source -> fork-specific
modifications -> `2to3` conversion -> small post-conversion edits ->
CodeFormatter vendored Python 3 copy**.

The `collections.Callable` substitutions were also noteworthy for future
Python compatibility: they were not produced by `2to3`, and
`collections.Callable` was later removed from modern Python. This fork has
now resolved that divergence by selectively restoring the original
`callable(...)` forms generated by `2to3`, while retaining the separate
namespace-isolation changes required by CodeFormatter.

### Hypothesis on updating CodeFormatter's `BeautifulSoup` to the upstream mainline

Based on the provenance research above, and in relation to CodeFormatter's [custom fork of Beautiful Soup](https://github.com/akalongman/python-beautifulsoup)...

Revision `16d60f0` imports the `BeautifulSoup` Python 2 source release [v4.4.1](https://www.crummy.com/software/BeautifulSoup/bs4/download/4.4/beautifulsoup4-4.4.1.tar.gz).
All 36 files shared by the two trees are byte-for-byte identical. The only differences are packaging/repository extras.

Revision `95e760c` applies the fork-specific modifications to the Python 2 source.

💡 The fork-specific code delta is very small: only `+7 -4` lines of actual code. Maintaining a full vendored fork for such a small behavioral change created a disproportionate long-term maintenance burden.

The actual custom Python 2 code diff is [here](https://github.com/akalongman/python-beautifulsoup/commit/95e760c75e517226f668901ca9c83401c131d94c#diff-9b9f42c49921229538b6bb7301174daa06310c2c8311a305206127053d89283c), and the actual patch [here](https://github.com/akalongman/python-beautifulsoup/commit/95e760c75e517226f668901ca9c83401c131d94c.patch).

The later CodeFormatter vendoring step introduced only a tiny additional
production-code delta after `2to3`: four `callable(formatter)` checks in
`element.py` were changed to `isinstance(formatter, collections.Callable)`.
Those changes are separate from the original custom indentation patch and
became a source of incompatibility with newer Python versions.

This fork has resolved that historical post-conversion divergence by
selectively restoring the `callable(formatter)` forms produced by `2to3`.
The namespace-isolation import change in `element.py` is intentionally
retained, since it prevents the vendored Beautiful Soup fork from colliding
with a shared `bs4` installation.

Therefore, it may be practical to carry the small CodeFormatter-specific behavior as a patch against a newer Beautiful Soup release. This still needs validation because the relevant Beautiful Soup APIs and internals have evolved substantially since 4.4.1.

What is the motivation to turn the custom BeautifulSoup fork into a mainline patch?

1. Replace the decade-old Beautiful Soup 4.4.1-derived vendored code with a maintained release.
1. Reduce exposure to removed/deprecated Python APIs as Sublime Text moves to newer plugin-host Python versions.
1. Retain CodeFormatter's custom indentation behavior as a small, explicit patch rather than maintaining an opaque historical fork.