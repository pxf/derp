
Project Notes
=============
----------------
* _Components_  
Auxiliary  
Render  
Post-Process  
* _Editor_  
Preview  
Timing Info  
Compiler Info  
* _Renderer_
Pipeline Tree

##Component Description
Components sent from the editor to the renderer are specified in JSON 
and are structured in the following way:

       {"blockName" : "...",  
        "blockType" : "...", 
        "blockInput" : ..., 
        "blockData" : {...},  
        "blockOutput" : ...  
       }

`blockName` is the name of the component and used to identify itself in the pipeline tree. `blockType` specifies the type of the component and can be one of; `AuxComp`, `Render`, `Post-Process`, `Output` and `PipelineTree`. `blockInput` (optional) is a list of inputs / prerequisites. `blockData` hold the component specific data for each component type. `blockOutput` holds a list of the different outputs.

####Component Feature List  

User should be able to move components around on the workspace area
Component minimization: component is collapsed / minimized to a smaller GUI box with only the component name and expand button visible  
Should be able to drag multiple wires from one output to different inputs  
On/Off button. Possible extension: Collapse / minimize GUI box when component is offline  

####Auxiliary Component
**_JSON Expamles_**

**AuxComp - Texture:**

        {"blockName" : "auxinput1",  
         "blockType" : "AuxComp",  
         "blockData" : {"auxType" : "texture",  
                        "filepath" : "loltexture.png",  
                        "minfilter" : "nearest"  
                       },  
         "blockOutput" : "texture"  
        }  

**AuxComp - Script:**

        {"blockName" : "auxinput2",  
         "blockType" : "AuxComp",  
         "blockData" : {"auxType" : "script",  
                        "script" : "math.sin(os.time() * 0.2)"  
                       } ,  
         "blockOutput" : "float"  
        }  
        
**AuxComp - Integer Constant:**

        {"blockName" : "auxinput3",  
         "blockType" : "AuxComp",  
         "blockData" : {"auxType" : "const",  
                        "value" : 1  
                       } ,  
         "blockOutput" : "int"  
        }  
        
**_Auxiliary Types_**

Texture - `"texture"`  
Lua Script - `"script"`  
Const - `"const"`  

**_Output Types_**

Texture - `"texture"`  
Integer - `"int"`  
Boolean - `"bool"`  
Float - `"float"`  
Vec2 - `"vec2"`  
Vec3 - `"vec3"`  
Mat2 - `"mat2"`  
Mat3 - `"mat3"`  
Mat4 - `"mat4"`

###Render Component
**_JSON Expamle_**

        {"blockName" : "render1",
         "blockType" : "Render",
         "blockInput" : {"cameraPos" : "auxinput1",
                         "cameraDirection" : "auxinput2"
                        },
         "blockData" : {"sceneFile" : "aoeu.scn",
                        "shaderVert" : ".......",
                        "shaderFrag" : "......."
                       },
         "blockOutput" : {"texture1" : "texture",
                          "texture2" : "texture"}
        }
        
###Post-Process Component
**_JSON Expamle_**

        {"blockName" : "postprocess1",
         "blockType" : "Post-Process",
         "blockOutput" : {"texture1" : "texture",
                          "texture2" : "texture"}
         "blockData" : {"shaderVert" : ".......",
                        "shaderFrag" : "......."
                       },
         "blockOutput" : {"texture3" : "texture"}
        }
        
###Output Component
**_JSON Example_**

        {"blockName" : "output1",
         "blockType" : "Output",
         "blockInput" : "texture3"
         "blockData" : {"host" : "localhost",
                        "port" : "4632",
                        "feedback" : true,
                        "realtime" : false,
                        "shaderVert" : ".......",
                        "shaderFrag" : "......."
                       }
        }
        
##Editor Description

####Editor Feature List

Editor menu options: 

* File  
New  
Save  
Load  
Exit
* Edit  
Undo  
Redo  
Settings
* Help(maybe excessive work, but adds to the completeness of program.. less priority)  
How to use  
About DERP  

####Aoeu?

_Preview_

_Timing Info_

_Compiler Info_

##Renderer Description

_Pipeline Tree_