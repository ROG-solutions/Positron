Gidday all, 

This is my printer.cfg setup, 

Let me know your thoughts, I am always looking for improvements. This is actually my first 
time coding Python, so it has been a bit of learning.

Some requirements to run this config on your Positron style printer:

  - Check heater and fan wiring! This caught me out.
      I have a different fan/heated bed setup, I think mine is the same as the SKR Pico docs, 
      but in any case. Please check your own matches the config!!!
      
  - Microprobe jumper
      If you are planning to run the BIQU Microprobe, on the SKR Pico, you need to ensure you 
      have the jumper set. 
      https://github.com/bigtreetech/SKR-Pico/blob/master/BTT%20SKR%20Pico%20V1.0%20Instruction%20Manual.pdf
      See point number 5.
  
  - Button macros/General macros
      I noticed a newer version of Mainsail has most macros hidden by default, there are some I think are more
      useful than others, have a quick read through the list, or search. Hopefully the names and descriptions 
      make sense. If not, feel free to DM me on Discord @ROGsolutions.
      
      I have a plan to make physical buttons for certain functions, warmup/cooldown, load/purge, and unload.
      The code for this has come from Meteyou's github, currently, they are just macros, the code is still 
      present for making them into fully fledged buttons, but I have yet to sort the hardware out. I will
      likely use GPIO pins on the Pi for this. 
      
  - Homing override code
      The homing override code should allow you to home individual axes, and gives an error when 
      homing the Z axis without the bed connected, and also when homing without first removing the inputhshaper.
      I use this warning, because currently my setup for the input shaper would be damaged if the Z axis homed 
      with it still connected.
    
  - KAMP install
      Klipper has native support for adaptive bed meshing now. The meshing.cfg should be all you need, then call G29
      to adaptive mesh (eg inside start_print)
      If you want to use KAMPs extra features like adaptive purging etc, you will need to install this yourself,
      instructions can be found at:
      https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging
      
  - Slicer Gcodes
      You may need to update your slicer Gcode, I use Orca slicer, and cannot speak for others, but you 
      need the following as a minimum in the start Gcode for Orca, you also need to ensure the slicer is defining objects.

          PRINT_START EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer_single]

          And the following in the slicer end Gcode:

          PRINT_END
          
  - Running input shaper
      The best thing to do is to run the macro "pre_resonance_test" this will position the print head, then include PIS.cfg
      in includes.cfg, then connect via usb, if you then run "Good_vibes_only" it will automatically run and save to config
      the recommended freq and shaper type. You will have to recheck the console for the recommended max_accel.
      I recommend checking the PIS.cfg, it has more details about the code to run to view the graphs, which may be more useful
      in determining max_accel and shaper type.
      I run the portable input shaper from FYSTEC, but others with the RP2040 are out there, there are more 
      specific details in the PIS.cfg, you need to run a few lines over SSH to find the USB ID.

  - Calibrate MCU Temp reading
      You will need to calibrate the MCU temps etc yourself, inside the fans and heaters .cfg
