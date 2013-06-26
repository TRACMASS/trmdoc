Integrated Diagnostics
======================


Lagrangian stream functions
---------------------------

A Lagrangian stream function can be calculated by summing over
trajectories representing the desired path. A particular water mass can
be isolated by following a set of trajectories between specific initial
and final sections. Each trajectory, indexed by :math:`n`, is associated
with a volume transport :math:`T_n` given by the velocity, initial area,
and number of trajectories released. During transit from the initial to
the final section the volume transport remains unchanged; the
transport/velocity field is non-divergent, permit- ting stream-function
representations. The volume transport linked to each trajectory is
inversely proportional to the number of trajectories released, viz. the
Lagrangian resolution (which should be sufficiently high to ensure that
the stream function does not change when the number of trajectories is
further increased). A non-divergent 3-D volume-transport field is
obtained by recording every instance of a trajectory passing a grid-box
wall. Every trajectory entering a grid-box also exits, and hence this
field exactly satisfies

.. math::
   T_{i,j,k,n}^{x} - T_{i-1,j,k,n}^{x} + T_{i,j,k,n}^{y} - 
   T_{i,j-1,k,n}^{y} + T_{i,j,k,n}^{z} - T_{i,j,k-1,n}^{z}=0
   :label: trans


where :math:`T_{i,j,k,n}^{x}`, :math:`T_{i,j,k,n}^{y}` and
:math:`T_{i,j,k,n}^{z},` are the trajectory-derived volume transports in
the zonal (i), meridional (j), and vertical (k) directions,
respectively.

.. _grid:
.. figure:: fig/grid.*
   :figwidth: 80%
   :width: 50%
   :align: center

   The Lagrangian stream function discretisation on a grid box seen 
   from above, with the grid lengths :math:`\Delta x` and :math:`\Delta y`. 
   An example of one trajectory passing through so that the transports
   through the walls are :math:`T_{i,j,k,n}^{y}=T_{i,j-1,k,n}^{x}=T_{n}`
   and :math:`T_{i-1,j,k,n}^{y}=T_{i,j,k,n}^{x}=0`. }}


.. _streamfunc_schematic:
.. figure:: fig/streamfunc_schematic.*
   :figwidth: 50%
   :width: 80%
   :align: center

   Schematic illustration by Bruno Blanke of how the transport of each
   trajectory is counted on each grid cell wall. Bottom: the resulting
   Lagrangian stream function. Figure made by Bruno Blanke for the EU 
   project TRACMASS.


.. _barstream:

Lagrangian barotropic stream function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By integrating vertically over the transports and over the trajectories
one obtains the Lagrangian barotropic stream function
:math:`\Psi_{i,j}^{LB}`

.. math::
   \Psi_{i,j}^{LB} - \Psi_{i-1,j}^{LB} = {\displaystyle
   \sum_{k}}{\displaystyle \sum_{n}}T_{i,j,k,n}^{y} \quad
   or\quad\Psi_{i,j}^{LB}-\Psi_{i,j-1}^{LB}=-{\displaystyle 
   \sum_{k}}{\displaystyle \sum_{n}}T_{i,j,k,n}^{x}
   :label: psixy


Lagrangian overturning stream functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By instead integrating zonally one obtains the Lagrangian meridional
overturning stream function :math:`\Psi_{j,k}^{LM}`

.. math::
   \Psi_{j,k}^{LM} - \Psi_{j,k-1}^{LM} = -{\displaystyle 
   \sum_{i}}{\displaystyle \sum_{n}}T_{i,j,k,n}^{y}\quad 
   or \quad\Psi_{j,k}^{LM}-\Psi_{j-1,k}^{LM}={\displaystyle  
   \sum_{i}}{\displaystyle \sum_{n}}T_{i,j,k,n}^{z}
   :label: psiyz

Finally by integrating meridionally one obtains the Lagrangian zonal
overturning stream function :math:`\Psi_{i,k}^{LZ}`

.. math::
   \Psi_{i,k}^{LZ}-\Psi_{i,k-1}^{LZ} = -{\displaystyle 
   \sum_{i}}{\displaystyle \sum_{n}}T_{i,j,k,n}^{x}\quad 
   or \quad\Psi_{i,k}^{LZ}-\Psi_{i-1,k}^{LZ}={\displaystyle
   \sum_{i}}{\displaystyle \sum_{n}}T_{i,i,k,n}^{z}
   :label: psixz


An example of a zonal Lagrangian stream function is shown in :num:`Fig. #grid_traj`.

The vertical index :math:`k` can be the level depth, temperature,
salinity or density for ocean GCMs. For atmospheric GCMs it can be the
model level, temperature, specific humidity, geopotential height or
pressure.

.. _grid_traj:
.. figure:: fig/psiyz9102lagr.*
   :figwidth: 80%
   :width: 80%
   :align: center

   Exemple of Lagrangian stream functions from  \citet{Doos-2008}.
   Meridional overturning stream function in depth coordinates in the 
   Southern Ocean. a) The Eulerian stream function. b) The Total Lagrangian 
   stream function, c) The difference between the total Lagrangian and the 
   Eulerian stream function. (d-h) Partial Lagrangian stream functions for 
   Figure 3d the sum of the paths between the north (30 $^\circ$ S) and 
   Drake Passage. e) Drake to North, d) North to Drake, g) Drake to Drake 
   around Antarctica in the ACC, and h) North to North without passing 
   through the Drake Passage. The Deacon Cell has its center in orange. 
   Positive streamlines correspond to anticlockwise circulation and negative 
   streamlines to clockwise circulation. Units in Sv.

Simulated tracer
----------------

A simulated tracer concentration can also be calculated by activating
the key -Dtracer. It calculates the trajectory particle concentration in
each grid at a given time. This is not a true tracer but is directly
comparable with a tracer. An example of this is shown in :num:`Fig. #tracer` from .

.. _tracer:
.. figure:: fig/tracer.*
   :figwidth: 80%
   :width: 80%
   :align: center

   Exemple of Lagrangian trajectory particle concentration a) without and b)
   with sub-grid parameterised turbulence. Real model tracers in c and d.
   Units in \% of maximum concentration. 
