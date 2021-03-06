# modxlib23

__modxlib23__ is a package of common functions, data, and classes used in the CTPS Model Data Explorer for TDM23 data.

## Version Identification

__Function__: __get_version()__ - Returns version ID string of library.

## Trip Table Management

__Summary__: TripTableMgr manages access to the trip tables for a given scenario

__Methods__:
1. open_trip_tables(scenario_dir) - opens the trip table OMXs for a given scenario; returns a dict containig the open OMXs
2. load_trip_tables(tt_omxs, veh_modes=None, per_modes=None) - open the trip table OMXs for the inidcated modes;
returns a two-level dictionary (i.e., first level = time period, second level = mode) containing the trip tables read into numPy arrays

## TAZ "shapefile" Management

__Summary__: The class "tazManager" provides a set of methods to perform _attribute_ queries
on an ESRI-format "Shapefile" that represents the TAZes in the model region.
The attributes are read from the Shapefile's .DBF file; other components of
the Shapefile are ignored.

The Shapefile's .DBF file _must_ contain the following attributes:
1. id
2. taz
3. type - 'I' (internal) or 'E' (external)
4. town - town name (upper case)
5. state - state abbreviation, e.g., 'MA'
6. town_state - town, state
7. mpo - abbreviation of MPO name: 
8. in_brmpo - 1 (yes) or 0 (no)
9. subregion - abbreviation of Boston Region MPO subregion or NULL
10. sector - 'analysis sector' as defined by CTPS's Bill Kuttner.
Either 'Northeast', 'North', 'Northwest', 'West', 'Southwest',
South', 'Southeast', 'Central' or ''; the empty string ('')
indicates that the TAZ is outsize of the 164 municipalities
comprising what was once known as the 'CTPS Model Region'.

An object of class tazManager is instantiated by passing in the fully-qualified path
to a Shapefile to the class constructor. Hence, it is possible to have more than one
instance of this class active simultaneously, should this be needed.

__Class__ __tazManager__  
__Methods__:  
1. __init__(path_to_shapefile) - class constructor
2. mpo_to_tazes(mpo) - Given the name (i.e., abbreviation) of an MPO,
return a list of the records for the TAZes in it
3. brmpo_tazes() - Return the list of the records for the TAZes in the Boston Region MPO
4. brmpo_town_to_tazes(town) - Given the name of a town in the Boston Region MPO,
return a list of the records for the TAZes in it
5. brmpo_subregion_to_tazes(subregion) - Given the name (i.e., abbreviation) of a Boston Region MPO subregion,
return a list of the records for the TAZes in it
6. sector_to_tazes - Given the name of an 'analysis sector', return the list of the records for the TAZes
in the sector.
7. town_to_tazes(town) - Given the name of a town, return the list of the records for the TAZes in the town.
Note: If a town with the same name occurs in more than one state, the  list of TAZes
in _all_ such states is returned.
8. town_state_to_tazes(town, state) - Given a town and a state abbreviation (e.g., 'MA'),
return the list of records for the TAZes in the town
9. state_to_tazes(state) - Given a state abbreviation, return the list of records for the TAZes in the state.
10. taz_ids(TAZ_record_list) - Given a list of TAZ records, return a list of _only_ the TAZ IDs from those records.

__Note__: For all of the above API calls that return a "list of TAZ records", each returned 'TAZ' is a Python 'dict' containing
all of the keys (i.e., 'attributes') listed above. To convert such a list to a list of _only_ the TAZ IDs, call taz_ids
on the list of TAZ records.

## Utilities for the Transit Mode

__TBD__

## Utilities for the Highway Mode

__TBD__

## "Skims" Management

__Summary__: Skims manages access to the "skims" for a given scenario

__Methods__:
1. load_highway_skims(self, skims_dir, periods=_skim_periods, matrices=_highway_skim_matrices) - loads the highway skims for a given scenario into
a two-level dict containing a numPy array for each skim matrix read
2. load_walk_skims(skims_dir, matrices=_walk_matrices) - loads the walk skims for a given scenario into 
a one-level dict containing a numPy array for each skim matrix read
3. load_bike_skims(skims_dir, matrices=_bike_matrices) - loads the bike skims for a given scenario into 
a one-level dict containing a numPy array for each skim matrix read
4. load_transit_auto_skims(skims_dir, periods=_skim_periods, matrices=_transit_auto_skim_matrices) - loads the transit-auto skims for a given scenario into
a two-level dict containing a numPy array for each skim matrix read
5. load_transit_walk_skims(skims_dir, periods=_skim_periods, matrices=_transit_walk_skim_matrices) - loads the transit-walk skims for a given scenario into
a two-level dict containing a numPy array for each skim matrix read

## Dataframe and Geo-dataframe Utilities

__Function__: __export\_gdf\_to\_geojson(geo_dataframe, geojson_fn)__

__Summary__: Export a GeoPandas gdataframe to a GeoJSON file.

__Parameters__:
* geo_dataframe - GeoPandas dataframe
* geojson_fn - Name of GeoJSON file

__Return value__: N/A

__Function__: __export\_gdf\_to\_shapefile(geo_dataframe, shapefile_fn)__

__Summary__: Export a GeoPandas gdataframe to an ESRI-format shapefile

__Parameters__:
* geo_dataframe - GeoPandas dataframe
* geojson_fn - Name of shapefile

__Note__: Attribute (property) names longer than 10 characters will be truncated,
due to the limitations of the DBF file used for Shapefile attributes.

__Return value__: N/A

__Function__: __bbox\_of\_gdf(gdf)__

__Summar__: Return the bounding box of all the features in a geo-dataframe.

__Parameters__:   
* gdf - a GeoPandas dataframe

__Return value__: Bounding box of all the features in the input geodataframe.
The bounding box is returned as a dictionary with the keys: \{ 'minx', 'miny', 'maxx', 'maxy' \}


__Function__: __center\_of\_bbox(bbox)__

__Summary__: Given a geomtric "bounding box", return its center point. 

__Parameters__:
* bbox - Bounding box in the form of a dictionary with the keys { 'minx', 'miny', 'maxx', 'maxy'}

__Return value__: Center point of the bounding box as a dictionary with the keys { 'x' , 'y' }.

