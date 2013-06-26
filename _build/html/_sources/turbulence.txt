Sub-grid turbulence
===================

The trajectory solutions in the previous sections only include the
implicit large scale diffusion due to along-trajectory changes of
temperature and salinity/humidity, and by the models parameterisation of
turbulent mixinging in the momentum equations. These trajectories do
not, however, explicitly represent sub-gridscale turbulence. There are
two ways to incorporate a sub-grid scale turbulence in TRACMASS.

There is, however, no general consesus wether it is physical to include
these sub-grid parameterisations since we are adding scales that are not
present in the GCM.

Adding a random :math:`u', v'`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This scheme, which was introduced by , adds a random horizontal
turbulent velocity :math:`u', v'` to the horizontal velocity
:math:`U, V` from the GCM and is illustrated by :num:`Fig. #turbu`. This
is added to each trajectory and each horizontal grid wall every time
step. This enabled us to use the TRACMASS code as it is but with a
velocity field that is somewhat shaken, resulting in stirred trajectory
particles. The new velocity from which the transport is calculated in
equation :eq:`U` is now :math:`u=U+u'`. The amplitude of the random
turbulent velocity is set to the same size as the circulation model
velocity :math:`U`, so that :math:`u'=RU`, where :math:`R` is a random
number uniformly distributed between :math:`-a` and :math:`a`. The best
results were obtained for :math:`a=1` in the study by .


The effect of this superimposed sub-grid turbulence is clearly visible
in :num:`Fig. #trajturb`, where a particle cluster is traced with and
without this sub-grid parameterisation. The turbulence smoothes the
trajectory positions and spreads them more evenly. The stirred particles
in Fig. [fig:trajturb-b] fill visibly regions where no particles were
present without sub-grid turbulence in Fig. [fig:trajturb-a].

This sub-grid parameterisation was tested in and will however need more
tests and evaluation in the future.


.. _turbu:
.. figure:: fig/turbu.*
   :align: center
   :width: 50%
   :figwidth: 80%

   Schematic illustration of the changed particle position by the sub-grid
   turbulence parameterisation due to the added random velocities 
   :math:`u', v'`.


Diffusion
~~~~~~~~~

This subroutine adds a random displacement to the trajectory position in
order to incorporate a sub-grid parameterisation of the non resolved
scales. The scheme was introduced in TRACMASS by .

The horizontal Laplacian diffusion equation, which is

.. math::
   \frac{\partial P}{\partial t} = A_H  \left( \frac{\partial^{2}P}{\partial x^{2}} + \frac{\partial^{2}P}{\partial y^{2}} \right)
   :label: laplace

is replaced by a random walk using a Gaussian distribution

.. math::
   P(x_d,y_d, \Delta t) =   \frac{1}{4 \pi A_H \Delta t} 
   \exp \left(   \frac{-x_d^2 -y_d^2}{4 A_H \Delta t} \right)
   :label: 2dgaus

Figure :num:`#diffusion` illustrates the added displacements to the
original position of the particle after each time-step of length
:math:`\Delta t`, which is:

.. math::
   x_d =  \sqrt{ - \, 4 \, A_H \, \Delta t \, \log(1-q_1) } \, 
   \cos{(2 \, \pi \, q_2)}
   :label: diff_disp_x

.. math::
   y_d =  \sqrt{ - \, 4 \, A_H \, \Delta t \, \log(1-q_1) } \, 
   \sin{(2 \, \pi \, q_2)}
   :label: diff_disp_y

.. math::
   z_d =  \sqrt{ - \, 4 \, A_V \, \Delta t \, \log(1-q_3) } \, 
   \cos{(2 \, \pi \, q_4)}
   :label: diff_disp_z

where :math:`A_H` and :math:`A_V` are the horizontal and vertical eddy
viscosity coefficients. They can be chosen to be the same as those used
in the momentum equation by the circulation model so the random motions
are on the same scale of the model subgrid-scale turbulence.
:math:`q_1`, :math:`q_2`, :math:`q_3` and :math:`q_4` are random numbers
between :math:`0` and :math:`1`. The added displacement in the
horizontal plane will hence lye on a distance from the original position
of

.. math::
   r_H =  \sqrt{ x_d^2 \, + y_d^2 } \, = 
   \, \sqrt{ 4 \, \Delta t \, A_H  \, \log(1-q_1)}
   :label: diff_disp_r_H

The added horizontal displacement will therefore lye on a random
position on a Gaussian distribution where one standard deviation is

.. math::
   R_H =  \, \sqrt{ 4 \, \Delta t \, A_H  }
   :label: diff_disp_R_H


.. _diffusion:
.. figure:: fig/diffusion.*
   :align: center
   :width: 50%
   :figwidth: 80%

.. figure:: fig/diffusion_circle.*
   :align: center
   :width: 40%
   :figwidth: 80%

   The added displacement due to diffusion. Left panel shows in 
   blue the original trajectory and in light blue the changed one 
   due to the added displacement. Right panel zooms in on the added 
   random displacement.


Anisotropic horizontal diffusion
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The horizontal diffusion in the previous section was isotropic and the
added random displacement lied on a circular disk. In this section we
introduce an anistropic diffusion that is higher along the isobaths and
weaker perpendicular to the isobaths. The random displacement will
therefore be based on an elliptic disk instead of a circular disk as
illustrated by :num:`Fig. #diffusion_ellipse`. This diffusion will
hence take into account the fact that observations of trajectories of
floats and drifters tend to follow isolines of constant planetary
vorticity (:math:`f/h`).

The implementation of this sub-grid parameterisation is the following.
The slope, i.e. the maximum depth gradient is first calculated

.. math::
   G = \sqrt{  \left( \frac{\Delta h}{ \Delta x}\right)^2 +
   \left( \frac{\Delta h}{ \Delta y}\right)^2  }
   :label: gradient

The circular disk from previous section is then stretched into an
elliptic disk by

.. math::
   x_d' = (c_1 G+1) x_d
   :label: eliptransx

.. math::
   y_d' = (c_1 G+1) y_d
   :label: eliptransy

where :math:`c_1` is a constant. The ellipse will be a circular disk
only if the bottom of the ocean is flat in the grid cells beneath the
trajectory particle.

The angle from the x-axis (eastward) and the isopleths is

.. math::
   \theta = \arcsin{  \left( \frac{  
   \frac{\Delta h}{ \Delta x} } { G } \right) }
   :label: theta

The ellipse is then inclined with the computed angle :math:`\theta` from
the x-axis (eastward) to be along the isopleth by the coordinate
transform:

.. math::
   x_d''= x_d' \cos(\theta)- y_d' \sin(\theta) \\
   :label: coordtrans_x

.. math::
   y_d''=-x_d' \sin(\theta)+ y_d' \cos(\theta)
   :label: coordtrans_y


.. _diffusion_ellipse:
.. figure:: fig/diffusion_ellipse.*
   :align: center
   :width: 50%
   :figwidth: 80%

   Schematic illustration of the anisotropic horizontal diffusion on 
   an elliptic disk


The Lagrangian dispersion has been shown to increase and be more
realistic despite that the injected parameterised diffusion remains the
same (in preparation, Döös, 2010).

.. _noturbu:
.. figure:: fig/traj_0615noturb.*
   :align: center
   :width: 50%
   :figwidth: 80%

.. _turbu:
.. figure:: fig/traj_0615turb.*
   :align: center
   :width: 50%
   :figwidth: 80%

   A cluster of Lagrangian trajectories released and followed until
   they exit the model domain. The trajectories’ positions are plotted 
   as colour dots every hour. The black line is the mean position of
   the trajectory cluster as it evolves in time. a) without and b)
   with sub-grid turbulence parameterisation. From ?.

