TRACMASS Documentation.

The documentation is compiled by Sphinx (http://sphinx-doc.org), a python package developed to auto-generate documentation for software projects. We use a number of add-ons that needs to be installed as well. 

Installation:

(It is advised that you use virtual environments: http://virtualenvwrapper.readthedocs.org/en/latest/)

.. code-block:: bash

  # (Download or clone the repo)
  cd trmdoc
  pip --user -r requirements.txt
  # or with virtualenvwrapper
  mkvirtualenv trmdoc
  pip -r requirements.txt

The documentation is in the *source* folder. Read more about the syntax here: http://sphinx-doc.org/rest.html

Compile the web pages with::
  
  make html

The resulting html files are generated in the *build/html* folder.

