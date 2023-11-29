# Metadata

## MHQ Terrestraial habitats

### Population units

The table `mhq_terr_popunits` contains the population units which have been assessed or which have been considered for assessment in MHQ. 
It contains following variabeles:

+ point_code
+ grts_ranking
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

### Assessments

The table `mhq_terr_assessments` contains the assessments for MHQ and has following variables:

+ assessment_date: date of the assessment
    + If the target type was not observed, the assessment date was not recorded. In this case we take the median of the
    years in which the assessments took place.
+ point_code: the id of the point_code
+ type: the evaluated type
+ is_present:
    + TRUE if the evaluated type was observed
    + FALSE if the evaluated type was not observed
    + NA if no assessment could be performed (inaccessible) or if the type’s presence is unknown
+ no_habitat:
    + TRUE if no habitat type was observed,  altough a regional import biotope (rib) type might be present (the presence of rib types was not evaluated)
    + FALSE when any type is present,
    + NA when the evaluated type is absent or unknown and it is not known if any other type is present (in case the point is inaccessible)
+ assessment_source: field assessment or orthophoto
+ inaccessible: long term or short term
+ not_measurable: long term or short term
+ change_location: has the population unit been replaced
