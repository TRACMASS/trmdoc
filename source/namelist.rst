

Namelist options
================

The namelist files are used by TRACMASS to define most of the
behavior. There are two types of namelists. One with the name of the
project, that defines default behaviour, and one for each case where
special settings are included.


Version number of the namelist
------------------------------
*INIT_NAMELIST_VERSION*

 .. option:: gridVerNum

    Version number of the namelist. This is used to check if the
    namelist file has the correct syntax

Description of the velocity fields
----------------------------------
*INIT_GRID_DESCRIPTION*

 .. option:: GCMname

    Name of the GCM that generated velocity fields

 .. option:: GCMsource

    URL to the GCM that generated velocity fields

 .. option:: gridName

    Name of the specific run/grid used to generate velocity fields

 .. option:: gridSource

    URL to the spcific run/grid used to generate velocity fields

 .. option:: gridDesc

    Longer descrition of of the grid and run used.

 .. option:: inDataDir

  Directory where velocity fields are stored. This can also be set by
  the environmental variable :envvar:`TRMINDATADIR`. TRACMASS will
  append the project name so that inDataDir = $TRMINDATADIR/PROJNAME/



Description of the current case
-------------------------------
*INIT_CASE_DESCRIPTION*

 .. option:: caseName
             
    Name of the current case

 .. option:: caseDesc  

    Longer desscriptions of the current case


Shape of the grid
-----------------
*INIT_GRID_SIZE*

 .. option:: imt

    Number of gridcells in x or zonal (longitude) direction

 .. option:: jmt

    Number of gridcells in y or meridional (latitude) direction

 .. option:: km

    Number of gridcells in z (depth) direction

 .. option:: nst

    Number of velocity fields in memory simultaneously. Defaults to 2
    but can can be used to have for example a full climatology in 
    memory to speed up the run.
 
 .. option:: subGrid:

    Select how to define the grid used by TRACMASS::

      0 = Use full grid.
      1 = Define subGrid boundaries.
      2 = Define subGrid in a separate file and subGridID.
      3 = Define subGrid in a separate file and MPI.
  
If SubGrid is 1:
 
 .. option:: subGridImin, subGridImax

    Min / Max i-position

 .. option:: subGridJmin, subGridJmax

    Min / Max j-position

 .. option:: subGridKmin, subGridKmax

    Min / Max k-position (direction as defined by input file)

If SubGrid is 2 or 3:

 .. option::  SubGridFile

    File to read grid definitions from ***REF***

If SubGrid is 2:

  .. option:: subGridID

     Which subgrid to use. This would preferable be set at runtime (***ref***) 



Base times to calculate Julian Dates
------------------------------------
*INIT_BASE_TIME*

These settings are used to calculate julian dates. Default is set for
how pylab/numpy defines JD's, but can be changed if you prefer for
example matlab's standard (baseYear=0) or Astronomical julian dates
(baseYear=-4713).

 .. option::  baseSec

    Reference second (default = 0)

 .. option::  baseMin

    Reference minute (default = 0)

 .. option::  baseHour

    Reference hour (default = 0)

 .. option::  baseDay

    Reference day (default = 0)

 .. option::  baseMon

    Reference month (default = 1)

 .. option::  baseYear

    Reference year (default = 1)


Time settings for the velocity fields
-------------------------------------
*INIT_GRID_TIME*

 .. option:: fieldsPerFile
    
    Number of velocity fields in each file.

 .. option:: ngcm
             
    Number of hours between each GCM velocity field

 .. option:: iter 
    
    Number of iterations by TRACMASS between each GCM velocity field.

 .. option:: intmax

    Total number of GCM velocity fields
             
 .. option::  minvelJD

    Sets the julian date (defined before) for the first velocity field
    to be used in the run. Set to -1 if not used

 .. option::  maxvelJD

    Sets the julian date (defined before) for the last velocity field
    to be used in the run. Set to -1 if not used

*intmax*, *minvelJD* and/or *maxvelJD* are used when looping the
velocity fields to allow for longer runs. *minvelJD* and
*maxvelJD* can define a subset of the data (e.g. loop over the year 2000 even if data spans from 1950 to 2100.


Start time for the TRACMASS run
-------------------------------
*INIT_START_DATE*

 .. option:: startHour

    Start hour for the run

 .. option:: startDay

    Start hour for the run

 .. option:: startMon

    Start hour for the run

 .. option:: startYear

    Start hour for the run

Alternatively

 .. option:: startJD

    Start Julian-Date (as defined above) for the run. Set to -1 if not used.

Or
 .. option:: intmin

    Start ints timestep for the run. Set to -1 if not used.

Timeperiod of seeding and advection
-----------------------------------
*INIT_RUN_TIME*
 
 .. option:: intspin

    Number of timesteps (changes of velocity fields) to seed 
    particles

 .. option:: intrun

    Number of timesteps (changes of velocity fields) to advect the 
    particles


.. _INITRUNWRITE:

Writing particle postions
-------------------------
*INIT_WRITE_TRAJS*

 .. option:: twritetype

    Format of time in output file::

      0 = timesteps since the beginning of the run
      1 = seconds since the beginning of the run
      2 = Julian date as defined above


 .. option:: kriva

    When to write out particle positions::
 
      0 = no output
      1 = write at time intervals of gcm datasets
      2 = write each grid-crossing and time change
      3 = write at each iteration (all the time)
      4 = write only start and end positions
      5 = write at chosen intervals
  
 .. option:: outDataDir
  
    Directory where output files are saved. This can also be set by
    the environmental variable :envvar:`TRMOUTDATADIR`. TRACMASS will
    append the project name so that outDataDir = $TRMOUTDATADIR/PROJNAME/

 .. option:: outDataFile
    
    Basename for the output files. This is selected automativally if not set.


Control how and where particles are seeded 
------------------------------------------
*INIT_SEEDING*

 .. option:: nff
    
    Direction in time that particles are followed::
      
      1 = Follow trajectories forward
      2 = Follow trajectories backward
      
 .. option:: isec

    Which plane particles are seeded in::
      
      1 = Seed particles meridionaly (y-z)
      2 = Seed particles zonally (x-z)
      3 = Seed particles horizontally (x-y)
      4 = Seed particles evenly in the T-box

 .. option:: idir

    Only follow particles in certain directions::

      0  = Follow all particles
      1  = Follow particles that start eastward/northward
      -1 = Follow particles that start westward/southward

 .. option:: nqua
 
    How the number of trajectories to be seeded is defined::

      1 = Seed same number of particles in all boxes
      2 = Number of particles reflect volume flux through gridcell 
      3 = Number of particles reflect volume in gridcell.
      4 = Number of particles is set in seed file.

 .. option:: partQuant

    Particles seeded in each gridcell. Units depends on **nqua**. 
    *1*: particles / gridcell, *2*: $m_3 s^{-1}$. per particle, 
    *3*: m3 per particle
 
 .. option:: ntracmax

    Maximum number of particles to be seeded

 .. option:: loneparticle

    This option allows you to only seed one particle for trouble
    shouting purposes. set **loneparticle** to the ntrac ID of the
    particle you want to follow. Set to zero to seed all particles.


 .. option:: seedType

    Method for seeding particles (all units are in model
    coordinates)::

      1 = Seed an area defined by ist, jst, and kst.
      2 = Use a list to define which cells to seed.
      3 = Use a 2-D mask file.

If seedType is 1:

 .. option:: ist1, ist2, jst1, jst2

    Lower-Southern / Upper-Northern /  Left-Western / Right-Eastern 
    boundary for seed area. -1 indicates imt/jmt.

 .. option:: kst1, kst2

    Deepest / Shallowest level to seed (0 at botton, km at surface)

 .. option:: tst1, tst2

    First and last timestep to seed particles. These variables 
    override :option:`intspin` if set.


If seedType is 2 or 3:

 .. option:: seedDir

    Directory to look for seed files

 .. option:: seedFile

    Name of seed file

 .. option:: varSeedFile
             
    Use variable seedfile names if set to 1. The file name will be
    constructed as /seedpath/seed0000.asc where 0000 is intstart. 
    This allows for the use of different seed files at different 
    start points.

 .. option:: seedTime 

    Sets whether seed time is an interval read in run.in namelist
    of from a text file. 

 .. option:: seedAll 

    Sets whether each seed time in the text file is for just one 
    particle or all of them. 

 .. option:: seedPos 

    Decides how positions in the seed file are intepreted::

      1 = Values (integers) in the seed file indicate grid cells 
          that are to be seeded. The amount of particels  is 
           controlled by :option:`mqua`. 

      2 = Values (floats) in the seed file indicate the exact 
          positions in model coordinates for where to seed particles.
          :option:`mqua` has no function.

Options to only seed a subset of particles:

 .. option:: seedparts

    Split up the seeded area in **seedparts** chunks and seed only 
    the one defined by **seedpart_id**. Very useful when running
    TRACMASS on multi-core machines.
 
 .. option:: seedpart_id

    Set the seedparts chunk to be seeded. Preferably set from
    commandline in a script.


Killzones for particles
-----------------------
*INIT_KILLZONES*

This functionality allows to define zones where all particles are
killed. Each zone is defined by ienw(n), iene(n), jens(n), jenn(n)
where n is the zone id. For example::

    ienw(1) =    5,
    iene(1) =    6,
    jens(1) =    0,
    jenn(1) =    200,

Will kill all particles that enter the rectangle i=5-6 and j=0-200.

 .. option:: nend

    Number of killzones to use

 .. option:: jens(n)

    Lower / Southern boundary of killzone

 .. option:: jenn(n)

    Upper / Northern boundary of killzone

 .. option:: ienw(n) 

    Left / Western boundary of killzone

 .. option:: iene(n)

    Right / Eastern boundary of killzone

 .. option:: timax
             
    maximum time length of a trajectory in days



Salt, temp or dens seed and kill particles
------------------------------------------
*INIT_TEMP_SALT*

This functionality is only available only when TRACMASS is compiled
with the option *tempsalt*.

 .. option:: tmin0, smin0, rmin0

    Min temperature/salinity/density to seed particle.

 .. option:: tmax0, smax0, rmax0

    Max temperature/salinity/density to seed particle.

 .. option::   tmine, smine, rmine

    Min temperature/salinity/density before killing particle.

 .. option::   tmaxe, smaxe, rmaxe

    Max temperature/salinity/density before killing particle.


Special functions
-----------------

*INIT_DIFFUSION*

 .. option:: ah

    Horizontal diffusion in m2/s

 .. option:: av

    Vertical diffusion in m2/s

*INIT_DIFFUSION*

 .. option:: critvel

    Critical velocity for sedimentation

 .. option:: cwamp

    Constant for approximating wave amplitude, a = cwamp*U(surface)

 .. option:: twave

    Approximative peak period. Average 4s for Baltic proper

 .. option:: partdiam
    
    Particle diameter for sedimentation (mm) 
    clay 0.0005-0.002, silt 0.002-0.06, fine sand 0.06-0.2,
    medium sand 0.2-0.6, coarse sand 0.6-2, gravel>2

 .. option:: rho

    Density of quartz particle: 2600-2650 g/cm^3, mean value 2620























(variables in all namelists)
----------------------------

 NTRACMAX,
 SeedPos,
 SeedType,
 SubGridFile,
 ah,
 arcscale,
 av,
 baseDay,
 baseHour,
 baseMin,
 baseMon,
 baseSec,
 baseYear,
 caseDesc,
 caseName,
 critvel,
 cwamp,
 degrade_time,
 directory,
 fieldsPerFile,
 gridDesc,
 gridName,
 gridSource,
 gridVerNum,
 iday,
 idir,
 iene,
 iene(1),
 iene(2),
 iene(3),
 iene(4),
 iene(5),
 ienw,
 ienw(1),
 ienw(2),
 ienw(3),
 ienw(4),
 ienw(5),
 ihour,
 imon,
 inDataDir,
 intmax,
 intmin,
 intminInOutFile,
 intrun,
 intspin,
 intstep,
 isec,
 ist1,
 ist2,
 iter,
 iyear,
 jenn,
 jenn(1),
 jenn(2),
 jenn(3),
 jenn(4),
 jenn(5),
 jens,
 jens(1),
 jens(2),
 jens(3),
 jens(4),
 jens(5),
 jst1,
 jst2,
 kriva,
 kst1,
 kst2,
 loneparticle,
 loopTime,
 maxvelJD,
 minvelJD,
 name,
 ncoor,
 nff,
 ngcm,
 nqua,
 outDataDir,
 outDataFile,
 partQuant,
 partdiam,
 rhos,
 rmax0,
 rmaxe,
 rmin0,
 rmine,
 runVerNum,
 seedAll,
 seedDir,
 seedFile,
 seedPos,
 seedTime,
 seedType,
 seedpart_id,
 seedparts,
 smax0,
 smaxe,
 smin0,
 smine,
 startDay,
 startHour,
 startMIN,
 startMin,
 startMon,
 startSec,
 startYear,
 subGrid,
 subGridID,
 subGridImax,
 subGridImin,
 subGridJmax,
 subGridJmin,
 subGridKmax,
 subGridKmin,
 timax,
 timeFile,
 tmax0,
 tmaxe,
 tmin0,
 tmine,
 topoDataDir,
 tst1,
 tst2,
 twave,
 twaves,
 twritetype,
 varSeedFile,
 varSeedName,
 wgribDir,
 yearmax,
 yearmin

