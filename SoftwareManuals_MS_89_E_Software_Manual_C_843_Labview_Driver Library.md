#  MS 89 E Software Manual C-843 Labview Driver  Library

---

## Table of Contents 

0. Disclaimer
1. Introduction
   1. PI General Coomand Set(GCS)
   2. Scope of this manual
   3. VI Stucture
   4. Working with two PI products which understand PI's General Command Set (GCS) in LabView
   5. First Steps For GCS-Compatible PI controller
2. Low level VIs
   1. Analog Controller VIs **ANALOG CONTROL.LLB**
   2. Communication VIs **COMMUNICATION.LLB**
   3. File Handling VIs **FILE HANDLING.LLB**
   4. General Command VIs **GENERAL COMMAND.LLB**
   5. Joystick specfic VIs **JOYSTICK.LLB**
   6. Limit and Reference Specific Commands **LIMIT.LLB**
   7. Old Commands And Commands With Alternate Implementations **OLD COMMANDS..LLB**
   8. Special Commands**SPECIA COMMAND.LLB**
   9. Support VIs **SUPPORT.LLB**
   10. Wave-Generator-Specific Commands **WAVEGENERATOR.LIB**
   11. Merge Driverset VIs **MERGEDRIVERS.LIB** 
3. Hight Level VIs
   1. PI TERMINAL.VI
   2. C843_SIMPLE_TEST.VI
   3. C843_CONFIGURATION_SETUP.VI
   4. C843_SAMPLE_APPLICATION_1.VI
   5. C843_SAMPLE_APPLICATION_2A.VI
   6. C843_QMC_EXAMPLE.VI
   7. C843_UNKNOWNSTAGE.VI
   8. DRP_EXAMPLE.VI
   9. JOYSTICK OPERATION.VI
   10. MOTIONPROFILE.VI
   11. SHOW_SAVE_LOAD_XY_DATA.VI
   12. SIMPLE_OPERATION.VI
4. PI system jcurrently supported by this driver set
5. Appendix A
6. Index

---

## High Level VIs

### C843_Configuration_Setup.vi

The VI performs a fully automatic initialization of the selected system (global settings and stage referencing) in the LabView enviroment. Use the *Help->Show Context Help* menu sequence in the LabView environment to display the *Contex Help* menu sequence in the LabVIEW environment to display the *Context Help* window with the VI and control/indicatior descriptions.

After the successful run of this VI, all command VIs are ready to use. Specify the correct parameters first:

+ C843 version : 
  + **TRUE**: C-843 4-axis
  + **FALSE**: C-843 2-axis

+ Board No: 1 for only once C-843 board 
+ System No: 1 in a one-system-only configuration
+ Reference mode: See the description of **RON.vi** for details and warnings
+ Use dialog to define connected stages 
  + **Yes** : One can select the connected stages after starting the VI.
  + **No** : Dialog allowing stage selection will not appear and you must type the correct stage names into "**Stage names input**". For axes without a stage connected, choose "**NOSTAGE**"

When one start the "**C843_Configuration_Setup.vi**", the procedure will perform the following initialization tasks:

+ "**PI Open Interface of one system.vi**": Open a connection to board
+ "**IDN?.vi**": Query for the controller identification string
+ Defines the selected system to be "C-843"
+ "**Define connected axes.vi**" with <u>Read from controller</u>=FALSE and <u>Connected axes</u> = 1,2 
+ "***CST.vi**": Determine  which stages are connected to the two channels of the C-843 board
+ "Define connected axes.vi" with <u>Read form controller</u> = TRUE. Axes with "NOSTAGE" as stage name will not appear as connected axes.
+ “**CST?.vi**” : Query for the connected stages (axes with NOSTAGE will not appear as connected stages).
+ “**INI.vi**”
+ Sets the <u>reference mode</u> for the axes specified in **RON** Affected axes according to the value in **Reference mode** (Reference mode tab).
+ Reads which axes have reference mode **OFF**.
+ Determines which of the remaining axes have a reference switch (**REF?**).
+ Moves those axes to the reference position (**REF**).
+ Reads which of the remaining axes have a limit switch.
+ Moves them to the positive or negative limit position (**MPL/MNL**) as specified.
+ Reads the position range (**TMN?, TMX?**).
+ If <u>Move to middle?</u> is TRUE, moves each axis which has been referenced or moved to a limit switch to the middle position of its range (**MOV**).
+ Waits for motion to stop
+ Defines home (if so specified).
+ Runs “**POS?.vi**” to query for the position of all axes.
+ Runs “**ERR?.vi**” to query the controller for its error status.
+ Runs “**GCSTranslateError.vi**” to append the error message which corresponds with a GCS error number returned by “**ERR?.vi**” to Source from Error out.

### C843_Sample_Application_2a.vi

This VI demonstrates how to statically define a C-843 configuration with two C-843 2-channel boards and connected stages.

### Simple_Operation.vi

This VI is runs in a loop until the stop button is pressed, the <u>stop reference</u> is TRUE or error occures. It lets the operator set the velocity, move the connected axes to relative, absolute or to the home position, and reads the current postion contunuosly.



==Run "XXXX_Configure_Setup.vi"==(with XXXX being the PI product number of ones system) prior to running this VI.





---

## Conclusions

+ The most essential things are tested with the file "**090812_MotorC84320_Test.vi**".
+ How to import the third party VIs to labview's control paletee?
  + [Follow the procedure of this link](http://zone.ni.com/reference/en-XX/help/371361L-01/lvhowto/add_object_to_subpalette/) 

