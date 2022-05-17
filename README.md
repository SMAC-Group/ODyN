# ODyN, an Online Dynamic Network solver

This repository hosts `ODyN`, a free Dynamic Network (DN) solver that runs online and uses the browser as an interface for data I/O, option settings and result visualization. `ODyN` fuses information from navigation (GNSS, inertial) and optical sensors (camera, lidar) in a single adjustment and it can be used to optimally estimate a high-frequency trajectory for precise geo-referencing, to improve photogrammetric and/or lidar reconstructions in challenging scenarios or to determine system and sensor calibration parameters.

`ODyN` is a [R Shiny](https://shiny.rstudio.com/) application featuring an user-friendly Graphical User Interface (GUI) for the [ROAMFREE](https://github.com/AIRLab-POLIMI/ROAMFREE) sensor fusion library, which contains the actual solver for Dynamic Network adjustment problems. The processing of the user-provided data happens on backend servers provided by the by the [ENAC-IT 4 Research](https://it4r.super.site) at Swiss Federal Institute of Technology Lausanne [EPFL](https://www.epfl.ch).

The white paper describing `ODyN` can be found at

1. Cucci, D.A., "ODyN: an online dynamic network solver for photogrammetry and LiDAR geo-referencing". *ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences*, (2022) to appear

for more information and for the scientific backgorund of Dynamic Networs and applications to navigation, mapping and sensor orientation in general, please refer to:

2. Cucci, D.A., Rehak. M, and Skaloud, J. "Bundle adjustment with raw inertial observations in UAV applications". *ISPRS Journal of photogrammetry and remote sensing* 130 (2017): 1-12. [PDF](https://drive.google.com/file/d/1U2RKh7T98bFJYvnMGHIOrxKyIRVDP0Oi/view?usp=sharing)
3. Brun, A., Cucci, D.A., and Skaloud, J. "Lidar point–to–point correspondences for rigorous registration of kinematic scanning in dynamic networks." *arXiv [preprint](https://arxiv.org/abs/2201.00596)*.
4. Mouzakkidou, K., Cucci, D.A., and Skaloud, J. "On the benefit of concurent adjustment of active and passive optical sensors with GNSS & raw inertial data", *ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences*, 2022 (to appear). 

`ODyN` is available for free, it can be used in any context, but it comes without any guarantee :)

# Access

`ODyN` is available at [odyn.epfl.ch](https://odyn.epfl.ch).

**N.B. ODyN is in active state of development**. There might be errors, or bugs, or malfunctions. Please, **help us** to improve our work! You can report any issue or problems you run into using the [Issues](https://github.com/SMAC-Group/ODyN/issues) page of this repository, we will do our best to assist you and correct problems as soon as possible.

# Examples

Example data and configuration is provided in this repository as described in the following (to use these files in ODyN please see the *"Documentation"* section later on). Among other things, this data allows to reproduce some of the results presented in [1,3,4]. 

Most of the data has been gently made available by Dr. Julien Vallet, director of [Helimap Sixense](https://helimap.ch), and described in the following scientific publication:

- Vallet, J., Gressin, A., Clausen, P., and Skaloud, J. "Airborne And Mobile Lidar, Which Sensors For Which Application?". *The International Archives of Photogrammetry, Remote Sensing and Spatial Information Sciences*, 43 (2020): 397-405.


### Configuration

All data files can be used with the same configuration file available at [data/configuration.RData](https://github.com/SMAC-Group/ODyN/raw/master/data/configuration.zip)

### Inertial Navigation

Data file: [data/INS.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/INS.zip)

### Inertial Navigation + LiDAR point-to-point correspondences

Data file: [data/INS+lidar.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/INS+lidar.zip)

### Inertial Navigation + image tie-points (photogrammetry)

Data file: [data/INS+photo.zip](https://github.com/SMAC-Group/ODyN/raw/master/data/INS+photo.zip)

# Documentation

The use of `ODyN` is straightforward according to the following steps: 

1. Upload a `.zip` file containing all sensor measurements (see section 'Input file formats' below).
2. Verify that the data files are loaded correctly (check the messages below the file upload input).
3. [Optional] Upload an `.RData`file containing the configuration of all parameters (see *Step 8* below).
4. Enter all required configuration parameter via GUI according to your setup (or in case of *Step 3* verify that these were loaded and interpretted correctly).
5. Hit the **Process** button and wait. Depending on the input, few minutes may be required.
6. If everything has gone well, check the results in the provided plots. Otherwise go back to *Step 4* and fix the problem.
7. Download the solution file.
8. [Optional] Download the configuration to be reused later on in *Step 3*.
9. [Optional] Generate a link to share the output on-line.

## Input file formats

All input files should be included in a `.zip` archive. This file should be then uploaded in the **Inputs** section of the interface (*Step 1* above). The archive should contain the following files, format of which is specified further below with sections 'File xxx.xxx', that is preceeded with a short overview of file contents related to *Navigation* (mandatory) and *Navigation + Lidar/Photogrammetry* (optional).  

>**Warning**: The file names *must match exactly*!

### Navigation

- `GPS.txt`: position measurements from a GNSS receiver,
- `IMU.txt`: raw specific force and angular velocity measurements from an Inertial Measurement Unit (IMU),
- `initial_guess.txt` **[optional]**: an initial trajectory solution that will be used to initialize the DN solver, 
- `config.Rdata` **[optional]**: configuration file obtained from previous `ODyN` execution (*applicable also to optical sensors below*)

The detailed description of the format of those files is further below. If the `initial_guess.txt` file  is not provided, `ODyN` will attempt to determine the initialization for the DN solver applying a [Savitzky–Golay](https://en.wikipedia.org/wiki/Savitzky%E2%80%93Golay_filter) filter to the provided GNSS positions to obtain an approximation of the body frame position at the frequency of the IMU. The initial orientation is derived assuming that the body frame mounting is either Front-Left-Up or Front-Right-Down and the x-axis is tangent to the body frame trajectory. For this approach to work, certain velocity of the body frame is required with the velocity vector being principaly along the x-axis in the body frame. 

>Note: IMU and GPS files are *always* required. `ODyN` uses  WGS84 normal gravity model. More detailed Earth Gravity Model(s) (e.g., EMG96) maybe implemented in the future. 

### Navigation + Photogrammetry

In case the user wants to fuse also image observations, the following additional files should be included into the archive:

- `bingo.txt`: image observations,
- `image_timestamps.txt`: exposure times for the images included in `bingo.txt`,
- `GCPs.txt` **[optional]**: 3D coordinates of Ground Control Points/Checkpoints

The detailed description of the format of those files is given below.

### Navigation + Lidar

In case the user wants to fuse also lidar point-to-point spatial constraints, the following additional file should be included in the archive:

- `lidar_tp.txt`: lidar observations of tie-points.   

The detailed description of the format of this file is given below.

### File `GPS.txt`

This file contains a sequence of position observations obtained from a GNSS receiver. It is a Coma Separated Value (CSV) file with **four** or **[opional] seven** columns and no header.

- Column 1: epoch time, unit *seconds*,
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meter*,
- ~~Column 5 - 7: **[optional]** incertitudes (*1-sigma*) in east-north-up directions per epoch, unit: *meter*.~~ (not yet implemented)  

An example of the content of this file (~~without incertitudes~~) is given below:

```
396400.000, 46.569360752778, 6.533852830556, 609.752100000000
396401.000, 46.569583791667, 6.533973002778, 607.718300000000
396402.000, 46.569719905556, 6.534210213889, 606.953900000000
396403.000, 46.569749733333, 6.534465997222, 607.169700000000
396404.000, 46.569701600000, 6.534688980556, 606.820900000000
```
If an `initial_guess.txt` file is provided, `ODyN` tolerates well GNSS outages. This also holds if such file is not provided, but complex maneuvers during GNSS outages can prevent the solver to converge.

### File `IMU.txt`

This file contains a sequence of raw specific force and angular velocity observations obtained from an IMU. It is a CSV file with **seven** columns and no header.

- Column 1: measurement timestamp, unit *seconds*, 
- Column 2 - 4: angular velocity, unit *rad/s*,
- Column 5 - 7: specific force, unit  *m/s<sup>2</sup>*.

The `IMU.txt` files sets the time limits for the processing, meaning that no solution can be computed for timestamps before the first or after the last entry in the file. 

The IMU observations must be synchronized with the GPS or GNSS receiver, in the sense that the measurement timestamps reported for the IMU readings should be in GPS time. If this is not the case, the two sensors can not be properly fused together by `ODyN`. Small jitters or delays can be tolerated, but a reduction of the performance (that is difficult to quantify) should be expected. An external solution for IMU sensor time-stamping and synchronization is to use the [SentiBoard](https://sentisystems.com/sales-2/).

Additionally, the IMU must have a constant rate and no samples can be missing. `ODyN` determines the nominal frequency of the IMU taking the difference between the timestamps of the first two measurements. The user should also make sure that a sufficient number of digits is employed to represent IMU readings in the CSV file.

An example of the content of this file for a 500 Hz IMU is given below:

```
396401.000, 0.016107580, 0.300693593, 0.649536309, -0.046519610, 7.331238752, -15.765452987
396401.002, 0.025510935, 0.288168501, 0.649539582, -0.007448115, 7.155350738, -16.547551439
396401.004, 0.006704225, 0.300678700, 0.640177259, -0.144198348, 7.546249986, -15.609187826
396401.006, 0.022376484, 0.297568526, 0.640160221,  0.070694876, 7.878268971, -15.628652537
396401.008, 0.034914290, 0.300723380, 0.640139446, -0.026983862, 7.038125840, -16.567082796
```

### File `initial_guess.txt` [optional]

This file contains an initial solution for the body frame position and orientation that is used to initialize the Dynamic Network solver. It is a CSV file with **eight** columns and no header.
- Column 1: timestamp, unit *seconds*,
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meter*,
- Column 5 - 8: a quaternion representing R<sup>*n*</sup><sub>*b*</sub>, where *n* is a local level frame, unit: *N/A* 

A row in the `initial_guess.txt` file should be present for each IMU measurement in the `IMU.txt` file. More rows are allowed, but not less.
An example of the content of this file is given below:

```
396401.000, -1905.483030452, 1448.567031339, 605.999008116, -0.218175913, 0.909282389, 0.338522248, -0.104916617
396401.020, -1905.182801199, 1448.991561633, 605.969088974, -0.218858188, 0.911660293, 0.331996401, -0.103706282
396401.040, -1904.878848204, 1449.412015129, 605.939719655, -0.219702240, 0.913903930, 0.325544215, -0.102623076
396401.060, -1904.571240963, 1449.828357413, 605.910750385, -0.220010575, 0.916213164, 0.319082538, -0.101661786
396401.080, -1904.259969614, 1450.240503639, 605.882340958, -0.220546280, 0.918408515, 0.312620830, -0.100763856
```

### File `bingo.txt`

This is a file format of image observations as used within BINGO adjustment software that is supported also by modern photogrammetric suites (e.g., Pix4D, AgiSoft). However, in contrary to BINGO, the unites of image coordinates are in *pixels*!

The input of the photo measures is effected with free format. The first line contains photo and camera number. If the camera number is missing, the camera number of the preceding photo is used. The default value for the camera number of the first photo is 1. Optionally, two dummy coordinate values between photo and camera number may be given. A negative point number (for example -99) indicates end of data for each photo. Then the next photo follows. End of data is indicated by an **END** statement of the last line in the file!


| First photo | photo no. camera no. |
| --- | ----------- |
| Image obs. | point no. x' y' |
|            | point no. x' y' |
|            | : |
|            | point no. x' y' |
| Last line  | -99 |
| Next photo | photo no. (camera no.) |
| Image obs. | point no. x' y' |
|            | point no. x' y' |
|            | : |
|            | point no. x' y' |
| Last line  | -99 |
| :  | : |
|    | END |

> units: *pixels*.  

An example of the content of this file for two photos is given below: 

```
1   1
11 -5.924 -0.277
12 -5.933 0.290
13 -5.942 0.868
14 -5.951 1.457
15 27.227 -6.921
16 28.036 -5.513
17 28.886 -4.032
18 29.782 -2.473
–99
2
11 -8.289 0.204
12 -8.294 0.658
13 -8.299 1.118
14 -8.303 1.583
15 9.802 -4.320
16 10.054 -3.229
17 10.314 -2.106
18 10.582 -0.949
-99
END
```
### File `image_timestamps.txt`

Photo ID with exposure time (1 per line) for all images included in `bingo.txt` in a chronological order.
Time unis needs to be the same as for IMU.txt and GPS.txt. 

- Column 1: photo no., unit *ID*, 
- Column 2: timestamp, unit *seconds*,

An example of the content of this file is given below: 

```
1 396774.806328
2 306776.213498
```

### File `GCPs.txt`

Ground control (or check-points) point coordinates

- Column 1: gcp_no, unit *ID*
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, units: *2x decimal degrees* and *meter*,


### File `lidar_tp.txt` 

This file contains the spatial conditions between two points observed by lidar from different parts of the trajectory. These tie-points are expressed by their respective time and coordinates in the *scanner frame* in a CSV file with **eight** columns and no header.

- Column 1 - 2: time stamps in *seconds* for the first and second tie-points, respectively
- Column 3 - 5: x-y-z coordinates of the **first** tie-point in the scanner frame, units: *meters* 
- Column 6 - 8: x-y-z coordinates of the **second** tie-point in the scanner frame, units: *meters* 
- ~~Column     9:**[optional]** incertitude (*1-sigma*) in the spatial proximity (Euclidean distance) both points, unit: *meter*~~ (not yet implemented)

``` 
396765.649713,396660.533676,271.420000,-2.570000,18.060000,263.280000,-2.520000,-135.520000
396767.558842,396659.053466,263.810000,-2.520000,-0.480000,259.870000,-2.470000,-126.380000
396770.806328,396656.979838,250.690000,-2.390000,-76.950000,257.880000,-2.460000,-38.400000
396775.153753,396651.601487,272.190000,-2.600000,-21.910000,278.930000,-2.650000,-110.880000
``` 

