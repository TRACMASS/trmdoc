Code Structure
==============

TRACMASS is written in Fortran 95. It consists of a number of
subroutines, which are preprocessed and compiled with two Makefiles. The
entire unix file tree is shown in Figure [fig:
unix_file_tree]. The flow chart of TRACMASS is shown
in Figure [fig:tracmass_flowchart] and the flowchart of the
subroutine loop in Figure [fig:tracmass_loop_flowchart].


The "first" Makefile
--------------------

There are two makefiles. This first one is under tracsvn/Makefile
compiles TRACMASS with the chosen Fortran compiler and libraries
(netcdf). It needs to be adapted to work with your computer platform.
There are a few examples that work with MacOSX and different linux
platforms. You will also need to set PROJECT to the the chosen
"project", which is the GCM data sets you want to run TRACMASS with.
There provided Makefile is set up to work with gfortran and g95 fortran
and is named Makefile_template. It needs to be adapted and
renamed Makefile

Here are a few of the many possible projects that can be used and set by
the very first line in this Makfile.

======   ==================================================
orc      NEMO with one of the ORCA grids.
ifs      The AGCM (IFS) from ECMWF, which is part of EC-Earth.
tes      Test basin with analytical time dependent velocity
         fields.
baltix   NEMO version of the Baltic and North sea.
======   ==================================================


Namelist files
--------------

These are the input files and subroutines that need to be adapted for
each run and changed for each new project.

<project>_grid.in
~~~~~~~~~~~~~~~~~

This input file defines the dimensions of the grid:

:IMT:       number of zonal grid points
:JMT:	    number of meridional grid points
:KM: 	    number of vertical grid points
:NTRACMAX:  maximum number of trajectories. TRACMASS will stop if 
            this number is exceeded but will consume a lot of central 
	    memory if set to a high number. Should be able to be set 
	    to a few millions.
:ngcm: 	    hours between GCM datasets
:iter:	    number of iterations between the two gcm data sets. 
            This sets the number of calculations between the two GCM 
	    data sets explained in section [sec:timesteping], where
	    dtmin=the time between two GCM data sets divided by iter. 
	    The trajectory solution becomes more accurate if set to a 
	    high number. 10 is however generally enough.
:intmax:    maximum number of GCM fields

<project>_run.in
~~~~~~~~~~~~~~~~

This input file sets many values of the actual run.


Project-specific modules
------------------------

setupgrid.f95
~~~~~~~~~~~~~

This file constains a subroutine for defining the grid of the GCM. 
It is invoked once before before the loop starts. The following
variables have to be populated in this file:

:dxdy:   Horizontal areas of all grid cells.
:dz:     Height of k-cells in 1 dim (z-grid). 
:dzt:    Height of k-cells i 3 dim (sigma grid).
:kmt:    HNumber of k-cells from surface to seafloor.

The following variables might be needed to calculate dxdy, uflux, and vflux:


:dzu:    Height of each u-gridcell.
:dzv:    Height of each v-gridcell.
:dxu:    Width  of each u-gridcell.
:dyu:	 Width  of each v-gridcell.


readfields.f95
~~~~~~~~~~~~~~

This subroutine reads the stored velocity, temperature and the
salinity/humidity fields from the GCM. It is this subroutine that needs
to be adapted when a new project is introduced.


The Makefile for the project pre-processing options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This makefile is under tracsvn/projects/<project>/Makefile and sets the
chosen pre-processing options.

The time pre-processing options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*-Dstat* Stationary velocity fields (does not work at the moment)
*-Dtimestep* Time changing velocity fields but using stationary solution
to the differential equations with time step changing velocities
*-Dtimeanalyt* Time changing velocity fields with time analytical
solutions to the differential equations based on . This scheme is not as
robust as -Dtimestep.

Output pre-processing options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-Dstreamxy     Calculate the Lagrangian barotropic stream function.
-Dstreamv      Calculate the Lagrangian vertical stream function as a
               function of model level
-Dstreamr      Calculate the Lagrangian vertical stream temperature,
               salinity/humidity, density/pressure.

The the latter two options have to be used with

-Dtempsalt     Calculate temperature, salinity and density for 
               the ocean and temperature, humidity and pressure for the 
               atmosphere
-Ddensity      Calculate only the density along the trajectory.
-Dtracer       Store trajectory particle positions as a simulated tracer


Non-passive advection pre-processing options
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These options will make the code non-Lagrangian, i.e. the trajectories
will not be passively advected by the velocity field as neutrally buoyant particles.

-Dsediment     Sediment code developed for RCO
-Dtwodim       Two-dimensional trajectories, which do not change depth. The
               vertical velocity is simply set to zero.
-Dturb         Add parameterised sub-grid turbulent velocities u’ and v’
-Ddiffusion    Add a diffusive random position to the trajectory

Other pre-processing options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-Drerun        This option is used in order to stores the Lagrangian stream
               functions as a function of the end positions. 
               The code is run a first time to find the end positions of 
               the trajectories at chosen sections. The code is then rerun  
               with -Drerun so that the Lagrangian stream
               functions will be divided into the different paths, 
               which has been calculated in the previous trajectory run.


General files 
--------------

The following subroutines are always used by TRACMASS and consists of
the pure Lagrangian trajectory calculated.

.. figure:: fig/unix_file_tree.*
   :align: center
   :figwidth: 80%
   :width: 75%

.. figure:: fig/tracmass_flowchart.*
   :align: center
   :figwidth: 80%
   :width: 50%
   

main
~~~~

This is the program main where the code starts and ends and from which
the subroutine loop is called.

modules
~~~~~~~

The modules located in modules.f95 is where the dimensions are defined
for the whole code.

Subroutine init_par
~~~~~~~~~~~~~~~~~~~~

This subroutine reads in the data from
<project>_grid.in and
<project>_run.in.

Subroutine init_seed
~~~~~~~~~~~~~~~~~~~~~

This subroutine reads the data for the initial positions of the
trajectories. That is where they are "seeded".

Subroutine coordinat
~~~~~~~~~~~~~~~~~~~~

This subroutine defines the horizontal and vertical grids as well as the
pressure, density, temperature, salinity and humidity coordinates by
specifying their minimum and maximum values. These need to be changed in
order to fit the chosen model domain and study.

Subroutine loop
~~~~~~~~~~~~~~~

This is the central subroutine of TRACMASS. The flow chart of this
subroutine is shown in Figure
[fig:tracmass_loop_flowchart]. The subroutine consists
of several loops over both time and the trajectories.

.. figure:: fig/tracmass_loop_flowchart.*
   :align: center
   :figwidth: 75%
   :width: 75%

Subroutine vertvel
~~~~~~~~~~~~~~~~~~

This subroutine computes the vertical velocities in the boxes of the
water/air column where the trajectory particle is located by integrating
Eq. [eq:cont].

Subroutine cross
~~~~~~~~~~~~~~~~

This subroutine to computes the time (sp,sn) from Eq. [eq:s1], when the
trajectory crosses the face of box (ia,ja,ka). Two crossings are
considered for each direction: east and west for longitudinal
directions, north and southward for latitudinal directions and up and
down for vertical directions. The subroutine is called for each of the
three directions from subroutine loop in order to determine which will
be the shortest of the 6 times, which will determine which of the 6 grid
walls the trajectory crosses.

Subroutine pos
~~~~~~~~~~~~~~

The shortest of the times obtained by the subroutine cross is here used
to calculate the new position of the trajectory with Eq. [eq:rs].

The subgrid turbulence subroutines
----------------------------------

These subroutines are attempts to include parameterisations of the the
non-resolved sub-grid scales. There are two two different options in the
TRACMASS code. These subroutines are activated by using -Dturb or
-Ddiffusion in the Makefile. Don’t use both of them!

It is important to realise that these scales are not included in the
original velocity fields from the GCM. There is no general consensus
wether it is physical to include these sub-grid parameterisations since
we are adding scales that are not present in the GCM.

Subroutine turbuflux
~~~~~~~~~~~~~~~~~~~~

This subroutine computes a random horizontal turbulent velocity
:math:`u', v'`, which are used in the subroutines vertvel, pos and cross
in order to incorporate a sub-grid parameterisation of the non resolved
scales. It is activated by the preprocessing option -Dturb in the
Makile. Don’t use together with -Ddiffusion.

Subroutine diffusion
~~~~~~~~~~~~~~~~~~~~

This subroutine adds a random position to the new trajectory position in
order to incorporate a sub-grid parameterisation of the non resolved
scales. It was originally written by Richard Levine and David Webb. It
is activated by the preprocessing option -Ddiffusion in the Makile.
Don’t use together with -Dturb.

The sedimentation subroutines
-----------------------------

The sedimentation subroutines have been written and used by Hanna Corell
in her PhD thesis. They are activated by the -Dsediment in the Makefile.
See the theoretical section for more details about the sedimentation
model.

Subroutine sedvel
~~~~~~~~~~~~~~~~~

This subroutine calculates the settling velocity of the modelled
particle size.

Subroutine resusp
~~~~~~~~~~~~~~~~~

This subroutine checks if water velocities are large enough for
resuspention of sedimentated trajectories. If large enough it puts the
particle on a given level in the water column. generally chosen to be
the middle of the bottom box.

Subroutine orbitalv
~~~~~~~~~~~~~~~~~~~

This subroutine to sedimentation model for the Baltic sea. The routine
calculates the a meta parameter "orb" that is multiplied with an
approximation of the wave amplitude in loop, and added to the velocity
in the bottom box to simmulate the water movements due to short surface
waves.

Diagnostic subroutines
----------------------

The diagnostic subroutines are only to generate outputs for the
trajectories and Lagrangian stream functions. They do not affect the
trajectory solutions themselves.

Subroutine sw_stat
~~~~~~~~~~~~~~~~~~~

This subroutine computes the density of the sea water from the equation
of state.

function sw_dens0
~~~~~~~~~~~~~~~~~~

This function computes the density of the sea water from the equation of
state.

Subroutine sw_seck
~~~~~~~~~~~~~~~~~~~

This subroutine computes the density of the sea water from the equation
of state but with better flexibility for the pressure.

Subroutine interp
~~~~~~~~~~~~~~~~~

Interpolates the temperature, salinity/humidity, density/pressure from
the grid box walls to the trajectory particle position for the storage
of the trajectory characteristics.

Subroutine sw_pres
~~~~~~~~~~~~~~~~~~~

Calculates pressure in dbars from depth in meters.

Subroutine writepsi
~~~~~~~~~~~~~~~~~~~

This subroutine writes the Lagrangian stream functions, which are
computed when any of the preprocessing options -Dstreamxy, -Dstreamv or
-Dstreamr are activated.

Subroutine writetracer
~~~~~~~~~~~~~~~~~~~~~~

This subroutine writes the particle tracer field, which is activated
with -Dtracer.



Added vertical velocity
~~~~~~~~~~~~~~~~~~~~~~~

The concept underlying the sedimentation model is that suspended
particulate matter is bound to follow the movements of the water. If the
sinking motion of the particles in quiescent water is known, and the
paths of the water can be calculated, then the particle trajectories
will be a combination of these displacements.

There are two general modes of settling; viscous settling for particles
smaller than 0.2 mm and inertial settling for particles larger than
approximately 2 mm. Between these sizes there is a transition zone. In
this study the particles modelled are silt and clay, *i.e* particles
with a diameter much smaller than 0.2 mm, and the settling is thus
viscous. In practice, the settling velocity of a particle has a basic
relation to its size and shape. Since it is not possible to account for
all different shapes a particle can have, the concept of equivalent size
is used, *viz.* the size of a quartz sphere having the same settling
velocity as a less spherical natural grain.

To the GCM-derived vertical velocity of the water, a settling velocity
:math:`w_{s}` for the particle is added. This is calculated on the basis
of Stokes law, using the particle density :math:`\rho_{s}`, diameter
:math:`d`, as well as water density and viscosity, :math:`\rho_{w}` and
:math:`\mu`:

.. math::
   w_{s} =\frac{\rho_{s}-\rho_{w}}{18\mu}gd^{2}.
   :label: stokes

This formula is valid when the Reynolds number is smaller than 1,
corresponding to an equivalent diameter of 0.2 mm and smaller. Since the
viscosity :math:`{\mu}` is included in the equation, the viscous
settling is temperature-dependent.

The particles are taken to enter the model domain along the shoreline,
and are from there transported by the motion of the water, at the same
time being affected by sedimentation and resuspension processes. The
horizontal motion is prescribed solely by the GCM field, while the
vertical displacements are derived from the GCM field together with the
settling velocity in Eq. [eq:stokes]. If a particle reaches the lower
boundary of the deepest grid box in the water column, *i.e.* the sea
floor, it will settle. Once settled it will stay at the settling
position and can only leave it by resuspension. If no resuspension
occurs, the particle will remain at its position until the simulation
ends.
