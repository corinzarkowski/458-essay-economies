# US Metropolitan Economies Web Map - Critique and Analysis

### Corin Zarkowski

---

## Overview
Tyler Hegwood’s web map of United States metropolitan economies exists to highlight both the primary means of economic production and their geographic presences throughout the country. On first glance, the user is visually directed towards his map—due to his use of vibrant colors and omittance of a basemap, his comparison of raw data is aesthetically appealing while also an effective representation of data. Even more important than his map is his sidebar. A panel containing helpful tools and documentation occupies the left third of the screen, allowing the selection of individual industries to compare (he provides the ability to view two at a time), a slider for the year of data in question, and individual numbers and graphs for further insight. Locations on the map are represented by differently sized circles, with larger circles showing greater GDP’s. The end-user is not limited to the information on the sidebar though; while hovering over a location will change the information on the sidebar, Hegwood also provides clickable pop-ups on each and every location. Information on the pop-ups is the same as that on the sidebar, but in a different format. This could be useful for quick, surface level analysis for those not willing to spend extra time sorting through the more in-depth data on the sidebar.

PICTURE

## Purpose
The information provided by Hegwood through his web map serves many possible uses through its dimensionality. His leading question, “What makes up local economies across the country?”, is broad to the point of encompassing many other possible insights. While Hegwood clearly intends for his map to be used in cross-location examination, the implementation of detailed statistics, different industries, and the year slider can all be used in tandem for the close analysis of singular locations across industries over time. Of course, Hegwood’s web map carries much more potential than this. A user could easily examine the relative differences in economic growth between two metropolitan areas and their industries with just a quick choice of settings and a turn of the year slider—this airtight functionality and robust feature-set gives the map insane potential for in-depth analysis, saving plentiful time of both information gathering and sorting.

## Major functions
A robust tool such as this web map relies on a multitude of dependencies and backend functions. Hegwood styles the entire site using barebones CSS and html; the bulk of his code is found in the vast use of UI listeners and functions controlling the drawing of the map circles themselves.
Three primary functions make up the majority of the web app’s composition: updateSymbols, infoWindow, and updateChart. Another large function, drawMap, mostly just takes geoJSON and uses leaflet functions to create the underlying map. The last large function, addUiListeners, makes calls to updateSymbols when sliders or selectors from the sidebar are used.
### updateSymbols
```js
data.eachLayer(function(layer) {
  var props = layer.feature.properties;
  var percent = Number(props[sectorData + currentYear]/props['TOT_'+currentYear])*100;
  var realGDP = Number(props[sectorData + currentYear]);
  var radius = calcRadius(realGDP);
  layer.setRadius(radius);
  allRadii.push(radius);
            
  layer.bindPopup("<b>" + props.metro + ',' + props.state + "</b><br>" + currentYear + "<br>" + '$'+(realGDP*1000000).toLocaleString() + "<br>" + percent.toFixed(2) +'% of total     GDP');
}
```
This function primarily works to assign values to each circle on the map, then takes said values and binds a popup to each circle. It also uses the calcRadius helper function while determining the circle size—a vital piece of the map’s visualizations.
### infoWindow
While this function is quite verbose in that it occupies over 100 lines of the web app’s code, its purpose is only to listen to the user’s mouse activity and show relevant info on the sidebar. infoWindow accomplishes this through use of vanilla Javascript event listeners on mouseover and mouseout for both the main and comparable industry sector datasets.
### updateChart
Just as infoWindow tends to the raw numbers appearing in the sidebar, updateChart works with the graph in the bottom of the info window. I’m not sure why updateChart is in a separate function from infoWindow, since they both appear to be listening for mouse movement and are utilizing the same data.
Other functions
There appear to be many other functions currently commented out in the web map’s source. The option to filter map items by GDP was omitted, as well as a legend, a second year-slider for the comparative industry sector, and further chart control. In the UI, the option to choose metropolitan areas from a dropdown menu was removed. While going through the leaflet code, I noticed that a mapbox basemap was specified even though it does not appear on the final map. This could be due to a bad access token, but I appreciated the lack of basemap.

## MAP ELEMENTS
Interestingly, this web map does not include very many common map-design elements. Though code for a dynamic legend is included in the source, it is omitted in the final product. A scale bar was never even implemented in the source. This makes sense given the data-driven nature of the map, but some elements would have been nice to contextualize the data.

## AUDIENCE
I found this map on the University of Kentucky map showcase, though it is also available on the creator’s GitHub page. His potential audience could include amateur financial analysts or students looking for preliminary data for a project. While this web app has the potential to serve meaningful data in an easily digestible form, using it as a basis for further meaningful analysis would be difficult—however, as a less intensive use, even experienced workers in the finance sector could search for prominent economic trends easily and efficiently.

## AUTHORS
The author of this web map is Tyler Hegwood, a former GIS graduate student from the University of Kentucky. After looking into his other works and his LinkedIn, I found that he worked as a realtor while making this map, and now serves as a senior GIS research analyst at a prominent real estate agency. His inspiration and intentions while creating this project could have reasonably been drawn from his experiences with finance in the real estate sector.

## DATA
The data used in this web map is publicly available from the United States Bureau of Economic Analysis. The author does not seem to have reformatted it from its original state. The data itself is on GDP of US metropolitan areas sorted by sector, ranging from the years 2001-2013. Today, the same data is released in datasets from 2001-2019. Since this project was created in 2016, and the data from originally individual sets from 2001-2013, one can reasonably assume the data was collected in the years of its release, and the author downloaded it in 2016.
PICTURE

## SYSTEMIC ARCHITECTURE
The client-server relations of this project are reasonably straightforward—all the geographic and GDP data is stored via geoJSON, and does not occupy much space, allowing it be entirely stored and accessed simultaneously on the server without external databases. All data is received by the client at once, allowing smooth adjustment of sliders and the map’s scope. The source specifies a Mapbox basemap, which would require the transfer of data between the Mapbox server to the user, but this does not appear in the final project. All vector imagery for map locations is drawn dynamically using leaflet’s setRadius function.

## DESIGN CRITIQUE
While this web app provides robust and detailed tools for both visualizing and analyzing large amounts of data, it is severely lacking in its presentation. One could argue that an aesthetically pleasing design is not necessary in this sort of project, but I would disagree. The color palette and style behind using circles to represent GDP from different sectors is both visually pleasing and effective, but the barebones sidebar layout leaves much to be desired. The text size does not scale, making it nigh unreadable on high resolution monitors, and the app does not support responsive design whatsoever. Data provided in the sidebar is raw with near to no context, and the graph’s axes labels are dark gay on gray—completely unreadable. Even though the circles on the map itself are great for drawing comparison, the lack of legend renders them useless for anything beyond barely preliminary analysis. On the bright side, the dropdown menus for the selection of sectors are both easy to navigate and snappy, even though the labels of each are hard to read on the gray backdrop. The lack of basemap in this web map is interesting. The sheer amount of data and bountiful labels work well to contextualize the map on a broader scale, but someone not familiar with United States geography might suffer from navigation. Since the web map’s source did specify the use of a basemap, one cannot judge the map’s design too much for this though. However, the blue and purple palette for the map circles would be completely impossible to read for those who are colorblind—a glaring accessibility issue.

## PROS AND CONS
### PROS
- Snappy. 
- Bug free (I consider the lack of basemap a feature, not a bug)
- Intuitive for data analysis.
- Pretty design for the map itself.
### CONS
- Sidebar design is lacking.
- Lack of legend makes contextualization of data on the map itself difficult, even if comparison is easy.
- Color palette on the map may be bad for colorblind people.

## REFLECTION
From a social standpoint, this map is reasonably objective and well-made with little bias from its creator. One may argue that this map paints those in rural economies with a negative light, but this is alleviated by the clear transparency of this map’s sole use in analyzing metropolitan areas. It could still affect some uninformed viewers on this issue though, as agricultural GDPs appear low on this map of metropolitan areas due to their naturally rural situation. The massive amount of data and user configurability of this map lowers room for bias and prejudice from the map itself, but potentially acts as a double-edged sword in the hands of a malicious individual with the intent of propagating their agenda. So while this map does not symbolize any social theories itself, due to its clear presentation and configurability one could easily cherry-pick and purposely misinterpret an image from this web map in an attempt to manipulate people less experienced with critical map-reading into disdain for certain populations.
