# About Cellang++
Cellang++ is a small language developed for building models examining developmental signaling and patterning. Models are encoded using the language, are then translated into C codes, and are compiled to executable codes using gcc. 

# System Requirements
The Cellang++ system is developed upon the previous Cellang, under CentOS (version 6.0) and Fedora (earlier Fedora versions). 

# Installation Guide
- There are two ways to build a model. First, under CentOS, use "cellcpp" to translate a source code model into a C code model, then gcc is called automatically to compile the C files into an executable program. The command is like "./cellcpp -msgqsize 20 -dimsize 124 -solution rk2e -dt 0.005 <sourcecodefile", where the command-line arguments "-msgqsize", "-dimsize", "-solution", and "-dt" indicate message queue size (=20), dimension size (=124*124), numeric solution (=2nd order Ronge-Kuta method with adaptive time step), and time step (=0.005), and "sourcecodefile" is a source code model, such as the wing growth model. Second, if the operating system is not CentOS, the "cellcpp" may not be runnable, one should re-build the system (install Cellang++).   
- The C code model contains 9 files, which are cell_head.h1, cell_head.h2, cell_head.h3, cell_OPDE.h, cell_OPDEderv0.c, cell_OPDEderv1.c, cell_main.c, cell_objs.c, and the generic file common.c.
- To install Cellang++, simply run "make". One may need to properly modify the makefile to make the final files (cellcpp, zhux11.o, zhuview.o) generated in a correct fold. 
- The Cellang++ system consists of two parts, a "compiler" (in the fold "newcompiler") and a graphic user-interface (in the fold "newviewer"). Specifically, the graphic user-interface is written using the X11 protocol, so one should install this package when installing the Linux system.
- The "newcompiler" fold generates the "cellcpp" program, and the "newviewer" fold generates the "zhux11.o" and "zhuview.o". The two .o files are linked to the .o files generated by gcc, after "cellcpp" is called.  
- After using "cellcpp" to generate a C code model, one can also manually perform the second step compilation, by typing the two commands: (1) "cc -g -c cell_main.c", and (2) "cc -o myexe -I/usr/X11R6/include/X11 -L/usr/X11R6/lib64 -lm -lX11 cell_main.o zhuview.o zhux11.o". 

# Running 
To run a model, the command is like

```
./myexe -s 2 -f 1 -fields 4,5,6,7,8,9,10,11,12,13,14,16,19,22,23,32,33,36,37,38 -map colormap -dispstep 0.3 < cellarray124
```

The command-line arguments "-s" and "-f" mean "solution" (how many screen pixels are used to display a cell) and "field" (which cell or molecular attribute is displayed in the main window). If "-s 2" and "-f 2" are used, this means 2*2 pixels are used to display a cell, and the second attribute (attributes are counted from the first cell attribute to the last attribute in the last object). The numers following "-f 2", such as 4,5,6,7,8,9,10, are attributes (also called fields). "-map" indicates the color map used to display the values of fields, and the following argument "colormap" is a specific color map file. One can define other color map files following the same format. The argument "dispstep 0.3" indicate that the time interval of graphic display is 0.3. If without this argument, the outputs are displayed every time step. The final argument, "cellarray124", is an input file that gives the values of some (or all) fields in some (or all) cells at T=0 (or T=x). 

A set of hotkeys are defined to control running, including to pause/continue running (shift-S), to open/close windows displaying selected fields in the whole cell space at each time step (the number key 3), and to open/close several windows displaying selected fields in one cell in a time period (the number key 1). For more details see the User Manual.

How to use one color map to graphically display many cell and molecular attributes/fields? Values of cell and molecular attributes/fields can be large or small. To graphically display them in windows using different colors, different color map files should be defined. But in each running only one color map can be opened. To display as more attributes/fields as possible using one color map, a way is to define alternative attributes/fields whose values are in the ranges of the color map. In the wing model "birth2" and "Wg->v10" are such cases. 

# Input
If one does not need initial inputs, in "cellarray124" simply give the first field in all cells a "0", such as:  

```
0
[0,0]=0
[0,1]=0
[0,2]=0
[0,3]=0
[0,4]=0
[0,5]=0
[0,6]=0
[0,7]=0
[0,8]=0
[0,9]=0
```

If one wants to reset the first field (say, to be 10) in some cells at a specific time step (say, at time step 1000), then the file should contain such lines following the input lines at time step 0:  

```
1000
[0,0]=10
[0,1]=10
[0,2]=10
[0,3]=10
[0,4]=10
[0,5]=10
[0,6]=10
[0,7]=10
[0,8]=10
[0,9]=10
```

This provides a way to perturbate the running of a model. In the Cellang++ program one can also make such perturbation using the "if" clause and proper conditions.


# Debugging & Bug reports 
To run a model in debugging model, first type "ddd myexe", then in the ddd environment type "run -s 2 -f 1 -map colormap < cellarray124".

The system is still in development, if any bug is identified, please write to zhuhao@smu.edu.cn.

