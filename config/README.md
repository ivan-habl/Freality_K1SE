# Creality-K1-Config
Creality K1 normal fan config

Orca start gcode:
```
M140 S0
M104 S0 
M141 S[chamber_temperature]
SET_FILAMENT TYPE=[filament_type]
START_PRINT EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer]
```
