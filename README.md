# Control state feedback plugin (Christmas tree)

## Description
Shows LED feedback of controls referenced during runtime by Component ID and control name.
Boolean-type controls show the current state, and trigger-type controls show feedback for
a specified period of time after an event occurs.

![Usage Example](https://github.com/user-attachments/assets/039f1353-3cb7-4353-a6d7-97b5e83b0df0)

## Requirements
Requires Q-SYS Designer version 9.10 or newer.

## Usage
Set the ID field to the component ID and control name that you'd like to monitor, in the format `<COMPONENT>.<CONTROL>` where `<COMPONENT>` is the name of the component (must have `Script Access` set to at least `Script`), and `<CONTROL>` is the name of the control to monitor (use the "View Component Controls Info" tool to see the script names of controls).

Spaces are allowed and left in as-is. The names are split on the first period, so control names may themselves contain periods but the component name may not. Remember, everything is case-sensitive.

## Design-time Properties
* **`Count`**: The number of possible controls to monitor (1 to 100, default 5)
* **`Trigger Feedback Time`**: The number of seconds to light an LED after a Trigger-style event occurs (in seconds, 0 to 300, default 1.5)
* **`Debug Print`** (available if `Show Debug` is `Yes`): Level of runtime debugging messages to print
  * `None`: Do not print debug messages (error messages will still be printed)
  * `Debugging`: Prints additional debugging messages and parameters
  * `Function Calls`: Prints each function name as it is called (and sometimes parameters passed)
  * `All`: Both `Debugging` and `Function Calls`

## Details
This plugin adds an `EventHandler` to each control to monitor its state. If the `Type` property is found in the `triggerControls` list, it is considered to be a Trigger-style controls and the `TriggerHandler` function is used for the `EventHandler`; for any other control type, the `Boolean` property is used and the `ValueHandler` is assigned to the `EventHandler`.

Trigger-style controls will turn the associated LED on for a brief period then off again. These include Trigger, Text, and Array/Enum. If the trigger is executed before the LED turns off, the second trigger will not re-start the timer. The `Trigger Feedback Time` property sets how long the LED will remain on (in seconds) for Trigger control feedback.

Other controls will set the LED feedback based on the control's `Boolean` value. Knobs, meters, faders, etc. are supported but they are not the intended use-case for this tool. See the [Q-SYS help](https://q-syshelp.qsc.com/#Control_Scripting/Using_Lua_in_Q-Sys/Controls_IO.htm) to learn more about what the `Boolean` property is for controls that aren't simply on/off. 

If a component or control referenced doesn't exist or is inaccessible, the LED for that line will be marked as indeterminate (or if no name is specified). This allows for quick determination if the component and control name entered are valid.

## License
```
Control state feedback plugin (Christmas tree)
Copyright (C) 2024 Jonathan Dean - Spectrum Integrators (jonathand@spectrumintegrators.com)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```
(see the _LICENSE_ file for full license details)
