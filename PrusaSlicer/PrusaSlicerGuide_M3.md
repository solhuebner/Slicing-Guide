## Slicing muti-color for M3 hotend 
[**:movie_camera: Video Tutorial**](https://github.com/ZONESTAR3D/Slicing-Guide/assets/29502731/339a212b-4aa4-4eee-bbde-8eee52f29974)    
### Step 1: choose printer presets "Z8 + M3 hotend"
![](pic/M3-1.jpg)
### Step 2: load 3d model files (stl/obj/AMF file etc.)
![](pic/M3-2.jpg) ![](pic/M3-3.jpg)
- :memo: Usually, "split model" is inneed to print multi colors 3d model files, that is, a 3d model has been split into multiple STL files according to colors, and these files use the same origin coordinate position so that they can be merged correctly.
- :star2: PrusaSlicer has a powerful new feature, it can paint a 3d model file into multi colors, for details, please refer to :movie_camera: [**Slicing guide - Convert one color 3d file to multi colors**](https://youtu.be/Yx4fKDRGEJ4). 
### Step 3: Choose filament type and set filament color
![](pic/M3-4.jpg)
### Step 4: Assign extruders to different parts
![](pic/M3-5.jpg)
### Step 5: Resize, cut, rotate, move the 3d model if need 
![](pic/M3-6.jpg)  
### Step 6: Set the print settings 
#### :warning: Please note that the "Retraction when tool is disabled" should be set to 0.    
![](pic/switch_length.jpg)  
#### set layer height, print speed, support, infill, etc.
![](pic/slicing_set.png)  
You need to set these parameters according to the shape of the model and your requirements for print quality. Even for some models, printing cannot be completed normally without support. For details please refer to:
- :point_right: [**PrusaSlicer introduction**](https://help.prusa3d.com/article/general-info_1910)      
- :point_right: [**Slic3r User Manuual**](https://manual.slic3r.org/)      
  
### Step 7: Set parameters for "wipe tower"
You may notice that a square will appear in the sliced figure, which is called "Wipe tower" in PrusaSlicer. Because for the multi-color printer, while switching extruders, there are still the previous color filaments inside the hotend, it need to be clean before printing another color.   
![](pic/M3-7.jpg)        
In order to obtain better cleaning effect and minimize to waste filament, we can set the purging volume according to different colors. Please see the following table, the columns shows the previous extruder and the rows shows the next extruder to be printed. When we change from the extruder with lighter color filament to the extruder with darker color filaments, we can set a smaller "purging volume". On the contrary, when we change from the extruder with darker color filaments to the extruder with lighter color filament, we need to set a bigger "purging volume".  
![](pic/M3-8.jpg)  
### Step 8: Slicing
![](pic/slicing_go.png)  
### Step 9: Preview the sliced result (gcode file) and then save to gcode file to your PC and then copy to SD card
![](pic/slicing_save.png)  

-----
## How to print more than 3 colors using M3 hot end
[**:movie_camera: Video Tutorial**](https://github.com/ZONESTAR3D/Slicing-Guide/assets/29502731/879fdff6-e5f4-44a5-9bac-7ebabb8bf3ee)     

M3 hot end can mix 2 ~ 3 actual extruders filament to produce a new color filament, and this new color filament can be used as a new extruder (called **"virtual extruder"**), the operation steps are as follows:   
***The following example shows how to set 6 extruders - 3 actual extruders and 3 virtual extruders - to print a 6 color 3d model.***     
### Step 1: Add virtual extruders
![](pic/M3-9.jpg)  
:warning: Suggest to save the settings to a new profile.   

### Step 2: Set mix rate of the new "virtual extruder"
#### Add "Set mix rate" commands to "Start Gcode". 
![](pic/M3-10.jpg)  
:warning: Suggest that these g-codes be placed at the front of the "Start G-code". 
>
    ;Set mix rate
    ;Extruder #4 = 50% Extruder #1 + 50% Extruder #2
    M163 S0 P50
    M163 S1 P50
    M163 S2 P0
    M164 S3
    ;Extruder #5 = 50% Extruder #2 + 50% Extruder #3
    M163 S0 P0
    M163 S1 P50
    M163 S2 P50
    M164 S4
    ;Extruder #4 = 50% Extruder #1 + 50% Extruder #3
    M163 S0 P50
    M163 S1 P0
    M163 S2 P50
    M164 S5

#### :memo: Introduction to "M163" and "M164" commands
>
    M163: Set a single mix factor for a mixing extruder, must be followed by M164 to normalize and commit them.
     S[index]   The channel (actual extruder) index to set
     P[float]   The mix value from (0.0 ~ 100.0)
     R   		Reset all mixing extruder settings to default

    M164: Normalize and commit the mix rate to a virtual extruder.
     S[index]   The virtual extruder to store
  
    Normalize:  Automatically scale the mixing ratio values of each extruder to meet machine requirements

### Step 3: Assign the new virtual extruders to 3D model and slicing
Now you can assign 6 extruders to the 3D model, slicing process is exactly the same as the 3 extruders.      
:warning: Choose the **printer profile**<sup>1</sup> before slicing, and you can clicl the color block to set the **filament color**<sup>2</sup> of the new extruders.      
![](pic/M3-11.jpg)   

-----
## Appendix
### [:book: Mixing color feature use guide](https://github.com/ZONESTAR3D/Document-and-User-Guide/tree/master/Mixing_Color)
