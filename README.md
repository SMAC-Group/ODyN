<img src="https://github.com/SMAC-Group/ODyN/raw/master/images/logo.png" width="400">

## an Online Dynamic Network solver

This repository hosts `ODyN`, a free Dynamic Network (DN) solver that runs online and uses the browser as an interface for data I/O, option settings and result visualization. `ODyN` fuses information from navigation (GNSS, inertial) and optical sensors (camera, lidar) in a single adjustment and it can be used to optimally estimate a high-frequency trajectory for precise geo-referencing, to improve photogrammetric and/or lidar reconstructions in challenging scenarios or to determine system and sensor calibration parameters.

`ODyN` is a [R Shiny](https://shiny.rstudio.com/) application featuring an user-friendly Graphical User Interface (GUI) for the [ROAMFREE](https://github.com/AIRLab-POLIMI/ROAMFREE) sensor fusion library, which contains the actual solver for Dynamic Network adjustment problems. The processing of the user-provided data happens on backend servers provided by the [ENAC-IT 4 Research](https://it4r.super.site) at Swiss Federal Institute of Technology Lausanne [EPFL](https://www.epfl.ch).

The white paper describing `ODyN` can be found at:

1. Cucci, D.A., "ODyN: an online dynamic network solver for photogrammetry and LiDAR geo-referencing". *ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences*, V-1-2022, 153–159. [PDF](https://doi.org/10.5194/isprs-annals-V-1-2022-153-2022)

For more information and for the scientific backgorund of Dynamic Networks and applications to navigation, mapping and sensor orientation in general, please refer to the following scientific publications:

2. Cucci, D.A., Rehak. M, and Skaloud, J., "Bundle adjustment with raw inertial observations in UAV applications". *ISPRS Journal of photogrammetry and remote sensing* 130 (2017): 1-12. [PDF](https://drive.google.com/file/d/1U2RKh7T98bFJYvnMGHIOrxKyIRVDP0Oi/view?usp=sharing)
3. Brun, A., Cucci, D.A., and Skaloud, J., "Lidar point–to–point correspondences for rigorous registration of kinematic scanning in dynamic networks." *ISPRS Journal of Photogrammetry and Remote Sensing*, 189 (2022): 185-200. [PDF](https://doi.org/10.1016/j.isprsjprs.2022.04.027)
4. Mouzakkidou, K., Cucci, D.A., and Skaloud, J., "On the benefit of concurrent adjustment of active and passive optical sensors with GNSS & raw inertial data", *ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences*, V-1-2022: 161-168. [PDF](https://doi.org/10.5194/isprs-annals-V-1-2022-161-2022). 

`ODyN` is available for free, it can be used in any context, but it comes without any guarantee :)

# Access

`ODyN` is available at [odyn.epfl.ch](https://odyn.epfl.ch).

>**Warning**: **ODyN is in active state of development**. There might be errors, or bugs, or malfunctions, or all of these at the same time. Please, **help us** to improve our work! You can report any issue or problems you run into using the [Issues](https://github.com/SMAC-Group/ODyN/issues) page of this repository, we will do our best to assist you and correct problems as soon as possible. When reporting an issue, be sure to include the data file (`.zip`) and the configuration (`.RData`) that you used, so that we can easily reproduce the problem.

# Examples

Example data and configuration is provided in this repository as described in the following (to use these files in ODyN please see the *"Documentation"* section later on). 

## Example 1 to 3

Most of the data has been gently made available by Dr. Julien Vallet, director of [Helimap Sixense](https://helimap.ch), and described in the following scientific publication:

- Vallet, J., Gressin, A., Clausen, P., and Skaloud, J., "Airborne And Mobile Lidar, Which Sensors For Which Application?". *The International Archives of Photogrammetry, Remote Sensing and Spatial Information Sciences*, 43 (2020): 397-405. [PDF](https://doi.org/10.5194/isprs-archives-XLIII-B1-2020-397-2020)

Among other things, this data allows to reproduce some of the results presented in [1, 3, 4].

### Configuration file

All data files can be used with the same configuration file available at [data/vallet/configuration.RData](https://github.com/SMAC-Group/ODyN/raw/master/data/vallet/configuration.RData)

### Data Files

- Example 1 - Inertial Navigation: [data/vallet/INS.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/vallet/INS.zip)
- Example 2 - Inertial Navigation + LiDAR point-to-point correspondences: [data/vallet/INS+lidar.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/vallet/INS+lidar.zip)
- Example 3 - Inertial Navigation + image tie-points (photogrammetry, corridor): [data/vallet/INS+photo.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/vallet/INS+photo.zip)

## Example 4

This data corresponds to the flight `ign8`, part of the open-data releases extensively discussed in:

- Skaloud, J., Cucci, D.A., and Paul, K.J., "Fixed-wing micro UAV open data with digicam and raw INS/GNSS". *ISPRS Annals of Photogrammetry, Remote Sensing and Spatial Information Sciences*, 51 (2021): 105-111. [PDF](https://doi.org/10.5194/isprs-annals-V-1-2021-105-2021)

The original data, including images and all raw sensor data, before conversion to the `ODyN` format, can be also found at [link](https://zenodo.org/record/4705424).

### Configuration file

[data/ign8/configuration.RData](https://github.com/SMAC-Group/ODyN/raw/master/data/ign8/configuration.RData)

###  Data file

 - Example 4 - Inertial Navigation + image tie-points (phtogrammetry, block, calibration): [data/ign8/INS+photo.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/ign8/INS+photo.zip)


# Documentation

The use of `ODyN` is straightforward according to the following steps: 

1. Upload a `.zip` file containing all sensor measurements (see section 'Input file format' below).
2. Verify that the data files are loaded correctly (check the messages below the file upload input).
3. [Optional] Upload an `.RData` file containing the configuration of all parameters (see *Step 8* below).
4. Enter all required configuration parameters via the GUI according to your setup (or in case of *Step 3* verify that these were loaded and interpreted correctly).
5. Hit the **Go!** button and wait. Depending on the input, few minutes may be required.
6. If everything has gone well, check the results in the provided plots. Otherwise go back to *Step 4* and fix the problem.
7. Download the solution file.
8. [Optional] Download the configuration to be reused later on in *Step 3* (N.B. this file can be downloaded even if *Step 5* has not been run or failed).
9. [Optional] Generate a link to share the output online.

## Input file format

All input files should be included in a `.zip` archive. This file should be then uploaded in the **Inputs** section of the interface (*Step 1* above). The archive should contain the following files, format of which is specified further below in the sections 'File xxx.xxx'. 

>**Warning**: The file names *must match exactly*!

### Coordinate system

`ODyN` allows to fuse navigation, lidar and photogrammetric data either in *Global* coordinate system (WGS84) or *Local* coordinate system (tangent plane) the latter intended for indoor use with low-cost inertial sensors. The coordinate system can be changed in `ODyN` **[Processing options]** panel and **must** correspond to the coordinate system of the input data.

A short overview of file contents related to *Navigation* (mandatory) and *Navigation + Lidar/Photogrammetry* (optional) follows.

### Navigation

- `GPS.txt` or `GPS.cmb`: position measurements from a GNSS receiver in global frame **or** position estimated via other means in local frame (e.g. visual inertial odometry)
- `IMU.txt`: raw specific force and angular velocity measurements from an Inertial Measurement Unit (IMU),
- `initial_guess.txt` or `initial_guess.out` **[optional]**: an initial trajectory solution that will be used to initialize the DN solver, 
- `reference.txt` or `initial_guess.out` **[optional]**: a reference trajectory to evaluate the output of the DN solver, 
- `config.Rdata` **[optional]**: configuration file obtained from previous `ODyN` execution (*applicable also to optical sensors below*).

The detailed description of the format of those files is further below. If the `initial_guess.txt` file  is not provided, `ODyN` will attempt to determine the initialization for the DN solver applying a [Savitzky–Golay](https://en.wikipedia.org/wiki/Savitzky%E2%80%93Golay_filter) filter to the provided GNSS positions to obtain an approximation of the body frame position at the frequency of the IMU. The initial orientation is derived assuming that the body frame mounting is either Front-Left-Up or Front-Right-Down and the x-axis is tangent to the body frame trajectory. For this approach to work, a certain velocity of the body frame is required with the velocity vector being principaly along the x-axis in the body frame. 

>NOTE: IMU and GPS files are *always* required (albeit the GPS solution may present gaps). `ODyN` uses  WGS-84 normal gravity model when fusing in global coordinate system. More detailed Earth Gravity Model(s) (e.g., EMG96) maybe implemented in the future.   
>In local frame, a constant gravity model is used. This is suitable only for low cost inertial sensors being operated indoors or over small areas. For any other case the global frame should be used. 

### Navigation + Photogrammetry

In case the user wants to fuse also image observations, the following additional files should be included into the archive:

- `bingo.txt`: image observations,
- `image_timestamps.txt`: exposure times for the images included in `bingo.txt`,
- `GCPs.txt` **[optional]**: 3D coordinates of Ground Control Points/Checkpoints.

The detailed description of the format of those files is given below.

### Navigation + Lidar

In case the user wants to fuse also lidar point-to-point spatial constraints, the following additional file should be included in the archive:

- `LiDAR_p2p.txt`: lidar observations of tie-points.   

The detailed description of the format of this file is given below.

### File `GPS.txt` 

>NOTE: *Alternative* file is `GPS.cmb` corresponding to (legacy) **Grafnav/Waypoint** format.

This file contains a sequence of position observations obtained from a GNSS receiver or other position-fixing sensor (see the note in [Navigation](#Navigation)). It is a Coma Separated Value (CSV) file with **four** ~~or **[optional] seven**~~ columns and no header. 

- Column 1: epoch time, unit *seconds*,
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meter* **or** x, y, z, in local coordinates, units: *3x meters*
- ~~Column 5 - 7: **[optional]** incertitudes (*1-sigma*) in east-north-up directions per epoch, unit: *meter*.~~ (not yet implemented, use a global const. value) 

An example of the content of this file is given below in global coordinates:

```
396400.000, 46.569360752778, 6.533852830556, 609.752100000000
396401.000, 46.569583791667, 6.533973002778, 607.718300000000
396402.000, 46.569719905556, 6.534210213889, 606.953900000000
396403.000, 46.569749733333, 6.534465997222, 607.169700000000
396404.000, 46.569701600000, 6.534688980556, 606.820900000000
```

If an `initial_guess.txt` file is provided, `ODyN` tolerates well GNSS outages. This also holds if such file is not provided, but complex maneuvers during GNSS outages can prevent the solver to converge.

### File `IMU.txt`

This file contains a sequence of raw specific force and angular velocity observations obtained from an Inertial Measurement Unit (IMU). It is a CSV file with **seven** columns and no header.

- Column 1: measurement timestamp, unit *seconds*, 
- Column 2 - 4: angular velocity, unit *rad/s*,
- Column 5 - 7: specific force, unit  *m/s<sup>2</sup>*.

>NOTE: The `IMU.txt` files sets the time limits for the processing, meaning that no solution can be computed for timestamps before the first or after the last entry in the file. 

The IMU observations must be synchronized with the GPS or GNSS receiver, in the sense that the measurement timestamps reported for the IMU readings should be expressed in GPS time. If this is not the case, the two sensors can not be properly fused together by `ODyN`. Small jitters or delays can be tolerated, but a reduction of the performance (that is difficult to quantify) should be expected. An external solution for IMU sensor time-stamping and synchronization is to use the [SentiBoard](https://sentisystems.com/sales-2/).

Additionally, the IMU must have **a constant rate** (up to 10% jitter with respect to the sampling period is accepted) and no samples can be missing.  The user should also make sure that a sufficient number of digits is employed to represent IMU readings in the CSV file.

An example of the content of this file for a 500 Hz IMU is given below:

```
396401.000, 0.016107580, 0.300693593, 0.649536309, -0.046519610, 7.331238752, -15.765452987
396401.002, 0.025510935, 0.288168501, 0.649539582, -0.007448115, 7.155350738, -16.547551439
396401.004, 0.006704225, 0.300678700, 0.640177259, -0.144198348, 7.546249986, -15.609187826
396401.006, 0.022376484, 0.297568526, 0.640160221,  0.070694876, 7.878268971, -15.628652537
396401.008, 0.034914290, 0.300723380, 0.640139446, -0.026983862, 7.038125840, -16.567082796
```
>NOTE: ODyN allows to reduce the number of IMU nodes using IMU pre-integration method. The number of IMU epochs to pre-integrate can be modified in **[Sensors > IMU]** panel. While higher pre-integration reduce computation time, it can introduce aliasing when the preintegrated frequency is lower than the Nyquist frequency of motion, i.e. $f_{IMU}/n_{pre-int} < f_{Nyquist}$.


### File `initial_guess.txt` and `reference.txt` [optional]

> NOTE: *Alterantive* files are `initial_guess.out` and `reference.out`, corresponding to Applanix's [SBET](https://github.com/schwehr/research-tools/blob/master/python-binary-files.org) (or SNV, which is the same) format.

This file contains an initial solution for the body frame position and orientation that is used to initialize the Dynamic Network solver. It is a CSV file with **eight** columns and no header.
- Column 1: timestamp, unit *seconds*,
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meter*, **or** x, y, z, in local coordinates, units: *3x meters*
- Column 5 - 8: a quaternion representing R<sup>*n*</sup><sub>*b*</sub>, where *n* is a local level frame, unit: *N/A* 

A row in the `initial_guess.txt` file should be present for each IMU measurement in the `IMU.txt` file. More rows are allowed, but not less.
An example of the content of this file is given below in global coordinates:

```
396401.000, 46.569701600000, 6.534688980556, 606.820900000000, -0.218175913, 0.909282389, 0.338522248, -0.104916617
396401.020, 46.569360752778, 6.533852830556, 609.752100000000, -0.218858188, 0.911660293, 0.331996401, -0.103706282
396401.040, 46.569583791667, 6.533973002778, 607.718300000000, -0.219702240, 0.913903930, 0.325544215, -0.102623076
396401.060, 46.569719905556, 6.534210213889, 606.953900000000, -0.220010575, 0.916213164, 0.319082538, -0.101661786
396401.080, 46.569749733333, 6.534465997222, 607.169700000000, -0.220546280, 0.918408515, 0.312620830, -0.100763856
```

### File `bingo.txt`

This is a file format of image observations as used within [BINGO](https://bingo-atm.de/) adjustment software that is supported also by modern photogrammetric suites (e.g., Pix4D, AgiSoft). Image and tiepoint ids are mandatory. *All* image ids must be present in the file `image_timestamps.txt`, see later on. 

`ODyN` interprets tipeoints ids between 1 and 999 as GCPs/checkpoints observations and uses a different standard deviation for image measurements (as specified in the GUI). Only those points will be searched for in the `GCPs.txt` file.

>**Warning**: In contrary to BINGO, the unites of image coordinates are in *pixels* and the origin of the pixel is at the top-left corner (and not at the center)!

An example of the content of this file for two photos is given below: 

```
1 CAMERA
1 3411.610 1707.340
1019 3041.023 3454.031
1032 338.923 1574.094
1036 1244.775 2092.072
1038 2847.057 2157.910
1119 1812.034 1858.000
1123 1286.664 2111.538
1163 3422.762 1695.999
1174 3023.624 2085.276
-99
2 CAMERA
1 4523.190 1209.450
1019 4260.974 2934.453
1032 1452.491 1240.774
1036 2414.891 1711.515
1038 4005.476 1684.510
1119 2972.928 1444.660
1123 2459.228 1728.136
1163 4533.667 1197.529
-99
```

### File `image_timestamps.txt`

Photo ID with exposure time (1 per line) for all images included in `bingo.txt` in a chronological order. Timestamps should be expressed in GPS time.

- Column 1: photo no., unit *integer*, 
- Column 2: timestamp, unit *seconds*

An example of the content of this file is given below: 

```
1, 396774.806328
2, 306776.213498
```

### File `GCPs.txt`

Ground control (or check-points) point coordinates

- Column 1: gcp_no, unit *integer*
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meters*, **or** x, y, z, in local coordinates, units: *3x meters*

An example of the content of this file is given below in global coordinates: 

```
1, 46.569826426, 6.545714107, 509.828
2, 46.571215814, 6.546098940, 510.910
3, 46.573408778, 6.543244972, 511.871
4, 46.569325989, 6.542046817, 518.303
6, 46.569889128, 6.539480720, 518.907
```

### File `LiDAR_p2p.txt` 

This file contains the spatial conditions between two points observed by lidar from different parts of the trajectory. These tie-points are expressed by their respective time and coordinates in the *scanner frame* in a CSV file with **eight** columns and no header.

- Column 1 - 2: time stamps in *seconds* for the first and second tie-points, respectively
- Column 3 - 5: x-y-z coordinates of the **first** tie-point in the scanner frame, units: *meters* 
- Column 6 - 8: x-y-z coordinates of the **second** tie-point in the scanner frame, units: *meters* 
- ~~Column     9: **[optional]** incertitude (*1-sigma*) in the spatial proximity (Euclidean distance) both points, unit: *meter*~~ (not yet implemented, uses global const. value)

``` 
396765.649713,396660.533676,271.420000,-2.570000,18.060000,263.280000,-2.520000,-135.520000
396767.558842,396659.053466,263.810000,-2.520000,-0.480000,259.870000,-2.470000,-126.380000
396770.806328,396656.979838,250.690000,-2.390000,-76.950000,257.880000,-2.460000,-38.400000
396775.153753,396651.601487,272.190000,-2.600000,-21.910000,278.930000,-2.650000,-110.880000
``` 

