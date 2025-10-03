# Performance Test Scripts

This repo contains scripts to generate BOB and OPI screens to run in Phoebus and CS-Studio to compare the performance. 
There is also a script to generate the EPICS DB file and a script to monitor the CPU and memory usage of a process.


## Usage

- Clone.

### Create DB file
- Create a DB file to run with EPICS that contains 200 PVs updating at 10Hz between the values 0 and 1. This creates the file `performanceTestDb.db`.
  ```
  ./create_db.sh -n 200
  ```
- Run:
  ```
  softIoc -d performanceTestDb.db
  ```
  This is needed for both performance tests against CS-Studio and Phoebus.

### CS-Studio Tests
- Create OPI files. The following command will create 10 OPI screens each with 20 TextUpdate widgets connecting to the PVs running in the IOC above. 
  ```
  ./create_opi.sh -n 20 -s 10
  ```
  These are saved in the `/opi/` directory
- To open these files with CS-Studio from the command line you may have to copy them into you CS-Studio workspace. Something like:
  ```
  cp opi/*.opi /home/<user>/cs-studio/<user>
  ```
- Start CS-Studio with these screens (adjust for your `<user>`):
  ```
  cs-studio --launcher.appendVmargs -clearPersistedState -share_link /home/<user>/cs-studio/<user>=/CSS/<user> -name cs-studio
  --launcher.openFile "/CSS/<user>/performanceTestPage0.opi Position=NEW_SHELL" "/CSS/<user>/performanceTestPage1.opi Position=NEW_SHELL"
  "/CSS/<user>/performanceTestPage2.opi Position=NEW_SHELL" "/CSS/<user>/performanceTestPage3.opi Position=NEW_SHELL"
  "/CSS/<user>/performanceTestPage4.opi Position=NEW_SHELL" "/CSS/<user>/performanceTestPage5.opi Position=NEW_SHELL"
  "/CSS/<user>/performanceTestPage6.opi Position=NEW_SHELL" "/CSS/<user>/performanceTestPage7.opi Position=NEW_SHELL"
  "/CSS/<user>/performanceTestPage8.opi Position=NEW_SHELL" "/CSS/<user>/performanceTestPage9.opi Position=NEW_SHELL"
  ```
 - Monitor the CPU of this process. First find the process ID (`<PID>`) and then from another terminal run:
    ```
    ./cpu_monitor.sh -p <PID>
    ```

### Phoebus Tests
- Note: in your Phoebus settings file, uncomment or add this line to ensure Phoebus does not limit our update rates:
  ```
  org.csstudio.display.builder.runtime/update_throttle=1
  ```
- Create BOB files. The following command will create 10 BOB screens each with 20 TextUpdate widgets connecting to the PVs running in the IOC above. 
  ```
  ./create_bob.sh -n 20 -s 10
  ```
  These are saved in the `/bob/` directory
- Start Phoebus with these screens (adjust for your `<user>`):
  ```
  phoebus -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage0.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage1.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage2.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage3.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage4.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage5.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage6.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage7.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage8.bob?target=window \
  -resource file:/home/<user>/performance_test_scripts/bob/performanceTestPage9.bob?target=window \
  ```
 - Monitor the CPU of this process. First find the process ID (`<PID>`) using System Monitor or another tool and then from another terminal run:
    ```
    ./cpu_monitor.sh -p <PID>
    ```
    - Try with the screens at start up size and then when all have been maximised to see the CPU difference.
