# Metadata

## MHQ Terrestraial habitats

### Population units

The table `mhq_terr_popunits` contains the population units which have been assessed or which have been considered for assessment in MHQ. 
It contains following variabeles:

+ point_code: unique id
+ grts_ranking: grts ranking according to [GRTSmaster_habitats](https://zenodo.org/records/2682323)
+ grts_ranking_draw: the grts_ranking which is used for drawing the sample
+ sac: located in N2000 special area of conservation
+ legacy_site: population that are used as sampling units in other monitoring programmes and have been included in mhq
+ type
+ polygon_id: id of habitatmap polygon in which the population unit is located 
+ phab: fraction (%) of the habitatmap polygon covered by the type
+ source: the data source based on which the population unit was selected
    + habitatmap 2020: the presence of the type in the population unit is solely based on the habitatmap from 2020, but the population unit has not been assessed in the field
    + assessment: the presence of the type in the population unit is based on a field based assessment but according to the 2020 habitatmap, the type is not present 
    + assessment/ habitatmap 2020: the presence of the type in the population unit is based on a field based assessment and the 2020 habitatmap

### Reference points

The table `mhq_terr_refpoints` provide the xy-coordinates of the reference points for each population unit.
When a grts-sample is drawn based on [GRTSmaster_habitats](https://zenodo.org/records/2682323), the reference points are always located in the centroid of the 32 meter x 32 meter grid cells.
However, in mhq we also make use of legacy sites which are not drawn from the master sample, and therefore are not located in the centroid of the grid cells.
Furthermore, in some cases (for types with a low detection rate), when the targeted type is not present at a reference point, a new refeerence point is selected within a distance of 100 meter.
This reference point is not always located in the center of a grid cell.

In MHQ the reference point corresponds with the southeastern corner of the vegetation plot.

Fllowing variables are stored in the table:

+ point_code: unique id
+ grts_ranking: grts ranking according to [GRTSmaster_habitats](https://zenodo.org/records/2682323)
+ is_centroid: is the reference point located in the centroid of a grid cell of [GRTSmaster_habitats](https://zenodo.org/records/2682323)? (TRUE/FALSE)
+ x-coordinate (Lambert 72, `crs = 31370`)
+ y-coordinate (Lambert 72, `crs = 31370`)


### Assessments

The table `mhq_terr_assessments` contains the assessments for MHQ and has following variables:

+ assessment_date: date of the assessment
    + If the target type was not observed, the assessment date was not always recorded. In this case we take the median of the
    years in which the assessments took place.
+ point_code: the id the sampling unit
+ type: the evaluated type
+ is_present:
    + `TRUE` if the evaluated type was observed
    + `FALSE` if the evaluated type was not observed
    + `NA` if no assessment could be performed (inaccessible) or if the type’s presence is unknown
+ no_habitat:
    + `TRUE` if no habitat type was observed,  altough a regional import biotope (rib) type might be present (the presence of rib types was not evaluated)
    + `FALSE` when any type is present,
    + `NA` when the evaluated type is absent or unknown and it is not known if any other type is present (in case the point is inaccessible)
+ assessment_source: `field assessment` or `orthophoto`
+ inaccessible: `long term` or `short term`
+ not_measurable: `long term` or `short term`
+ change_location: has the population unit been replaced? (`TRUE`/`FALSE`)

### Measurements

The table `mhq_terr_measurements` contains the measurements for MHQ and has following variables:

+ fieldwork team: `anb` or `inbo`
+ measurement_date: date of measurement
+ point_code: the id the sampling unit
+ type: the observed type
+ db_ref: unique id for a measurement event
+ user_reference: the user reference in the INBOVEG database  
+ recording_givid: the recording_givid of the square and the circle plot in the INBOVEG database

### Validation of measurements

*work in progress*

The table `mhq_terr_validation` stores information on the validity of the measurements based on some quality checks.
The quality checks depend on the version of the standardized field protocol (sfp).

In the first version (v1) of the sfp:

+ We allow a measurement for another type than the original target type as long as:
    + grts_ranking_draw is smaller than the maximum ranking in the sample for the observed type
    
+ We allow a replacement from the original location
    + when the distance of the replacement is smaller than 110 meters
    + when the distance of the replacement is smaller than 1 meter for legacy sites 

In the second version (v2) of the sfp following rule was applied: 
    + When the target type is not observed in a sampling unit, we allow it to be replaced by the sampling unit containing the target type with the lowest grst-ranking, located within the same habitatmap polygon. Legacy sampling units are not replaced.
    
The table contains following variables:

+ sfp: version of the standardized field protocol applied
+ point_code
+ sac
+ legacy_site
+ type
+ measurement date
+ valid_sampling_unit: `TRUE` when `valid_type` = `TRUE` and `valid_reference_point` = `TRUE`
+ valid_type: 
    + `TRUE` when observed type matches the target type 
    + in case of `sfp v1`: `TRUE` when the grts-ranking of the observed type is lower than the maximum grts-ranking in the sample of the observed type
+ valid_refpoint:
    + in case of `sfp v1`:`TRUE` when `valid_distance` = `TRUE`
    + in case of `sfp v2`:`TRUE` when `valid_polygon`, `valid_centroid`, and `valid_ranking` are `TRUE`
+ valid_distance: 
    + only applies to `sfp v1`:`TRUE` when a new reference point is selected within a distance of 110 meters from the original reference point
+ valid_centroid:
    + only applies to `sfp v2`: `TRUE` when the location of the measurement is within a distance of 1 meter from the centroid of a grts grid cell
+ valid_polygon
    + only applies to `sfp v2`: `TRUE` when the location of the measurement is within the same habmap polygon as the originally selected sampling unit
+ valid_ranking
    + only applies to `sfp v2`: here we check if the relative ranking within the habmap polygon is followed to replace the sampling unit. `TRUE` when the difference between the relative ranking of the measured sampling unit and the relative ranking of the original sampling unit is =< 10 (when > 10 it is highly unprobable that the target type is not present in the 10 sampling units with the lowest relative ranking) 



