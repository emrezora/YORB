# YORB 2020 Workflow

This document will contain some of our workflow for creating and publishing models. To publish scenes to hubs, we are using [Spoke (by Mozilla)](https://hubs.mozilla.com/spoke).  This project was built in tandem with [Hubs (by Mozillla)](https://hubs.mozilla.com/#/), the platform we are using to host and run our scene. Spoke is not a modelling software, but allows us to place models, light them, add spawn points for users, create a walk-able floor plan, etc.  Spoke allows models to be uploaded as `.glb` files - a 3D file format known as [GLTF](https://www.khronos.org/gltf/) which is optimized for the web.

The floor model is built in the [Rhino3D](https://www.rhino3d.com/) software package, which does not contain a GLTF exporter.  As such, we are using [Blender](https://www.blender.org/) to export a GLTF file.  The current workflow is as follows:

## Rhino3D 🏗🏗🏗

* create floor model 
* export model as MotionBuilder(FBX) as meshes

## Blender 🎨🖌🖼
* import MotionBuilder(FBX) file
    * Use `Manual Orientation` with +Z Up 
* apply [GLTF compliant materials](https://docs.blender.org/manual/en/2.80/addons/io_scene_gltf2.html) to meshes
* export as GLTF Binary files (i.e. `.glb` file).  Note: it seems that a single `.glb` file cannot cast shadows on itself in Spoke.  So, in order for Spoke to create lighting / shadows maps for the imported objects, it is necessary to export separate `.glb` files for each shadow casting / shadow receiving object. I export the following sets of meshes as `.glb` files: 
    * floor
    * ceiling
    * I-beams
    * all walls
    * all glass (these won't cast shadows)
    * everything else (these won't cast shadows)

## Spoke 💡🚶‍♂️🚶🏾‍♀️🕴🏼


#### Setting up a new Scene

* Log in to your Mozilla Spoke account and create an new project: Projects > New Project > New Empty Project
* The default view is the Terrain Crater1.glb, which you can delete from the Hierarchy panel on the right.

#### Importing Models

* To add your Blender files, click on `My Assets` in the bottom pane, and `Upload` or drag-and-drop your Blender `.glb` files. Then, drag them one at a time into your Spoke scene or right-click on each of them to place it at the origin.

#### Lighting
* Each `.glb` file you place must be marked as a 'shadow casting' or 'shadow receiving' object or both.  The floor should receive shadows only.  The walls should cast shadows and receive shadows.  The ceiling should cast shadows only.  Use your judgement here.
* Angle and color your directional light to reflect the time of day for your scene. Mark this light as shadow casting and set the size of the shadow map

#### User Navigation
* User navigation is managed by a floor plan, which must be regenerated once the models have been imported. 
    * Set the floor to be walkable
    * Set the walls / glass / etc. to be both walkable and collidable.
    * make sure things in the air (ceiling, lights, etc.) are neither walkable nor collidable or you get strange behavior
    * click `Regenerate` on the floor plan and look at the results

#### Populating the Scene
* This is the time to add additional Spoke assets, such as furniture, audio files (YORB Kokomo and jack hammers) and the YORB videos from YouTube!

#### Publishing the Scene to Hubs
* GOTCHA: it appears there is a bug where you need to be in `Shadows` mode in the Spoke viewport in order for your exported scene to contain shadows. There is a dropdown on the top right of the Spoke viewport where you can set the current render mode to `Shadows`...
* When you finish, publish the scene to Hubs.  Note that the Scene is a template from which playable rooms can be created.

## Hubs / Discord 💬🤖
* To create a room from a Scene template, and have that room linked to a Discord channel, go to the channel and type the following command to the Hubs Discord Chatbot:

    ```
    !hubs create https://hubs.mozilla.com/scenes/MY_SCENE_ID MyRoomName
    ```

    This will link the room to the Discord channel (and events happening in the Hubs Room will be simul-cast to the Discord  channel)
