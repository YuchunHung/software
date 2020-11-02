# Memo of GEMC
The current development version of RTPC in GEMC is located at `/group/bonus/gemc/devel/`, which was built on 12 July 2020 with the author Nate Dzbenski. It differs from the official GEMC release in the expressions/parameters for drift electron behavior. In this directory, you can run the gemc executable file with the RGF configuration,  `rgf_spring2020_lund.gcard`. This gcard runs with an input lund file from an external event generator, until May 2020, this lund file is located in `lund/ep_34118_20.lund`. 
This lund file has one primary ep event and 20 additional protons to act as "background." The RTPC hit process class in the GEMC source code takes those background protons and shifts the ionization electron times randomly between -8 and 8 µsec.

There are a few files in the GEMC source directory that are important specifically for RTPC. That is: `source/hitprocess/clas12/rtpc_hitprocess.h` and `source/hitprocess/clas12/rtpc_hitprocess.cc`, where the Garfield++ generated parameters have been implemented. Whenever you change something in the source directory, you have to recompile with scons.

To update the Official GEMC source code, you need to pull the latest version from the GitHub repo at `github.com/gemc/source`, update the relevant code and create a pull request.

(The Instructions above came from Nate's web page, https://userweb.jlab.org/~nathand/rgf/sim/gemc.html .)

=======

# Make the executable gemc and run the simulation. 

** To make the executable file and run the simulation, you will need to check the environment setting. To run the simulation successfully, you will need to load the suitable software into your environment, which, in general, would be clas12/2.1.


>  module load clas12/2.1

If you have set the software with other versions(e.g., clas12/pro, which has the latest versions of the software), your loading process will not succeed. In this case, you will need to unload them from your environment first. 

>  module unload [the-versions-that-are-confilct]  
>  module load clas12/2.1
  

## 1. Clone the developing directory. 
To develop the simulation, it would be better you copy the current `/group/bonus/gemc/devel/` directory to your own working directory and make any changes in your directory first. 
You "might" can clone the gemc git repo from https://github.com/gemc/source as well because both of them contain similar structures. However, I am concerning that which has the up-to-date version for the simulation...

>  cp -rf /group/bonus/gemc/devel/ [your-own-directory]  
or
>  git clone github.com/gemc/source

  
## 2. Compile the code and get the executable file "gemc".
Go to the source directory, which is located in your gemc folder, and this is where you can compile the code and get the executable file "gemc" if the compiling process is successful.


>  cd [your-own-gemc-directory]/source
>  scons OPT=1 -jN 

where N = number of cores you want to use. It's an arbitrary choice or you can make it simple to set N=4.   
If everything works well, you will find a "gemc" file in the folder after the compiling. 

## 3. Run the Simulation
Since I don't think it's a good idea to run the simulation in the source directory, it might be better to move the gemc to another directory and run your simulation. At this moment, let's move gemc to the previous folder, which has the RGF configuration file "rgf_spring2020_lund.gcard", and then run the simulation. 


> mv gemc ../ ＜/br＞  
>  ./gemc rgf_spring2020_lund.gcard -N=100 -RUNNO=11

where N is the nummber of events that you want to generate.  
Therefore, you are supposed to control the setting with the GUI of the gemc. If you want to generate the events without GUI, you need to add an additional option:
>  ./gemc rgf_spring2020_lund.gcard -USE_GUI=0 -N=100 -RUNNO=11  
  
The default output file name of the simulation is "out.evio". You can specify the name of the output file with the option "OUTPUT", i.e.,  
>  ./gemc rgf_spring2020_lund.gcard -OUTPUT="evio, [name-of-output-file].evio" -USE_GUI=0 -N=100 -RUNNO=11
  
Another way to change these settings is to modify the content in your .gcard file. 
  


