# ARPA-E PERFORM datasets produced by NREL

[Overview of the ARPA-E PERFORM project](#Overview-of-the-ARPA-E-PERFORM-project)  
[Dataset description](#Dataset-description)  
[Dataset access](#Dataset-access)  
[Directory structure](#Directory-structure)  
[File format](#File-format)  
[Additional resources](#Additional-resources)   
[Recommended citation](#Recommended-citation)  

## Overview of the ARPA-E PERFORM project

The PERFORM Program is an [ARPA-E](https://arpa-e.energy.gov) funded program that aims to enhance that ability to model and plan for risk in grid systems:

> PERFORM seeks to develop innovative management systems
> that represent the relative delivery risk of each asset and balance the
> collective risk of all assets across the grid. A risk-driven paradigm allows
> operators to: (i) fully understand the true likelihood of maintaining a
> supply-demand balance and system reliability, (ii) optimally manage the system,
> and (iii) assess the true value of essential reliability services. This
> paradigm shift is critical for all power systems and is essential for grids
> with high levels of stochastic resources. Projects will propose methods to
> quantify and manage risk at the asset level and at the system level. 

For more details on PERFORM, see the [ARPA-E PERFORM website](https://arpa-e.energy.gov/technologies/programs/perform).

In support of the ARPA-E PERFORM project, NREL has produced a set of
time-coincident load, wind, and solar generation profiles, including actual and
forecasts time series. Both actuals and forecasts are provided in form of
time-series with high temporal and spatial fidelity. Both deterministic and
probabilistic forecasts are contained in the dataset.

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## Dataset description

The NREL datasets developed to support PERFORM include the following:

- [Actuals](#Actuals)
  - [Solar and wind actuals](#Solar-and-wind-actuals)
  - [Load actuals](#Load-actuals)
- [Forecasts](#Forecasts)
  - [Solar and wind forecasts](#Solar-and-wind-forecasts)
  - [Load forecasts](#Load-forecasts)

Each dataset is provided for the following U.S. Indendepent System Operator (ISO) territories:
- The Electric Reliability Council of Texas (ERCOT) 
- The New York Independent System Operator (NYISO) 
- The Midcontinent Independent System Operator (MISO)
- The Southwest Power Pool (SPP)

Data for ERCOT is available for 2017-2018, whereas data for the other three ISOs is available for 2018-2019. Note that forecast data is only available for the second year of each set due to the requirement to have a year of data to train the forecasts. 

Please be aware that all timeseries data is published in UTC. 

Additional data on the methods for generating each of the components of the data sets can be found in the following sections.

### Actuals

Actuals data are provided with a 5-min resolution for two years at the
site-level (only for solar and wind), zone-level, and system-level.

#### Solar and wind actuals

Actual solar generation data are generated based on the National Solar
Radiation Database (NSRDB) at the site-level, the zone-level, and the
system-level, respectively. Solar power time series is simulated by the NREL's
System Advisor Model (SAM) with meteorological data and solar plant configuration at
the site-level. Zone-level and system-level actual time series are then
obtained by aggregating site-level data accordingly.

Meteorological wind resource data is generated using the Weather Research and
Forecasting model (WRF). This data is then used to produce actuals wind
generation data using the methodology discussed above for solar actuals.

#### Load actuals

Load data for each ISO's load zone is collected from each of the ISO websites. In the case of SPP and MISO, only hourly load data is publically available. In those cases, we developed a load-downscaling method to generator synthetic 5-min load data and applied it to the available hourly load. 

### Forecasts

Forecast data are provided at three temporal scales with different
operational characterstics. Day-ahead forecasts are generated with a
11-hour-ahead lead time, a 48-hour horizon, an hourly resolution, and a daily
update rate. Intra-day forecasts are generated with a 6-hour-ahead lead time,
a 6-hour horizon, an hourly resolution, and a 6-hour update rate. Intra-hour
forecasts are generated with a 1-hour-ahead lead time, a 2-hour horizon, a
15-minute resolution, and a hourly update rate. The figure below provides an overview
of how lead time, horizon, resolution, and update rate combine to describe the temporal
aspects of a forecast.

| ![MicrosoftTeams-image (2)](https://user-images.githubusercontent.com/6035236/191108128-08fd7ef8-18d1-4638-b815-e4a3c2ffaf77.png) |
|:--:|
| Image source: Doubleday, Van Scyoc Hernandez, and Hodge, "Benchmark probabilistic solar forecasts: Characteristics and recommendations", <i>Solar Energy</i> (206) 2020. |


Probabilistic forecasts are in the form of 1-99 percentiles.

#### Solar and wind forecasts

Day-ahead and intra-day forecasts rely on the European Centre for
Medium-Range Weather Forecasts (ECMWF) output that consists of deterministic
forecasts from 51 ensemble members. The Bayesian Model Averaging is used to generate
probabilistic forecasts on top of the deterministic forecasts from the 51 ECMWF
members. Intra-hour forecasting relies on the Machine Learning-based
Multi-Model (M3) and the historical synthetic actual data.

#### Load forecasts

Load forecasting at three time-scales are generated by the deep learning
ensemble. Recurrent neural network, convolutional neural network, and extreme
gradient boosting are used to generate deterministic forecasts, which are
converted to probabilistic forecasts by the adaptive Gaussian model.

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## Dataset access

The ARPA-E PERFORM data is made available as a series of .h5 files and can be
found on AWS at `s3://arpa-e-perform/`. The AWS registry for the data is located at [https://registry.opendata.aws/arpa-e-perform/](https://registry.opendata.aws/arpa-e-perform/).

Information on the dataset can also be viewed via its Open Energy Data Initiative (OEDI) catalog page at [https://data.openei.org/submissions/5772](https://data.openei.org/submissions/5772).

Examples for accessing AWS S3 data via python can be found in the [ERCOT_demo.md](ERCOT_demo.md).

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## Directory structure

Data for each ISO is organized according to the following structure:

- {ISO}/
  - MetaData/
    - solar_meta.xlxs
    - wind_meta.xlxs
    - load_meta.xlxs
  - {YEAR}/
    - Solar/
      - Actuals/
        - Site_level/{SITE}\_solar_actuals_{YEAR}.h5
        - BA_level/{BA}\_solar_actuals_{YEAR}.h5
        - Zone_level/{ZONE}\_solar_actuals_{YEAR}.h5
      - Forecasts\*
        - Day-ahead
          - Site_level/{SITE}\_solar_day-ahead_fcst_{YEAR}.h5
          - BA_level/{BA}\_solar_day-ahead_fcst_{YEAR}.h5
          - Zone_level/{Zone}\_solar_day-ahead_fcst_{YEAR}.h5
        - Intra-day
          - Site_level/{SITE}\_solar_intra-day_fcst_{YEAR}.h5
          - BA_level/{BA}\_solar_intra-day_fcst_{YEAR}.h5
          - Zone_level/{Zone}\_solar_intra-day_fcst_{YEAR}.h5
        - Intra-hour
          - Site_level/{SITE}\_solar_intra-hour_fcst_{YEAR}.h5
          - BA_level/{BA}\_solar_intra-hour_fcst_{YEAR}.h5
          - Zone_level/{Zone}\_solar_intra-hour_fcst_{YEAR}.h5
    - Wind/
      - Actuals/
        - Site_level/{SITE}\_wind_actuals_{YEAR}.h5
        - BA_level/{BA}\_wind_actuals_{YEAR}.h5
        - Zone_level/{Zone}\_wind_actuals_{YEAR}.h5
      - Forecasts\*
        - Day-ahead
          - Site_level/{SITE}\_wind_day-ahead_fcst_{YEAR}.h5
          - BA_level/{BA}\_wind_day-ahead_fcst_{YEAR}.h5
          - Zone_level/{Zone}\_wind_day-ahead_fcst_{YEAR}.h5
        - Intra-day
          - Site_level/{SITE}\_wind_intra-day_fcst_{YEAR}.h5
          - BA_level/{BA}\_wind_intra-day_fcst_{YEAR}.h5
          - Zone_level/{Zone}\_wind_intra-day_fcst_{YEAR}.h5
        - Intra-hour
          - Site_level/{SITE}\_wind_intra-hour_fcst_{YEAR}.h5
          - BA_level/{BA}\_wind_intra-hour_fcst_{YEAR}.h5
          - Zone_level/{ZONE}\_wind_intra-hour_fcst_{YEAR}.h5
    - ECMWF/
      - Control/
        - Day-ahead/
        - Intra-day/
    - Load/
      - Actuals/
        - BA_level/{BA}\_load_actuals_{YEAR}.h5
        - Zone_level/{ZONE}\_load_actuals_{YEAR}.h5
      - Forecasts\*
        - Day-ahead
          - BA_level/{BA}\_load_day-ahead_fcst_{YEAR}.h5
          - Zone_level/{ZONE}\_load_day-ahead_fcst_{YEAR}.h5
        - Intra-day
          - BA_level/{BA}\_load_intra-day_fcst_{YEAR}.h5
          - Zone_level/{ZONE}\_load_intra-day_fcst_{YEAR}.h5
        - Intra-hour
          - BA_level/{BA}\_load_intra-hour_fcst_{YEAR}.h5
          - Zone_level/{ZONE}\_load_intra-hour_fcst_{YEAR}.h5

where ISO={ERCOT,MISO,NYISO,SPP}, YEAR={2017,2018,2019} (ISO dependent), and SITE,BA,and ZONE represent the various spatial levels of the data.

\* Note that forecast data is only available for 1 of the 2 years (2018 for ERCOT and 2019 for MISO, NYISO, and SPP).
  
([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## File format

The data is provided in high density data file format HDF5 (.h5). The files
contain the following datasets with following shapes:

### Actuals
  - actuals (time-steps, sites)
  - meta (sites, )
  - time_index (time-steps, )

### Forecasts
  - forecasts (time-steps, 99 # percentiles)
  - meta (1,) # forecasts are provided by size, zone, or BA
  - issue_time (time-steps, )
  - forecast_time (time-steps, )

Examples for working with h5 file format can be found in the [ERCOT_demo.md](ERCOT_demo.md).

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## Additional resources

The following technical reports provide additional details on the datasets, including methods, validation, and error metrics:
- Phase I dataset (ERCOT): https://www.nrel.gov/docs/fy23osti/79498.pdf.
- Phase II datasets (MISO, NYISO, and SPP): https://www.nrel.gov/docs/fy24osti/83828.pdf. 

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))

## Recommended citation

> Sergi, Brian, Feng, Cong, Zhang, Flora, Hodge, Bri-Mathias, Ring-Jarvi, Ross, Bryce, Richard, Doubleday, Kate, Rose, Megan, Buster, Grant, and Rossol, > Michael. 2022. "ARPA-E PERFORM datasets". United States. https://dx.doi.org/10.25984/1891136. https://data.openei.org/submissions/5772.

([return to top](#ARPA-E-PERFORM-datasets-produced-by-NREL))
