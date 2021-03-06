============================================
 Jedi.el - Python auto-completion for Emacs
============================================

Links:

* `Documentation (at GitHub Pages) <http://tkf.github.com/emacs-jedi/>`_
* `Repository (at GitHub) <https://github.com/tkf/emacs-jedi>`_
* `Issue tracker (at GitHub) <https://github.com/tkf/emacs-jedi/issues>`_
* `Travis CI <https://travis-ci.org/#!/tkf/emacs-jedi>`_ |build-status|

.. |build-status|
   image:: https://secure.travis-ci.org/tkf/emacs-jedi.png
           ?branch=master
   :target: http://travis-ci.org/tkf/emacs-jedi
   :alt: Build Status


What is it?
===========

Jedi.el is a Python auto-completion package for Emacs.
It aims at helping your Python coding in a non-destructive way.
It also helps you to find information about Python objects, such as
docstring, function arguments and code location.

Jedi.el uses jedi_ library to compute completion and EPC_ (an RPC
stack for Emacs Lisp) and its `Python binding`_ to communicate with
Python process.  It also uses excellent Emacs auto-complete_ module to
start completion automatically.  As Jedi.el always calls Python
function asynchronously (thanks to EPC_), it will not block your Emacs
while your are editing.

.. _jedi: https://github.com/davidhalter/jedi
.. _EPC: https://github.com/kiwanami/emacs-epc
.. _Python binding: python-epc_
.. _python-epc: https://github.com/tkf/python-epc
.. _auto-complete: https://github.com/auto-complete/auto-complete


Requirements
============

Emacs lisp modules:
-------------------
- EPC_
- deferred.el_ (> v0.3)
- auto-complete_

If your completion popup is broken when the completion candidates
width are large, try the newest version of popup.el_.

.. _deferred.el: https://github.com/kiwanami/emacs-deferred
.. _popup.el: https://github.com/auto-complete/popup-el

Python modules:
---------------
- Jedi_
- python-epc_
- argparse (for Python 2.6)

Optional dependencies for automatic installation:
-------------------------------------------------
- virtualenv
- make


Install
=======

el-get
------

The easiest way to install Jedi.el is to use el-get_:
just do ``M-x el-get-install jedi``.
You need to have `virtualenv` to automatically install Python module
dependencies.  If your el-get does not have the recipes for Jedi.el
yet, get them from `this pull request`_.

.. _el-get: https://github.com/dimitri/el-get
.. _this pull request: https://github.com/dimitri/el-get/pull/927

Manual install
--------------

1. Install EPC_ and auto-complete_.
2. Install Jedi.el.  Download the repository of Jedi.el and add it to
   `load-path`.
3. Install Jedi_ and python-epc_ by

   - ``make requirements`` or
   - ``pip install jedi epc`` if you want to determine where to
     install them.

4. Add ``(autoload 'jedi:setup "jedi" nil t)`` in your Emacs configuration.


Setup
=====

All you need to do is to call `jedi:setup` in python buffer.
To do that, add the following in your Emacs configuration::

   (add-hook 'python-mode-hook 'jedi:setup)

If auto-completion is all you need, use `jedi:ac-setup` instead::

   (add-hook 'python-mode-hook 'jedi:ac-setup)

To setup recommended keybinds for Jedi.el, add this to your Emacs
configuration.  See its docstring (``<f1> v jedi:setup-keys``) for
more information.::

   (setq jedi:setup-keys t)


Debugging
=========

Start Jedi server in terminal.  When an error occurs, `ipdb` opens up.::

   python jediepcserver.py --port-file jedi-port.log --ipdb

Setup Jedi client like this.::

  (setq jedi:server-command (list "cat" (expand-file-name
                                         "jedi-port.log" jedi:source-dir))
        jedi:server-args nil)

To toggle the above setting and your normal setting, use
``M-x jedi:toggle-debug-server``.
