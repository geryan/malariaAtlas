---
output: md_document
---

[![Build Status](https://travis-ci.org/malaria-atlas-project/malariaAtlas.svg)](https://travis-ci.org/malaria-atlas-project/malariaAtlas)
[![codecov.io](https://codecov.io/gh/malaria-atlas-project/malariaAtlas/coverage.svg?branch=master)](https://codecov.io/gh/malaria-atlas-project/malariaAtlas?branch=master)




# malariaAtlas 
### An R interface to open-access malaria data, hosted by the Malaria Atlas Project. 

# Overview 

This package allows you to download parasite rate data (*Plasmodium falciparum* and *P. vivax*) and modelled raster outputs from the [Malaria Atlas Project](https://map.ox.ac.uk/).

## Available Data: 
The data can be explored at [https://map.ox.ac.uk/explorer/#/explorer](https://map.ox.ac.uk/explorer/#/explorer).



### list* Functions


`listData()` retrieves a list of available data to download. 

Use: 

* listData(datatype = "points") OR listPoints() to see for which countries PR survey point data can be downloaded.

* use listData(datatype = "rasters") OR listRaster() to see rasters available to download. 

* use listData(datatype = "shape") OR listShp() to see shapefiles available to download. 


```r
listData(datatype = "points")
```

```r
listData(datatype = "raster")
```

```r
listData(datatype = "shape")
```

### is_available

`isAvailable` confirms whether or not PR survey point data is available to download for a specified country. 

Check whether PR data is available for Madagascar:

```r
isAvailable(country = "Madagascar")
```

```
## Confirming availability of PR data for: Madagascar...
```

```
## PR points are available for Madagascar.
```

Check whether PR data is available for the United States of America

```r
isAvailable(ISO = "USA")
```

```
## Confirming availability of PR data for: USA...
```

```
## Error in isAvailable(ISO = "USA"): Specified location not found, see below comments: 
##  
## Data not found for 'USA', did you mean UGA OR SAU?
```


## Downloading & Visualising Data: 
### get* functions & autoplot methods

### Parasite Rate Survey Points
`getPR()` downloads all publicly available PR data points for a specified country and plasmodium species (Pf, Pv or BOTH) and returns this as a dataframe with the following format: 



```r
MDG_pr_data <- getPR(country = "Madagascar", species = "both")
```

```
## Observations: 1,077
## Variables: 28
## $ dhs_id                    <fct> , , , , , , , , , , , , , , , , , , ...
## $ site_id                   <int> 6221, 6021, 15070, 15795, 7374, 1309...
## $ site_name                 <fct> Andranomasina, Andasibe, Ambohimarin...
## $ latitude                  <dbl> -18.7170, -19.8340, -18.7340, -19.76...
## $ longitude                 <dbl> 47.4660, 47.8500, 47.2520, 46.6870, ...
## $ rural_urban               <fct> , , , , , , , , , , rural, , , , , r...
## $ country                   <fct> Madagascar, Madagascar, Madagascar, ...
## $ country_id                <fct> MDG, MDG, MDG, MDG, MDG, MDG, MDG, M...
## $ continent_id              <fct> Africa, Africa, Africa, Africa, Afri...
## $ month_start               <int> 1, 3, 1, 7, 4, 1, 1, 7, 4, 7, 11, 4,...
## $ year_start                <int> 1987, 1987, 1987, 1995, 1986, 1987, ...
## $ month_end                 <int> 1, 3, 1, 8, 6, 1, 1, 8, 4, 8, 11, 6,...
## $ year_end                  <int> 1987, 1987, 1987, 1995, 1986, 1987, ...
## $ lower_age                 <dbl> 0, 0, 0, 2, 7, 0, 0, 2, 6, 2, 2, 7, ...
## $ upper_age                 <int> 99, 99, 99, 9, 22, 99, 99, 9, 12, 9,...
## $ examined                  <int> 50, 246, 50, 50, 119, 50, 50, 50, 20...
## $ positive                  <dbl> 0.075, 126.000, 0.025, 0.060, 37.000...
## $ pr                        <dbl> 0.0015, 0.5122, 0.0005, 0.0012, 0.31...
## $ species                   <chr> "P. falciparum", "P. falciparum", "P...
## $ method                    <fct> Microscopy, Microscopy, Microscopy, ...
## $ rdt_type                  <fct> , , , , , , , , , , , , , , , , , , ...
## $ pcr_type                  <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ malaria_metrics_available <fct> true, true, true, true, true, true, ...
## $ location_available        <fct> true, true, true, true, true, true, ...
## $ permissions_info          <fct> , , , , , , , , , , , , , , , , , , ...
## $ citation1                 <fct> Lepers, J.P., Ramanamirija, J.A., An...
## $ citation2                 <fct> , , , , , , , , , , , , , , , , , , ...
## $ citation3                 <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
```


`autoplot.pr.points` configures autoplot method to enable quick mapping of the locations of downloaded PR points. 



```r
autoplot(MDG_pr_data)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png)

N.B. Facet-wrapped option is also available for species stratification. 

```r
autoplot(MDG_pr_data,
         facet = TRUE,
         map_title = "Example MAP of PR point locations,\nstratified by species")
```

```
## Error in new_shps[names(new_shps)[names(new_shps) %in% names(.malariaAtlasHidden$stored_polygons)]]: cannot get a slot ("Polygons") from an object of type "NULL"
```

### Shapefiles
`getShp()` downloads a shapefile for a specified country (or countries) and returns this as either a spatialPolygon or data.frame object.


```r
MDG_shp <- getShp(ISO = "MDG", admin_level = "both")
```

```
## OGR data source with driver: ESRI Shapefile 
## Source: "/tmp/RtmpEHpB5d/shp/mapadmin_0_2013.shp", layer: "mapadmin_0_2013"
## with 1 features
## It has 9 fields
## OGR data source with driver: ESRI Shapefile 
## Source: "/tmp/RtmpEHpB5d/shp/mapadmin_1_2013.shp", layer: "mapadmin_1_2013"
## with 22 features
## It has 6 fields
```

```
## Error in new_shps[names(new_shps)[names(new_shps) %in% names(.malariaAtlasHidden$stored_polygons)]]: cannot get a slot ("Polygons") from an object of type "NULL"
```

```
## Error in tibble::glimpse(MDG_shp): object 'MDG_shp' not found
```

`autoplot.MAPshp` configures autoplot method to enable quick mapping of downloaded shapefiles.


```r
MDG_shp <- as.MAPshp(MDG_shp)
```

```
## Error in rownames(object@data): object 'MDG_shp' not found
```

```r
autoplot(MDG_shp)
```

```
## Error in autoplot(MDG_shp): object 'MDG_shp' not found
```

N.B. Facet-wrapped option is also available for species stratification. 


```r
autoplot(MDG_shp,
         facet = TRUE,
         map_title = "Example of facetted shapefiles.")
```

```
## Error in autoplot(MDG_shp, facet = TRUE, map_title = "Example of facetted shapefiles."): object 'MDG_shp' not found
```

### Modelled Rasters 

`getRaster()`downloads publicly available MAP rasters for a specific surface & year, clipped to a given bounding box or shapefile


```r
MDG_shp <- getShp(ISO = "MDG", admin_level = "admin0")
```

```
## OGR data source with driver: ESRI Shapefile 
## Source: "/tmp/RtmpEHpB5d/shp/mapadmin_0_2013.shp", layer: "mapadmin_0_2013"
## with 1 features
## It has 9 fields
```

```
## Error in new_shps[names(new_shps)[names(new_shps) %in% names(.malariaAtlasHidden$stored_polygons)]]: cannot get a slot ("Polygons") from an object of type "NULL"
```

```r
MDG_PfPR2_10 <- getRaster(surface = "PfPR2-10", shp = MDG_shp, year = 2013)
```

```
## Error in getRaster(surface = "PfPR2-10", shp = MDG_shp, year = 2013): object 'MDG_shp' not found
```
N.B. to use downloaded rasters and shapefiles directly with autoplot, use as.MAPraster() and as.MAPshp() to convert these to data.frames. Alternatively autoplot_MAPraster() will work directly with RasterLayer, RasterStack or RasterBrick objects downloaded with getRaster().

`autoplot.MAPraster`&`autoplot_MAPraster`configures autoplot method to enable quick mapping of downloaded rasters.


```r
MDG_PfPR2_10_df <- as.MAPraster(MDG_PfPR2_10)
```

```
## Error in inherits(raster_object, c("RasterLayer", "RasterBrick")): object 'MDG_PfPR2_10' not found
```

```r
MDG_shp_df <- as.MAPshp(MDG_shp)
```

```
## Error in rownames(object@data): object 'MDG_shp' not found
```

```r
p <- autoplot(MDG_PfPR2_10_df, shp_df = MDG_shp_df)
```

```
## Error in autoplot(MDG_PfPR2_10_df, shp_df = MDG_shp_df): object 'MDG_PfPR2_10_df' not found
```


### Combined visualisation 

By using the above tools along with ggplot, simple comparison figures can be easily produced. 


```r
MDG_shp <- getShp(ISO = "MDG", admin_level = "admin0")
```

```
## OGR data source with driver: ESRI Shapefile 
## Source: "/tmp/RtmpEHpB5d/shp/mapadmin_0_2013.shp", layer: "mapadmin_0_2013"
## with 1 features
## It has 9 fields
```

```
## Error in new_shps[names(new_shps)[names(new_shps) %in% names(.malariaAtlasHidden$stored_polygons)]]: cannot get a slot ("Polygons") from an object of type "NULL"
```

```r
MDG_shp_df <- as.MAPshp(MDG_shp)
```

```
## Error in rownames(object@data): object 'MDG_shp' not found
```

```r
MDG_PfPR2_10 <- getRaster(surface = "PfPR2-10", shp = MDG_shp, year = 2013)
```

```
## Error in getRaster(surface = "PfPR2-10", shp = MDG_shp, year = 2013): object 'MDG_shp' not found
```

```r
MDG_PfPR2_10_df <- as.MAPraster(MDG_PfPR2_10)
```

```
## Error in inherits(raster_object, c("RasterLayer", "RasterBrick")): object 'MDG_PfPR2_10' not found
```

```r
p <- autoplot(MDG_PfPR2_10_df, shp_df = MDG_shp_df, printed = FALSE)
```

```
## Error in autoplot(MDG_PfPR2_10_df, shp_df = MDG_shp_df, printed = FALSE): object 'MDG_PfPR2_10_df' not found
```

```r
pr <- getPR(country = c("Madagascar"), species = "Pf")
p[[1]] +
geom_point(data = pr[pr$year_start==2013,], aes(longitude, latitude, fill = pf_pos / examined, size = examined), shape = 21)+
scale_size_continuous(name = "Survey Size")+
 scale_fill_distiller(name = "PfPR", palette = "RdYlBu")+
 ggtitle("Raw PfPR Survey points\n + Modelled PfPR 2-10 in Madagascar in 2013")
```

```
## Error in eval(expr, envir, enclos): object 'p' not found
```


## Basic Spatial utility tools 

### extractRaster 










