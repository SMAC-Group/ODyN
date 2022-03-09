# ODyN, an Online Dynamic Network solver

This repository hosts `ODyN`, a free Dynamic Network (DN) solver that runs online in the browser. `ODyN` fuses information from cameras, GNSS and inertial sensors in a single adjustment and it can be used to estimate a high-frequency trajectory for precise direct geo-referencing, to improve photogrammetric reconstructions in challenging scenarios or to determine several types of system calibration parameters.

`ODyN` is a [R Shiny](https://shiny.rstudio.com/) application providing an user-friendly Graphical User Interface (GUI) for the [ROAMFREE](https://github.com/AIRLab-POLIMI/ROAMFREE) sensor fusion library, which contains the actual solver for Dynamic Network adjustment problems. The processing of the user-provided data happens on backend servers provided by the [Data Analytics Laboratory](https://data-analytics-lab.netlify.app/), University of Geneva, CH. 

For more details and for the scientific background behind Dynamic Networks and `ODyN`, please refer to

1. Cucci, Davide Antonio, 2022. ODyN: an online dynamic network solver for photogrammetry and LiDAR geo-referencing. ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences (to appear)
2. Cucci, Davide Antonio, Martin Rehak, and Jan Skaloud. "*Bundle adjustment with raw inertial observations in UAV applications.*" ISPRS Journal of photogrammetry and remote sensing 130 (2017): 1-12. [PDF](https://drive.google.com/file/d/1U2RKh7T98bFJYvnMGHIOrxKyIRVDP0Oi/view?usp=sharing)

`ODyN` is available for free, it can be used for free in any context, but it comes without any guarantee :)

# Access

`ODyN` is available [ --> here <--](http://129.194.40.13:3839/app/dn_gui).

**N.B. ODyN is in active state of development**. There might be errors, or bugs, or malfunctions. Please, **help us** to improve our work! You can report any issue or problems you run into using the [Issues](https://github.com/SMAC-Group/ODyN/issues) page of this repository, we will do our best to assist you and correct problems as soon as possible.

To reproduce the results presented in [1], one can use the following data files and configuration:
- Data:
- Configuration:

# Documentation

The use of `ODyN` is straightforward.

1. Upload a `.zip` file containing all sensor measurements (see section 'Input file formats' below).
2. Verify that the data files are loaded correctly (check the messages below the file upload input).
2. [Optional] Upload an `.RData`file containing the configuration of all parameters (see point 8 below).
3. Provide all required configuration parameter according to your setup.
4. Hit the **Process** button and wait. Depending on the input, few minutes may be required.
6. If everything has gone well, check the results in the provided plots. Otherwise go back to 4 and fix the problem.
7. Download the solution file
8. [Optional] Download the configuration to be reused later on.
9. [Optional] Generate a shareable link to the output.

## Input file formats

All input files should be included in a `.zip` archive. This file should be then uploaded in the **Inputs** section of the interface. The archive should contain the following files (the file name must match exactly)

### Inertial navigation

- `GPS.txt`: position measurements from a GNSS receiver,
- `IMU.txt`: raw specific force and angular velocity measurements from an Inertial Measurement Unit (IMU),
- `initial_guess.txt` **[optional]**: an initial solution that will be used to initialize the DN solver.

If the `initial_guess.txt` file  is not provided, `ODyN` will attempt to determine the initialization for the DN solver applying a [Savitzky–Golay](https://en.wikipedia.org/wiki/Savitzky%E2%80%93Golay_filter) filter to the provided GNSS positions to obtain an approximation of the body frame position at the frequency of the IMU. The initial orientation is derived assuming that the body frame mounting is either Front-Left-Up or Front-Right-Down and the x axis is tangent to the body frame trajectory.

### Inertial navigation + photogrammetry

In case the user wants to fuse also image observations, the following additional files should be included in the archive:

- `bingo.txt`: image observations in bingo format
- `image_timestamps`: exposure times for the images included in `bingo.txt`
- `GCPs.txt` **[optional]**: 3D coordinates of Ground Control Points/Checkpoints

The detailed description of the format of those files is given below.

### File `GPS.txt`

This file contains a sequence of position observations obtained from a GNSS receiver. It is a Coma Separated Value (CSV) file with **four** columns and no header.

- Column 1: epoch time in seconds,
- Column 2 - 4: latitude, longitude and altitude, in WGS-84 ellipsoidal coordinates, expressed in degrees.


An example of the content of this file is given below:

```396400.000, 46.569360752778, 6.533852830556, 609.752100000000
396401.000, 46.569583791667, 6.533973002778, 607.718300000000
396402.000, 46.569719905556, 6.534210213889, 606.953900000000
396403.000, 46.569749733333, 6.534465997222, 607.169700000000
396404.000, 46.569701600000, 6.534688980556, 606.820900000000
```

If an `initial_guess.txt` file is provided, `ODyN` tolerates well GNSS outages. This also holds if such file is not provided, but complex maneuvers during GNSS outages can prevent the solver to converge.

### File `IMU.txt`

This file contains a sequence of raw specific force and angular velocity observations obtained from an IMU. It is a CSV file with **seven** columns and no header.

- Column 1: measurement timestamp,
- Column 2 - 4: angular velocity, in rad/s
- Column 5 - 7: specific force, in m/s<sup>2</sup>

The `IMU.txt` files sets the time limits for the processing, meaning that no solution can be computed for timestamps before the first or after the last entry in the file. 

The IMU must be synchronized with the GNSS receiver, in the sense that the measurement timestamps reported for the IMU readings should be in GPS time. If this is not the case, the two sensors can not be properly fused together by `ODyN`. Small jitters or delays can be tolerated, but a reduction of the performances, difficult to quantify, should be expected. A solution for sensor time-stamping and synchronization is to use the [SentiBoard](https://sentisystems.com/sales-2/).

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
- Column 1: timestamp
- Column 2 - 4: the position of the body frame, in terms of East, North, and Up coordinates with respect to an user-defined local level frame
- Column 5 - 8: a quaternion representing R<sup>*n*</sup><sub>*b*</sub>, where *n* is a local level frame 

A row in the `initial_guess.txt` file should be present for each IMU measurement in the `IMU.txt` file. More rows are allowed, but not less.


`ODyN` assumes that the gravity vector is directed along the Up axis of the local level frame and it is the same at all positions. Thus, the user should pick the local level frame so that it is *not too far* from the trajectory to avoid undesirable effects related to the change of the direction of the gravity vector. Soon, `ODyN` will support inertial navigation in ECEF frame, thus removing the issue.


An example of the content of this file is given below:

```
396401.000, -1905.483030452, 1448.567031339, 605.999008116, -0.218175913, 0.909282389, 0.338522248, -0.104916617
396401.020, -1905.182801199, 1448.991561633, 605.969088974, -0.218858188, 0.911660293, 0.331996401, -0.103706282
396401.040, -1904.878848204, 1449.412015129, 605.939719655, -0.219702240, 0.913903930, 0.325544215, -0.102623076
396401.060, -1904.571240963, 1449.828357413, 605.910750385, -0.220010575, 0.916213164, 0.319082538, -0.101661786
396401.080, -1904.259969614, 1450.240503639, 605.882340958, -0.220546280, 0.918408515, 0.312620830, -0.100763856
```