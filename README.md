# mbtiles4j (Android)

Forked from [MBTiles4j](https://github.com/imintel/mbtiles4j). Writer implementation for android systems using [SQLdroid](https://github.com/SQLDroid/SQLDroid)

## Dependencies

In your build.gradle

```java
compile 'org.sqldroid:sqldroid:1.0.3'
compile 'commons-io:commons-io:2.5'
```

### Examples

#### Reading `.mbtiles`

```java
MBTilesReader r = new MBTilesReader(new File("control-room-0.2.0.mbtiles"));
//metadata
MetadataEntry metadata = r.getMetadata();
String tileSetName = metadata.getTilesetName();
MetadataEntry.TileSetType type = metadata.getTilesetType();
String tilesetVersion = metadata.getTilesetVersion();
String description = metadata.getTilesetDescription();
MetadataEntry.TileMimeType tileMimeType = metadata.getTileMimeType();
MetadataEntry.MetadataBounds bounds = metadata.getTilesetBounds();
String attribution = metadata.getAttribution();
//tiles
TileIterator tiles = r.getTiles();
while (tiles.hasNext()) {
	TileIterator.Tile next = tiles.next();
	int zoom = next.getZoom();
	int column = next.getColumn();
	int row = next.getRow();
	InputStream tileData = next.getData();        
}
tiles.close();
r.close();
```

#### Writing `.mbtiles`

```java
MBTilesWriter w = new MBTilesWriter(new File("example.mbtiles"));
MetadataEntry ent = new MetadataEntry();
//Add metadata parts
ent.setTilesetName("An example Tileset")
	.setTilesetType(MetadataEntry.TileSetType.BASE_LAYER)
	.setTilesetVersion("0.2.0")
	.setTilesetDescription("An example tileset description")
	.setTileMimeType(MetadataEntry.TileMimeType.PNG)
	.setAttribution("Tiles are Open Source!")
	.setTilesetBounds(-180, -85, 180, 85);
w.addMetadataEntry(ent);
//add someTile at Zoom (0), Column(0), Row (0)
w.addTile(someTileBytes, 0, 0, 0);
File result = w.close();
```

#### Tips

* Use an AsyncTask to download data tiles
* Convert LatLng to X Y with this [Slippy](https://wiki.openstreetmap.org/wiki/Slippy_map_tilenames#Java) Java implementation

