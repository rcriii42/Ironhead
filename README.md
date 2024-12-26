# Ironhead
Agent-based modeling of the Project 11 HSC expansion.

TODO: Update with latest visualization

![Screenshot_20220311](https://user-images.githubusercontent.com/9353408/157890463-c954ba51-fbe6-4ef4-b6f6-f62f8621dd18.jpg)

# Features

The simulation has the following features:
* Inputs are CSV files:
    * Shapefiles defining the channel sections (red and green side and channel centerline)
    * Matching files with quantities and productions by station
    * Scenarios laying out the order of work as distinct tasks digging channel stations in a given area. Tasks have the following:
        * A unique task number
        * The sections can be broken out in as much detail as required
        * A start date can be set
        * A start and end station (increasing or decreasing stations)
        * A predecessor task
        * Prep time, representing a delay before starting a new task
        * A production factor that is applied to the productions. I use this to tune the days to match the proposed schedules. That way we get the contractors overall days while preserving the detail of more and less productive work.
        * Whether the 5-mile rule applies or not (for example in BSC)
        * The name of the assigned dredge
        * A comment string to document the task
* A visualization showing the dig areas, dredges, and buffers overlaid on a navigation chart.
    * The dredges are 10x scale, colored black for clamshell and magenta for CSD
    * The buffers are circles 5-miles in diameter centered on the dredge bucket or cutter.  When they touch the dredges are in conflict.
    * The buffers are colored by project and channel. Blue is 3-4a HSC, orange is 4b-5 HSC, yellow is 4b-5 BSC.
    * The buffer size is settable using a slider bar to the left. 
    * For areas where the buffer is inactive (e.g. the BSC), the circle is much smaller, but I left it to keep the dredge visible.
    * The visualization is zoomable, with the detail of the nav chart varying as you zoom in. 
* You switch between scenarios using a dropdown box on the left. You have to reset the simulation after changing scenarios
* You can tell the simulation to randomize productions (+/- 40%) using an on-off button to the left
* A conflict plot below the nav chart.  The blue line is the number of conflicts (dredges within 5 miles), the black is the maximum conflict (how much closer than 5 miles they are). 
* A table showing a static view of the simulation results. This has descriptions of the basis of the scenario, and dig quantities and dates for the various channel sections. I use this to check the various scenarios, to ensure that I am digging all the materials and the dates match. 
* Since I tune the days with a production factor, the days may not match exactly
* The quantities vary a bit (<1%) with schedule as well. I am working on why that is, but still think the simulation is valid.
* A graph of quantity per day for both projects.

# Installation
The program currently runs under Linux, using Python 3.8+.

Download the source code:

`path/to/parent/directory$ git --recurse-submodules clone https://github.com/DredgingResources/Steelhead.git Steelhead`

Note if you forget to `--recurse-submodules` you will need to do `git submodule update --init`.

Change to the Steelhead directory and create a virtual environment (not required but recommended):

`path/to/parent/directory/Steelhead$ python -m venv ./venv`

Activate the virtual environment:

* In Bash (Linux, Unix, etc): `source venv/bin/activate`
* In Windows Powershell: `venv\scripts\activate.ps1`
* Commands for other command-line environments [here](https://docs.python.org/3/library/venv.html#creating-virtual-environments) (scroll down to the table)

After activating the virtual environment, the command prompt will be preceded by `(venv) `.

Install the requirements:
`(venv) path/to/parent/directory/Steelhead$ pip install -r requirements.txt`

# Running the Simulation
```
channel_def: prod_csv\cl_HSC.csv does not exist                   <<< List of channel files with no associated
channel_def: prod_csv\cl_BSC.csv does not exist
channel_def: prod_csv\cl_BCC.csv does not exist
channel_def: prod_csv\disp_3-4a_LBI.csv does not exist
channel_def: prod_csv\disp_TBI.csv does not exist
channel_def: prod_csv\disp_SLOM_0.csv does not exist
channel_def: prod_csv\disp_SLOM_10.csv does not exist
channel_def: prod_csv\disp_DPOM_0.csv does not exist
channel_def: prod_csv\disp_DPOM_6.csv does not exist
+-------------------------------------------------------------------------------+
| Scenario: AM22                                                                |
| Run Date: 2024-12-26 12:01:45                                                 |
| Buffer: 5.0                                                                   |
| 4b-5 Contractor 2 Schedule 2024-07-10, progress through 2024-07-20            |
| Contractor 2 Dredge - HSC AS-4/5/6 at 100% production                         |
| 6 CSD 1c  2024-06-27 schedule                                                 |
| Contractor 4 Dredge at 100% production with 12+000 start                      |
+-------------------------------------------------------------------------------+
 Project      Section           Dig CY Days Start Date   End Date OL Days Max OL
 4b-5         CSD 1B           2822350  220 2024-04-19 2024-11-27      21   0.33
=================================================================================
 4b-5         totals           2822350  220 2024-04-19 2024-11-27      21   0.33
---------------------------------------------------------------------------------
 6            CSD 1C           3121773  190 2024-08-07 2025-02-19      21   0.33
=================================================================================
 6            totals           3121773  190 2024-08-07 2025-02-19      21   0.33
---------------------------------------------------------------------------------
