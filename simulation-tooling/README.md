# Simulation Tooling Support for Event Cameras
The objective of this document is to consolidate information about all of the various simulation-tooling options out in the larger development community for event cameras. A significant amount of work has been done across a number of different organizations; there are some existing documents out there with comprehensive summaries of every single resource available for event cameras, however this will serve to highlight only those resources related to simulation. 

# Overview
There has been some open-source work on supporting event camera simulation, the most notable/reliable being that of ESIM from University of Zurich's Robotics and Perception Group (RPG). Notable open source robotics simulators which seem to support event camera simulation are CARLA and ESIM; Gazebo classic had some support years ago, but since the upgrade to Ignition, the DVS camera plugin did not get migrated, so support seems to be floating at this point for Gazebo. 

The main approaches today working with event cameras without the real hardware tend to revolve around either a.) simulation whereby the event camera frames are generated using a combination of real-hardware inspired pixel activation thresholding, rgb frames input, and inter-frame interpolation using different techniques (optical flow, scene dynamics, known camera trajectories, etc. etc.), or b.) post-processing techniques which take as an input and as an output via the conversion process (and tuning) a stream of event-camera frames.   


# Simulation Platforms with Existing Event Camera Simulation Support

- [rpg_esim](https://github.com/uzh-rpg/rpg_esim) (__Supported__)  
    - Unreal Engine based  
    - Designed to accurately model an event camera output given a 3D scene and a trajectory as inputs. Introduced a novel adaptive rendering approach to efficiently leverage the scene dynamics to interpolate and meet the high-frequency frame targets that real event cameras can require.  
- [CARLA](https://github.com/carla-simulator/carla/blob/ue5-dev/Unreal/CarlaUnreal/Plugins/Carla/Source/Carla/Sensor/DVSCamera.cpp) (__Supported__) 
    - Added in CARLA 0.9.10  
    - Frame-based approximation of a true event camera  
    - Sim-to-real gap is real and [problematic](https://arxiv.org/html/2506.13722v1) due to the fact simulation support is always using rgb frames as the source and doing some sort of conversion.  
        - Some proposals on how to improve simulation performance multiple ordesrs of magnitude have been made, this paper as one [example](https://arxiv.org/pdf/2209.04634)  
            - Generally approaches to "improve" event camera simulation leverage somethign like optical flow and fast linear interpolation to artificially increase the frame rate, which this paper leverages.  
- Gazebo (__Unsupported__) 
    - [DVS Gazebo Classic plugin](https://github.com/HBPNeurorobotics/gazebo_dvs_plugin) - Old, deprecated
    - ["Modern" Gazebo](https://github.com/gazebosim/gz-sensors/tree/gz-sensors10) - Not supported/gazebo classic support not migrated, doesn't seem to have a .cc for dvs camera  
        - Closest thing to directly generating event camera frames from the gazebo rendering, seems to do the inter-frame comparison within the plugin rather than using a pipeline of separate processes to generate event frames  
    - [Arizona State University "SEBVS" Project, Aug' 25](https://eventbasedvision.github.io/SEBVS/) - [Arvix link](https://arxiv.org/abs/2508.17643)
        - Provides an open-source ROS2 package for the Gazebo simulator that generates these event streams from standard RGB cameras  
        - Still takes RGB frames and converts them to event camera frames, effectively a ROS 2 node wrapping the SensorsINI v2e emulator 
        - Conversion is a secondary process from the initial rgb frame production (i.e. need to start ros2 process, subscribe to rgb frames already being published from gazebo to a ros2 topic, convert, re-publish)   
- NVidia Isaac Sim (__Unsupported__)  
    - Event camera sensor directly/natively in isaac sim seems unsupported, but recently some work on taking camera frames from Isaac Simulation and pushed them through an existing v2e toolchain - [link](https://arxiv.org/html/2503.04838v1#:~:text=This%20paper%20presents%20a%20simulation,To%20validate%20both%20this)  
- Vista 2.0 [paper](https://arxiv.org/abs/2111.12083) - [Documentation site](https://vista.csail.mit.edu/)  - (__Supported__)  
    - Supports an event camera model which uses RGB frames as the basis for synthesizing event camera frames. Supports two types of operation:  
        1. synthesize event frames using rgb frames and interpolating and generating event pixels, very similar to the typical approaches seen in prior work  
        2. Instead of synthesizing, use the metavision_core.event_io.raw_reader.RawReader object from the metavision sdk and read a stream of event data in from the filesystem  
  
# Dataset Tooling (video-2-event) and Datasets  

## Dataset Tooling  

- [rpg_vid2e](https://github.com/uzh-rpg/rpg_vid2e)  
    -  Event camera simulation implementation in the Prophesee metavision SDK is based on this & the corresponding paper  
- [Prophesee Metavision SDK Video-to-Event Simulation API](https://docs.prophesee.ai/stable/api/python/core_ml/video_to_event.html#module-metavision_core_ml.video_to_event.simulator)  
    -  Implementation is based on the following publications:  
        - Video to Events: Recycling Video Datasets for Event Cameras: Daniel Gehrig et al.  
        - V2E: From video frames to realistic DVS event camera streams: Tobi Delbruck et al.  
- [Institute of Neuroinformatics (INI) V2E (Video 2 Event) Toolbox](https://openaccess.thecvf.com/content/CVPR2021W/EventVision/papers/Hu_v2e_From_Video_Frames_to_Realistic_DVS_Events_CVPRW_2021_paper.pdf) - [Code](https://github.com/SensorsINI/v2e)    
    - Brings improvements to the generation of synthetic datasets covering a range of illumination conditions, addressing the shortcomings of rpg_vid2e with generating event-data sets from video with non-ideal/poor lighting conditions  
    - v2eCore can be imported and used in python software, as is demonstrated in the SEBVS project out of ASU  
- v2e Tool hosted in google [colab](https://colab.research.google.com/drive/1czx-GJnx-UkhFVBbfoACLVZs8cYlcr_M?usp=sharing)  
    - Nice example of the limitations still of the v2e tool on realness of output of the v2e tool - [link](https://sites.google.com/view/video2events/home#h.p_P7CP9Cu4bygh)  

## Datasets  
- N-Caltech101 dataset - [download](http://rpg.ifi.uzh.ch/data/VID2E/ncaltech_syn_images.zip)  
- 