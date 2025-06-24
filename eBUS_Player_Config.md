# eBUS Player Recommended settings

The settings in this document have been successfully used to communicate with and capture images from a Xineos-1511 using the PT1000-LV and the adapter PCB. Your configuration may differ depending on your desired setup, but these settings work well as a baseline for capturing still images. Note that any settings not specified can be left in their default states.

You may need to set the "Visibility" dropdown in eBus Player to "Expert" or "Guru" to see the necessary settings in the three menus.

## In the Device Control menu:

- DeviceControl -> 
  - DeviceScanType: Areascan
- ImageFormatControl -> 
  - SensorDigitizationTaps: One
  - Width: 1488
  - Height: 1148
  - PixelFormat: Mono14
  - TestImageSelector: Off
- AcquisitionControl ->
  - AcquisitionMode: Continuous
  - TriggerMode: Off
- TransportLayerControl ->
  - *These settings will vary depending how you want the PT1000 to connect to your network. However, I use the following:*
  - GevCurrentIPConfigurationLLA: True
  - GevCurrentIPConfigurationDHCP: False
  - GevCurrentIPConfigurationPersistentIP: True
  - GevPersistentIPAddress: *(A valid IP on your subnet, e.g. 192.168.1.50)*
  - GevPersistentSubnetMask: *(The subnet mask for the subnet the devices is on, e.g. 255.255.255.0)*
  - GevPersistentDefaultGateway: *(The next hop for communicating with devices on a different subnet. Not needed if the PC running eBUS Player is on the same subnet and no devices on other subnets need to communicate with the PT1000. e.g. 192.168.1.1)*
- IPEngine -> 
  - PortCommunication ->
    - Uart0 ->
      - Uart0BaudRate: Baud115200
      - Uart0BaudRateFactor: 1
      - Uart0NumOfStopBits: One
      - Uart0Parity: None
      - Uart0ReadyToReceive: True
      - Uart0BreakDetection: False
  - Grabber ->
    - Channel0 ->
      - AcquisitionConfiguration ->
        - GrbCh0AcqCfgPixelBusDataPortMapping: CBA
        - GrbCh0AcqCfgInvertPixelData: True *(Personal Preference- depends how you plan to process captures)*
  - PixelBusInterface ->
    - PixelBusSerialSelect: UART0 *(set to Unused if you want to use the PLC connector on the PT1000 to do serial communication- it can only communicate via one of these interfaces at a time)*
    - PixelBusDataValidEnabled: True
    - PixelBusDataValidPolarity: High
    - PixelBusLineValidPolarity: High
    - PixelBusLineValidEdgeSensitivity: Level
    - PixelBusFrameValidEdgeSensitivity: Level
    - PixelBusClockPresent: *(This is a read-only field, but it will show True if the PT1000 can detect the pixel clock signal coming from the Xineos-1511. Useful for debugging)*

Optionally, once you have all the settings set to a working configuration, you can use the following menu items to save the current configuration to the PT1000's startup configuration:

- UserSetControl ->
  - UserSetSelector: UserSet1
  - UserSetDefaultSelector: UserSet1
  - UserSetSave: *(Press the UserSetSave button)*

As of publishing the initial version of this document, no testing has been done with the PLC control options, but they can be used to configure image capture triggering, various timing parameters, and other logic inputs and outputs. For example, PT1000 and Xineos-1511 can be synchronized with a pulsed X-ray tube, in order to only capture images during an exposure. The PLC options could also be used to synchronize captures with a linear motion mechanism if the Xineos-1511 is being used as a line scan detector or in conjunction with a rotating stage for CT applications.

## Communication Control

During testing, nothing needed to be changed from the defaults in the Communication Control menu. However, it was discovered that framerate can be increased slightly in free-running/continuous mode by settings StreamingPacketSize -> AutoNegotiation to False and DefaultPacketSize to 9000. This sets TransportLayerControl -> GevSCPSPacketSize in the Device Control menu to 8976, which seems to be the maximum packet size that the PT1000-LV supports. Note that this will only work if the network interface of the PC you are running eBUS Player on supports jumbo packets and is configured to use them. The same goes for any switches or routers between the PT1000 and the PC. This is not an optimal configuration in my opinion, but it is possible if you really need the framerate and performance.

## Image Stream Control

During testing, nothing needed to be changed from the defaults in the Image Stream Control menu, but it does contain useful pixel, frame, and block statistics for troubleshooting, as well as the PT1000s current IP configuration.
