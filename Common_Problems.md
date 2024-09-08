# Some Commmon Problems that I Encounter

## "Permission Denied": Attempting to Connect to a Serial Device

1. Check permissions (ensure that the read and write bits are set)
2. Add "dialout" to users group.
   ```
   !#/bin/bash

   sudo adduser $USER dialout
   ```
