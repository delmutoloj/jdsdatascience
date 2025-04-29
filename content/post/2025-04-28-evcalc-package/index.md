---
title: "evCalc Package"
author: "James Del Mutolo"
date: "2025-04-28"
slug: evcalc-package
categories: ["R"]
tags: ["evCalc", "Electric Vehicle", "R Package"]
---



The evCalc (Electric Vehicle Calculator) package contains useful functions for analyzing logs from the OBD2 interface of an electric vehicle.

This package can be used to calculate battery capacity, battery degradation, efficiency, and more!

I created this package to streamline some of the calculations that I regularly make, and to gain experience in developing an R package.

The evCalc package can be installed from GitHub using the install_github() function from devtools.

``` r
devtools::install_github("delmutoloj/evCalc")
```

[GitHub Repository](https://github.com/delmutoloj/evCalc)

# Datasets

The evCalc package contains the following datasets...

### Speed Dependent Logs

- **evCalc::log_40mph**
- **evCalc::log_50mph**
- **evCalc::log_60mph**
- **evCalc::log_70mph**
- **evCalc::log_80mph**
- **evCalc::log_90mph**

Separate logs from driving at a constant speed of 40, 50, 60, 70, 80, and 90 mph.

Each speed dependent log is five minutes in length.

Used for comparing efficiency at different speeds.

600 observations and 5 variables
- Time: Log time (seconds)
- Speed: Vehicle speed (miles-per-hour)
- Distance: Trip distance (miles)
- Power: Instantaneous power (kilowatts)
- SoC: Battery state of charge (%)

### evCalc::log_city

Log of a city driving scenario. Lower speeds, lots of stop-and-go.

Used for calculating efficiency.

1263 observations and 5 variables
- Time: Log time (seconds)
- Speed: Vehicle speed (miles-per-hour)
- Distance: Trip distance (miles)
- Power: Instantaneous power (kilowatts)
- SoC: Battery state of charge (%)

### evCalc::log_degradation

Very lengthy log of a nearly full discharge of the battery.

Used to calculate degradation.

19,293 observations and 5 variables
- Time: Log time (seconds)
- Speed: Vehicle speed (miles-per-hour)
- Distance: Trip distance (miles)
- Power: Instantaneous power (kilowatts)
- SoC: Battery state of charge (%)


```
## [1] "Log Duration: 2.68 Hours"
```

```
## [1] "Starting SoC: 100%"
```

```
## [1] "Ending SoC: 4.71%"
```


### evCalc::log_charging

Log of power and temperatures during a DC fast charging session.

Used to create a charging curve.

5,743 observations and 5 variables
- Time: Log time (seconds)
- Power: Instantaneous power (kilowatts)
- coolantTemp: Coolant temperature (Fahrenheit)
- SoC: Battery state of charge (%)
- avgModuleTemp: Average temperature of all battery modules (Fahrenheit)

# Functions

### evCalc::capacity()

The capacity() function estimates the total battery capacity in kilowatt-hours (kWh) based on energy consumption and the percentage of battery used during a logging session.

Example:

``` r
# Estimate battery capacity in kWh based on degradation log.
capacity(evCalc::log_degradation)
```

```
## [1] 62.16143
```

With an advertised factory capacity of 65 kWh, we can see that this pack holds 2.84 kWh less than when it was new.

### evCalc::chargingCurve()

The chargingCurve() function uses ggplot2 to plot a charging curve (x: time (min), y: power (kW)) during a DC fast charging session.

This function still needs some more work. The time displays in seconds instead of minutes, and both axis need labels. I would also like to improve the appearance of the plot by adding some color.

Example:

``` r
# Create a charging curve plot with log_charging
chargingCurve(evCalc::log_charging)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

### evCalc::degradation()

The degradation() function computes the percentage decrease in usable battery capacity compared to its original capacity when new.

This function accepts two arguments, a log output of a full discharge, and the advertised capacity of a new battery pack.

For this function to work accurately, the log output must be as close to a full discharge as possible (preferably 100%-5%).

Example:

``` r
# Calculate percent decrease in usable battery capacity given 65 kWh new.
degradation(evCalc::log_degradation, 65)
```

```
## [1] -4.367035
```

### evCalc::efficiency()

The efficiency() function computes the vehicle’s efficiency in miles per kilowatt-hour (mi/kWh), based on the total distance traveled and energy consumed.

Example:

``` r
# Calculate efficiency in mi/kWh of log_city
efficiency(evCalc::log_city)
```

```
## [1] 5.905292
```

``` r
# Calculate efficiency at different speeds, store results in table
effTable <- data.frame(
  Speed_mph = c(40, 50, 60, 70, 80, 90),
  Efficiency_mi_per_kWh = c(
    efficiency(log_40mph),
    efficiency(log_50mph),
    efficiency(log_60mph),
    efficiency(log_70mph),
    efficiency(log_80mph),
    efficiency(log_90mph)
  )
)

effTable
```

```
##   Speed_mph Efficiency_mi_per_kWh
## 1        40              5.985972
## 2        50              4.501435
## 3        60              4.375991
## 4        70              3.498407
## 5        80              2.667952
## 6        90              2.098121
```

### evCalc::estimatedRange()

The estimatedRange() function computes the estimated driving range (in miles) based on efficiency and projected battery capacity.

This function is more accurate with longer logs.

Example:

``` r
# Calculate estimated range of a full battery in miles given log_degradation
estimatedRange(evCalc::log_degradation)
```

```
## [1] 195.1097
```

### evCalc::kWhUsed()

The kWhUsed() function computes the total energy consumption in kilowatt-hours (kWh) during a logging session.

This function is used as a component in some of the other functions this package.

Example:

``` r
# Calculate energy used in 5 min at different speeds, store results in table
energyTable <- data.frame(
  Speed_mph = c(40, 50, 60, 70, 80, 90),
  Energy_kWh = c(
    kWhUsed(log_40mph),
    kWhUsed(log_50mph),
    kWhUsed(log_60mph),
    kWhUsed(log_70mph),
    kWhUsed(log_80mph),
    kWhUsed(log_90mph)
  )
)

energyTable
```

```
##   Speed_mph Energy_kWh
## 1        40  0.5546301
## 2        50  0.9219282
## 3        60  1.1380278
## 4        70  1.6636143
## 5        80  2.4925486
## 6        90  3.5507955
```

# Improvements

Overall, I think the evCalc is at a good starting point, but there are still many improvements that I want to make.

There are the mentioned improvements to the chargingCurve() function. The package also needs much more documentation and examples within. 
