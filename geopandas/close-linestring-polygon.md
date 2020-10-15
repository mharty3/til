# Convert a closed linestring to a polygon

```
model_bounds = gpd.read_file('P://Shapefiles/NM_SebRegional_Poly.shp')
model_bounds.geometry = model_bounds.geometry.apply(lambda x: Polygon(x.coords))
```