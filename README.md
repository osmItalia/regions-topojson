regions-topojson
================

Example with french regions : https://github.com/laem/regions-topojson/blob/master/france/lala.json

Follow these steps to get them for other countries :

create a file named my-country.xml. 

Just change FR for DE to get german regions for example, and check http://wiki.openstreetmap.org/wiki/Tag:boundary%3Dadministrative to get the 'admin_level' that you need. 

```
<osm-script timeout="1000">
  <!-- gather results -->

  <id-query into="searchArea" ref="3600365331" type="area"/>
  <union>
    <query type="relation">
      <has-kv k="admin_level" modv="" v="8"/>
      <area-query from="searchArea" ref=""/>
    </query>
  </union>

  <!-- print results -->
  <print mode="body"/>
  <recurse type="down"/>
  <print mode="skeleton" order="quadtile"/>
</osm-script>
```

then run this command...
```
wget www.overpass-api.de/api/interpreter --post-file=my-country.xml -O country.osm
```

after waiting few minutes (depending on size), you will get one file from the API, proceed by installing: 

```
npm install osmtogeojson
osmtogeojson my.osm > MYREGIONS.geojson
```
You can stop here and use geojson, but files will be huge. Simplify them with topojson :

```
npm install topojson
topojson -o lala.json MYREGIONS.geojson -q 10000 --simplify-proportion 0.02 -p
```


Developed for [qunb.com](https://www.qunb.com)
