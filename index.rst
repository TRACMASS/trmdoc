
Preface
=======

Welcome to TRACMASS!
--------------------

The TRACMASS documentation starts with the basic theory of the
analytical trajectory solutions and the Lagrangian stream functions.
Followed by a description of how the code is constructed with it
corresponding subroutines. There is at the end a short manual of how to
set up the TRACMASS code. This documentation is far from complete but is
continuously upgraded in order to help the users all over the world.

The code was originally written in Fortran 77 for the FRAM ocean model
at the Institute of Oceanographic Sciences, Deacon Laboratory (IOSDL) in
Wormley, UK in the early 90’s. The name TRACMASS comes from the EU
project with the same name, where it served together with the similar
trajectory code Ariane by The present code is written in Fortran 95 and
should be possible to apply to most GCMs based on finite differences.
The TRACMASS code is continuously upgraded and adapted. The code can be
downloaded from
`http://doos.misu.su.se/tracmass/ <http://doos.misu.su.se/tracmass/>`_.

There is, however, no user support for the code but all constructive
comments or advice from the users are welcome. Furthermore the user must
be familiar, in order to be able to use the TRACMASS code, with 1) the
equation of motion for the ocean-atmosphere circulation, 2) the finite
differences of these equations, 3) The TRACMASS theory, 4) Unix and 5)
Fortran. There is, however, no user support for the code but all
constructive comments or advice from the users are welcome. This manual
has been developed with the support from the BONUS project BalticWay.
*Stockholm University*
*Kristofer Döös, Bror Jönsson, Joakim Kjellsson, and Hanna Corell*

Contents:

.. toctree::
   :maxdepth: 2

   theory
   turbulence
   sedimentation
   diagnostics
   code
   run_trm
   references

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
