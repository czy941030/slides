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

### Realizing time-continuous estimation
- Use Poisson distribution to represent counting data
$$p(m|\lambda) = \frac{1}{m!}\lambda^{m}e^{-\lambda}$$
- Consider $\lambda$ as a function of time $\lambda(t)$
$$p(m|\lambda(\cdot)) =  \frac{1}{m!}E[m]^{m}e^{-E[m]}$$
$$E[m] = \int_{t_1}^{t_2}\lambda(t)\mathrm{d}t$$

### Realizing time-continuous estimation
- Decomposed $\lambda(t)$ into $f(t)$ and $\lambda_0$
$$\lambda_0 = \int_0^T \lambda(t) \mathrm{d} t\;\;\;\;\; \lambda(t) = \lambda_0 f(t) \;\;\;\; \int_0^T f(t) \mathrm{d}t = 1$$
- $f(t)$ is estimated using Gaussian kernel density estimation
$$f(t) = \frac{1}{h_t n} \sum_{i=1}^n k(\frac{t-t_i}{h_t})\;\;\;\;\;k(t) = \frac{1}{ \sqrt{2\pi} }e^{-\frac{1}{2}t^2}$$

### Realizing time-continuous estimation
\begin{figure}
\includegraphics[width = 8cm]{pic7.png}
\end{figure}

### Dealing with data sparsity in space
- Data’s weight $w_i = 1$ if the data are in the target area
- If the data are near the target area
$$w_i = \frac{1}{ \sqrt{2\pi} }e^{-\frac{1}{2}(d/h_d)^2}$$
- d is distance between data point and target area centroid
- $f(t)$ changed into
$$f(t) = \frac{1}{h_t\sum w_i}\sum_{i=1}^{n}w_i k(\frac{t-t_i}{h_t})$$

### Parameter selection
- Traffic censuses using tally counters were conducted for this research.
- The parameters to be determined are $K$, means how many pedestrians are represented by a single data point.
$$\hat{K_p}=\frac{M_p}{(\sum_{i=1}^n w_i)(\int_{t_1}^{t_2}f(t)\mathrm{d}t)}$$
- $t_1$ and $t_2$ are the census start and end times
- $M_p$ is total pedestrian number according to traffic census data of the day.

# Experiment

## Reference data

### Reference data
- Traffic censuses using tally counters were conducted in five areas on 11 days in Japan in September 2013
\begin{figure}
\includegraphics[height = 4cm]{pic8.png}
\includegraphics[height = 4cm]{pic9.png}
\end{figure}

### Reference data
\begin{figure}
\includegraphics[width = 10cm]{pic10.png}\\
Table: Date and time of traffic census using tally counters.\\
All censuses were conducted in 2013.
\end{figure}

### Reference data
\begin{figure}
\includegraphics[width = 9cm]{pic11.png}\\
Reference data manually obtained by \\
traffic census using tally counters
\end{figure}

### Reference data
\begin{figure}
\includegraphics[width = 9cm]{pic12.png}\\
Reference data manually obtained by \\
traffic census using tally counters
\end{figure}


## Evaluation metrics

### Evaluation metrics
1. Mean squared error of hourly estimation
$$metric\; 1 = \frac{1}{t_2-t_1}\sum_{t=t_1}^{t_2-1}(\hat{m_t}-m_t)^2$$
2. Absolute error of peak time
$$metric\; 2 = |arg_t\, max(\hat{m_t}) - arg_t\, max(m_t)|$$
3. Absolute error of daily total number
$$metric\; 3 = |\sum_{t=t_1}^{t_2}\hat{m_t}-\sum_{t=t_1}^{t_2}m_t|$$

## Comparative approach

### Comparative approach
- \textbf{Approach 1} is the simple approach. This is a method for counting the number of location data in a target area and hour and multiplying K.
- \textbf{Approach 2} is almost the same as the proposed approach using time continuous method and data supplement using a spatial kernel approach, but a data elimination step is not applied. 

## Result

### Result
\begin{figure}
\includegraphics[width = 9cm]{pic13.png}\\
\includegraphics[width = 9cm]{pic14.png}
\end{figure}

### Result
\begin{figure}
\includegraphics[width = 9cm]{pic15.png}
\end{figure}

### Result
\begin{figure}
\includegraphics[width = 9cm]{pic16.png}
\end{figure}

# Conclusion

### Conclusion
- Location data from smartphone applications were studied for monitoring the number of pedestrians.
- Intended to be an alternative to traffic censuses using tally counters.
- Has advantages in the cost and flexibility compared to manual censuses

### Conclusion
- Assume that the number of location data in an area is proportional to the population volume
- To estimate numbers of only pedestrians
- Deals with temporal and spatial sparsity of data by introducing non-homogeneous Poisson processes and Gaussian kernels.