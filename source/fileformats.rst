
Where and when to seed particles
================================


The seeding of particles in TRACMASS can be controlled in two 
different ways. 1) By defining a region of grid cells and time steps
(variables ist1, ist2, jst1, jst2, kst1, kst2, tst1, tst2), e.g. seed
particles every time step during one year at 30S latitude line. 



2) Simulate surface drifters by seeding at given positions and times, e.g. one particle at (x,y) at 15m depth at 2 Jan 1992 at 6am. 
Then there are a number of combinations of the above, e.g. define a list of grid boxes along a coastline and seed every 4th time step. 

I introduced four variables to TRACMASS to handle at least a few different kinds of seeding. 
This was done when simulating surface drifters in the Baltic, so some of these options may only be available for RCO or BaltiX data and the rco_run.in provides some description of the set up. 

seedPos sets whether seed positions is an interval read in the run.in namelist or from a text file. 
seedTime sets whether seed time is an interval read in run.in namelist of from a text file. 
seedAll sets whether each seed time in the text file is for just one particle or all of them. 
seedType sets whether the seed positions in the text file is grid box indices (integers) or exact positions in model coordinates (floats). 

So seeding as in example 1) above would be done by setting seedPos = 1 and seedTime = 1.
Seeding three surface drifters at different positions and time would
be done by seedPos = 2, seedTime = 2, seedAll = 1, seedType = 1 and
the seed text file would look like::

      177       137        38     4     0  1962071500
      165       143        38     4     0  1962072518
      174       149        38     4     0  1962080506

The first three columns are i,j,k indices for the grid box in which to seed the drifter, the fourth column is isec = 4 (seed in center of grid box) and the fifth column is idir = 0 (allow particle to travel in any direction). The sixth column are the times in year-month-day-hour format which must coincide with model time steps (in this case 6 hours). 
If seedAll = 2 all three particles above would be seeded each time step so that we would get nine seeds instead of three. 

So you might be able to set up a list of grid boxes in a text file and seed them all for time steps also given in a text file (seedPos = 2, seedTime = 2, seedAll = 2, seedType = 1). 

This kind of seeding might leave TRACMASS to be running although the time is not yet right to seed any particles. So if seedTime = 2 TRACMASS will run until some trajectories have been seeded and ended, while in all other cases TRACMASS stops whenever the number of active trajectories is 0. 
