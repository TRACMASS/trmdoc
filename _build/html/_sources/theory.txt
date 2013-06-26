
Trajectory Theory
=================

The TRACMASS model calculates Lagrangian trajectories from Eulerian velocity fields, which have been simulated by ocean or atmosphere general circulation models (GCM). The trajectories are calculated off-line, *i.e.* after the GCM has been integrated and the velocity fields have been stored. This makes it possible to calculate many more trajectories than would be possible on-line (*i.e.* simultaneously with the GCM run). TRACMASS has been applied to many different general circulation models, both for the ocean and the atmosphere. The original feature of the method is that it solves the trajectory path through each grid cell with an analytical solution of a differential equation which depends on the velocities on the grid-box walls. The scheme was originally developed by and for stationary velocity fields and hereafter further developed by for time-dependent fields by solving a linear interpolation of the velocity field both in time and in space over each grid box, this in contrast to the Runge-Kutta methods where the trajectories are iterated forward in time with short time steps. The TRACMASS code has been further developed over the years and used in many studies in the atmosphere and ocean for global large scales studies and regional ones for the and Mediterranean and Baltic Seas .

.. figure:: fig/Bgrid.*
   :figwidth: 80%
   :width: 80%
   :align: center
 
   B-grid

.. _bcgrid:
.. figure:: fig/Cgrid.*
   :figwidth: 80%
   :width: 80%
   :align: center
   
   C-grid

Left: B-grid, Right C-grid


.. _grid_traj:
.. figure:: fig/grid_traj2.*
   :figwidth: 80%
   :width: 80%
   :align: center

   Illustration of a trajectory [x(t), y(t)] through one grid box. 
   The model velocities are defined at the walls of the grid box.


.. _grid_xz_traj:
.. figure:: fig/c-grid-traj.*
   :figwidth: 80%
   :width: 80%
   :align: center

   Vertical trajectory discretisation in model grids.}}

.. _grid_sigma:
.. figure:: fig/sigma-grid.*
   :figwidth: 80%
   :width: 80%
   :align: center

Vertical trajectory discretisation in model grids.

Trajectory solution for rectangular grids in ocean models
---------------------------------------------------------

This section is here only for pedagogical reasons, since it is only valid for rectangular grids. The TRACMASS code is written on a more general way in order to enable non-rectangular grids and is presented in the next section.

The velocity on a C-grid box (:num:`Fig. #bcgrid`) is given by :math:`u_{i,j,k}`, where :math:`i,j,k` denote the discretised longitude, latitude and model level, respectively; :math:`u` is the zonal velocity. Meridional and vertical velocities are defined analogously. It is possible to define the velocity inside a grid box by interpolating linearly between the discretised velocity values of the opposite walls. For the zonal direction, for example one obtains

.. math::
   u(x) = u_{i-1,j,k} + \frac{(x-x_{i-1})}{ \Delta x} (u_{i,j,k}-u_{i-1,j,k})
   :label: ux

Local zonal velocity and position are related by :math:`u=dx/dt`. The approximation in equation :eq:`ux` can now be written in terms of the following differential equation:

.. math::
   \frac{dx}{dt} + \beta \, x + \delta = 0
   :label: dxdt


with :math:`\beta \equiv \left(u_{i-1,j,k} - u_{i,j,k} \right)/ \Delta x and :math:`\delta \equiv - u_{i-1,j,k} - \beta \, x_{i-1}`. Using the initial condition :math:`x(t_0)=x_0` , the zonal displacement of the trajectory inside the considered grid box can be solved analytically and is given by

.. math::
   x(t) =  \left(x_0 + \frac{\delta}{\beta}   \right) e^{- \beta (t-t_0)} - \frac{\delta}{\beta}
   :label: xt

The time :math:`t_1` when the trajectory reaches a zonal wall can be determined explicitly:

.. math::
   t_1 =  t_0 - \frac{1}{\beta}  log \left[ \frac{x_1+\delta / \beta}{x_0+\delta / \beta} \right]
   :label: t1

where :math:`x_1=x(t_1)` is given by either :math:`x_{i-1}` or :math:`x_i`. For a trajectory reaching the wall :math:`x=x_i`, for instance, the velocity :math:`u_i` must necessarily be positive, so in order for equation :eq:`t1` to have a solution, the velocity :math:`u_{i-1}` must then be positive also. If this is not the case, then the trajectory either reaches the other wall at :math:`x_{i-1}` or the signs of the transports are such that there is a zero zonal transport somewhere inside the grid box that is reached exponentially slow. For the meridional and vertical directions, similar calculations of :math:`t_1` are performed determining the meridional and vertical displacements of the trajectory, respectively, inside the considered grid box. The smallest transit time :math:`t_1-t_0` and the corresponding :math:`x_1` denote at which wall of the grid box the trajectory will exit and move into the adjacent one. The exact displacements in the other two directions are then computed using the smallest :math:`t_1` in the corresponding equation :eq:`xt`. The resulting trajectory through the grid box is illustrated by :num:`Fig. #grid_xz_traj` and  :num:`Fig. #grid_sigma`.


Scheme for volume transports and non-rectangular grids in ocean models
----------------------------------------------------------------------

The disadvantage with the scheme presented in previous section is that it requires rectangular grid cells and GCMs generally use some sort of spherical grids as in the case of :num:`Fig. #conbelt`, where two spherical grids have been used for the world ocean.

.. _conbelt:
.. figure:: fig/trajatm365arctic.*
   :figwidth: 80%
   :width: 80%
   :align: center

   Atmospheric trajectories released once a day during one year from a 
   point in the Arctic and followed backward in time until reaching the
   lowest layer above the ocean indicated by a black dot. The velocities
   are from the ERA Interim reanalysis from ECMWF. 

.. figure:: fig/conbelt.*
   :figwidth: 80%
   :width: 80%
   :align: center

   The Ocean Conveyor Belt with velocites simulated by the OCCAM ocean
   general circulation model . The red trajectories are part of the 
   shallow warmer part of the Conveyor Belt with transports toward the 
   North Atlantic. The blue trajectories represent the flow of the dense 
   and cold North Atlantic Deep Water from the North Atlantic into the 
   Indo-Pacific.  

Exemple of TRACMASS trajectories in a) the atmosphere and b) in the ocean.}}
\end{figure*}


The longitudinal size (:math:`\Delta x_{i,j}`) on the northern and southern walls will typically be different on a spherical grid and on a curvilinear grid both :math:`\Delta x_{i,j}` and :math:`\Delta y_{i,j}` (latitudinal size) can be a function of its horizontal position.

The Equations :eq:`ux` - :eq:`t1` are not valid for a non-rectangular grid. This can however be solved by replacing the the velocities by volume transports. The transport through the eastern wall of the :math:`{i,j,k}` grid box is given by

.. math::
   U_{i,j,k} = u_{i,j,k} \Delta y_{i,j}  \Delta z_k
   :label: U

The distance is non-dimensionalised by using :math:`r=x/\Delta x`, and the linear interpolation of the velocity (Eq. :eq:`ux`) is replaced by

.. math::
   U(r) = U_{i-1,j,k} + (r-r_{i-1})(U_{i,j,k}-U_{i-1,j,k})
   :label: Ur

The local transport and position are now related by :math:`U=dr/ds`, where the scaled time variable :math:`s \equiv t/(\Delta x_{i,j} \Delta y_{i,j} \Delta z_k)`, where the denominator is the volume of the particular grid box. The differential equation :eq:`dxdt` is replaced by

.. math::
   \frac{dr}{ds} + \beta \, r + \delta = 0
   :label: drds

with :math:`\beta \equiv U_{i-1,j,k} - U_{i,j,k}` and :math:`\delta \equiv - U_{i-1,j,k} - \beta \, r_{i-1}`. Using the initial condition :math:`r(s_0)=r_0` , the zonal displacement of the trajectory is now given by

.. math::
   r(s) =  \left(r_0 + \frac{\delta}{\beta}   \right) e^{- \beta (s-s_0)} - \frac{\delta}{\beta}
   :label: rs

The scaled time :math:`s_1` becomes

.. math::
   s_1 =  s_0 - \frac{1}{\beta}  log \left[ \frac{r_1+\delta / \beta}{r_0+\delta / \beta} \right]
   :label: s1

where :math:`r_1=r(s_1)` is given by either :math:`r_{i-1}` or :math:`r_i`. With the use of equation :eq:`U`, the logarithmic factor can be expressed as :math:`log[ U(r_1)/U(r_0)]`. For a trajectory reaching the wall :math:`r=r_i`, for instance, the transport :math:`U(r_1)` must necessarily be positive, so in order for equation :eq:`s1` to have a solution, the transport :math:`U(r_0)` must then be positive also. If this is not the case, then the trajectory either reaches the other wall at :math:`r_{i-1}` or the signs of the transports are such that there is a zero zonal transport somewhere inside the grid box that is reached exponentially slow.

The meridional solution is calculated similarly as the zonal one but using the meridional transport defined as

.. math::
   V_{i,j,k} = v_{i,j,k}\Delta x \Delta z_k\\
   :label: V

The vertical solution is calculated similarly as the zonal one but using the meridional transport defined as

.. math::
   W_{i,j,k} = w_{i,j,k}\Delta x \Delta y
   :label: W

The scheme is mass conserving since the vertical transport is directly calculated from the continuity equation in the same way as in the ocean GCM, which is due to the incompressibility in the ocean

.. math::
   \frac{\partial u}{\partial x}+\frac{\partial v}{\partial y}+\frac{\partial w}{\partial z}=0
   :label: contz


that is discretised with finite differences on a C-grid into

.. math::
   \frac{u_{i,j,k} - u_{i-1,j,k}}{\Delta x_{i,j}} + \frac{v_{i,j,k} - v_{i,j-1,k}}{\Delta y_{i,j}} + \frac{w_{i,j,k} - w_{i,j,k-1}}{\Delta z_k} = 0
   :label: contzdisc

Equation :eq:`contzdisc` can be explained by that the sum of all the volume fluxes in or out of the grid box is zero. We rewrite in flux form so that the continuity equation becomes

.. math::
   W_{i,j,k}=W_{i,j,k-1} -(U_{i,j,k}-U_{i-1,j,k}+V_{i,j,k}-V_{i,j-1,k})
   :label: cont

and is integrated from the bottom and upwards with the boundary condition :math:`W=0`. Since the trajectory solutions are exact and that the continuity equation is respected the TRACMASS trajectories will therefore never hit the coast or the sea floor.

The calculations of :math:`s_1` are performed determining the zonal, meridional and vertical displacements of the trajectory, respectively, inside the considered grid box. The smallest transit time :math:`s_1-s_0` and the corresponding :math:`r_1` denote at which wall of the grid box the trajectory will exit and move into the adjacent one. The exact displacements in the other two directions are then computed using the smallest :math:`s_1` in the corresponding equation :eq:`rs`.

The trajectories can be both calculated forward and backward in time and hence it is possible to trace origins of water or air masses. The differential equation :eq:`dxdt` is strictly only valid for stationary velocity fields. developed an extension of the TRACMASS code for time dependent velocities. It is however possible to use the present stationary code with negligible loss of accuracy by simply changing the velocity and pressure fields at regular time intervals, which are the same as the output data from the used GCM is stored.

.. _time_analyt:
.. figure:: fig/time_analyt.*
   :figwidth: 80%
   :width: 80%
   :align: center 

   Example of trajectory $r(s)$ exhibiting two extrema (zero-transport
   points) inside the relevant rs "box". Regions with positive and 
   negative transports are shown. Extrema for trajectories with differing
   initial conditions must lie on the hyperbola (dotted curves). }}


Atmospheric scheme with hybrid vertical coordinates
---------------------------------------------------

The atmospheric version of TRACMASS uses however the conservation of mass instead of volume. Most Atmospheric GCMs today use terrain-following vertical coordinates. Following the atmosphere is divided into :math:`NLEV` layers, which are defined by the pressures at the interfaces between them and these pressures are given by :math:`p_{k+1/2} = A_{k+1/2} + B_{k+1/2} \, p_{S}` for :math:`k=0,1,..,NLEV`, with :math:`k=0` at the top of the atmosphere and :math:`k=NLEV` at the Earth’s surface. The :math:`A_{k+1/2}` and :math:`B_{k+1/2}` are constants, whose values effectively define the vertical coordinate and :math:`p_{S}` is the surface pressure. The dependent variables, which are the zonal wind (:math:`u`), the meridional wind (:math:`v`), the temperature (:math:`T`) and the specific humidity (:math:`q`) are defined in the middle of the layers, where the pressure is defined by :math:`p_{k} = \frac{1}{2} (p_{k-1/2} + p_{k+1/2})`, for :math:`k=1,2,..,NLEV`. The vertical coordinate is :math:`\eta=\eta(p,p_S)` and has the boundary value :math:`\eta(0,p_S)=0` at the top of the atmosphere and :math:`\eta(p_S,p_S)=1` at the Earth’s surface.

For the ocean, in the previous sections, we used volume transport because of the incompressibility approximation. In the atmosphere we need instead to use mass transport so Equation :eq:`U` is now replaced by the zonal and meridional mass transports in the model layers:

.. math::
   U_{i,j,k} = u_{i,j,k} \frac{ \Delta y \, \Delta p_k}{g} \;;\; V_{i,j,k} = v_{i,j,k} \frac{ \Delta x_{j} \, \Delta p_k}{g}
   :label: Um


where :math:`\Delta p_k = \Delta A_k + \Delta B_k p_{S \, i,j,k}`,
:math:`\Delta A_k =  A_{k+1/2} - A_{k-1/2}` and
:math:`\Delta B_k =  B_{k+1/2} - B_{k-1/2}` .

The vertical mass transport through the model layer interfaces :math:`W` is also required by the trajectory caclulations but cannot be as simply solved as for the ocean with its continuity equation. The mass of the grid box is, due to hydrostaticity,

.. math::
   M_{i,j,k}  =   \frac{\Delta p_{i,j,k}}{g}   \Delta x_{i,j} \Delta y_{i,j} = \frac{ \Delta A_k + \Delta B_k p_{S \, i,j}}{g}   \Delta x_{i,j} \Delta y_{i,j}
   :label: M

The mass conservation of a grid box as illustrated in :num:`Fig. #atm-grid`  yields :math:`d M_{i,j,k}/d t =  0`. The rate of mass change is hence equal to the mass transports through the grid box’s walls:

.. math::
   \frac{\partial M_{i,j,k}}{\partial t} =  - \left(  U_{i,j,k} - U_{i-1,j,k} + V_{i,j,k} - V_{i,j-1,k} +W_{i,j,k} - W_{i,j,k-1} \right)
   :label: Masscons

Eleminating :math:`M_{i,j,k}` between Eqs. :eq:`M` - :eq:`Masscons`:

.. math::
   \frac{\partial p_{S \, i,j}}{\partial t} \frac{\Delta B_k \Delta x_{i,j} \Delta y_{i,j} }{g} =  - \left(  U_{i,j,k} - U_{i-1,j,k} + V_{i,j,k} - V_{i,j-1,k} +W_{i,j,k} - W_{i,j,k-1} \right)
   :label: M2

The rate of change of surface pressure (:math:`{\partial p_{S}}/{\partial t}`) can be obtained by integrating Eq. :eq:`M2` over the entire air column from the Earth’s surface to the top of the atmosphere so that :math:`W` is eliminated by using the boundary conditions :math:`W_{k=0}=W_{k=NLEV}=0` so that

.. math::
   \frac{\partial p_{S \, i,j}}{\partial t} =  -  \frac{g }{ \Delta x_{i,j} \Delta y_{i,j} } \sum_{k=1}^{NLEV} \left(  U_{i,j,k} - U_{i-1,j,k} + V_{i,j,k} - V_{i,j-1,k} \right)
   :label: intvert

Eq. :eq:`M2` can now be succesively integrated upwards form the surface with the boundary condition :math:`W_{k=0}=0` to each :math:`k`-level so that

.. math::
    W_{i,j,k} = W_{i,j,k-1}  - U_{i,j,k} + U_{i-1,j,k} - V_{i,j,k} + V_{i,j-1,k} - \frac{\partial p_{S \, i,j}}{\partial t} \frac{\Delta B_k \Delta x_{i,j} \Delta y_{i,j} }{g}
   :label: W_int

The trajectory differential Eq. :eq:`drds` and its solutions Eqs. :eq:`rs` - :eq:`s1` remain the same but now feeded with mass transports on atmospheric terrain-following vertical coordinates and the scaled time is now :math:`s \equiv t g /(\Delta x_{i,j} \Delta y_{i,j} \Delta p_{k} )`.

Time dependence with time stepping
----------------------------------
The schemes in the previous sections are strictly only valid for steady state velocity fields. The scheme can however be used with time dependent fields by assuming that the velocity fields are in steady state during a chosen time step :math:`dtmin`.


Analytical time dependent scheme
--------------------------------

In the present section, we will present a truly time dependent scheme which is solved analytically, which was introduced by . This scheme is however not as robust as the stationary one.

Analytical solution
~~~~~~~~~~~~~~~~~~~

Given a set of velocities :math:`\mathbf{V}_n` for each model point, where :math:`n` represents a discretised time, a bi-linear interpolation of transports in space as well as in time leads to

.. math::
   \begin{gathered}
   F(r,s) = F_{i-1,n-1} + (r-r_{i-1})(F_{i,n-1}-F_{i-1,n-1}) + \\
    \frac{s-s_{n-1}}{\Delta s} \left[ F_{i-1,n} - F_{i-1,n-1} + (r-r_{i-1})(F_{i,n}-F_{i-1,n} - F_{i,n-1} + F_{i-1,n-1}) \right]\end{gathered}
   :label: Frs


which is the genral expression for the three directions where :math:`i` signifies either a longitudinal, meridional, or vertical direction. The transport :math:`F=(U,V,W)` and as before :math:`r=(x,y,z)`, :math:`r=(x/\Delta x, y/\Delta y, z/\Delta z)`, :math:`s \equiv t/(\Delta x \Delta y \Delta z)`, where the denominator is the volume of the particular grid box. :math:`\Delta s` is the scaled time step between two data sets

.. math::
   \Delta s = s_n-s_{n-1} = (t_n-t_{n-1})/(\Delta x \Delta y \Delta z) = \Delta t /(\Delta x \Delta y \Delta z)
   :label:ds

where :math:`dt` the time step between two data sets in true time
dimension (seconds).

Connecting the local transport to the time derivative of the position
with :math:`F=dr/ds`, we get the differential equation

.. math::
   \frac{dr}{ds} + \alpha \, r \, s + \beta  \, r + \gamma \, s + \delta= 0
   :label: drds_time

where the coefficeints are defined by

.. math::
   \alpha \equiv - \frac{1}{\Delta s}\, (F_{i,n}-F_{i-1,n} - F_{i,n-1} + F_{i-1,n-1})
   :label: alpha

.. math::
   \beta \equiv F_{i-1,n-1} -F_{i,n-1} -\alpha \,  s_{n-1}
   :label: beta

.. math::
   \gamma \equiv - \frac{1}{\Delta s} \, (F_{i-1,n}- F_{i-1,n-1}) - \, \alpha \,  r_{i-1}
   :label: gamma

.. math::
   \delta \equiv F_{i-1,n-1} + r_{i-1}(F_{i,n-1}-F_{i-1,n-1}) -\gamma \,  s_{n-1}
   :label: delta

Analytical solutions can be obtained for the following three cases:
:math:`\alpha > 0`, :math:`\alpha < 0` and :math:`\alpha = 0`, Note that inside the grid box, the acceleration, :math:`d^2r/ds^2 = -\alpha r - \gamma`, consists of a constant and an :math:`r-`\ dependent term proportional to :math:`\alpha`. For :math:`\alpha > 0`, the latter term implies a varying deceleration across the grid box. If :math:`\alpha > 0`, we define the timelike variable :math:`\xi = (\beta + \alpha \,  s)/\sqrt{2 \alpha}` and get

.. math::
   r(s) =  \left(r_0 + \frac{\gamma}{\alpha}   \right) e^{\xi^2_0- \xi^2} - \frac{\gamma}{\alpha} + \frac{\beta \gamma -\alpha \delta}{\alpha} \sqrt{\frac{2}{\alpha}}  \left[ D(\xi) -  e^{\xi^2_0- \xi^2} D(\xi_0)  \right]
   :label: rs_time

using Dawson’s integral

.. math::
   D(\xi) \equiv  e^{- \xi^2} \int_0^\xi e^{x^2} dx
   :label: dawson

and the initial condition :math:`r(s_0)=r_0`. If :math:`\alpha < 0`, :math:`\xi` becomes imaginary. By defining :math:`\zeta \equiv i \xi = (\beta + \alpha s)/\sqrt{-2 \alpha}`, Eq. [eq:rs:sub:`t`\ ime] can be re-expressed as

.. math::
   r(s) =  \left(r_0 + \frac{\gamma}{\alpha}   \right) e^{\zeta^2- \zeta^2_0} - \frac{\gamma}{\alpha} - 
   \frac{\beta \gamma -\alpha \delta}{\alpha} \sqrt{\frac{\pi}{-2\alpha}} \, e^{\zeta^2}  \left[ erf(\zeta) -  erf(\zeta_0)  \right],
   :label: rs_time2

where :math:`erf(\zeta)=(2/\sqrt(\pi)\int_0^\zeta e^{-x^2} dx`. The case :math:`\alpha = 0` will occur occasionally in practice. Its corresponding solution of The differential Eq. ([eq:drds:sub:`t`\ ime]) is

.. math::
   r(s) =  \left(r_0 + \frac{\delta}{\beta}   \right) e^{-\beta(s-s_0)} - \frac{\delta}{\beta} + 
   \frac{ \gamma }{\beta^2}  \left[ 1 -  \beta s +(\beta s_0-1)  e^{-\beta(s-s_0)} \right]
   :label: rs_time3

Compare this with the solution :eq:`rs` for time-independent velocity fields. A major difference compared to the time-independent case is that now the transit times :math:`s_1 - s_0` cannot in general be obtained explicitly. Using the solutions :eq:`rs_time` - :eq:`rs_time3`, the relevant root :math:`s_1` of

.. math::
   r(s_1) -r_1 =  0
   :label: rs1

has to be computed numerically for each direction. In the following subsection, we describe how the roots :math:`s_1` and the corresponding exiting wall :math:`r_1` can be determined. The displacement of the trajectory inside the considered grid box then proceeds as discussed previously for stationary velocity fields.


Determination of :math:`s_1` and :math:`r_1`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here we consider how to determine the roots :math:`s1` of Equation :eq:`rs1` and the corresponding :math:`r_1` needed to compute trajectories inside a grid box. In the following, :math:`s_{n-1}   \leqslant s_0 < s_n` and the relevant roots :math:`s_1 are to obey :math:`s_0 < s_1 \leqslant s_n` . We also focus on the cases :math:`\alpha > 0` and :math:`\alpha < 0`, since the considerations below can easily be adapted for :math:`\alpha = 0`. For numerical purposes, we use

.. math::
    \frac{\beta \gamma -\alpha \delta}{\alpha} =  \frac{ F_{i,n} F_{i-1,n-1} - F_{i,n-1} F_{i-1,n} }{F_{i,n}-F_{i-1,n} - F_{i,n-1} + F_{i-1,n-1}}
   :label: betagamma

.. math::
    \frac{\gamma}{\alpha} =  \frac{  F_{i-1,n} -  F_{i-1,n-1} }{F_{i,n}-F_{i-1,n} - F_{i,n-1} + F_{i-1,n-1}} -  r_{i-1}
   :label: gammalpha

.. math::
   \xi =   \frac{  F_{i-1,n-1} -  F_{i,n-1} + \alpha (s-s_{n-1})}{ \sqrt{2\alpha}}
   :label: xi

.. math::
   \zeta =   \frac{  F_{i-1,n-1} -  F_{i,n-1} + \alpha (s-s_{n-1})}{ \sqrt{-2\alpha}}
   :label: zet

The coefficients :eq:`betagamma` appearing in the solutions
[eq:rs:sub:`t`\ ime] and [eq:rs:sub:`t`\ ime2] is exactly zero when either the :math:`r_{i-1}` or :math:`r_i` wall represents land, the transports :math:`F_i` or :math:`F_{i-1}` being zero for all :math:`n`, respectively. In these instances, the opposite wall fixes :math:`r_1` , and the root :math:`s_1 > s_0` can then be computed analytically. If there is no solution, we take :math:`s_1 = s_n`. When all three transit times equal :math:`s_n`, the trajectory will not move into an adjacent grid box but will remain inside the original one. Its new position is subsequently computed, and the next time interval is considered.

If :math:`(\beta \gamma -\alpha \delta ) / \alpha \neq 0`, the computation of the roots of Eq. :eq:`rs1` can only be done numerically. This is also true for locating the extrema of the solutions [eq:rs:sub:`t`\ ime] and [eq:rs:sub:`t`\ ime2]. Alternatively, one can consider :math:`F(r, s) = 0` using Eq. :eq:`Frs` to analyze where possible extrema are located. It follows that in the :math:`sr` plane, extrema lie on a hyperbola of the form :math:`r = (as + b)/(c + ds)`. Of course, only the parts defined by :math:`s_{n-1} \leq s \leq s_n` and :math:`r_{i-1} \leq r \leq r_i` are relevant. Depending on which parts of the hyperbola, if any, lie in this "box" and on the initial condition :math:`r(s_0) = r_0` , the trajectory :math:`r(s)` exhibits none, one, or at most two extrema. In the latter case, the trajectory will not cross either the wall at :math:`r_{i-1}` or the one at :math:`r_i` (see Fig. :num:`Fig. time_analyt` for an example). Hence, those trajectories :math:`r(s)` determining the transit time :math:`s_1 - s0 will have at most one extremum, that is, there is at most one change of sign in the local transport :math:`F`.

An efficient way to proceed then is as follows. First, consider the wall at :math:`r_i`. For a trajectory to reach this wall, the local transport must be nonnegative, which depends on the signs of the transports :math:`F_{i-1,n}` and :math:`F_{i,n}`. Four distinct configurations may arise:

#. :math:`F(r_i,s) > 0` for :math:`s_{n-1} < s < s_n`

#. sign of :math:`F(r_i,s)` changes from positive to negative at
   :math:`s = \tilde{s} < s_n`

#. sign of :math:`F(r_i,s)` changes from negative to positive at
   :math:`s = \hat{s} < s_n`

#. :math:`F(r_i,s) < 0` for :math:`s_{n-1} < s < s_n`

For case 1, evaluate :math:`r(s_n)` using the appropriate analytical
solution. If :math:`r(s_n) \geq r_i`, the trajectory has crossed the
grid-box wall for :math:`s_1 \leq s_n`. If the initial transport
:math:`F(r_0,s_0) < 0`, the trajector y may have crossed the opposite
wall at an earlier time. The latter is only possible if case 3 applies
for the wall at :math:`r i_1` and :math:`\hat{s} >  0`, in which case
one checks whether :math:`r(\hat{s}) \leq r_{i-1}`. If this is not so,
then there is a solution to :math:`r(s_1) - r_1 = 0` for
:math:`r_1 = r_i` and :math:`s_0 < s_1 \leq s_n`. Subsequently, this
root can be calculated numerically using a root-solving algorithm. On
the other hand, if :math:`r(s_n) < r_i` or, if applicable,
:math:`r(\hat{s} ) \leq r_{i-1}` continue with considering the other
wall. The arguments for the wall at :math:`r_{i-1}` are similar to those
relating to :math:`r`.

If case 2 applies and :math:`s_0 < \tilde{s}`, follow the considerations
given for case 1 using :math:`\tilde{s}` instead of :math:`s_n`. If
there is a root for :math:`r_1 = r_i` , then
:math:`s_0 < s_1 \leq \hat{s}` .

For case 3, follow the considerations given for case 1. If there is a root for :math:`r_1=r_i`, then :math:`\hat{s} < s_1 \leq s_n` .

For case 4, no solution of Eq. :eq:`rs1` is possible for
:math:`r_1 = r_i`. Turn attention to the other wall.

All these considerations is applied to each direction.
