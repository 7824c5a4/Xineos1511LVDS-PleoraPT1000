# Xineos1511LVDS-PleoraPT1000

This circuit maps the IO (LVDS and UART) of the Teledyne Dalsa Xineos-1511 to the Pleora PT1000-LV frame grabber, allowing the LVDS detector with an antiquated 68 pin VHDCI connector to be used as a more accessible, GigE sensor without losing functionality. 

The files in this repository include a KiCAD project with a schematic, a PCB layout, custom footprints and 3D models of the components that match the pinouts of the devices being adapted, as well as a bill of materials. The circuit features a UART to RS-232 level translator based on the MAX3221, as the Xineos uses TTL-level serial and the PT1000 employs RS-232.

The PT1000 can be interfaced with via Pleora's eBUS Player application. eBUS Player can be used to configure the PT1000 and to capture images, but more granular or programatic control of image capture may require Pleora's SDK, which is outside the scope of this project.

The files in this repository contain no datasheets or proprietary content from Teledyne Dalsa or Pleora Technologies, and thus they will need to be obtained via licensed channels by anyone wanting to further develop this project. If you have specific questions about the design, feel free to contact me via issues on this repository, or via email at 7824c5a4(at)gmail.com.
