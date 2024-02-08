Formative Assignment: Street network analysis
-------------------------------

### Task 1: Investigating the Relationship Between Retail Locations and Street Networks
For this task, I have chosen to center my exploration around **Holloway Road Station**, examining the relationship between the road network within a radius of 2000 meters and retail establishments. The primary focus of this study involves the analysis of the **driving network** within the designated research area.

```python
import pandas as pd
import matplotlib.pyplot as plt
import osmnx as ox
import networkx as nx
import contextily as ctx
```

In this study, all data are sourced from OpenStreetMap. Relevant functions from the **OSMnx** are employed to retrieve the street network map around Holloway Road, and **NetworkX** is utilized for subsequent analysis.

Simple statistical analysis is performed on the acquired road network data: the research area encompasses a total of 1664 nodes and 2744 edges. The average street length is 85 meters, with an average street count of 2.61 per node.


```python
G=ox.graph_from_address('Holloway Road, London', dist=2000, network_type='drive')
# Convert the graph G to GeoDataFrames
gdf_nodes, gdf_edges = ox.graph_to_gdfs(G)

stats = ox.basic_stats(G)
stats
```




    {'n': 2138,
     'm': 4952,
     'k_avg': 4.632366697848457,
     'edge_length_total': 432022.5349999992,
     'edge_length_avg': 87.24203049273005,
     'streets_per_node_avg': 2.610383536014967,
     'streets_per_node_counts': {0: 0, 1: 474, 2: 71, 3: 1413, 4: 174, 5: 6},
     'streets_per_node_proportions': {0: 0.0,
      1: 0.22170252572497662,
      2: 0.03320860617399439,
      3: 0.6608980355472404,
      4: 0.0813844714686623,
      5: 0.002806361085126286},
     'intersection_count': 1664,
     'street_length_total': 233709.94700000025,
     'street_segment_count': 2744,
     'street_length_avg': 85.1712634839651,
     'circuity_avg': 1.0558203843508747,
     'self_loop_proportion': 0.0010932944606413995}



In order to further explore the spatial distribution of the street network within the research area, **Closeness Centrality** and **Betweenness Centrality** methods are employed to measure centrality. The former is utilized to analyze the geometric center of the street network, while the latter is employed to assess the movement potential of streets. This study applies both methods at the **edge level**, as centrality is calculated on the streets, and node level analysis may not yield highly meaningful results.


```python
DG = ox.get_digraph(G)

edge_cc = nx.closeness_centrality(nx.line_graph(DG))
nx.set_edge_attributes(DG, edge_cc,'cc')
G1 = nx.MultiGraph(DG)

%matplotlib inline
# convert graph to geopandas dataframe
gdf_edges = ox.graph_to_gdfs(G1,nodes=False,fill_edge_geometry=True)
gdf_edges = gdf_edges.to_crs(epsg=3857) # setting crs to 3857
ax=gdf_edges.plot('cc',cmap='plasma',figsize=(10,10))

# add a basemap using contextilly
ctx.add_basemap(ax,source=ctx.providers.CartoDB.Positron)
plt.axis('off')
```
![Alt Text](https://raw.githubusercontent.com/Xinyi1204/xinyi1204.github.io/master/closeness_centrality.png)   
    

The above figure illustrates the Closeness Centrality of the road network within the research area. Roads with a more yellowish hue indicate proximity to other locations, positioning them at the center of the road network. Specifically, the streets around **Holloway Road Station** and the intersection of **Caledonian Road and Market Road** are highlighted in yellow, signifying their central positions within the studied road network. 
Upon comparison with the basemap, it is observed that Closeness Centrality does not exert a significant influence on the presence of roadside retail businesses. In other words, retail establishments do not necessarily tend to be located in areas with high Closeness Centrality in this study area.


```python
edge_bc = nx.betweenness_centrality(nx.line_graph(DG))
nx.set_edge_attributes(DG, edge_bc,'bc')
G1 = nx.MultiGraph(DG)

# convert graph to geopandas dataframe
gdf_edges = ox.graph_to_gdfs(G1,nodes=False,fill_edge_geometry=True)
gdf_edges = gdf_edges.to_crs(epsg=3857) # setting crs to 3857
ax=gdf_edges.plot('bc',cmap='plasma',figsize=(10,10))

# add a basemap using contextilly
ctx.add_basemap(ax,source=ctx.providers.CartoDB.Positron)
plt.axis('off')
```
![Alt Text](https://raw.githubusercontent.com/Xinyi1204/xinyi1204.github.io/master/betweenness_centrality.png)
    

The above figure presents the Betweenness Centrality of the road network within the research area. Betweenness Centrality to some extent reflects the frequency with which a street lies on the shortest paths between other streets in the road network, indicating the significance of a street in handling traffic flow within the network.
 One of the streets with the highest Betweenness Centrality in the above figure is situated at the intersection of **Holloway Road and Camdon Road**, as illustrated in the interactive map below. On either side of this road are major supermarkets such as Waitrose, Morrisons, Lidl, as well as a variety of retail stores, highlighting its crucial role in accommodating traffic flow and commercial activity.


```python
G=ox.graph_from_address('Waitrose,Holloway Road, London', dist=100, network_type='drive')
gdf_nodes, gdf_edges = ox.graph_to_gdfs(G)
Base_map = gdf_edges.explore(color='yellow')
Base_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;

    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;

        &lt;script&gt;
            L_NO_TOUCH = false;
            L_DISABLE_3D = false;
        &lt;/script&gt;

    &lt;style&gt;html, body {width: 100%;height: 100%;margin: 0;padding: 0;}&lt;/style&gt;
    &lt;style&gt;#map {position:absolute;top:0;bottom:0;right:0;left:0;}&lt;/style&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-3.7.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;

            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_b8fefb710e923681ffd0042adc660eaf {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;


                    &lt;style&gt;
                        .foliumtooltip {

                        }
                       .foliumtooltip table{
                            margin: auto;
                        }
                        .foliumtooltip tr{
                            text-align: left;
                        }
                        .foliumtooltip th{
                            padding: 2px; padding-right: 8px;
                        }
                    &lt;/style&gt;

&lt;/head&gt;
&lt;body&gt;


            &lt;div class=&quot;folium-map&quot; id=&quot;map_b8fefb710e923681ffd0042adc660eaf&quot; &gt;&lt;/div&gt;

&lt;/body&gt;
&lt;script&gt;


            var map_b8fefb710e923681ffd0042adc660eaf = L.map(
                &quot;map_b8fefb710e923681ffd0042adc660eaf&quot;,
                {
                    center: [51.556040800000005, -0.1166365],
                    crs: L.CRS.EPSG3857,
                    zoom: 10,
                    zoomControl: true,
                    preferCanvas: false,
                }
            );
            L.control.scale().addTo(map_b8fefb710e923681ffd0042adc660eaf);





            var tile_layer_6e6c8bf3bc0012c53f9cf36ff4972e35 = L.tileLayer(
                &quot;https://tile.openstreetmap.org/{z}/{x}/{y}.png&quot;,
                {&quot;attribution&quot;: &quot;\u0026copy; \u003ca href=\&quot;https://www.openstreetmap.org/copyright\&quot;\u003eOpenStreetMap\u003c/a\u003e contributors&quot;, &quot;detectRetina&quot;: false, &quot;maxNativeZoom&quot;: 19, &quot;maxZoom&quot;: 19, &quot;minZoom&quot;: 0, &quot;noWrap&quot;: false, &quot;opacity&quot;: 1, &quot;subdomains&quot;: &quot;abc&quot;, &quot;tms&quot;: false}
            );


            tile_layer_6e6c8bf3bc0012c53f9cf36ff4972e35.addTo(map_b8fefb710e923681ffd0042adc660eaf);


            map_b8fefb710e923681ffd0042adc660eaf.fitBounds(
                [[51.555294, -0.1176892], [51.5567876, -0.1155838]],
                {}
            );


        function geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_styler(feature) {
            switch(feature.id) {
                default:
                    return {&quot;color&quot;: &quot;yellow&quot;, &quot;fillColor&quot;: &quot;yellow&quot;, &quot;fillOpacity&quot;: 0.5, &quot;weight&quot;: 2};
            }
        }
        function geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_highlighter(feature) {
            switch(feature.id) {
                default:
                    return {&quot;fillOpacity&quot;: 0.75};
            }
        }
        function geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_pointToLayer(feature, latlng) {
            var opts = {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#3388ff&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#3388ff&quot;, &quot;fillOpacity&quot;: 0.2, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 2, &quot;stroke&quot;: true, &quot;weight&quot;: 3};

            let style = geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_styler(feature)
            Object.assign(opts, style)

            return new L.CircleMarker(latlng, opts)
        }

        function geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_onEachFeature(feature, layer) {
            layer.on({
                mouseout: function(e) {
                    if(typeof e.target.setStyle === &quot;function&quot;){
                        geo_json_f1aa0cb260d883e636e0d1c3ec6f8011.resetStyle(e.target);
                    }
                },
                mouseover: function(e) {
                    if(typeof e.target.setStyle === &quot;function&quot;){
                        const highlightStyle = geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_highlighter(e.target.feature)
                        e.target.setStyle(highlightStyle);
                    }
                },
            });
        };
        var geo_json_f1aa0cb260d883e636e0d1c3ec6f8011 = L.geoJson(null, {
                onEachFeature: geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_onEachFeature,

                style: geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_styler,
                pointToLayer: geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_pointToLayer,
        });

        function geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_add (data) {
            geo_json_f1aa0cb260d883e636e0d1c3ec6f8011
                .addData(data);
        }
            geo_json_f1aa0cb260d883e636e0d1c3ec6f8011_add({&quot;bbox&quot;: [-0.1176892, 51.555294, -0.1155838, 51.5567876], &quot;features&quot;: [{&quot;bbox&quot;: [-0.1169698, 51.5561177, -0.1155838, 51.5567876], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1155838, 51.5567876], [-0.1156851, 51.5567396], [-0.1157457, 51.556707], [-0.1159332, 51.5566062], [-0.1162782, 51.5564188], [-0.1164328, 51.5563374], [-0.116613, 51.5562438], [-0.1166398, 51.5562311], [-0.1167111, 51.5562005], [-0.1167209, 51.5561951], [-0.1167554, 51.5561766], [-0.1168326, 51.5561465], [-0.1169698, 51.5561177]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(199542, 199545, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: [&quot;1&quot;, &quot;3&quot;], &quot;length&quot;: 122.08, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Tollington Road&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: [1128361268, 246017141, 1130517558, 1130517560, 1130517561, 1130517562, 1128360287], &quot;ref&quot;: &quot;A503&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1170951, 51.5561177, -0.1169698, 51.5562008], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1169698, 51.5561177], [-0.1170951, 51.5562008]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(199545, 2204174075, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 12.666, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Holloway Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 39188791, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1176892, 51.5559058, -0.1169698, 51.5561177], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1169698, 51.5561177], [-0.1171975, 51.5560737], [-0.1173207, 51.556042], [-0.1174381, 51.5560049], [-0.1176892, 51.5559058]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(199545, 9694225, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: [&quot;2&quot;, &quot;3&quot;], &quot;length&quot;: 55.370000000000005, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Camden Road&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: [1128360288, 1064384196, 39188797], &quot;ref&quot;: &quot;A503&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1169698, 51.555294, -0.115708, 51.5561177], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1169698, 51.5561177], [-0.1167752, 51.5560516], [-0.1167171, 51.5560167], [-0.1166821, 51.5559976], [-0.1164021, 51.5557854], [-0.1159536, 51.555465], [-0.115708, 51.555294]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(199545, 279651360, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 127.379, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Holloway Road&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 47009458, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1170951, 51.5561177, -0.1169698, 51.5562008], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1170951, 51.5562008], [-0.1169698, 51.5561177]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(2204174075, 199545, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 12.666, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Holloway Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 39188791, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}], &quot;type&quot;: &quot;FeatureCollection&quot;});



    geo_json_f1aa0cb260d883e636e0d1c3ec6f8011.bindTooltip(
    function(layer){
    let div = L.DomUtil.create(&#x27;div&#x27;);

    let handleObject = feature=&gt;typeof(feature)==&#x27;object&#x27; ? JSON.stringify(feature) : feature;
    let fields = [&quot;osmid&quot;, &quot;oneway&quot;, &quot;lanes&quot;, &quot;ref&quot;, &quot;name&quot;, &quot;highway&quot;, &quot;maxspeed&quot;, &quot;reversed&quot;, &quot;length&quot;];
    let aliases = [&quot;osmid&quot;, &quot;oneway&quot;, &quot;lanes&quot;, &quot;ref&quot;, &quot;name&quot;, &quot;highway&quot;, &quot;maxspeed&quot;, &quot;reversed&quot;, &quot;length&quot;];
    let table = &#x27;&lt;table&gt;&#x27; +
        String(
        fields.map(
        (v,i)=&gt;
        `&lt;tr&gt;
            &lt;th&gt;${aliases[i]}&lt;/th&gt;

            &lt;td&gt;${handleObject(layer.feature.properties[v])}&lt;/td&gt;
        &lt;/tr&gt;`).join(&#x27;&#x27;))
    +&#x27;&lt;/table&gt;&#x27;;
    div.innerHTML=table;

    return div
    }
    ,{&quot;className&quot;: &quot;foliumtooltip&quot;, &quot;sticky&quot;: true});


            geo_json_f1aa0cb260d883e636e0d1c3ec6f8011.addTo(map_b8fefb710e923681ffd0042adc660eaf);

&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



Another street exhibiting exceptionally high Betweenness Centrality is located near the three-way intersection of **Highbury Corner, Canonbury Road, and St. Paul's Road**. The interactive map below illustrates that this street is flanked by a series of food-related retail stores and small to medium-sized supermarkets.


```python
G=ox.graph_from_address('Highbury Island, London', dist=100, network_type='drive')
gdf_nodes, gdf_edges = ox.graph_to_gdfs(G)
Base_map = gdf_edges.explore(color='yellow')
Base_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe srcdoc="&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;

    &lt;meta http-equiv=&quot;content-type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;

        &lt;script&gt;
            L_NO_TOUCH = false;
            L_DISABLE_3D = false;
        &lt;/script&gt;

    &lt;style&gt;html, body {width: 100%;height: 100%;margin: 0;padding: 0;}&lt;/style&gt;
    &lt;style&gt;#map {position:absolute;top:0;bottom:0;right:0;left:0;}&lt;/style&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://code.jquery.com/jquery-3.7.1.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js&quot;&gt;&lt;/script&gt;
    &lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js&quot;&gt;&lt;/script&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/leaflet@1.9.3/dist/leaflet.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.2.0/css/all.min.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css&quot;/&gt;
    &lt;link rel=&quot;stylesheet&quot; href=&quot;https://cdn.jsdelivr.net/gh/python-visualization/folium/folium/templates/leaflet.awesome.rotate.min.css&quot;/&gt;

            &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,
                initial-scale=1.0, maximum-scale=1.0, user-scalable=no&quot; /&gt;
            &lt;style&gt;
                #map_de53e3ae203808cdc89070f967684ddf {
                    position: relative;
                    width: 100.0%;
                    height: 100.0%;
                    left: 0.0%;
                    top: 0.0%;
                }
                .leaflet-container { font-size: 1rem; }
            &lt;/style&gt;


                    &lt;style&gt;
                        .foliumtooltip {

                        }
                       .foliumtooltip table{
                            margin: auto;
                        }
                        .foliumtooltip tr{
                            text-align: left;
                        }
                        .foliumtooltip th{
                            padding: 2px; padding-right: 8px;
                        }
                    &lt;/style&gt;

&lt;/head&gt;
&lt;body&gt;


            &lt;div class=&quot;folium-map&quot; id=&quot;map_de53e3ae203808cdc89070f967684ddf&quot; &gt;&lt;/div&gt;

&lt;/body&gt;
&lt;script&gt;


            var map_de53e3ae203808cdc89070f967684ddf = L.map(
                &quot;map_de53e3ae203808cdc89070f967684ddf&quot;,
                {
                    center: [51.545639800000004, -0.10223189999999999],
                    crs: L.CRS.EPSG3857,
                    zoom: 10,
                    zoomControl: true,
                    preferCanvas: false,
                }
            );
            L.control.scale().addTo(map_de53e3ae203808cdc89070f967684ddf);





            var tile_layer_b76a3b39ad579be2d4bb2f96c32588c4 = L.tileLayer(
                &quot;https://tile.openstreetmap.org/{z}/{x}/{y}.png&quot;,
                {&quot;attribution&quot;: &quot;\u0026copy; \u003ca href=\&quot;https://www.openstreetmap.org/copyright\&quot;\u003eOpenStreetMap\u003c/a\u003e contributors&quot;, &quot;detectRetina&quot;: false, &quot;maxNativeZoom&quot;: 19, &quot;maxZoom&quot;: 19, &quot;minZoom&quot;: 0, &quot;noWrap&quot;: false, &quot;opacity&quot;: 1, &quot;subdomains&quot;: &quot;abc&quot;, &quot;tms&quot;: false}
            );


            tile_layer_b76a3b39ad579be2d4bb2f96c32588c4.addTo(map_de53e3ae203808cdc89070f967684ddf);


            map_de53e3ae203808cdc89070f967684ddf.fitBounds(
                [[51.5451497, -0.1025251], [51.5461299, -0.1019387]],
                {}
            );


        function geo_json_c3baf7476b0055375c8f195a682a55de_styler(feature) {
            switch(feature.id) {
                default:
                    return {&quot;color&quot;: &quot;yellow&quot;, &quot;fillColor&quot;: &quot;yellow&quot;, &quot;fillOpacity&quot;: 0.5, &quot;weight&quot;: 2};
            }
        }
        function geo_json_c3baf7476b0055375c8f195a682a55de_highlighter(feature) {
            switch(feature.id) {
                default:
                    return {&quot;fillOpacity&quot;: 0.75};
            }
        }
        function geo_json_c3baf7476b0055375c8f195a682a55de_pointToLayer(feature, latlng) {
            var opts = {&quot;bubblingMouseEvents&quot;: true, &quot;color&quot;: &quot;#3388ff&quot;, &quot;dashArray&quot;: null, &quot;dashOffset&quot;: null, &quot;fill&quot;: true, &quot;fillColor&quot;: &quot;#3388ff&quot;, &quot;fillOpacity&quot;: 0.2, &quot;fillRule&quot;: &quot;evenodd&quot;, &quot;lineCap&quot;: &quot;round&quot;, &quot;lineJoin&quot;: &quot;round&quot;, &quot;opacity&quot;: 1.0, &quot;radius&quot;: 2, &quot;stroke&quot;: true, &quot;weight&quot;: 3};

            let style = geo_json_c3baf7476b0055375c8f195a682a55de_styler(feature)
            Object.assign(opts, style)

            return new L.CircleMarker(latlng, opts)
        }

        function geo_json_c3baf7476b0055375c8f195a682a55de_onEachFeature(feature, layer) {
            layer.on({
                mouseout: function(e) {
                    if(typeof e.target.setStyle === &quot;function&quot;){
                        geo_json_c3baf7476b0055375c8f195a682a55de.resetStyle(e.target);
                    }
                },
                mouseover: function(e) {
                    if(typeof e.target.setStyle === &quot;function&quot;){
                        const highlightStyle = geo_json_c3baf7476b0055375c8f195a682a55de_highlighter(e.target.feature)
                        e.target.setStyle(highlightStyle);
                    }
                },
            });
        };
        var geo_json_c3baf7476b0055375c8f195a682a55de = L.geoJson(null, {
                onEachFeature: geo_json_c3baf7476b0055375c8f195a682a55de_onEachFeature,

                style: geo_json_c3baf7476b0055375c8f195a682a55de_styler,
                pointToLayer: geo_json_c3baf7476b0055375c8f195a682a55de_pointToLayer,
        });

        function geo_json_c3baf7476b0055375c8f195a682a55de_add (data) {
            geo_json_c3baf7476b0055375c8f195a682a55de
                .addData(data);
        }
            geo_json_c3baf7476b0055375c8f195a682a55de_add({&quot;bbox&quot;: [-0.1025251, 51.5451497, -0.1019387, 51.5461299], &quot;features&quot;: [{&quot;bbox&quot;: [-0.1025251, 51.5461077, -0.1024788, 51.5461077], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1025251, 51.5461077], [-0.1024788, 51.5461077]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(221736, 9503835778, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 3.202, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 1031053440, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022056, 51.5451497, -0.1019387, 51.5455763], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022056, 51.5455763], [-0.1021366, 51.5454557], [-0.1021137, 51.5454163], [-0.1020826, 51.5453716], [-0.1020102, 51.5452467], [-0.1019968, 51.5452253], [-0.1019907, 51.5452138], [-0.101967, 51.5451818], [-0.1019387, 51.5451497]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6384726979, 8974355097, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;primary&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 50.961, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Canonbury Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: [18017473, 686204225, 686188420, 685541830, 228479564, 686188432], &quot;ref&quot;: &quot;A1200&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022471, 51.5455763, -0.1022056, 51.5458995], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022056, 51.5455763], [-0.1022133, 51.5456026], [-0.1022233, 51.5456392], [-0.1022307, 51.5456758], [-0.1022377, 51.5457336], [-0.1022471, 51.5458995]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6384726979, 6422173271, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;4&quot;, &quot;length&quot;: 36.107, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: [43585216, 969804220], &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022887, 51.5460572, -0.1022214, 51.5460609], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022887, 51.5460609], [-0.1022214, 51.5460572]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6384753132, 9503835780, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 4.672, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 685336980, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1024021, 51.5460393, -0.1022887, 51.5460609], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022887, 51.5460609], [-0.1024021, 51.5460393]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6384753132, 6422173274, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk_link&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 8.201, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 1031053442, &quot;ref&quot;: null, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022214, 51.5460572, -0.1021217, 51.5461176], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1021217, 51.5461176], [-0.1021446, 51.5461061], [-0.1021749, 51.5460868], [-0.1022214, 51.5460572]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6384755933, 9503835780, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;primary&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 9.635000000000002, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;St. Paul\u0027s Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 194329112, &quot;ref&quot;: &quot;A1199&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022471, 51.5458995, -0.1022214, 51.5460572], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022471, 51.5458995], [-0.1022293, 51.5459814], [-0.1022237, 51.5460377], [-0.1022214, 51.5460572]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6422173271, 9503835780, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;3&quot;, &quot;length&quot;: 17.636, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 685336983, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1024021, 51.5458995, -0.1022471, 51.5460393], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022471, 51.5458995], [-0.102267, 51.5459136], [-0.1022972, 51.5459544], [-0.1023232, 51.545979], [-0.102354, 51.546006], [-0.1024021, 51.5460393]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6422173271, 6422173274, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 19.011, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 685336984, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022471, 51.5455763, -0.1022056, 51.5458995], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022471, 51.5458995], [-0.1022377, 51.5457336], [-0.1022307, 51.5456758], [-0.1022233, 51.5456392], [-0.1022133, 51.5456026], [-0.1022056, 51.5455763]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6422173271, 6384726979, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;4&quot;, &quot;length&quot;: 36.107, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: [43585216, 969804220], &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1025251, 51.5460393, -0.1024021, 51.5461077], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1024021, 51.5460393], [-0.1024529, 51.5460465], [-0.1025251, 51.5461077]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(6422173274, 221736, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 12.043, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 43585214, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022056, 51.5451497, -0.1019387, 51.5455763], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1019387, 51.5451497], [-0.101967, 51.5451818], [-0.1019907, 51.5452138], [-0.1019968, 51.5452253], [-0.1020102, 51.5452467], [-0.1020826, 51.5453716], [-0.1021137, 51.5454163], [-0.1021366, 51.5454557], [-0.1022056, 51.5455763]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(8974355097, 6384726979, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;primary&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 50.961000000000006, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Canonbury Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: [686204225, 18017473, 686188420, 685541830, 228479564, 686188432], &quot;ref&quot;: &quot;A1200&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1024788, 51.5461077, -0.1021217, 51.5461299], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1024788, 51.5461077], [-0.1024126, 51.5461299], [-0.1023667, 51.5461288], [-0.1022434, 51.5461267], [-0.1022047, 51.5461267], [-0.1021217, 51.5461176]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(9503835778, 6384755933, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk_link&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 25.41, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: [1031053441, 2418014], &quot;ref&quot;: &quot;A1199&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1024788, 51.5460609, -0.1022887, 51.5461077], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1024788, 51.5461077], [-0.1024094, 51.546093], [-0.10238, 51.5460869], [-0.1023319, 51.5460801], [-0.1022887, 51.5460609]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(9503835778, 6384753132, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 14.296000000000001, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: true, &quot;osmid&quot;: 1028658849, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022887, 51.5460572, -0.1022214, 51.5460609], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022214, 51.5460572], [-0.1022887, 51.5460609]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(9503835780, 6384753132, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;1&quot;, &quot;length&quot;: 4.672, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 685336980, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: true}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022214, 51.5460572, -0.1021217, 51.5461176], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022214, 51.5460572], [-0.1021749, 51.5460868], [-0.1021446, 51.5461061], [-0.1021217, 51.5461176]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(9503835780, 6384755933, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;primary&quot;, &quot;lanes&quot;: &quot;2&quot;, &quot;length&quot;: 9.635, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;St. Paul\u0027s Road&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 194329112, &quot;ref&quot;: &quot;A1199&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}, {&quot;bbox&quot;: [-0.1022471, 51.5458995, -0.1022214, 51.5460572], &quot;geometry&quot;: {&quot;coordinates&quot;: [[-0.1022214, 51.5460572], [-0.1022237, 51.5460377], [-0.1022293, 51.5459814], [-0.1022471, 51.5458995]], &quot;type&quot;: &quot;LineString&quot;}, &quot;id&quot;: &quot;(9503835780, 6422173271, 0)&quot;, &quot;properties&quot;: {&quot;__folium_color&quot;: &quot;yellow&quot;, &quot;highway&quot;: &quot;trunk&quot;, &quot;lanes&quot;: &quot;3&quot;, &quot;length&quot;: 17.636, &quot;maxspeed&quot;: &quot;20 mph&quot;, &quot;name&quot;: &quot;Highbury Corner&quot;, &quot;oneway&quot;: false, &quot;osmid&quot;: 685336983, &quot;ref&quot;: &quot;A1&quot;, &quot;reversed&quot;: false}, &quot;type&quot;: &quot;Feature&quot;}], &quot;type&quot;: &quot;FeatureCollection&quot;});



    geo_json_c3baf7476b0055375c8f195a682a55de.bindTooltip(
    function(layer){
    let div = L.DomUtil.create(&#x27;div&#x27;);

    let handleObject = feature=&gt;typeof(feature)==&#x27;object&#x27; ? JSON.stringify(feature) : feature;
    let fields = [&quot;osmid&quot;, &quot;oneway&quot;, &quot;lanes&quot;, &quot;ref&quot;, &quot;name&quot;, &quot;highway&quot;, &quot;maxspeed&quot;, &quot;reversed&quot;, &quot;length&quot;];
    let aliases = [&quot;osmid&quot;, &quot;oneway&quot;, &quot;lanes&quot;, &quot;ref&quot;, &quot;name&quot;, &quot;highway&quot;, &quot;maxspeed&quot;, &quot;reversed&quot;, &quot;length&quot;];
    let table = &#x27;&lt;table&gt;&#x27; +
        String(
        fields.map(
        (v,i)=&gt;
        `&lt;tr&gt;
            &lt;th&gt;${aliases[i]}&lt;/th&gt;

            &lt;td&gt;${handleObject(layer.feature.properties[v])}&lt;/td&gt;
        &lt;/tr&gt;`).join(&#x27;&#x27;))
    +&#x27;&lt;/table&gt;&#x27;;
    div.innerHTML=table;

    return div
    }
    ,{&quot;className&quot;: &quot;foliumtooltip&quot;, &quot;sticky&quot;: true});


            geo_json_c3baf7476b0055375c8f195a682a55de.addTo(map_de53e3ae203808cdc89070f967684ddf);

&lt;/script&gt;
&lt;/html&gt;" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



Through the map displaying street Betweenness Centrality above, along with the corroborating evidence from the two local interactive maps, it is evident that Betweenness Centrality to some extent effectively illustrates the relationship between retail locations and the road network. Specifically, retail establishments tend to be situated in close proximity to streets with higher Betweenness Centrality.
