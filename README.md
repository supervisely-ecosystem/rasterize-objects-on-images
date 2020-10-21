<div align="center" markdown> 

<img src="https://i.imgur.com/CLlwsFC.png"/>

# Rasterize objects on images
  
<p align="center">
  <a href="#Overview">Overview</a> •
  <a href="#How-To-Run">How To Run</a>
</p>

[![](https://img.shields.io/badge/slack-chat-green.svg?logo=slack)](https://supervise.ly/slack) 
![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/supervisely-ecosystem/rasterize-objects-on-images)
[![views](https://dev.supervise.ly/public/api/v3/ecosystem.counters?repo=supervisely-ecosystem/rasterize-objects-on-images&counter=views&label=views)](https://supervise.ly)
[![used by teams](https://dev.supervise.ly/public/api/v3/ecosystem.counters?repo=supervisely-ecosystem/rasterize-objects-on-images&counter=runs&label=used%20by%20teams)](https://supervise.ly)
[![runs](https://dev.supervise.ly/public/api/v3/ecosystem.counters?repo=supervisely-ecosystem/rasterize-objects-on-images&counter=downloads&label=runs&123)](https://supervise.ly)

</div>

## Overview 
During labeling objects are treated as layers. Objects order matters and defines final visualization. For example, objects rasterization is needed to train semantic segmentation model (every pixel on the image have to belong to a single class). 

Let's see the example below. First labeled object is road, second object is car. We labeled them separately. Now some image pixels (car) belong to two objects simultaneously. 

<img src="https://media.giphy.com/media/xOUlIfGk7kXlmcWwKb/giphy.gif"/>

How mo make a hole in road object? 
**Option 1: during labeling**. Drawbacks: 
- It requires more clicks (car will be labeled twiсe - as a hole in the road and as ar itself) - even with a specially designed labeling tools. 
- It is hard to modify objects. If the car is labeled wrong, we have to modify both car and hole in the road. Therefore we make more clicks and spend more time.
- Sometimes it is impossible because annotation requirements may change over time. For example: at the beginning we labeled only `road` surfaces and then requirements changed - new class `car` have to be labeled too.   






### What is AnyShape class? 

Let's concider the following case: semantic segmentation of cars. You would like to label cars faster with SmartTool (NN that integrated to labeling interface and produces masks). If SmartTool produces inaccurate predictions you will label objects manually with Polygonal tool. So, you want to label objects of class `car` with absolutely different instruments: Polygonal tool (vector) and SmartTool (raster). How to do it?

**First option**: Create two separate classes: `car_bitmap` with shape `Bitmap` and `car_polygon` with shape `Polygon`. Thus SmartTool objects will be of class `car_bitmap` and polygonal objects will be of class `car_polygon`. The main drawback: this approach doubles up the number of classes and later (before NN training) you have to merge class pairs to a single one anyway. And what if you have tens or even hundreds of classes?

**Second option**: Create class `car` with shape `AnyShape`. It means that you can use all annotation instruments to label objects. The main drawback: your labeling team have to understand annotation requirements well. For example, if labelers are not restricted to specific instrument, you expect polygons but they can label objects with rectangles. `¯\_(ツ)_/¯`


## How To Run

### Step 1: Run from context menu of project / dataset

Go to "Context Menu" (images project or dataset) -> "Run App" -> "Transform" -> "Unpack AnyShape Classes"

<img src="https://i.imgur.com/r8AlpZC.png" width="600"/>

### Step 2:  Waiting until the task finishes

Once app is started, new task appear in workspace tasks. Monitor progress from "Tasks" list.

<img src="https://i.imgur.com/JqHh9pZ.png"/>

## Explanation

- Result project name = original name + "(without AnyShape)" suffix

- Your data is safe: app creates new project with modified classes and objects. The original project remains unchanged

- All "AnyShape" classes will be unpacked to equivalent classes with strictly defined shapes. In our example the class `car` will be splited to classes `car_polygon`, `car_bitmap`, `car_rectangle`. 

- Colors of new classes will be generated randomly

