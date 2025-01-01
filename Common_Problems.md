# Some Commmon Problems that I Encounter

## "Permission Denied": Attempting to Connect to a Serial Device

1. Check permissions (ensure that the read and write bits are set)
2. Add "dialout" to users group.
   ```
   #!/bin/bash

   sudo adduser $USER dialout
   ```

## Cannot Program the Arduino Nano ESP32-S3 Using the Out-of-Box Upload command from the Arduino CLI

The normal `arduino-cli upload` command does not do well when attemtping to program the ESP32-S3. Instead, I have opted to compile the files and output the binary files to a seperate folder, which the actual esptool will flash the ESP32-S3 with. The workflow looks somewhat like this:

```
# Make a seperate folder 'bin' or whatever your choice is
mkdir bin

# Now, compile the sketch and put the bins into the 'bin' folder
arduino-cli compile -b arduino:esp32:nano_nora --build-path bin/

# Use esptool.py to load the bins to the ESP32
# Note the memory locations used for this command:
#       0x0000 -> Bootloader Memory Address
#       0x8000 -> Partition Table
#       0xe000 -> Boot App (if present)
#       0x10000 -> Main Application (your sketch)
esptool.py --chip esp32s3 --port /your/port --baud 921600 write_flash -z --flash_mode dio --flash_size detect \
   0x0000 ./bin/YOUR_SKETCH.ino.bootloader.bin \
   0x8000 ./bin/YOUR_SKETCH.ino.partitions.bin \
   0xe000 ./bin/boot_app0.bin \ # May be an optional bin that appears; have not encountered it yet. This will not always be visible. 
   0x10000 ./bin/YOUR_SKETCH.ino.bin
```

This has worked for me and replicates the behavior of the Arduino IDE. I'll probrobly write a bash script for this at some point because I need this to be automated. 

## Manually adding Vim plugin help

If one is installing Vim plugins manually, the help files will need to be loaded in this manner:

```
# We are assuming that the plugin has been moved to .vim/pack/{plugin}/start/
# We are naviagting to the docs folder just to ensure that it exists
$ cd .vim/pack/{plugin}/start/{plugin}/doc
```

Back in Vim, run the command `:helptags .vim/pack/{plugin}/start/{plugin}/doc`. From here, the files *should* be accessible via help.

# A Few Other Helpful Coding Snippets

## Source Code Header Template

*Provided by ChatGPT*

```
/****************************************************
 * File Name: <filename.ext>
 * Author: <Your Full Name or Username>
 * Date Created: YYYY-MM-DD
 * Last Updated: YYYY-MM-DD
 * Version: <version number> (e.g., 1.0.0)
 * 
 * Description:
 *   A brief overview of what the file does, what the main
 *   algorithm accomplishes, and any important design decisions.
 *
 * Dependencies:
 *   List any required external libraries, frameworks, or
 *   environment requirements.
 *
 * Usage:
 *   Briefly explain how to use this file or the functions it contains,
 *   including example command-line arguments or method calls if applicable.
 *
 * Inputs:
 *   A description of input parameters or data files (if any).
 *
 * Outputs:
 *   A description of the output, return values, or files produced.
 *
 * License:
 *   <Specify the license type (e.g., MIT, GPL-3.0, Apache 2.0)>
 *   <Optional copyright notice: © 2024 Your Name>
 *
 * Known Issues:
 *   Mention any known bugs, limitations, or areas for improvement.
 *
 * References:
 *   Cite any sources, documentation, or research papers that were
 *   used in the development of this algorithm.
 ****************************************************/

```
