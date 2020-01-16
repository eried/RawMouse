
# RawMouse

---

RawMouse is a Cura plugin that provides raw (driverless) access to HID mice such as the 3Dconnexion spacemouse.

Primarily intended for use on Linux (tested on Ubuntu 16.04.6), it does also work on Windows (tested on Windows 10) and MacOS (tested on MacOS 10.13).

The plugin includes binary components (cython-hidapi) that are required to access USB devices.

---

### Limitations

This plugin can only be used with devices whose HID message protocol it knows about. At this time, the only supported devices are a bunch of 3Dconnexion Spacemice products
and a Gravis Tiltpad (which I don't seriously expect anyone to try and use with Cura). If you have some other device that you would like to use and have a description of the
HID messages it uses then it can be added.

On all systems you need to disable any existing driver so that RawMouse can gain access to the device. On Windows, there is a *Stop 3DxWare* command that disables the
normal Spacemouse driver.

Other programs cannot access the device while Cura is running.

Currently, RawMouse simply translates the device's output into the equivalent input from the standard mouse. It does not provide any new modes of operation.

---

### Installation

Download or clone this repository into [Cura configuration folder]/plugins/RawMouse
The configuration folder can be found via Help -> Show Configuration Folder inside Cura.

Alternatively, download and unzip a release.

Cloning this repository is best because then you can make local edits to the configuration file and even RawMouse.py and still be able to merge future updates without losing any changes you have made.

---

### Linux Permissions

On Linux, you need to allow non-root users access to the hidraw devices. That can be done by creating a file **/etc/udev/rules.d/10-hidraw.rules** that contains the following:

`KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0666"`

If you don't like giving everyone r/w access to all HID devices you can use a different rule to allow only users of a particular group to have access, e.g:

`KERNEL=="hidraw*", SUBSYSTEM=="hidraw", MODE="0660" GROUP="plugdev"`

---

### Configuration

RawMouse is configured using a JSON file that is located in the RawMouse plugin directory.

The configuration file can be edited when Cura is already running and reloaded using the Extensions -> RawMouse -> Restart menu item.

JSON syntax is quite unforgiving so it's easy to make an invalid file by missing (or adding) a comma, bracket, etc. If there are any problems with the configuration,
it will be reported in the cura.log file.

The configuration file elements are:

**maxhz** max number of screen updates per second

**devices** is an array of device definitions, one for each supported device. Each definition specifies the vendor and product USB ids for the device,
the name of the device profile to use and a description.

**profiles** is dictionary of profile definitions. Each profile definition defines the axes and buttons the profile knows about.

**axes** is an array of axis definitions, one for each of the device's axes. Each definition specifies the *offset*, *scale*, *threshold* and *target* values for the axis.

**offset** is added to the axis value to remove any bias the device may have.

**scale** scales the axis value. If you want to make the axis faster, increase the value. To slow the axis down, reduce the value. To flip the direction of movement,
change the sign.

**threshold** is the minimum value an axis must have before it has any effect.

**target** is the name of the function that will be invoked when the axis value is greater than the threshold.
 Current target names are: "movx", "movy", "rotx", "roty" and "zoom".

**buttons** is a dictionary of button definitions. The element keys are strings that match the button state and the value is a dictionary that specifies *value* and and *target* for the button.
When a button is activated, the specified target function is passed the value.

The Spacemice are configured so that button 1 resets the view (useful when you get lost in space) and button 2 toggles between the Cura prepare and preview screens which
is useful if the preview screen is slow to render as you can quickly switch to the prepare screen, manoeuvre, and then switch back to the preview screen.

---

### Warranty & License

RawMouse is supplied with no warranty.

RawMouse is released under the terms of the [AGPLv3](LICENSE) or higher.

---

### Kudos

RawMouse uses [cython-hidapi](https://github.com/trezor/cython-hidapi) to access the HID devices.

---

### Known Issues

* On Windows, the Wireless Spacemouse that comes with a universal receiver silently fails. The same Spacemouse will work if connected by USB cable.

* Complex models can be slow to move when the preview screen is active but there's not much that can be done about that here.

* The rotation motion is rather weird because it's still using the original 2D mouse code. More work is needed here.

---

### History

1.0.0 - initial release.

---
