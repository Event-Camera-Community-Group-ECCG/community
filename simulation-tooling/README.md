# Simulation Tooling Support for Event Cameras
The objective of this document is to consolidate information about all of the various simulation-tooling options out in the larger development community for event cameras. A significant amount of work has been done across a number of different organizations; there are some existing documents out there with comprehensive summaries of every single resource available for event cameras, however this will serve to highlight only those resources related to simulation. 

# Simulation Platforms with Existing Event Camera Simulation Support

- [rpg_esim](https://github.com/uzh-rpg/rpg_esim) (__Supported__)  
    - Unreal Engine based  
- [CARLA](https://github.com/carla-simulator/carla/blob/ue5-dev/Unreal/CarlaUnreal/Plugins/Carla/Source/Carla/Sensor/DVSCamera.cpp) (__Supported__) 
    - Added in CARLA 0.9.10  
- Gazebo (__Unsupported__) 
    - [DVS Gazebo Classic plugin](https://github.com/HBPNeurorobotics/gazebo_dvs_plugin) - Old, deprecated
    - ["Modern" Gazebo](https://github.com/gazebosim/gz-sensors/tree/gz-sensors10) - Not supported/gazebo classic support not migrated, doesn't seem to have a .cc for dvs camera    
- NVidia Isaac Sim (__Unsupported__)  
  
# Dataset Tooling (video-2-event) and Datasets  

## Dataset Tooling  

- [rpg_vid2e](https://github.com/uzh-rpg/rpg_vid2e)  
    -  Event camera simulation implementation in the Prophesee metavision SDK is based on this & the corresponding paper  
- [Prophesee Metavision SDK Video-to-Event Simulation API](https://docs.prophesee.ai/stable/api/python/core_ml/video_to_event.html#module-metavision_core_ml.video_to_event.simulator)  
    -  Implementation is based on the following publications:  
        - Video to Events: Recycling Video Datasets for Event Cameras: Daniel Gehrig et al.  
        - V2E: From video frames to realistic DVS event camera streams: Tobi Delbruck et al.  
- [Institute of Neuroinformatics (INI) V2E (Video 2 Event) Toolbox](https://openaccess.thecvf.com/content/CVPR2021W/EventVision/papers/Hu_v2e_From_Video_Frames_to_Realistic_DVS_Events_CVPRW_2021_paper.pdf)  
    - Brings improvements to the generation of synthetic datasets covering a range of illumination conditions, addressing the shortcomings of rpg_vid2e with generating event-data sets from video with non-ideal/poor lighting conditions  


## Datasets  
