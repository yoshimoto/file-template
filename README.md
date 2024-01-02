# file-template.el #

file-template.el is an emacs package for expanding the predefined template file when creating a new file.

This originates from a fork of https://www.emacswiki.org/emacs/file-template.el. 
Special thanks to the original author, Scott Frazer.

# Configuration #

## Copy file-template.el into your Emacs load path ##

For example, you might copy it to a directory like `~/.emacs.d/lisp/`:
```sh
cp file-template.el ~/.emacs.d/lisp/
``` 
## Update your .emacs or init.el file ##

Add this.
``` elisp
(use-package file-template
    :commands (file-template-find-file-not-found-hook)
    :config
    (setq file-template-insert-automatically t)
    (setq file-template-paths '("~/.emacs.d/template/"))
    (setq file-template-mapping-alist
          '(("\\.el$"                    . ".template.el")
            ("\\.c$"                     . ".template.c")
            ("\\.\\(cpp\\|cc\\)$"        . ".template.cpp")
            ("\\.h$"                     . ".template.h")
            ("\\.hpp$"                   . ".template.hpp")
            ("\\.sh$"                    . ".template.sh")
            ("\\.tex$"                   . ".template.tex")
            ("\\.py$"                    . ".template.py")
            ("\\Makefile$"               . ".template.make")))
    :init
            (push #'file-template-find-file-not-found-hook find-file-not-found-functions))
```

# Template file #

The template files are searched recursively starting from the current
directory up to the root directory. If no template file is found, the
default template file saved in `~/.emacs.d/template/` will be used.

Here's an example of a template for C source code. You can use this as
the default template by saving it as `~/.emacs.d/template/.template.c`.

``` c
/**
 * @file   %b
 * @author %U <%a>
 * @date   %T
 *
 * @brief
 */
```

The `%b`, `%U`, `%a`, and `%T` are predefined tags, which will be expanded to
the filename, your name with an email address, and the current time.

The full list of predefined tags is as follows.

| tag   | desc.                                                       |
|-------|-------------------------------------------------------------|
| %u    | user's login name                                           |
| %U    | user's full name                                            |
| %a    | user's mail address (from the variable "user-mail-address") |
| %f    | file name with path                                         |
| %b    | file name without path                                      |
| %n    | file name without path and extension                        |
| %N    | file name without path and extension, capitalized           |
| %e    | file extension                                              |
| %E    | file extension capitalized                                  |
| %p    | file directory                                              |
| %T    | current local time, as a human-readable string              |
| %d    | day                                                         |
| %m    | month                                                       |
| %M    | abbreviated month name                                      |
| %y    | last two digits of year                                     |
| %Y    | year                                                        |
| %q    | fill-paragraph                                              |
| %[ %] | prompt user for a string                                    |
| %1-%9 | refer to the nth strings prompted for with %[ %]            |
| %( %) | elisp form to be evaluated                                  |
| %%    | inserts %                                                   |
| %@    | sets the final position of "point"                          |

# Usage #

Create a new file of the specified type in `file-template-mapping-alist`. The template file should be applied automatically.

`file-template.el` supports project-specific templates; it automatically uses template files placed in the project's root directory.

# Examples of template files #

Template file for C header file.
``` c
/**
 * @file   %b
 * @author %U <%a>
 * @date   %T
 *
 * @brief
 *
 *
 */

#if ! defined __%N_%E_INCLUDED__
#define __%N_%E_INCLUDED__

#endif // #if ! defined __%N_%E_INCLUDED__

/**
 * Local Variables:
 * mode: c
 * c-basic-offset: 4
 * coding: utf-8
 * End:
 */
```

Template file for C++ source file.
```c++
//!
// @file   %b
// @author %U <%a>
// @date   %T
//
// @brief
//

// Local Variables:
// mode: c++
// c-basic-offset: 4
// coding: utf-8
// End:
```
