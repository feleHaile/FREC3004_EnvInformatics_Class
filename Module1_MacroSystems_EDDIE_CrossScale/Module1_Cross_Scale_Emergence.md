Module 1: Cross-Scale Interactions Module
================
FREC 3004 Environmental Informatics

## Cross-Scale Interactions Module

This module was initially developed by Carey, C.C. and K.J. Farrell. 13
Aug. 2017. Macrosystems EDDIE: Cross-Scale Interactions. Macrosystems
EDDIE Module 2, Version 1. module2.macrosystemseddie.org Module
development was supported by NSF EF 1702506.

**R code for students to work through the module activities A, B, and
C.**

This module consists of 6 objectives. Activity A consists of Objectives
1-2, Activity B consists of Objectives 3-4, & Activity C consists of
Objectives 5-6.

## ACTIVITY A - OBJECTIVE 1

Download R packages and GLM files onto your computer.

``` r
if (!"sp" %in% installed.packages()) install.packages("sp")
```

NOTE: depending on your computer, you may get output that says, “There
is a binary version available. Do you want to install from sources that
need compilation? y/n” If this pops up, type ‘y’ (without the quotes)
and hit enter. You may now be prompted to download the command line
developer tools in a pop-up window. Command line developer tools is a
program used to run modeling software. Click Install and then re-run the
install.packages(sp) once the install of the tools is finished. This
should now successfully load- when it’s done, it should say ‘DONE(sp)’
if it worked.

``` r
if (!"devtools" %in% installed.packages()) install.packages("devtools")
```

This is another R package used to run modeling software. If you get an
error message that says, “package ‘devtools’ is not available (for R
version x.x.x)”, be sure to check that your R software is up to date to
the most recent version.

load the packages you just downloaded

``` r
library(sp)
library(devtools)
```

Download the GLMr software. This may take a few minutes. If downloaded
successfully, you should see “DONE (GLMr)” at the end of the
output.

``` r
if (!"GLMr" %in% installed.packages()) devtools::install_github("CareyLabVT/GLMr", force = TRUE)
```

This step downloads the R packages that allow you to work with GLM in
R.

``` r
if (!"glmtools" %in% installed.packages()) devtools::install_github("CareyLabVT/glmtools", force = TRUE)
```

Load the two packages that you need to analyze GLM output

``` r
library(glmtools)
library(GLMr)
```

NOTE: you may get lots of output messages in red at this step- if this
worked successfully, you should read a lot of text that starts with:
“This information is preliminary or provisional…”

If this worked, GLMr should load without error messages. Hooray\!

See what version of GLM you are running- should be v.2.x.x

``` r
glm_version()
```

*CONGRATS\!* You’ve now succesfully loaded GLM onto your computer\!

Now, we will explore the files that come with your downloaded GLM files

**NOTE\!** Throughout the rest of the module, you will need to modify
some of the lines of code to run on your computer. If you need to modify
a line, I put the symbols \#\#\!\! before that line’s annotation. If you
do not see those symbols, you do not need to edit that line of code and
can run it as written.

When working in R, we set the sim\_folder to tell R where your files,
scripts, and model output are stored.

To find your folder path, navigate to the ‘cross\_scale\_interactions’
folder on your Desktop. Right click on the folder that matches your
model lake (Mendota or Sunapee), then select Properties (Windows) or Get
Info (Mac). Look under Location (Windows) or Where (Mac) to find your
folder path (examples below):

Windows: C:/Users/KJF/Desktop/cross\_scale\_interactions/LakeName

Mac: Users -\> careylab -\> Desktop -\> cross\_scale\_interactions -\>
LakeName

to define the sim\_folder location for your model lake. You will need to
change the part after Users/ to give the name of your computer (e.g., my
computer name is cayelan, but yours will be different\!) AND change the
word LakeName to be the name of your model lake (Mendota or Sunapee).

``` r
##!!Edit this
sim_folder <- '/Users/quinn/Dropbox/Teaching/Environmental Informatics/FREC3004_EnvInformatics_Class/Module1_MacroSystems_EDDIE_CrossScale/Mendota/'
```

This line of code is used to reset your working directory to the
sim\_folder. The point of this step is to make sure that any new files
you create (e.g., figures of output) end up together in this folder.

``` r
setwd(sim_folder)
```

This step sets the nml\_file for your simulation to be in the new
sim\_folder location.

``` r
baseline_nml_file <- paste0(sim_folder,"/glm2_baseline.nml")
nml_file <- paste0(sim_folder,"/glm2.nml")
file.copy(baseline_nml_file,nml_file, overwrite = TRUE)
```

Read in your nml file from your new directory

``` r
nml <- read_nml(nml_file) 
```

This shows you what is in your nml file. This is the ‘master script’ of
the GLM simulation; the nml file tells the GLM model all of the initial
conditions about your lake, how you are defining parameters, and more -
this is a really important file\! There should be multiple sections,
including glm\_setup, morphometry, meteorology, etc.

``` r
print(nml)
```

Plot the meterological input data for the simulation: short wave & long
wave radiation, air temp, relative humidity, etc. for the duration of
the simulation.

``` r
plot_meteo(nml_file)
```

## ACTIVITY A - OBJECTIVE 2

Now, the fun part- we get to run the model and look at output\!

``` r
run_glm(sim_folder, verbose=TRUE)
```

So simple and elegant… if this works, you should see output that says
“Simulation begins..” and then shows all the time steps. At the end,
it should say “Run complete” if everything worked ok. This may take a
few minutes.

We need to know where the output data from your simulation (the
output.nc file) is so that the glmtools package can plot and analyze the
model output. We tell R where to find the output file using the line
below:

``` r
#This says that the output.nc file is in the sim_folder.  
baseline <- file.path(sim_folder, 'output.nc') 
```

Plot your simulated water temperatures in a heat map, where time is
displayed on the x-axis, lake depth is displayed on the y-axis, and the
different colors represent different temperatures.

``` r
plot_temp(file=baseline, fig_path=FALSE)
```

To copy your plot (e.g., onto a PowerPoint slide), right click on the
plot. and click “copy image”. You can then paste your plot into Word,
PowerPoint, etc.

This pair of commands can be used to list the variables that were output
as part of your GLM run.

This will print a list of variables that the model simulates.

``` r
var_names <- sim_vars(baseline)
print(var_names) 
```

We are particularly interested in the amount of total chlorophyll-a
(chl-a), because that is related to phytoplankton blooms. The variable
name for chl-a is “PHY\_TCHLA”, and it is reported in units of
micrograms per liter of water (ug/L). Search through the list of
variables to find PHY\_TCHLA.

Use the code below to create a heatmap of chl-a in the lake over time.

``` r
plot_var(file = baseline, "PHY_TCHLA")
```

What do you notice about seasonal patterns in chl-a?

We also want to save the model output of the daily chlorophyll-a
concentrations in the lake during our baseline simulation, because we’ll
be comparing it to our climate and land use scenarios later. To do this,
we use the following commands:

Save the chl-a from the surface
only:

``` r
chla_output <- get_var(file=baseline, "PHY_TCHLA", reference='surface', z_out=c(1)) 
```

Here we rename the chl-a column so we remember it is from the Baseline
scenario:

``` r
colnames(chla_output)[2] <- "Baseline_Chla" 
```

## ACTIVITY B - OBJECTIVE 3

For Activity B, you will work with your partner to model your lake, plus
another team that is modeling another lake. With your partner and
another team, select one of the pre-made climate scenarios. Remember
that both teams should run the SAME climate scenario on their separate
lakes and compare the output.

Once you have selected your climate scenario, you need to edit the
glm2\_baseline.nml file to change the name of the input met file so that
it reads in the met data for our climate scenario, not the default
‘met\_hourly.csv’.

Open the .nml file by clicking ‘glm2\_baseline.nml’ in the Files tab of
RStudio, then scroll down to the meteorology section, and change the
‘meteo\_fl’ entry to the new met file name (e.g., from
‘met\_hourly.csv’ to ‘met\_hourly\_plus2.csv’).

USE **SAVE AS** to save your modified ‘glm2\_baseline.nml’ file as
‘glm2\_climate.nml’

Once you have edited the nml file name, you should check to make sure
that it is correct with the following commands:

``` r
climate_nml_file <- paste0(sim_folder,"/glm2_climate.nml")
nml_file <- paste0(sim_folder,"/glm2.nml")
file.copy(climate_nml_file, nml_file, overwrite = TRUE)
nml <- read_nml(nml_file)
```

The printout here should list your NEW meteorological file for your
climate scenario. If it doesn’t, make sure you pressed the Save icon
(the floppy disk) after you changed your glm2.nml file.

``` r
get_nml_value(nml, 'meteo_fl') 
```

You can now run the model for your climate change scenario using the new
edited nml file using the commands below.
Exciting\!

``` r
run_glm(sim_folder, verbose=TRUE) # Run your GLM model for your lake climate scenario. 
```

Again, we need to tell R where the output.nc file is so that the
glmtools package can plot and analyze the model output. We tell R where
to find the output file using the line below:

``` r
climate <- file.path(sim_folder, 'output.nc')
```

This defines the output.nc file as being within the sim\_folder. Note
that we’ve called this output “climate” since it is the output from our
climate change simulation.

As before, we want to save the model output of the daily chlorophyll-a
concentrations in the lake during our climate change simulation, to
compare to our baseline and land use scenarios later.

Extract surface
chl-a:

``` r
climate_chla <- get_var(file=climate, "PHY_TCHLA", reference='surface', z_out=c(1)) 
chla_output["Climate_Chla"] <- climate_chla[2]
```

Here we attach the chl-a data from your climate simulation to the same
file that contains your baseline scenario chl-a concentrations.

To check that your climate change scenario ran correctly, run the
command below, and compare the chl-a data between your baseline and
climate scenarios. They’ll likely be similar, but if they’re EXACTLY the
same after the first few rows, something might have gone wrong in
setting up your climate scenario (likely with changing the glm2.nml
file\!)

``` r
View(chla_output)
```

## ACTIVITY B - OBJECTIVE 4

Plot the output using the commands you learned above.

``` r
plot_temp(file=climate, fig_path=FALSE)
```

Create a heatmap of the water temperature.

How does this compare to your baseline?

Note: If you want to control the maximum value of the color scale on
your heatmaps, add the following (without quotes) after fig\_path=FALSE:
‘col\_lim= c(0,35)’ This tells R that you want your maximum value to be
35, and your min. to be
0

``` r
plot_var(file=climate, "PHY_TCHLA") # Create a heatmap of chlorophyll-a. How 
```

How does this compare to your baseline? You can add the ‘col\_lim’
command to this plot, too\! Look at the Note from the plot\_temp()
command to learn how.

Do these plots from the climate scenario and the baseline support or
contradict your hypotheses about climate change effects on
chlorophyll-a?

## ACTIVITY C - OBJECTIVE 5

Now, with your partner and the other team, select one of the pre-made
land use scenarios based on changes in phosphorus concentrations in the
inflow file. Remember that both teams should run the SAME land use
scenario on their separate lakes and compare the output.

Once you have selected your land use scenario, you need to edit the
glm2\_baseline.nml file to change the name of the inflow file so that it
reads in the inflow file for your land use scenario, not the default
“inflow.csv”.

Open the glm2\_baseline.nml file, scroll down to the inflows section,
and change the inflow\_fl entry to the new file name (e.g., from
‘inflow.csv’ to ‘inflow\_fourP.csv’).

USE **SAVE AS** to save your modified ‘glm2\_baseline.nml’ file as
‘glm2\_landuse.nml’

Once you have edited the nml file name, check to make sure that it is
correct with the commands:

``` r
landuse_nml_file <- paste0(sim_folder,"/glm2_landuse.nml")
nml_file <- paste0(sim_folder,"/glm2.nml")
file.copy(landuse_nml_file, nml_file, overwrite = TRUE)
nml <- read_nml(nml_file)
```

Read in your updated nml file

``` r
get_nml_value(nml, 'inflow_fl')
```

You should get an output that lists the name of your ALTERED inflow
file.

``` r
get_nml_value(nml, 'meteo_fl')
```

You should get an output that lists the name of your BASELINE
meteorological file (‘met\_hourly.csv’).

You can now run the model for your land use scenario using the new
edited nml file using the commands below. Exciting\!

``` r
run_glm(sim_folder, verbose=TRUE)
```

Run your GLM model for your lake land use scenario. At the end of the
model run, it should say “Run complete” if everything worked.

Again, we need to tell R where the output.nc file is so that the
glmtools package can plot and analyze the model output. We tell R where
to find the output file using the line below:

``` r
landuse <- file.path(sim_folder, 'output.nc')
```

This defines the output.nc file as being within the sim\_folder.

Note that we’ve called this output “landuse” since it is the output from
our land use change simulation.

As before, we want to save the model output of the daily chlorophyll-a
concentrations in the lake during our land use change simulation, to
compare to our baseline and climate scenarios later.

Extract surface
chl-a:

``` r
landuse_chla <- get_var(file=landuse, "PHY_TCHLA", reference='surface', z_out=c(1)) 
```

``` r
chla_output["LandUse_Chla"] <- landuse_chla[2]
```

Here we attach the chl-a data from your land use simulation to the same
file that contains your baseline scenario and climate change scenario
chl-a concentrations.

Plot the output of your land use scenario using the commands you learned
above.

``` r
plot_var(file=landuse, "PHY_TCHLA")
```

Heatmap of chla. How does your phytoplankton heatmap look in comparison
to the baseline? Be sure to check the scale of the color gradient
representing chl-a when comparing plots\!

Finally, we want to see what happens when land use and climate
interact\! Luckily, testing the combined effects of your land use and
climate change scenarios will be pretty easy\! Since we already have
modified met data (climate scenario) and inflow data (land use
scenario), we just have GLM read them both at once. We can do this by
changing the glm2.nml file to include our modified files.

In the glm2\_baseline.nml file, make the following TWO changes:

1)  In the meteorology section, change the ‘meteo\_fl’ entry to the met
    file that represents your climate change scenario (e.g.,
    ‘met\_hourly\_plus2.csv’)

2)  In the inflow section, check that the ‘inflow\_fl’ file represents
    your land use change scenario (e.g., ‘inflow\_fourP.csv’)

USE **SAVE AS** to save your modified ‘glm2\_baseline.nml’ file as
‘glm2\_climate\_landuse.nml’

Read in your updated nml
file

``` r
climate_landuse_nml_file <- paste0(sim_folder,"/glm2_climate_landuse.nml")
nml_file <- paste0(sim_folder,"/glm2.nml")
file.copy(climate_landuse_nml_file, nml_file, overwrite = TRUE)
nml <- read_nml(nml_file)
```

If you have done this correctly, you should get an output that lists the
name of your ALTERED inflow file.

``` r
get_nml_value(nml, 'inflow_fl')
```

If you have done this correctly, you should get an output that lists the
name of your ALTERED meteorological file.

``` r
get_nml_value(nml, 'meteo_fl')
```

Run GLM one more time\!

``` r
run_glm(sim_folder, verbose=TRUE)
```

Run your GLM model for your lake climate + land use scenario

As above, we need to tell R where the output.nc file is:

``` r
climate_landuse <- file.path(sim_folder, 'output.nc')
```

This defines the output.nc file as being within the sim\_folder. Note
that we’ve called this output “climate\_landuse” since it is the output
from our simultaneous climate AND land use change simulations.

As before, we want to save the model output of the daily chlorophyll-a
concentrations in the lake, to compare to our baseline, climate, and
land use scenarios.

Extract surface
chl-a:

``` r
combined_chla <- get_var(file=climate_landuse, "PHY_TCHLA", reference='surface', z_out=c(1)) 
chla_output["Climate_LandUse_Chla"] <- combined_chla[2]
```

Here we attach the chl-a data from your combined simulation to the same
file that contains your baseline, climate change, and land use scenario
chl-a concentrations

Plot the output of your land use scenario using the commands you learned
above.

``` r
plot_var(file=climate_landuse, "PHY_TCHLA") # Heatmap of chlorophyll-a
```

Now that you’ve run four different scenarios (baseline, climate only,
land use only, and climate + land use), let’s plot how the chl-a in the
lakes responded to the different scenarios. We can do this by:

The command below plots DateTime vs. Observed data in black:

**Note** that the command ylim=c(0, 100) tells R what you want the
minimum and maximum values on the y-axis to be (here, we’re plotting
from 0 to 100 ug/L). You should adjust this range to make sure all your
data are shown in the plot.

``` r
attach(chla_output)
plot(DateTime, Baseline_Chla, type="l", lwd=2, col="black", ylim=c(0, 100),
     ylab="Chlorophyll-a (ug/L)", xlab="Date")  
lines(DateTime, Climate_Chla, lwd=2, col="firebrick") # add a red line of the climate change scenario
lines(DateTime, LandUse_Chla, lwd=2, col="deepskyblue") # add a blue line of the land use scenario
lines(DateTime, Climate_LandUse_Chla, lwd=2, col="darkgreen") # add a green line of the climate + land use scenario
legend("topleft",c("Baseline", "Climate Only", "Land Use Only", "Combined C + LU"),  # add a legend
       lty=1, lwd=2, col=c("black","firebrick","deepskyblue", "darkgreen"),bty='n')
```

Now we can analysis the output for the four scenarios to check for
non-linear interactions

First, we need to pick a day to examine. Lets pick September 12, 2013,
which is day 225.

``` r
focal_day <- 225
```

Now redo the plot above with the focal date highlighted as a vertical
line

``` r
attach(chla_output)
plot(DateTime, Baseline_Chla, type="l", lwd=2, col="black", ylim=c(0, 100),
     ylab="Chlorophyll-a (ug/L)", xlab="Date")  
lines(DateTime, Climate_Chla, lwd=2, col="firebrick") # add a red line of the climate change scenario
lines(DateTime, LandUse_Chla, lwd=2, col="deepskyblue") # add a blue line of the land use scenario
lines(DateTime, Climate_LandUse_Chla, lwd=2, col="darkgreen") # add a green line of the climate + land use scenario
legend("topleft",c("Baseline", "Climate Only", "Land Use Only", "Combined C + LU"),  # add a legend
       lty=1, lwd=2, col=c("black","firebrick","deepskyblue", "darkgreen"),bty='n')
abline(v = as.POSIXct("2013-09-12"))
```

Now examine the ouput from that day to answer question 15

``` r
chla_output[focal_day,]
```

## ACTIVITY C - OBJECTIVE 6

Using the line plot you just created, and the other team’s line plot
from their lake, put together a brief presentation of your model
simulation and output to share with the rest of the class (you’ll
present as a group of 4\!)

Make sure your presentation answers the questions listed in your
handout.

Bravo, you are done\!\!

## Feedback

We welcome feedback on this module and encourage you to provide
comments, questions, and suggestions.

Please visit our website (<http://MacrosystemsEDDIE.org>) to submit
feedback to the module developers.
