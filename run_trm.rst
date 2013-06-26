Set-up and run the code
=======================

Download tracmass.zip from http://doos.misu.su.se/tracmass/ and put
where you want it.

How to run TRACMASS
-------------------

Make necessary changes in the Makefile by setting the appropriate
preprocessing options described in previous chapter.
Set the apropriate parameters in <project>\_grid.in and
<project>\_run.in. Compile by 

.. code-block:: bash

    cd ../tracmass/tracsvn
    make clean
    make 
    (alt) 
    make -f Makefile.something

Run TRACMASS by executing

.. code-block:: bash

    ./runtracmass



How to read the output files
----------------------------

The output data from the TRACMASS runs are stored in the directory
<outDataDir>, which is set in <project>\_run.in. There are basically two
types od output data: trejctories and integrated quantitites such as
stream functions.

The trajectories
~~~~~~~~~~~~~~~~

The trajectories are stored in <outDataDir>/<outDataFile>\_run.asc,
which has to be specified in <project>\_run.in
The trajectories are stored in colums of
**ntrac,niter,x1,y1,z1,tt,t0,subvol,temp,salt,dens**
where

:ntrac:      Trajectory number
:niter:      TRACMASS code iteration (only important for TRACMASS modellers)
:x1:         Zoonal position of the trajectory particle
:y1:         Meridional position of the trajectory particle
:z1:         Vertical position of the trajectory particle
:tt:         Time of the trajectory particle (in days)
:t0:         Initial time of the trajectory particle
:subvol:     The "volume" transport in m3/s of the trajectory
:temp:       Temperature of the trajectory particle
:salt:       Salinity/specific humidity of the trajectory particle
:dens:       Density of the trajectory particle


The integrated quantities
~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the trajectories there are integrated quantities stored
in the code. These are activated by the preprocessing options in the
Makefile. The Lagrangian stream funtions are descibed by Equation
[eq:psixy] and [eq:psiyz]. They should be read in the same way as they
are written in writepsi.f95. The same goes for the simulated tracer
field written in writetracer.f95.

-Dtracer     Turn on :ref:`barstream`.
-Dtra        You can set this variable to select a paper size.





Implementing a new GCM grid 
----------------------------

You will need to penetrate the tracmass code in case you want to implement a new setup from a GCM with a grid that do not exist in the present TRACMASS. This requires good knowlidge in the numerical discretisation of your gcm and of Fortran programing. You do not get any support from the TRACMASS team for this. You will here just get some basic guidence how to do this.

1. Make a new <project>, which we call "mymodel" here. 
Copy the directory from an existing project that has a similar grid (A,B or C-grid) or has a similar format (netcdf, grib, etc.). So that

.. code-block:: bash
  
    cd tracsvn/projects
    cp -R orc mymodel
    or
    cp -R rco mymodel
    or
    cp -R ifs mymodel

You will now have to modify and adapt the files under tracsvn/projects/mymodel, which are readfield.f95, mymodel\_grid.in and mymodel\_run.in.

2. Modify tracsvn/coord.f95 if necssary unless you specify your new grid in tracsvn/mymodel/readfield.f95

3. You will now have to add your project into the TRACMASS code itself with the many preprocessing options depending on the project you are running.
Search for a project with a similar vertical grid as yours (depth, sigma, atmospheric/ifs):

.. code-block:: bash
   
    cd tracsvn
    grep rco *.f95
    or
    grep ifs *.f95

4) Adapt the Makefile and set PROJECT = mymodel
