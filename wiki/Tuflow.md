Tuflow
=====

***

- [Model setup](#start)
- [Model optimization](#optimize)
- [Run models](#run)
- [Output](#output)

***

## Get Started<a name="start"></a>

*Hy2Opt*'s tabs invites the user for the creation of a model.

![ragui](https://github.com/sschwindt/hy2opt/raw/master/assets/images/tuflow_start.png)



## Model creation<a name="mc"></a>

The *Create Model* window guides in X steps through the creation of a template for generating  a *Tuflow*  model. 

### Model control parameters<a name="mcp"></a>
The first step inquires input for model controls, which will be written to *runs/init/`MODEL_NAME.tcf`*. It asks for the following parameters, where drop-down menus provide valid *Tuflow* options (where applicable):

| Parameter               | Description      |
| ----------------------- | ---------------- |
| `License type`          | *Tuflow* provides different licensing options that enable or restrict the modelling performance. ***Hy2Opt* enables the definition of subsequent parameters as a function of the select license.** |
| `Model Precision` |  Recommended: `Single`. `Double` precision slows down the calculation on GPUs and should only be used for the `Classic` scheme with elevations of more than 100-1000m or with direct rainfall models (mass errors may otherwise occur here). *Hy2Opt* defines model precision via definition of the relevant *Tuflow* executable in the batchfile. |
| <sup>\*</sup>`Solution Scheme` | The `Classic` scheme is a (relatively) slow implicit solver that runs on one single core. The `HPC` scheme uses multiple CPUs (or GPU hardware) with an explicit solver for high speed calculation.|
| `Units`                 | Select a unit system (must comply with the units of the later on required topography / DEM raster).    |
| <sup>\*</sup>`Hardware` | If your computer has a **powerfull** GPU, this can increase the computing speed many times over. Otherwise (non or non-powerfull GPU), select `CPU`.|
| `Viscosity Formulation` |   Preferably use `SMAGORINSKY`. Use `CONSTANT` only when the cell size is much larger than the (expected) flow depth or when other turbulence sources govern (e.g., bed roughness).|
| `Viscosity Coefficients` |   *C<sub>s</sub>* is the dimensionless [*Smagorinsky* coefficient][smag] and has a default value of 0.5. *C<sub>c</sub>* is the constant *Smagorinsky* coefficient and has a default value of 0.05 m²/s (US costomary: use 0.5382 ft²/s). Lower values tend to increase stability, higher values increase calculation speed.  If `Viscosity Formulation`==`CONSTANT` is used, *Hy2Opt* automatically uses the first `Viscosity Coefficient` only, but then in units of m²/s (or ft²/s), where the recommended value is 1.0 m²/s.|
| `Cell Size` |  Min. the resolution of the input DEM raster. Coarse resolution results in higher calculation speed an less accuracy. The default is 1.0m. |
| `Timestep` |  The timestep (in seconds) depends on the cell size and should be max. 1/2 to 1/5 of the cell size in meters. The timestep must be small enough to satisfy the [Courant-Friedrichs-Lewy condition][cfl] for numerical stability. The `HPC` solutions scheme uses adaptive timesteps and the here defined value only repreents the initital condition. *Hy2Opt* comes with a calculator (`Estimate` button) for estimating the adequate timestep. |
| `Cell Wet/Dry Depth` | Default: 0.002m. Define according to estimated flood magnitude depth. Should be smaller for direct rainfall approaches (shallow sheet flow).|
| `Initial water level (IWL)` | Default: `"AUTO"`. Should correspond to the target initial flow water level at the model outlet (downstream). *Hy2Opt* will override this value with a GIS file when model optimization is performed. |

<sup>\*</sup>`PARAMETER`s marked with an asterisk are only available with a full *Tuflow* license.


### Model output parameters<a name="mop"></a>
The next step inquires output parameters:
| Parameter               | Description      |
| ----------------------- | ---------------- |
| `Map Output Format` | Multiple selection of map output formats is possible (hold the `CTRL` key and click multiple entries). |
| `Map Output Data Types` | Select a map output format and define output data types. Multiple selection of map output data types is possible (hold the `CTRL` key and click multiple entries). [See definitions of Map Output Data Types](#modt).|
| `Map Output Interval` | Default: 360 seconds. Enter an integer number to replace the default value. |


### Geometry<a name="geo"></a>
The next step inquires geometry and geographic parameters, as well as geofiles, which will be written to the *model/`MODEL_NAME.tgc`* files.
| Parameter              | Default      | Description      |
| -----------------------|--------------|------------------|
| `READ GIS LOCATION` | `model\grid\2d_loc_MODEL_L.shp` | *Shapefile (`shp`)* defining the location where *Tuflow* creates the boundary. |
| `Grid Size` | `(X,Y)` - `tuple`| Dimensions of the grid (`X`-rows and `Y`-columns in m or ft *?*); must not be an exact multiple of `Cell Size`.|
| `Set Zpts` | `3000` (`Int` in m) | Default definition of *Zpts* (Section 6.2 in the [*Tuflow* manual][tfman]). The relevant points are overwritten with the next parameter (`Read GRID Zpts`). A high value ensures that gaps in the `asc` or `flt` DEM raster can easily be identified. |
| `Read GRID Zpts` | `model\grid\asc` or `flt` DEM raster | The preferred method to define Z-point elevations in *Hy2Opt* (alternatives: `Read GRID <option>` as defined in the [*Tuflow* manual][tfman] in Appendix C). |
| `Set Code` | `0` (`Int` as Boolean)| Defines that the DEM is trimmed to the model boundary. Note: `Set Code` must be defined after `Read GRID Zpts`. |
| `Read GIS Code` | `model\gis\2d_code_MODEL_R.shp`|  Select the shapefiles that indicates the initial expected wetted area within a polygon. Active (wetted) polygons must have a code-value=1 and may be oriented at aerial imagery. |
| `Set Mat` | `1` (`Int` of default material)  | Define the default material ID for all cells that are not defined in the materials polygon (see `Read GIS Mat`).|
| `Read GIS Mat` | `model\gis\2d_mat_MODEL_R.shp` | Polygon shapefile defining material IDs. Parameter can be ignored for using the default material (see `Set Mat`) everywhere. |

The following geometry parameters will be written to the *model/`MODEL_NAME.tbc`* file. 
| Parameter              | Default      | Description      |
| -----------------------|--------------|------------------|
| `Read GIS BC` | `model\gis\2d_bc_MODEL_HT_L.shp` | *Shapefile (`shp`)* defining the downstream flow exit line (`L`) that will be defined with a water surface elevation (`H`) over time (`T`). |
| `Read GIS SA` | `model\gis\2d_sa_MODEL_QT_R.shp`| *Shapefile (`shp`)* defining the *Source Area* (`sa`) in terms of a polygon that delineates the upstream inflow region (`R`), where a discharge (`Q`) will be defined over time (`T`). Use a polygon rather than a line to enable the GPU solver (cannot run with a line).|

| <sup>\*</sup>`PAR` |                    |
| `PARR` |                    |

Note that *Hy2Opt* writes the `Cell Size` parameter in the [`.tcf`](#mcp) file.

***

## Model optimization<a name="optimization"></a>
text

## Run Tuflow models<a name="run"></a>

text

## Output<a name="output"></a>

text

## Technical details<a name="tec"></a>
### Map Output Data types<a name="modt"></a>
The following table is taken from the [Tuflow Manual][tfman] (Section 9.7).

| Flag | Map Output Data Type | Supported Formats | Description   |
|:----:|:---------------------|:------------------|:--------------|
| AP   | Atmospheric Pressure         | All formats excluding WaterRIDE | Atmospheric pressure in hPa.  Atmospheric Pressure is only applied if using the Read GIS Cyclone or Read GIS Hurricane commands.  Maximum and minimum output is not available.  |
| BSS  | Bed Shear Stress             | All formats excluding WaterRIDE | Bed Shear Stress as given by the equation below where ρ is density, g gravity, V velocity, n Manning’s n and y depth. The Bed Shear Stress map output can be misleading at very shallow depths as the BSS formula divides by the depth.  The BSS and SP outputs are linearly reduced to zero once the depth is below a threshold (by default, 0.1m).  This threshold can be changed using the .tcf command BSS Cutoff Depth.   Tracking of maximum BSS was enabled in  version 2016-03-AB of TUFLOW Classic and 2017-09-AC of TUFLOW HPC.Prior to the 2017 release BSS output in English Units were in Poundals per square foot (pdl/ft2). From the 2017 release onwards the units will be Pounds Force per square foot (lbf/ft2), therefore the BSS values are 32.174 times smaller than for releases prior to 2017.  |
| CI   | Cumulative Infiltration      | All formats excluding WaterRIDE | The cumulative infiltration over the entire simulation in mm or inches when a soils infiltration method has been used (see Section 6.10).  See also the IR (infiltration rate) map output type below.  Maximum and minimum output is not available, as it is a cumulative output, the maximum will be the output at the final timestep.  |
| Cr   | Courant Number               | All formats excluding WaterRIDE | Courant number (2D domains only at present). Maximum and minimum output is not available. |
| CWF  | Cell Width Factor            | SMS HIGH RES only               | Cell side flow width factor.  Used to monitor the changes in cell side widths over time if using Read GIS Layered FC Shape.  Maximum and minimum output is not available. |
| d    | Depth                        | All formats excluding WaterRIDE | Water depths.  For the cell cornered results formats (see Section 9.6.7.3) the depths are calculated as the interpolated water level at the nodes (see _h.dat below) less the ZH value.  The interpolated water level may occasionally lie below the ZH value, in which case a negative depth may result which is set to zero by default (see Zero Negative Depths).  Both maximum and minimum output is available. For maximum depth output this is calculated at the end of the simulation based on the maximum water level and the ground elevation.  For models that utilise varying ground elevations (using the Read GIS Variable Z Shape or variable geometry (VG)  boundaries), care should be taken when interpreting maximum depth outputs.  Hazard outputs (based on velocity and depth) are tracked at each timestep, and the maximum for these is the maximum at any timestep during the model. |
| dGW  | Depth to Groundwater         | All formats excluding WaterRIDE | Depth to groundwater (from the ground surface) over time in metres or feet when a groundwater depth or level has been defined (see Section 6.10.5). Maximum and minimum output is not available. |
| E    | Energy                       | All formats excluding WaterRIDE | Scalar data file containing the energy levels at the element nodes (cell corners).  The energy levels are based on the interpolated water levels calculated at the cell centres plus the dynamic head (V2/2g).  Due to the interpolation, occasionally an “increase” in energy can occur - an alternative approach to correctly display energy without interpolation is being trialled using the HIGH RES options (see Map Output Format). For 1D areas, this output should be treated with caution as it is derived from interpolation of water levels and approximations of the channel velocities across the WLLs, which can be problematic in 1D channels with high velocities.  The energy output for 1D nodes is available as part of the plotting output (Section 13.2). Maximum energy levels is for when the maximum water level occurs (Note: This may cause undulations in the energy due to variations in the time of the maximum water level, except for the HIGH RES options (see Map Output Format), which monitor the energy level every timestep to set the maximum.)  |
| F    | Froude Number                | All formats excluding WaterRIDE | Froude number output.  No maximum and minimum output is available at this stage.  |
| FLC  | Form Loss Coefficient        | SMS HIGH RES only               | Form Loss coefficient.  Used to monitor the changes in the form loss coefficient over time if using Read GIS Layered FC Shape. Maximum and minimum output is not available.  |
| h    | Water Level                  | All formats                     | Water level output.  For the cell cornered results formats (see Section 9.6.7.3) the water levels are interpolated from the water levels calculated at the cell centres.  Both maximum and minimum outputs are available.  If using the HIGH RES options (see Map Output Format), interpolated water levels allow for the effects of upstream controlled flow regimes (e.g. supercritical flow).  |
| IR   | Infiltration Rate            | All formats excluding WaterRIDE | The infiltration rate in mm/hr or inches/hr over time when a soils infiltration method has been used (see Section 6.10).  See also the CI (cumulative infiltration) map output type above.  Maximum and minimum output is not available. |
| MB1  | Mass Balance                 | All formats excluding WaterRIDE | Measure of the convergence level of the solution.  The measure is a cumulative value since the last output time, therefore is an effective way of identifying problem areas in a model that repeatedly have poor convergence and most likely mass error.  Very useful for identifying problem areas within a model.    |
| MB2  | Mass Balance                 | All formats excluding WaterRIDE | Same as MB1 above but is accumulated over the entire simulation.This output does not include 1D output from WLLs.  |
| n    | Manning’s n                  | All formats excluding WaterRIDE | Manning’s n values.  The n values only vary over time for materials using the Manning’s n varying with depth feature.  The n values at the cell corners in the _n.dat file are interpolated from the surrounding four cell mid-sides.  Maximum and minimum output is not available.   |
| q    | Vector Unit Flow             | All formats excluding WaterRIDE | Unit flow (m2/s, flow per unit width) at the nodes (cell corners).  The resulting flow vector is calculated from the surrounding u and v-points and the depth determined in _d.dat above.  Unit flow may also be used as a measure of flood hazard (i.e. velocity by depth or VxD). Note:  The maximum unit flow is not tracked for the q output, the Z0 hazard value option can be used, as this output is tracked at each timestep. |
| R    | Flow Regime                  | All formats excluding WaterRIDE | Flow regime.  The output value is 0 (zero) for normal (sub-critical flow with momentum); greater than 1 for upstream controlled friction flow (e.g. supercritical flow); ‑1.5 for broad-crested weir flow; and ‑1 for flow through a flow constriction when the deck is submerged.  No maximum and minimum output is available at this stage. When using the default cell corner approach, the flow regime is a weighted average of the flow regimes at the four adjoining cell mid-sides, which can therefore be misleading.  Using the Map Output Format == SMS HIGH RES option will output the exact flow regime occurring at the cell mid-sides.  |
| RC   | Route Category               | All formats excluding WaterRIDE | The route category output over time for evacuation routes.  The definition and number of categories is based on the values specified within the Cut_Off_Values attribute of the 2d_zshr GIS layer (see Section 9.5.1).  The RC values are output as an integer representing the closure category specified by the user. The maximum RC category value is tracked every timestep and output (if tracking maximums is switched on, which is the default). |
| RFC  | Cumulative Rainfall          | All formats excluding WaterRIDE | The cumulative rainfall in mm or inches over time when direct rainfall has been applied to the model (refer to Sections 7.4.3.2).  See also the RFR (rainfall rate) map output type below.  Both the RFC and RFR outputs (see next item) are inclusive of any boundary adjustments (e.g. in the boundary database) and rainfall losses applied in the materials file.  Soil infiltration is applied once the rainfall has been applied to the cells, so this is not accounted for in the rainfall outputs, see also CI (cumulative infiltration) and IR (infiltration rate) output types. Maximum and minimum output is not available, as it is a cumulative output, the maximum will be the output at the final timestep. |
| RFML | Material Based Rainfall Loss | All formats excluding WaterRIDE | The output contains the total rainfall losses applied due the initial and continuing rainfall losses specified in the “Read Materials File ==” (.tmf or .csv) file.  The RFML option can be used to track the rainfall based material losses that have been applied spatially.  The RFC and RFR map output data types can be used to output the cumulative rainfall and rainfall rate. |
| RFR  | Rainfall Rate                | All formats excluding WaterRIDE | The rainfall rate in mm/hr or inches/hr over time when direct rainfall has been applied to the model (refer to Sections 7.4.3.2).  See also the RFC (cumulative rainfall) map output type above. Maximum and minimum output is not available. |
| SP   | Stream Power                 | All formats excluding WaterRIDE | Stream Power as given by the equation below where τbed is bed shear stress (see BSS above) and V is velocity.The Stream Power map output can be misleading at very shallow depths as the BSS formula divides by the depth.  The BSS and SP outputs are linearly reduced to zero once the depth is below a threshold (by default, 0.1m).  This threshold can be changed using the .tcf command BSS Cutoff Depth.  Tracking of maximum SP was enabled in  version 2016-03-AB of TUFLOW Classic and 2018-03-AB of HPC. Prior to the 2017 release SP output in English Units were in Poundals per square foot (pdl/ft2). From the 2017 release onwards the units will be Pounds Force per square foot (lbf/ft2), therefore the SP values are 32.174 times smaller than for releases prior to 2017.|
| SS   | Sink / Source Flow           | All formats excluding WaterRIDE | The net source/sink inflows.  Note the flow rate for a cell is shown at the ZH point (top right of the cell), except for the HIGH RES options (see Map Output Format), which are spatially correct (note the HIGH RES CORNERS ONLY option will interpolate sink/source flow rates to the cell corners). Maximum and minimum output is not available.   |
| t    | Viscosity Coeff              | All formats excluding WaterRIDE | Eddy viscosity coefficient.  This is useful for checking the Smagorinsky coefficient values.  No maximum and minimum output is available at this stage. |
| tau  | Shear stress                 | All formats excluding WaterRIDE | This output contains the shear stress values applied via the external stress file (.tesf). The output values are in Newtons per square metre (N/m2) for SI units and pound-force per square foot (lbf/ft2) for US customary (English) units.  |
| V    | Vector Velocity              | All formats                     | Flow velocity.  The resulting velocity vector is calculated from the surrounding u and v-points. Note:  The maximum and minimum velocities are tracked over time.  By default the maximum velocities are tracked over 0.1m depth, below this depth the velocity at maximum water level is used.  See the Maximum Velocity Cutoff Depth command for more information. |
| ZH   | Bathymetry                   | All formats excluding WaterRIDE | Elevations at the cell corners (ZH points).  This information is already contained in the .2dm file, however, this option is useful if the model’s bathymetry varies over time because of variable geometry (2d_vzsh or VG boundaries) or for morphological modelling.  This output is very useful if you are comparing two or more runs that have different topography (e.g. before and after scenarios), and you wish to easily view or compare the topography for each scenario within SMS or post-process using TUFLOW_to_GIS (refer to Section 15.2.1). If the topography in the model does not change over time (i.e. no variable Z shapes or morphological changes), for the default .xmdf output format the ZH Zpt values are output once, rather than every timestep, thereby not consuming disk space unnecessarily.   The ZH map output will appear under a XMDF folder “Fixed”.   This feature is only available if using the XMDF format, for other output formats, the bathymetry will be output at each output interval. For builds 2016-03-AC onwards, this output is available for TUFLOW GPU.  Builds prior to 2016-03-AC had this output disabled for GPU simulations. No maximum and minimum output is available at this stage. |


[cfl]: https://en.wikipedia.org/wiki/Courant%E2%80%93Friedrichs%E2%80%93Lewy_condition
[smag]: https://en.wikipedia.org/wiki/Large_eddy_simulation#Smagorinsky%E2%80%93Lilly_model
[tfman]: https://www.tuflow.com/Download/TUFLOW/Releases/2018-03/TUFLOW%20Manual.2018-03.pdf
