Version 1.0 (synced with AWS 2022-08-18)
- Added planned offshore wind facilities to NYISO
- Small adjustment to lat/long of some planned sites to ensure future plants are not in bodies of water
- Removed 2 solar plants from SPP
- Correct small issues with the dataset (e.g., NAs appeared in some timeseries)

Version 1.0.1 (synced with AWS 2022-09-21)
- Small change to remove commas from plant names (e.g., Site_Adams_Wind_Generations,_LLC_wind_2day-ahead_2019.h5 --> Site_Adams_Wind_Generations_LLC_wind_2day-ahead_2019.h5) as the commas were causing file read issues (see [#Issue 17](https://github.com/PERFORM-Forecasts/documentation/issues/17)). Note that this may require care when matching with plant names in the metadata.
