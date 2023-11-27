WCS 2.0
- GetCapabilities:
  - ows:BoundingBox only supports double values as common in spatial bounding boxes, not suitable for provision of temporal information. Also not suitable for categorical dimensions.
  - wcs:CoverageId doesn't support the identifiers stemming from ds.earthserver.xyz as not NCName

- DescribeCoverage 
  - grassland_status_virtual_coverage_index: valid
  - ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: Fail
    - ID not NCName (causes many issues)
	- Timestamp not valid in gml:Envelope, gml:pos
  - near_surface_air_temperature: Failed
    - Timestamp not valid in gml:Envelope, gml:pos, gmlrgrid:coefficients
  - corine_land_cover_virtual_coverage_index: valid
	
- GetCoverage
  - grassland_status_virtual_coverage_index: Fail  
ExceptionText:
Failed internal rasql query: SELECT encode((CONCAT ( MARRAY pt in [0:0] VALUES project(c0[pt[0],180000:180014,-5015:-5001], "4500000.0,3100000.0,4500300.0,3100300.0", "EPSG:3035", "4500000,3100000,4500300,3100300", "EPSG:3035", 10.0, 10.0) ) WITH ( MARRAY pt in [0:0] VALUES project(c1[pt[0],-50000:-49971,-90030:-90001], "4500000.0,3100000.0,4500300.0,3100300.0", "EPSG:3035", "4500000,3100000,4500300,3100300", "EPSG:3035", 10.0, 10.0) ) ALONG 0), "json" , "{\"geoReference\":{\"crs\":\"EPSG:3035\",\"bbox\":{\"xmin\":4500000,\"ymin\":3100000,\"xmax\":4500300,\"ymax\":3100300}},\"formatParameters\":{\"enableNull\":\"false\"},\"nodata\":[250.0]}")   FROM grassland_2015_index AS c0, grassland_2018_index AS c1  
  - ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: works via the GUI, but not via WCS request
  https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.0.1&REQUEST=GetCoverage&COVERAGEID=ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor&SUBSET=ansi("2019-08-04T12:00:00.000Z")&SUBSET=lat(48,49)&SUBSET=lon(15,16)&FORMAT=application/gml+xml
  returns: ExceptionText: Trimming subset in WCS must follow this syntax 'lowerBound,upperBound', given '2019-08-04T12:00:00.000Z'
  
  - near_surface_air_temperature - 1day slice: Valid
  - near_surface_air_temperature - 3day subset: Failed
    - Timestamp not valid in gml:Envelope, gml:pos, gmlrgrid:coefficients
  - corine_land_cover_virtual_coverage_index: Valid

WCS 2.1
- General: Schema construction for WCS 2.1 is corrupt, utilizes concepts from 2.0 instead of including within 2.1

- GetCapabilities: not available for WCS 2.1

- DescribeCoverage 
  - grassland_status_virtual_coverage_index: valid
  - ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: not valid
    - wcs20:CoverageId must be NCName
  - near_surface_air_temperature: Valid	
  - corine_land_cover_virtual_coverage_index: valid	
  
- GetCoverage
  - grassland_status_virtual_coverage_index: Fail  
ExceptionText:
Failed internal rasql query: SELECT encode((CONCAT ( MARRAY pt in [0:0] VALUES project(c0[pt[0],180000:180014,-5015:-5001], "4500000.0,3100000.0,4500300.0,3100300.0", "EPSG:3035", "4500000,3100000,4500300,3100300", "EPSG:3035", 10.0, 10.0) ) WITH ( MARRAY pt in [0:0] VALUES project(c1[pt[0],-50000:-49971,-90030:-90001], "4500000.0,3100000.0,4500300.0,3100300.0", "EPSG:3035", "4500000,3100000,4500300,3100300", "EPSG:3035", 10.0, 10.0) ) ALONG 0), "json" , "{\"geoReference\":{\"crs\":\"EPSG:3035\",\"bbox\":{\"xmin\":4500000,\"ymin\":3100000,\"xmax\":4500300,\"ymax\":3100300}},\"outputType\":\"GeneralGridCoverage\",\"formatParameters\":{\"enableNull\":\"false\"},\"nodata\":[]}")   FROM grassland_2015_index AS c0, grassland_2018_index AS c1 
In addition, the component coverages are no longer available, just the resampled data from the virtual_coverage_index

- ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: works via the GUI, but not via WCS request
  https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=GetCoverage&COVERAGEID=ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor&SUBSET=ansi("2019-08-04T12:00:00.000Z")&SUBSET=lat(48,49)&SUBSET=lon(15,16)&FORMAT=application/gml+xml&outputType=GeneralGridCoverage
  returns: ExceptionText: Trimming subset in WCS must follow this syntax 'lowerBound,upperBound', given '2019-08-04T12:00:00.000Z'
  
  - near_surface_air_temperature - 1day slice: Valid
  - near_surface_air_temperature - 3day subset: Valid  
  - corine_land_cover_virtual_coverage_index: valid	