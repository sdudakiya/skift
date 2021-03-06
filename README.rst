skift
######
|PyPI-Status| |PyPI-Versions| |Build-Status| |Codecov| |LICENCE|

``scikit-learn`` wrappers for Python ``fastText``.

.. code-block:: python

  >>> from skift import FirstColFtClassifier
  >>> df = pandas.DataFrame([['woof', 0], ['meow', 1]], columns=['txt', 'lbl'])
  >>> sk_clf = FirstColFtClassifier(lr=0.3, epoch=10)
  >>> sk_clf.fit(df[['txt']], df['lbl'])
  >>> sk_clf.predict([['woof']])
  [0]

.. contents::

.. section-numbering::


Installation
============

Dependencies:

* ``numpy``
* ``scipy``
* ``scikit-learn``
* ``fastText`` Python package

.. code-block:: bash

  pip install skift
  

**NOTICE:** Installing ``skift`` will not install ``fasttext`` itself. To have ``skift`` install ``fasttext`` for you, including `the official Python bindings <https://github.com/facebookresearch/fastText/tree/master/python>`_, run:

.. code-block:: bash

  pip install skift[fasttext] --process-dependency-links


Features
========

* Adheres to the ``scikit-learn`` classifier API, including ``predict_proba``.
* Also caters to the common use case of ``pandas.DataFrame`` inputs.
* Enables easy stacking of ``fastText`` with other types of ``scikit-learn``-compliant classifiers.
* Pickle-able classifier objects.
* Built around the `official fasttext Python bindings <https://github.com/facebookresearch/fastText/tree/master/python>`_.
* Pure python.
* Supports Python 3.4+.
* Fully tested.


Wrappers
=========

``fastText`` works only on text data, which means that it will only use a single column from a dataset which might contain many feature columns of different types. As such, a common use case is to have the ``fastText`` classifier use a single column as input, ignoring other columns. This is especially true when ``fastText`` is to be used as one of several classifiers in a stacking classifier, with other classifiers using non-textual features. 

``skift`` includes several ``scikit-learn``-compatible wrappers (for the `official <https://github.com/facebookresearch/fastText/tree/master/python>`_ ``fastText`` Python bindings) which cater to these use cases.

**NOTICE:** Any additional keyword arguments provided to the classifier constructor, besides those required, will be forwarded to the ``fastText.train_supervised`` method on every call to ``fit``.

Standard wrappers
-----------------

These wrappers do not make additional assumptions on input besides those commonly made by ``scikit-learn`` classifies; i.e. that input is a 2d ``ndarray`` object and such.

* ``FirstColFtClassifier`` - An sklearn classifier adapter for fasttext that takes the first column of input ``ndarray`` objects as input.

.. code-block:: python

  >>> from skift import FirstColFtClassifier
  >>> df = pandas.DataFrame([['woof', 0], ['meow', 1]], columns=['txt', 'lbl'])
  >>> sk_clf = FirstColFtClassifier(lr=0.3, epoch=10)
  >>> sk_clf.fit(df[['txt']], df['lbl'])
  >>> sk_clf.predict([['woof']])
  [0]

* ``IdxBasedFtClassifier`` - An sklearn classifier adapter for fasttext that takes input by column index. This is set on object construction by providing the ``input_ix`` parameter to the constructor.

.. code-block:: python

  >>> from skift import IdxBasedFtClassifier
  >>> df = pandas.DataFrame([[5, 'woof', 0], [83, 'meow', 1]], columns=['count', 'txt', 'lbl'])
  >>> sk_clf = IdxBasedFtClassifier(input_ix=1, lr=0.4, epoch=6)
  >>> sk_clf.fit(df[['count', 'txt']], df['lbl'])
  >>> sk_clf.predict([['woof']])
  [0]



pandas-dependent wrappers
-------------------------

These wrappers assume the ``X`` parameters given to ``fit``, ``predict``, and ``predict_proba`` methods is a ``pandas.DataFrame`` object:

* ``FirstObjFtClassifier`` - An sklearn adapter for fasttext using the first column of ``dtype == object`` as input.

.. code-block:: python

  >>> from skift import FirstObjFtClassifier
  >>> df = pandas.DataFrame([['woof', 0], ['meow', 1]], columns=['txt', 'lbl'])
  >>> sk_clf = FirstObjFtClassifier(lr=0.2)
  >>> sk_clf.fit(df[['txt']], df['lbl'])
  >>> sk_clf.predict([['woof']])
  [0]

* ``ColLblBasedFtClassifier`` - An sklearn adapter for fasttext taking input by column label. This is set on object construction by providing the ``input_col_lbl`` parameter to the constructor.

.. code-block:: python

  >>> from skift import ColLblBasedFtClassifier
  >>> df = pandas.DataFrame([['woof', 0], ['meow', 1]], columns=['txt', 'lbl'])
  >>> sk_clf = ColLblBasedFtClassifier(input_col_lbl='txt', epoch=8)
  >>> sk_clf.fit(df[['txt']], df['lbl'])
  >>> sk_clf.predict([['woof']])
  [0]

Contributing
============

Package author and current maintainer is Shay Palachy (shay.palachy@gmail.com); You are more than welcome to approach him for help. Contributions are very welcomed.

Installing for development
----------------------------

Clone:

.. code-block:: bash

  git clone git@github.com:shaypal5/skift.git


Install in development mode, including test dependencies:

.. code-block:: bash

  cd skift
  pip install -e '.[test]'
  # or with pipenv
  pipenv install --dev


To also install ``fasttext``:

.. code-block:: bash

  cd skift
  pip install -e '.[test,fasttext]' --process-dependency-links


Running the tests
-----------------

To run the tests use:

.. code-block:: bash

  cd skift
  pytest
  # or with pipenv
  pipenv run pytest


Adding documentation
--------------------

The project is documented using the `numpy docstring conventions`_, which were chosen as they are perhaps the most widely-spread conventions that are both supported by common tools such as Sphinx and result in human-readable docstrings. When documenting code you add to this project, follow `these conventions`_.

.. _`numpy docstring conventions`: https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt
.. _`these conventions`: https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt

Additionally, if you update this ``README.rst`` file,  use ``python setup.py checkdocs`` (or ``pipenv run`` the same command) to validate it compiles.


Credits
=======

Created by Shay Palachy (shay.palachy@gmail.com).


.. |PyPI-Status| image:: https://img.shields.io/pypi/v/skift.svg
  :target: https://pypi.python.org/pypi/skift

.. |PyPI-Versions| image:: https://img.shields.io/pypi/pyversions/skift.svg
   :target: https://pypi.python.org/pypi/skift

.. |Build-Status| image:: https://travis-ci.org/shaypal5/skift.svg?branch=master
  :target: https://travis-ci.org/shaypal5/skift

.. |LICENCE| image:: https://img.shields.io/github/license/shaypal5/skift.svg
  :target: https://github.com/shaypal5/skift/blob/master/LICENSE

.. |Codecov| image:: https://codecov.io/github/shaypal5/skift/coverage.svg?branch=master
   :target: https://codecov.io/github/shaypal5/skift?branch=master
