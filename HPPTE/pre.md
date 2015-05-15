# Overview

## Abstract

### Abstract
- Describes a pedestrian population trend estimation method using location data of smartphone users.
- Intended to be an alternative to traffic censuses using tally counters.
- Using smartphone users’ location data accumulated on Yahoo! Japan.
- Tackles the problem of data shortage when a target area is a small region by using a Gaussian kernel.

## Background

### Background
- Knowing the number of pedestrians in a time or place can be an essential data source for market research.
- Triditional method: Traffic censuses using tally counters are still commonly used.
	1) Requires many survey crews and much time.
	2) Temporal and spatial limitation.
- Location-based services (LBSs) are widely used by smartphone users
- two problems remain
	1) How to pick out only pedestrians.
	2) How to set the research area and period.

# Data Set

### Data Set
- The location data are provided by smartphone users through services of Yahoo! Japan, contains:
	- Latitude
	- Longitude
	- Horizontal accuracy
	- Time stamp (at a second rate)
	- Anonymized user ID (changes after 24 hours)

### Data Set
\begin{figure}
\includegraphics[width = 10cm]{pic1.png}\\
Table: Examples of the location data
\end{figure}

### Data Set
\begin{figure}
\includegraphics[height = 4cm]{pic2.png}
\includegraphics[height = 4cm]{pic3.png}
\end{figure}

# Method

## Simple approach

### A simple approach
- Assumes that the number of records in an area is proportional to the people present in the area.
	+ Determine target areas(polygonal) and target days
	+ Number of records existing in the area hourly is counted and then multiplied by a proper factor.
	+ The date has errors more than 300 meters are eliminated.

### A simple approach
\begin{figure}
\includegraphics[height = 4cm]{pic4.png}
\includegraphics[height = 4cm]{pic5.png}
\end{figure}

### Limitation of the simple approach
- Non-pedestrian records
	- stationary users (e.g. in offices)
	- passing users (e.g. on trains)
- Time continuous estimation
- Sparsity with the smallness of target areas.

## Improvement method

### Eliminating non-pedestrian data
\begin{figure}
\includegraphics[height = 4cm]{pic6.png} 
\end{figure}
- Data are extracted user-by-user(by user id).
- Approximate mean velocity is estimated an hour before or after a person visits the target area.
- The records where velocities both before and after are high or near zero are eliminated.