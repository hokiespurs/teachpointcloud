# <u>**Introduction to UAV Mapping**</u>

Over the last decade, we have seen a dramatic increase in technology and interest in consumer grade Unmanned Aerial Vehicles(UAV/UAS).  Much of this interest has came from the desire to use these platforms for aerial photography and video making. To feed this need, manufactures made made the UAVs easier to fly, safer, and a very capable camera platform. Seeing an opportunity, hardware/software developers have developed very capable aerial mapping platforms. This document gives a brief overview of UAS data collection, processing, and web visualization.

### What is UAV Mapping?

UAV mapping is a generic term that is used to describe the use of an unmanned platform to produce 2D and 3D geospatial data. These data are generated at a much higher temporal and spatial resolution than previous methodologies at an inexpensive cost that makes it available to many researchers.  These products do include orthomosaic photos, elevation models, 3D pointclouds, and vegetation indexing (with the appropriate sensor).  There are many, many applications for UAS mapping.  A few examples are:

- Farm Mapping to look at plant health
- Landfills to look at volumes of trash
- Quarries to measure volumes of extracted rock
- Construction sites to look at progress and elevation gradients
- Forest mapping to look at plant abundance and health
- Beach erosion/accretion mapping
- [Mapping Coral Reefs](https://hokiespurs.github.io/buck-island-ortho-map/)

![mapping](images/mapping.jpg)

Knowing the location and orientation of the photo when it is taken allows, the mapping software to perform the photogrammetry process. **Photogrammetry** is the science of making measurements from photographs, especially for recovering the exact positions of surface points. Photogrammetry is as old as modern photography, dating to the mid-19th century and in the simplest example, the distance between two points that lie on a plane parallel to the photographic [image plane](https://en.wikipedia.org/wiki/Image_plane), can be determined by measuring their distance on the image, if the scale of the image is known.

Photogrammetric analysis may be applied to one photograph, or may use [high-speed photography](https://en.wikipedia.org/wiki/High-speed_photography) and [remote sensing](https://en.wikipedia.org/wiki/Remote_sensing) to detect, measure and record complex 2-D and 3-D motion fields by feeding measurements and [imagery analysis](https://en.wikipedia.org/wiki/Imagery_analysis) into [computational models](https://en.wikipedia.org/wiki/Computer_simulation) in an attempt to successively estimate, with increasing accuracy, the actual, 3-D relative motions. 

![ortho1](images/DEM1.jpg)

![ortho1](images/ortho1.jpg)

*Some of the types of products that are generated by UAV mapping.*



In basic terms, the software looks for identical pixels in multiple photos, considers where that image was taken in relation to the other photos, and reconstructs the surface based on this relationship. 



### Why is it beneficial?

At the end of the day, using a UAV for photogrammetry can be very beneficial for many different types of people. As a remote sensing platform, it is much safer and financially accessible to the scientific community than its manned aircraft counterpart. A $1000 dollar UAV is very capable of creating very detailed maps. In many cases, the mapping software is much more expensive than the actual UAV. But even then, the financial commitment is much less than a manned aircraft. Since this the case, most customers contract an outside company to perform the data collection process. In this case, the customer can perform this operations on their own. This gives them not only a temporal advantage, but in many cases a spatial resolution advantage as well. Outside of the scientific community, farmers and construction companies can benefit as well. These products are very accessible and fairly easy to generate. This allows these professionals to more effectively manage their crops or construction site, which should increase their profit margin.

![index](images/index.jpg)

![volume](images/volume.jpg)

*Indexed vegetation and volume calculation examples.*



### Intuition for how the algorithm works

**Structure from Motion (SfM)** is the algorithm at the foundation of the data processing.  This algorithm uses photogrammetry and computer vision to solve the camera positions and a pointcloud of the scene.  The algorithm relies on tracking keypoints in multiple images to infer depth, much like how our eyes compute depth.  In the figure below, 3 images represented by pyramids show how rays intersect at the true point on the box.

![box](images/sfm.png)



Texture is very important for SfM.  For example, this spinning cube below looks like it has a flat top, but it's hard to say because there is no texture.

![no texture](images/cubenotex.gif)

But when we add texture to the same "cube", as shown below, we can get a much better picture of the shape of the object.  When processing imagery using SfM, this same principal is very important.  If there is no "texture" to the object, then there are no keypoints to track, and the 3D model will be poor.

![no texture](images/cubetex.gif)

Another assumption is that the surface is not shiny.  The color associated with each pixel must come from the surface of the object, and not a reflection of another object.  For example, the Bean in Chicago would not work for SfM processing.

![no texture](images/bean.jpg)



### How to collect data 

If you are interested in flying a UAS for mapping, it is highly recommended that you take a class to familiarize yourself with UAS safety and legal regulations.  At the time of writing this document (May, 2018), the FAA requires a drone pilot to be certified under Part107 by taking a test at an FAA testing site in order to perform **commercial operations**.  For recreational UAS flying, no formal tests are required, though an online class or video is highly recommended.   When conducting UAV mapping in the name of research while affiliated with OSU, the pilot must complete a flight safety course.  The regulations at OSU are constantly updated and evolving, so consult the [research office](https://research.oregonstate.edu/unmanned-systems-initiative) for the most up to date restrictions.

##### Flight Planning

First, the pilot must determine if the desired location can be mapped legally. In many cases, the desired airspace could be restricted. Such as specific airspace for a nearby airport, National Park, and wildlife refuges. Additionally an organization, such as the USFS, may require additional approval to perform a flight in the name of research.

Careful thought needs to be given when planning the flight for the data collection. The pilot needs to consider what is the goal of the flight. Is it to generate a 3D model or 2D model? And if it is a 3D model, is it one object or multiple objects? The answer to this question determines what kind of flight profile will be needed. 

For a 2D model, a simple parallel grid pattern works best. The altitude and photo overlap will determine the level of resolution for the model.

![2D_grid](images/2D_grid.jpeg)



For a 3D model of one object, performing multiple altitudes will work the best. The benefit of flying multiple orbits is that it gives the software a better perspective of the sides of the particular object.  

![3d_grid](images/3d_grid.jpeg)



For a 3D model of multiple objects, the pilots best profile option is performing two parallel grid patterns that intersect each other at a perpendicular angle. In the end, it most likely will not render as good of a model as the single object, but this is a sacrifice that will have to be made when modelling multiple objects.

![3d_gridmulti](images/3d_gridmulti.jpg)



##### Time of Day and Weather

The pilot needs to consider the time of day the flight will be conducted and what the weather may be like. The orientation of the sun will not only affect how much of shadowing an object will have, but also how much light saturation the images will have as well.

![DJI_0027](images/DJI_0027.JPG)

![DJI_0030](images/DJI_0030.JPG)

*Photos illustrating the affects of sunlight orientation*



The weather also affects the effectiveness of a flight as well. First, consumer UAVs are not attended to fly in inclement weather. Second, if the cloud cover is scattered, it can affect the constancy of lighting and ultimately the image quality of the model. However, if the clouds provide a solid overcast, it may be satisfactory conditions. That being said, it should be anticipated that the images will be darker.![DJI_0021](images/DJI_0021.JPG)



Ultimately, the best time to perform the flight is in clear conditions and with the sun at its highest point in the sky.



##### Safety

A pilot is required to consider some safety hazards in order to ensure that the flight is legally and safely executed. The below image provides the basics of the FAA requirements. It is important to realize that these are the basics of recreational use. When conducting flights for research or commercial application, there are many other requirements that most be made.![FAA](images/FAA.JPG)



Additionally, the pilot must ensure that the flight profile is sufficiently above all obsticales in the area. This is to ensure that the UAV does not run into an object when it is flying its grid pattern. Finally, the pilot should also consider the wind conditions not only at the surface, but at the profile altitude as well.



##### Desired accuracy

Depending on the application of the model data, the accuracy must be considered. If it is just for general use or to make a basic basemap, the GPS accuracy of the UAV may be satisfactory. However, if accuracy is critical to the purpose of the model, then the inclusion of ground control points (GCP) must be considered.![gcp](images/gcp.jpeg)



### Performing the flight profile

The type of profile (2D vs 3D) and the pilots proficiency will ultimately determine how it is executed. If the profile is for a 3D model and in a tight setting, the pilot may decide to fly the profile manually. However, in most cases, most profiles can be completed through the use of a automated application from their smart device. These applications are intended to be used with their companion software package. When utilized, the profile is programmed through the smart device and is flown completely automatically when executed. After profile completion, regardless of execution, is uploaded to the desired software. From there, the image processing may begin. Remember to keep the UAV in sight at all times and be ready to take manual control if necessary. 

![IMG_1854](images/IMG_1854.PNG)

![IMG_1855](images/IMG_1855.PNG)

*Interface of the Pix4D Capture (top) and DroneDeploy (bottom) applications*



### Photo Processing

When profile is completed, the processing steps varies dramatically depending on the individual software package. Some options are completely cloud based and allow for a simplified process. Additionally, because of the extremely high amount of computer processing that is required, a cloud based option allows you to free up your personal workstation. Other options provide the user much more control over the processing, but require more time and attention to detail. 

In most cases, the first step is uploading the images to the software. Next, all the photos must be aligned and orientated. When this step is performed, the software identifies specific points (such as sharp points or edges) which renders a primitive point cloud.

![align](images/align.PNG)



Next, the software will construct a dense point cloud. This step, in most cases, is the most time consuming in terms of shear processing time. Depending on the number of photos and selected settings, this process could easily take over a day. Once created, the software will be able to generate a 3D model and digital elevation model. Finally, once the elevation model has been generated, the orthophoto may be constructed.

![dense](images/dense.PNG)

![tiled](images/tiled.PNG)

![dem](images/dem.PNG)

![ortho](images/ortho.PNG)

*Images of dense cloud (top), 3D model (middle-top), DEM (middle-bottom), and orthophoto (bottom)*

Remember that the processing time is greatly affected by the number and file size of photos, computer hardware, and the user settings.



### Exporting

##### 3D Pointcloud

A pointcloud should be output as a `.las` file.

##### 2D Imagery

2D data products (orthophoto, dsm), should be output as a `.tiff`  in the desired coordinate system.

If you want to make a web map, Agisoft Photoscan allows you to export 2D data products as tiles, which are easily added as a layer using leaflet.

##### 3D Mesh

A 3D mesh is most often exported as a `.obj` file

![qgis](images/qgis.PNG)

*DEM created from UAV loaded into QGIS*



### Popular Software Options

https://pix4d.com/

https://www.dronedeploy.com/

http://www.agisoft.com/

http://www.esri.com/products/drone2map



### Videos Illustrating Capabilities

https://www.youtube.com/watch?v=AOXmju2bCpQ

With a multispectral camera:

https://www.youtube.com/watch?v=XF306Hp6Q4I

## Web Visualization of UAS data

### 3D Pointcloud

The 3D pointcloud data can be visualized in modern web browsers using webGL.  [Potree](http://potree.org/) is a javascript library that enables the visualization of millions of points.  A windows binary executable to generate octrees for the pointcloud data and make a basic web page is on github [here](https://github.com/potree/PotreeConverter).

![potree](images/potree.gif)

A basic script to generate a webpage using PotreeConverter is shown below.  This will generate a webpage named "pageName.html" in the `C:/potree_converted` directory.  This webpage will contain the data from `data.las`.  For more detailed examples involving coordinate systems and custom settings, see the [PotreeConverter](https://github.com/potree/PotreeConverter) github repository.

```
./PotreeConverter.exe C:/data.las -o C:/potree_converted -p pageName
```

### 2D Orthophoto or DSM

Tiles can easily be referenced to overlay on a basemap using leaflet.  Here is an example map hosted on the OSU Engineering server.

![buck island](images/buck.gif)

This example uses the following javascript code:

```js
L.tileLayer('http://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',{
    minZoom: 10,
    maxNativeZoom: 21,
    maxZoom: 23
}).addTo(mymap);

var tiles = L.tileLayer('http://research.engr.oregonstate.edu/lidar/pointcloud/20180319_USVI/googlemaps/20180325_MavicBuckIsland/{z}/{x}/{y}.png',{
    minZoom: 10,
    maxNativeZoom: 22,
    maxZoom: 23,
    noWrap: true,
    bounds: mybounds,
    attribution: '<div id="credits" style="display:inline">UAS Imagery acquired by Oregon State University in collaboration with NOAA and NPS | Basemap &copy; ArcGIS | Made By Richie Slocum | <a href="https://github.com/hokiespurs/usvi-noaa-data"><i class="fa fa-github"></i> More Info </a></div>'
}).addTo(mymap);
```

### Conclusion

The area of UAV mapping is rapidly growing. Manufactures and software developers alike are rapidly advancing the technology and capabilities of the platforms. For many of us in the geospatial analysis community, these platforms can provide us an ease of access to legitimate remote sensing platforms. Just remember to educate yourself prior to attempting to ensure that you are doing so safely and legally. 

