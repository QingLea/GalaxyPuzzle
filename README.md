# Galaxy Distribution Analysis using CUDA

## Overview
This project involves the design, implementation, and execution of a CUDA-based program to analyze the 2-point angular correlation function of galaxies. The objective is to determine if the observed distribution of galaxies statistically differs from a random distribution by comparing histograms of galaxy pair angles.

## Files and Structure

The project includes the following CUDA source files:

1. **galaxy_shared_mem.cu**: Implements the galaxy distribution analysis using shared memory for optimization.
2. **galaxy_no_cache.cu**: Implements the analysis without utilizing cache for memory optimization.
3. **galaxy_manual.cu**: A manual approach to analyzing the galaxy distribution without specific memory optimization techniques.
4. **template.cu**: A template file setting up the basic environment and defining common functions for the analysis.

## Requirements

- NVIDIA CUDA toolkit
- A GPU-enabled machine
- Access to the real and synthetic galaxy data (available on Moodle)


## Compilation and Execution
To compile and run the CUDA programs, use the following commands:
```bash
nvcc galaxy_shared_mem.cu -o galaxy_shared_mem
./galaxy_shared_mem real.dat synthetic.dat output.dat

nvcc galaxy_no_cache.cu -o galaxy_no_cache
./galaxy_no_cache real.dat synthetic.dat output.dat

nvcc galaxy_manual.cu -o galaxy_manual
./galaxy_manual real.dat synthetic.dat output.dat
```
## Project Description

### Input Data

The input consists of two lists of 100,000 galaxy locations: real measured galaxies and synthetic evenly distributed random galaxies. Each list contains the galactic coordinates in the following order:

- **Right Ascension (a)**: in arc minutes
- **Declination (d)**: in arc minutes

These coordinates should be converted to radians by multiplying with $( \frac{\pi}{10800} )$.

### Objective

The main task is to compute three histograms representing the 2-point angular correlation function:

- **DD**: Histogram of angles between pairs of real galaxies.
- **DR**: Histogram of angles between pairs of real and random galaxies.
- **RR**: Histogram of angles between pairs of random galaxies.

These histograms cover angles from 0 to 180 degrees with a bin width of 0.25 degrees.

### Calculation of Angles

To calculate the angle $( \theta_{12} )$ between two points on a sphere, use the formula:

$\theta_{12} = \arccos(\sin(d1)\sin(d2) + \cos(d1)\cos(d2)\cos(a1 - a2)) $

Where:
- $( a )$ is the right ascension converted to radians
- $( d )$ is the declination converted to radians

### Statistical Measure

The scientific measure to determine differences between the distributions is:

$[ w_i(\theta) = \frac{DD_i - 2DR_i + RR_i}{RR_i} ]$

Where $( DDi, DRi, RRi )$ are the values in histogram bin $( i )$.

If $( w_i )$ values are close to zero (in the range [-0.5, 0.5]), the distribution of real galaxies is approximately random. If $(w_i)$ values are significantly different from zero, the distribution is non-random.

## Implementation Details

### Threads and Thread Blocks

- Design threads and thread blocks to efficiently compute pairwise angles.
- Experiment with thread block sizes and workload distribution.
- Synchronize threads as needed and consider atomic operations for histogram updates.

### Output Data

- Compute and store the histograms DD, DR, and RR.
- Plot the histograms to visualize the differences between distributions.
- Compute and analyze the $w_i(\theta)$ values.
