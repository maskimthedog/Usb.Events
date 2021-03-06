# Usb.Events
Subscribe to the Inserted and Removed events to be notified when a USB drive is plugged in or unplugged, or when a USB device is connected or disconnected. Usb.Events is a .NET Standard 2.0 library and uses WMI on Windows, libudev on Linux and IOKit on macOS.

How to use:

1. Include NuGet package from https://www.nuget.org/packages/Usb.Events

        <PackageReference Include="Usb.Events" Version="1.1.0.1" />
        
2. Subscribe to events:

        using Usb.Events;

        class Program
        {
            static readonly IUsbEventWatcher usbEventWatcher = new UsbEventWatcher();

            static void Main(string[] _)
            {
                usbEventWatcher.UsbDeviceRemoved += (_, device) => Console.WriteLine("Removed:" + Environment.NewLine + device + Environment.NewLine);

                usbEventWatcher.UsbDeviceAdded += (_, device) => Console.WriteLine("Added:" + Environment.NewLine + device + Environment.NewLine);

                usbEventWatcher.UsbDriveEjected += (_, path) => Console.WriteLine("Ejected:" + Environment.NewLine + path + Environment.NewLine);

                usbEventWatcher.UsbDriveMounted += (_, path) =>
                {
                    Console.WriteLine("Mounted:" + Environment.NewLine + path + Environment.NewLine);

                    foreach (string entry in Directory.GetFileSystemEntries(path))
                        Console.WriteLine(entry);

                    Console.WriteLine();
                };

                Console.ReadLine();
            }
        }

TO DO:

- [ ] Automatically mount USB drive on `UsbDeviceAdded` event in Linux
- [ ] Automatically mount USB drive on `UsbDeviceAdded` event in macOS

Version history:

- 1.1.0.1:
    - Bug fix
- 1.1.0.0:
    - Added:
        - `MountedDirectoryPath`
        - `IsMounted`
        - `IsEjected`
    - Breaking changes:
        - `DevicePath` renamed to `DeviceSystemPath`
        - `UsbDriveInserted` renamed to `UsbDriveMounted`
        - `UsbDriveRemoved` renamed to `UsbDriveEjected`
        - `UsbDeviceInserted` renamed to `UsbDeviceAdded`
- 1.0.1.1:
    - Bug fix
- 1.0.1.0:
    - Events for all USB devices
- 1.0.0.1:
    - Bug fix
- 1.0.0.0:
    - Events for USB drives and USB storage devices
