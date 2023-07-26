# SBCPiezoGUI

A user interface for acoustic digitzer signals. 

## Acknowledgements
The files in include/SBCBinaryFormat were written by Hector Hawley Herrera and can be found [here](https://github.com/SBC-Collaboration/SiPMCharacterization). Other dependencies include [imgui](https://github.com/ocornut/imgui), [implot](https://github.com/epezent/implot), [ImGuiFileDialog](https://github.com/aiekick/ImGuiFileDialog/tree/master), [glfw](https://github.com/glfw/glfw) and [spdlog](https://github.com/gabime/spdlog); these do not need to be separately installed.

## Control Panel
There are four tabs on the control panel:

#### Acquisition Options
![acqoptions](https://github.com/cmitch819/SBCPiezoGUI/assets/22796402/a358e155-17d2-4f86-a1b9-cf15919936d6)

These control parameters that remain constant while the digitizer is acquiring data, including the number of samples per event to collect (both before and after a trigger) and the total number of events. Data saving is also included; you can save in .txt or .sbc.bin format, with an additional option to save whatever signals are currently being displayed on the UI (in case you didn't select Save File before acquiring and now want to save your data). 

#### Channel Options
![chanoptions](https://github.com/cmitch819/SBCPiezoGUI/assets/22796402/f7853fa9-265f-4e6e-bf13-e69c8b8b3cd4)

These control channel-specific parameters, including input range in mV and DC offset as a percentage of the input range. See SBC-Piezo-Base-Code repository or the Gage INI file documentation for more info.

#### Trigger Options
![trigoptions](https://github.com/cmitch819/SBCPiezoGUI/assets/22796402/41a00d63-93c1-4e24-99de-9f8620d8a4b7)

These control trigger parameters, including number of triggers and parameters for each.

#### Display Options
![dispoptions](https://github.com/cmitch819/SBCPiezoGUI/assets/22796402/fd3f8f52-3ad4-4cd8-b131-8a64f567e75e)

This tab controls displaying data from previous runs. It can only open from .sbc.bin files, but it recognizes files with multiple events and these can be viewed using event number input as well as arrows to go from one event to the next consecutive event. It can display files that have been recorded directly from the UI, as well as files recorded with the SBC-Piezo-Base-Code repository.

### Known Issues
- Currently, external trigger range is set by default to 10000 mV (+/-5 V), so the level percentage is with respect to this; I can fix this in a future revision if necessary
- If you don't set the right parameters in Acquisition Options and want to stop a run, you have to close the entire UI to do this (it is not multithreaded, and multithreading may not even work on Linux, so this is something that we just have to deal with for now).
- In file saving, the timestamp of the first event appears to be wrong (it's always much higher than the next event, although the following timestamps seem correct relative to each other). Since timestamps are a minor feature of this program and the UI may not even be used when recording actual runs (since there is a no-UI version that exists already), I haven't dedicated a lot of time to fixing this yet, although I can do that if necessary.

## Graph Panel
![graphs](https://github.com/cmitch819/SBCPiezoGUI/assets/22796402/80794811-4a8e-4826-ab8b-ec144cf97bad)

Each graph corresponds to a digitizer channel. If your signal is not visible on the y-axis, double click on it to zoom to fit the data. You can also click and drag or scroll to adjust position and scale.

### Known Issues

- If your data is too long (around 12-15k samples or more), you won't be able to view all data at the same time on the x-axis (you shouldn't double click to rescale or the program will throw an error as it won't have enough space to display all the data). You can still scroll/click and drag to view later samples, though, and all your desired data will still be collected and saved. I'm considering adding functionality to hide certain graphs from view or changing layout to allow for longer graphs.
- Not really an issue, but I could probably do error handling a bit better for SBCBinaryFormat and ImGui errors (Gage errors should be handled ok)
