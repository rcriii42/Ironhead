# Ironhead
Agent-based modelling of dredge schedules to manage dredge spacing in a busy channel

![Screenshot_20220311](https://user-images.githubusercontent.com/9353408/157890463-c954ba51-fbe6-4ef4-b6f6-f62f8621dd18.jpg)

# Features

The simulation has the following features:
* Inputs are CSV files:
    * Shapefiles defining the channel sections (red and green side and channel centerline)
    * Matching files with quantities and productions by station
    * Scenarios laying out the order of work as distinct tasks digging channel stations in a given area. Tasks have the following:
        * A unique task number
        * The dredge type
            * In the dredge type is 'description'
        * The segment of channel with associated dredge productions
        * An optional start date (if not given starts after a predecessor task)
        * A start and end station (increasing or decreasing stations)
        * A predecessor task
        * Prep time, representing a delay before starting a new task that is included in the total task days
        * A production factor that is applied to the productions.
            * I use this to tune the days to match the proposed schedules. That way we get the contractors overall days while preserving the detail of more and less productive work.
        * Whether the 5-mile rule applies or not
            * In the HSC it only applies in the main ship channel, while dredges may have assignments in side channels
        * A comment string to document the task
            * Assign a name to the dredge in comments
* A visualization showing the dig areas, dredges, and buffers overlaid on a map.
    * The visualization runs in the browser from a local server
    * The dredges are 10x scale, colored black for clamshell and magenta for CSD
    * The buffers are ellipses 5-miles in diameter centered on the dredge bucket or cutter.
        * CSD buffers are narrow, clam buffers are wide
        * When they touch the dredges are in conflict.
        * In side channels with no 5-mile rule, the buffer is shown as a small circle to provie zoomed-out visibility on the map.
    * The buffers are colored by project and channel.
    * The buffer size is settable using a slider bar to the left. 
    * The visualization is zoomable, with the detail of the map varying as you zoom in. 
    * You switch between scenarios using a dropdown box on the left. You have to reset the simulation after changing scenarios
    * A conflict plot below the map.  The blue line is the number of conflicts (dredges within 5 miles), the black is the maximum conflict (how much closer than 5 miles they are).
    * A graph of quantity per day for all projects.
    * A table with the simulation results, including:
       * The description lines from the scenario
       *  Any warnings thrown by the task scheduler
       *  Dates, quantities, and total dredging days for the various channel sections
       *  Days of overlap for each section and maximum overlap distan
*  Two scripts to batch run scenarios:
    *  A script to step through start dates for a set of tasks, to see how start date affect the 5-mile rule
    *  A script that randomly vary dredge productions over the course of multiple project runs to see the effect on 5-mile overlaps
*  Output files (4 total)
    * A log file with detailed entries for scenario starts, task starts, task steps, and overlap events
    * A summary Excel sheet with simulation results table, and a table breaking the segments further into acceptance sections
    * A list events for each step and task
    * A list of overlap events for each step and task

# Installation
The program currently runs under Windows, using Python 3.11+.

Download the source code:

`path/to/parent/directory$ git --recurse-submodules clone https://github.com/DredgingResources/Ironhead.git Ironhead`

Note if you forget to `--recurse-submodules` you will need to do `git submodule update --init`.

Change to the Ironhead directory and create a virtual environment (not required but recommended):

`path/to/parent/directory/Ironhead$ python -m venv ./venv`

Activate the virtual environment:

* In Bash (Linux, Unix, etc): `source venv/bin/activate`
* In Windows Powershell: `venv\scripts\activate.ps1`
* Commands for other command-line environments [here](https://docs.python.org/3/library/venv.html#creating-virtual-environments) (scroll down to the table)

After activating the virtual environment, the command prompt will be preceded by `(venv) `.

Install the requirements:
`(venv) path/to/parent/directory/Steelhead$ pip install -r requirements.txt`

# Running the Simulation
```
(venv) path/to/parent/directory/Steelhead$ python run.py
channel_def: prod_csv/cl_HSC.csv does not exist                           <<< List of channel files with no associated
channel_def: prod_csv/cl_HSC.csv does not exist                           ... productions
channel_def: prod_csv/cl_BSC.csv does not exist
channel_def: prod_csv/red_6_CSD.csv does not exist
channel_def: prod_csv/green_6_CSD.csv does not exist
Loading WP3_PoH20220217                                               <<< Loading the base scenario and running to 
Loading WP3_PoH20220217                                               ... generate the table
+----------------------------------------------------------------------+  <<< Table of simlation results for the base 
| Scenario: WP3_PoH20220217                                        |  ... scenario
| 3-4a Work plan rev 3                                            |  <<< Multi-line scenario description
| 4b-5 PoH Prelim Schedule 20220217                                    |
+----------------------------------------------------------------------+
 Project Section                   Dig CY    Days Start Date   End Date
------------------------------------------------------------------------
 BSC     Green BSC Berth 1         388358      25 2023-05-03 2023-05-27
 BSC     Green BSC Berth 7         391977      29 2023-05-28 2023-06-25
 BSC     Red BSC CSD              2458232     157 2022-11-27 2023-05-02
 BSC     Green BSC Flare           350984      17 2023-06-26 2023-07-12
========================================================================
 BSC     totals                   3589550     228 2022-03-27 2023-07-12
------------------------------------------------------------------------
 3-4a    Red 3-4a CSD             1545468     140 2022-05-09 2022-12-08
 3-4a    Green 3-4a CSD            879311      74 2022-05-22 2022-10-26
 3-4a    Red 3-4a ODMDS            596974      86 2022-03-28 2022-08-09
 3-4a    Green 3-4a ODMDS         1295391     178 2022-04-10 2022-12-16
========================================================================
 3-4a    totals                   4317144     478 2022-03-27 2022-12-16
------------------------------------------------------------------------
 4b-5    Red 4b-5 CSD 1           1999541     117 2024-02-03 2024-07-27
 4b-5    Green 4b-5 CSD 1         1788593     103 2024-05-05 2024-09-09
 4b-5    Green 4b-5 CSD 2          890989      74 2023-07-13 2023-09-24
 4b-5    Red 4b-5 CSD 2           1695010     131 2023-09-25 2024-02-02
 4b-5    Green 4b-5 Mitigation     690999      94 2022-11-21 2023-02-22
 4b-5    Red 4b-5 Mitigation       943622     127 2023-02-23 2023-06-29
 4b-5    Red 4b-5 ODMDS Full      2297086     173 2023-02-16 2023-08-07
 4b-5    Green 4b-5 ODMDS Full    1927353     145 2023-08-08 2023-12-30
 4b-5    Green 4b-5 Strip          446773      29 2022-11-21 2022-12-19
 4b-5    Red 4b-5 Strip            945453      58 2022-12-20 2023-02-15
========================================================================
 4b-5    totals                  13625419    1051 2022-03-27 2024-09-09
------------------------------------------------------------------------

Loading WP3_PoH20220217
Interface starting at http://127.0.0.1:8521                               <<< Starting the Tornado web server
Socket opened!
{"type":"get_params"}
{"type":"reset"}
Loading GLDDWP3_PoH20220217                                               <<< Displaying the base scenario in
                                                                          ... the visualization
```
