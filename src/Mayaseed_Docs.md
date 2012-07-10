Mayaseed docs
=============


About Mayaseed
--------------

###What is Mayaseed?###

Mayaseed is a plugin for Maya that exports geometry, light and materials to the Applsessed renderer. Mayaseed has been tested and runs on Maya 2010 or newer, it should also run on older versions but has not been tested. 

###What is appleseed?###

appleseed is an open source physically based raytracing renderer that does a whole bunch of cool things that a lot of other open source renderers don't. Take a look here.

http://appleseedhq.net/

###Mayaseed Licence###

Mayaseed is released under the MIT licence
Like Appleseed Mayaseed is released under the MIT licence which means its free to use and modify to your hearts content. 

###How it's made###

Mayaseed is Written mainly in Python with a little bit of MEL so its cross platform and easy to modify. You can find more about the structure of the plugin in the **Plugin structure** section. New releases will be announced on the Appleseed website here:

http://appleseedhq.net/blog/jon

You can also download the latest development snapshot from the appleseed gitHub page here:

https://github.com/jonathantopf/Mayaseed

Because Mayaseed is only written by one person ([Jonathan Topf](http://www.jonathantopf.com/)) and that person is not a professional developer by any stretch of the imagination you may encounter a bug from time to time. If you do find a bug file a report and ill try to fix it, heres the link for bug reports.

https://github.com/jonathantopf/Mayaseed/issues?direction=desc&sort=created&state=open


Installing Mayaseed
-------------------

To install Mayaseed just open 'open_me_to_install.ma' and it should guide you through the steps to install. If this doesnt work or you'd prefer to install in manually you just need to edit your userSetup.mel file which should be here:

    Mac: /Users/<username>/Library/Preferences/Autodesk/maya/<maya version>/scripts

    Windows Vista and higher: C:\Users\<username>\Documents\maya\<maya version>\scripts (you may have My Documents instead of Documents)

    Windows XP and lower: C:\Documents and Settings\username\My Documents\maya\<maya version>\scripts

    Linux: /usr/aw/userconfig/maya/<maya version>/scripts

If you don't have one of these files thats ok, just create a plain text file with a .mel extension and edit that.

So copy the following lines to your useSetup.mel file


    // mayaseed  --------------
    
    $env_script_path = `getenv MAYA_SCRIPT_PATH`;
    $env_plugin_path = `getenv MAYA_PLUG_IN_PATH`;
    putenv MAYA_SCRIPT_PATH ($env_script_path + "<mayaseed_root>/scripts");
    putenv MAYA_PLUG_IN_PATH ($env_plugin_path + "<mayaseed root>/plugins");


    // mayaseed  --------------


This just tells maya to search in your install directory for the Mayaseed plugin when it starts up. You will also want to replace any occurences of <mayaseed_root> with the path to your Mayaseed install directory, so for example I would replace the following line:

    putenv MAYA_SCRIPT_PATH ($env_script_path + ":<mayaseed root>/plugins");

with this:

    putenv MAYA_PLUG_IN_PATH ($env_plugin_path + ":/projects/mayaseed/plugins");


Finally you will want to start up Maya and enable the plugin, to do this choose **Window -> Settings Preferences -> Plugin manager** and load 'mayaseed.py', you will also want to click autoload so you don't have to do this step every time Maya loads.

That should be everything, if everything has gone to plan you should have a new menu called Mayaseed where you can create new ms_renderSettings nodes. This exporter is very much a work in progress and many improvements/features are planned for the future. Also please submit any bugs/feature requests as this is meant to be practical and usable software and I'd love to hear how it is being used.


Getting started
---------------

###The Mayaseed menu###

The Mayaseed menu will appear when the plugin is correctly installed, if you can't see it make sure the plugin installed correctly and is enabled.


###The Render Settings node###

The ms\_renderSettings (render settings) node is the workhorse of Mayaseed, it contains most of the settings to control your export and is also one of the places where you can launch the export. To export your scene you only need one ms\_renderSettings node but it is also possible to have many per scene, this can be useful for making proxy resolution renders or exporting for different render passes. 

>Note: The ms_renderSettings node's attributes are organised in a way that mirrors the internal file structure, so if an attribute seems like its in a strange place there is a good reason. By using Mayaseed you are also learning about appleseed at the same time.

###Your first export###

To export a scene you first need to create a render settings node, to do this choose **Mayaseed -> create render settings node**.

Now in the attribute editor you can Set up your export

>Note: if you deselect a render settings node then you can easily re select it by choosing Mayaseed -> Select render setting node

To set up your first export you will only really need to do two things; set up your output destination and choose your render camera. 

First you will need to set your output directory, and then choose a name for your output file, both attributes are in the export settings section of the renderSettings node. 

Next you need to choose your render camera in the output settings.

That should be all you need for your first export, not all you need to do is click the blue **Export** button at the top of your Render Settings node's attribute editor.


Node reference
--------------

This section contains information on the nodes that make up the Mayaseed plugin and their atributes.


###ms_renderSettings node###

The ms\_renderSettings (render settings) node is the biggest node in the Mayaseed plugin and contains most of the attributes that you will use to control your export. 

It is also one of the places where you can start your export from, at the top of the render settings node's attribute editor there is a blue button marked **Export** use this button to start your export.

The following section is a list of attributes in the render settings node and a description of their functions.

>Note: The ms_renderSettings node's attributes are organised in a way that mirrors the internal file structure, so if an attribute seems like its in a strange place there is a good reason. By using Mayaseed you are also learning about appleseed at the same time.


####Export settings section####

#####Output Directory#####

This is where you tell Mayaseed where to export the appleseed scene file and other relevant files to.

#####Output File#####

This is where you choose the name of your output file, the `#` character will be replaced with the frame number padded to 5 characters. So `my_scene.#.appleseed` will export as my_scene.00001.appleseed`.

#####Convert Shading Nodes To Textures#####

Use this checkbox to convert maya shading networks to textures on export. If a color attribute has a shading network attached Mayaseed will check to see the shading network is a texture node, if not with this option checked Mayaseed will bake the shading network to an exr image. 

#####Overwrite Existing EXRs#####

If this checkbox is checked then Maya will convert and overwrite texture files with every export.

#####Export Motion Blur#####

This attribute is only a placeholder at this point.

#####Shutter Open Time#####

This attribute is only a placeholder at this point.

#####Shutter Close Time#####

This attribute is only a placeholder at this point.

#####Export Animation#####

This attribute is only a placeholder at this point.

#####Animation Start Frame#####

This attribute is only a placeholder at this point.

#####Animation End Frame#####

This attribute is only a placeholder at this point.


####Environment Settings section####

#####Environment#####

A Maya scene can contain many environment nodes, here you can select which environment node to use in your export and also create new ones.


####Camera Settings Section####

#####Export All Cameras#####

Although appleseed can only use one camera at a time it is possible to have more than one included in the scene file, use this checkbox to export all the maya cameras. 

#####Export All Cameras As Thinlens#####

Appleseed has two types of cameras: **Pinhole** and **Thinlens**, the main difference being that a Thinlens camera can simulate depth of field and the Pinhole cannot. By default Mayaseed will export cameras with depth of field turned off as Pinhole and with depth of field turned on as Thinlens. Use this option to force Mayaseed to export all cameras as Thinlens.


####Assembly Settings Section####

> Note: Appleseed uses the concept of Assembles to divide up the scene into smaller components. 

#####Interpret Sets As Assemblies#####

With this option checked Mayaseed will export any sets containing geometry as an assembly.

#####Double Sided Shading#####

Use this option to turn on double shading in your appleseed scene file. Double shading causes geometry to be rendered on both sides of the shading normal. This can help reduce rendering artifacts especially on low poly geometry with smoothed normals.


####Output Settings Section####

#####Camera#####

Use this attribute to select the camera you would like to export.

> Note: If you do not select a camera the export will fail.

#####Resolution Width#####

This attribute sets the width of the framebuffer.

#####Resolution Height#####

This attribute sets the height of the framebuffer.

#####Color Space#####

This sets the color space that appleseed will use, the default is **sRGB**.


####Configuration Settings Section####

Appleseed configurations contain information on the rendering method and quality setting of a render. Appleseed can have an arbitrarily high number of these render settings but must contain at least two, an **Interactive Config** and a **Final config**. These configurations control the quality of appleseed's default interactive render and final render. Without checking **Export Custom Interactive COnfig** or **Export Custom Final Config** Mayaseed will export a default settings. 

Both Interactive Config and Final COnfig have the following attributes.

#####Lighting Engine#####

This drop-down menu currently has two options: **Path Tracing** and **Distributed Ray Tracing**. Path tracing is more physically accurate and will compute color bleeding and caustics whereas distributed ray tracing is slower but less accurate.

#####Min Samples#####

Use this attribute to set the minimum render samples.

#####Max Samples#####

Use this attribute to set the maximum render samples.

#####Max Ray Depth#####

Max Ray depth controls the maximum number of bounces that a ray can go through. Higher numbers are more accurate but slower to render.

#####Light Samples#####

This attribute controls the number of samples per light.



###ms_environment node###

The ms\_environment (environment) node is used to control the environment settings for your export. Right now it could easily be implemented in the render settings node but when environment transformations are implemented in appleseed it will be useful to have the environment node as a separate entity with its own transform node.

Below is a list of the node's attributes and their functions.

#####Model#####

This drop down menu contains the different types of environment models that appleseed provides(below).

+ Constant Environment
+ Gradient Environment
+ Latitude Longitude Map
+ Mirrorball Map

#####Constant Exitance#####

Use this attribute if you have selected **Constant Environment** as your model. 

> Note: only modify the color of this attribute, connecting a shading network will have no effect.

#####Gradient Horizon Exitance#####

Use this attribute to set the horizon color of the evironment if you have selected **Gradient Environment** as your model. 

> Note: only modify the color of this attribute, connecting a shading network will have no effect.

#####Gradient Zenith Exitance#####

Use this attribute to set the zenith (highest point) color of the environment if you have selected **Gradient Environment** as your model. 

> Note: only modify the color of this attribute, connecting a shading network will have no effect

#####Latitude Longitude Exitance#####

Attach a texture node to this attribute if you have selected **Latitude Longitude Map** as your environment model. The image should be in latitude longitude format for correct results.

#####Mirror Ball Exitance#####

Attach a texture node to this attribute if you have selected **Mirror Ball Map** as your environment model.


###Maya shaders###

Mayaseed will automatically translate maya shaders as best as possible to appleseed shaders but this often isn't perfect, when automatic shader translation isn't enough you can add a **Custom Shader Translation**. With an object or shader selected you can Choose **Mayaseed -> Add Custom Shader Translation**to add some Mayaseed specific attributes to your shader.

> Note: Custom shader translation is an experimental feature and is still limited in functionality.

Once you have added a custom shader translation to your shader you will now find three new attributes in the **Extra Attributes** section of the Shaders attribute editor, the following is a list of the attributes and their functions.

####Mayaseed BSDF####

The **Mayaseed BSDF** drop-down menu lets you choose the BSDF model that Mayaseed will translate your shader to on export. BSDF stands for Bidirectional scattering distribution function and controls how light is reflected off the surface of an object. Although appleseed contains more BSDF options Mayaseed only supports the following BSDF's.

+ Labertian
+ Ashikhmin-Shirley
+ Kleeman
+ Specular_BSDF
+ \<None>

####Mayaseed EDF####

The **Mayaseed EDF** drop-down menu lets you choose the EDF model that Mayaseed will translate your shader to on export. EDF stands for **emittance distribution function** and controls how light is emitted from a surface. Mayaseed has the following 2 options available:

+ \<None\>
+ Diffuse

####Mayaseed Surface Shader####

The **Mayaseed Surface Shader** controls how an object is rendered when it is directly visible to the camera. By default this is set to **Physical** which means that the object will be rendered according to the BSDF, this is usually the most physically accurate setting. Mayaseed has the following options available:

+ Physical
+ Constant
+ \<None>


Menu reference
--------------
The Mayaseed menu will appear when the plugin is correctly installed, if you cant see it make sure the plugin installed correctly and is enabled.

The following section contains all the items in the Mayaseed menu and their uses.

###Add Render Settings Node###

Use this to create a new instance of the ms\_renderSettings node.

###Select Render Settings Node###

Use this to select any ms\_renderSettings nodes that exist in the maya scene.

###Add Environment Node###

Use this to create a new instance of the ms\_environment node.

###Select Environment Node###

Use this to select any ms\_environment nodes that exist in the maya scene.

###Add Custom Shader Translation###

With a shader or an object selected use this to add a custom shader translation to a shader, more in the **Maya Shaders** section of the 

###Remove Custom Shader Translation###

Use this to remove a custom shader translation from a Maya shader

###About ###

Show information about Mayaseed.



Module reference
-----------------

With mayaseed correctly installed you also have the following new python modules available to you.

+ ms_commands 
+ ms_export

###ms_commands module###

**ms_commands** is where all the common functions, classes and variables reside, this section describes them. 


####Variable: ms\_commands.MAYASEED_VERSION#####

This variable will return the current Mayaseed version.


####Variable: ms\_commands.MAYASEED_URL####

This variable will return the URL of the Mayaseed website.


####Variable: ms\_commands.APPLESEED_URL####

This variable will return the URL of the appleseed website.


####Variable: ms\_commands.ROOT_DIRECTORY####

This variable will return the URL of the Mayaseed install's root directory.


####Function: ms\_commands.addShadingAttribs()####

With an object or shader selected use this function to add custom shader translation attributes to a shader.


####Function: ms\_commands.removeShadingAttribs()####

With an object or shader selected use this function to remove custom shader translation attributes to a shader


####Class: ms\_commands.MsInfoDial()####

This function shows the Mayaseed info dialogue.


####Function: ms\_commands.normalizeRGB(Tuple[Float:R,Float:G,Float:B]: Color) Returns Tuple[Float:R,Float:G,Float:B,Float:M]####

Use this function normalises a 3 value [R,G,B] tuple and returns a normalised tuple with the RGB values normalised to a 0-1 range with a 4th multiplier value.


####Function: ms\_commands.convertConnectionToImage(String:shader, String:attribute, String:dest\_file, Int:resolution=1024) Returns String:dest\_file

This function will bake a given shading connection to an image file. 

#####Argument: String:shader#####

Name of the shader. e.g. "Lambert1"


#####Argument: String:attribute#####

The name of the attribute you want to bake. e.g. "Color"


#####Argument: String:dest\_file#####

The destination of the file you'd like to export


#####Argument: Int:resolution#####

The resolution of the image you would like to bake, images are always square.


####Function: convertTexToExr(String:file\_path, String:dest\_dir, Boolean: overwrite=True) Returns String:dest_file####

Use this function to convert an image to an .exr file using the **imf_copy** utility that ships with maya. The function returns a string containing the path to the destination file.


#####Argument: String:file\_path#####

File path of the image to be converted.


#####Argument: String:dest\_dir#####

The directory that you would like to save the converted image to. 

> Note: The converted image will have the same name as the source file but with the .exr extension.


#####Argument: Boolean:overwrite = True#####

By default Mayaseed will overwrite any images that have the same name as the output file, set this argument to false if you want to cancel image conversion for existing images


####Function: ms\_commands.shapeIsExportable(String:node\_name) Returns Boolean

This function will check to see if a maya shape node is exportable based on whether it is visible and is a valid shape node.


#####Argument: String:node_name#####

This string is the name of the node you wish to check


####Function: ms\_commands.hasShaderConnected(String:node\_name) Returns Boolean####

This function checks whether a shape node has a shader connected.


#####Argument: String:node\_name#####

This string is the name of the shape node you want to check.


###ms_export module###

This module contains the main bulk of the code that handles the export and essentially only has one useful function.


####Function: ms_export.export(String:render_settings_node)####

This function is the workhorse of Mayaseed and does all the work in translating, exporting and writing your scene to disk based on the settings from your render settings node.


#####Argument: String:String:render_settings_node#####

This string is the name of the render settings node that contains the settings of your export.



Plugin structure
----------------

You will find the following files and folders inside the Mayaseed **src** directory that make up the Mayaseed plugin.

+ graphics (directory)
 + mayaseed_graphic.png
+ INSTALL.txt
+ Mayaseed_Docs.html
+ Mayaseed_Docs.md
+ open_me_to_install.ma
+ plugins (directory)
 + mayaseed.py
+ README.txt
+ scripts
 + about.txt
 + AEms_environmentTemplate.mel
 + AEms_renderSettingsTemplate.mel
 + mayaseed.ui
 + ms_commands.py
 + ms_export.py
 + ms_menu.py
 
###graphics (directory)###

The **graphics** directory contains any graphics used in the ui. 


###INSTALL.txt##

This file contains instructions on how to install Mayaseed, the instructions are mirrored in the Mayaseed docs but are included here just in case.


###Mayaseed\_Docs.md & Mayaseed_Docs.html###

**Mayaseed\_Docs.html** is generated from the **Mayaseed\_Docs.md** file. The .md file is a plain text file formatted using the markdown language.


###open\_me\_to\_install.ma###

**open\_me\_to\_install.ma** is a regular maya file that contains a python script node that executes on opening. The python script adds the Mayaseed plugins and scripts directory to the maya PATH so that maya can find the source files.


###plugins (directory)###

This directory contains the main plugin python file meaning it contains a single file that defines the Mayaseed nodes.


###mayaseed.py###

This file contains the code that defines the ms_renderSettings node and the ms_environment node.


###README.txt###

Simple text file containing information about Mayaseed.


###scripts (directory)###

The scripts directory contain all the functions, classes and attribute editor templates plus a few other things. 


###about.txt###

**about.txt** contains the text that the **About Mayaseed** dialogue displays.


###AEms\_environmentTemplate.mel###

This file describes how the ms_environment node is displayed in the attribute editor.


###AEms\_renderSettingsTemplate.mel###

This file describes how the ms_renderSettings node is displayed in the attribute editor.


###ms\_commands.py###

**ms\_commands.py** contains commands used in the Mayaseed menu and utility variables,functions and classes for the **ms\_export** module. 


###ms\_export.py###

**ms\_export.py** contains the **export()** function that does most of the hard work in translating the maya scene to the .appleseed format. Broadly speaking a **writeXML** object is handed down through each XML entity in the appleseed scene description writing to the file as it goes.


###ms\_menu###

This python script sets up the Mayaseed menu and is called by **mayaseed.py** in the **initializePlugin()** function. The **uninitializePlugin()** function also calls this module to delete the Mayaseed menu when the plugin is unloaded.

