
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
**_JSON Examples_**

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
**_JSON Examples_**

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
**_JSON Examples_**

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
**_JSON Examples_**

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

####Editor Feature Description

_Preview_

The final rendering from the output component is sent back to the editor and displayed in a preview window.
The preview window can be displayed in target size (output texture size), fullscreen mode or manually resized.
Since preview data is sent via network communication, the editor will need a resource stream technique to avoid GUI hangup.
We could perhaps segment the data into chunks and transfer these over several frames.
The preview window could then be periodically updated with this information to display partial output.
This could enable the user to abort the preview transfer before the data is finished, if he is not satisfied with the results.
I'm freeballin' this one guys!

_Timing Info_

When rendering the output, the backend profiles each component and sends this data along with the finished frame back to the editor. The timing information is displayed somewhere on the component itself in the editor window. 

_Compiler Info_

When the render compiles each component to GLSL code, the backend checks the compile result for errors.
In the case that an error has occured, the renderer will be unable to execute the pipeline and must therefore notify
the editor which component in the chain that failed. The faulty component will then display an error icon somewhere on the
widget that tells the user which one is incorrect. The renderer should also send the actual error messages back to the editor
so that the user knows whats wrong and what line produced it. 

##Renderer Description

_Pipeline Tree_