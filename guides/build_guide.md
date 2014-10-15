# FreeFalcon Build Guide

Follow this guide to get FreeFalcon building on your computer. I'd read the
entire guide before you actually do anything so that you know what you're
getting into.

This guide was developed with Windows 8.1. Different Windows versions may
vary. Linux users, we're coming!

It's smart to take a full system image backup before you get going, as an
installation may go haywire and you don't want registry settings interfering
should you need to bail and start over.

## Dependencies

Please download and install all of these components. Installation order is
important.

1. FreeFalcon 6 Installer (parts
   [1](http://www.mediafire.com/download/fht1chqt4is56y3/FF6Installer.part1.rar)
   [2](http://www.mediafire.com/download/weu8ioyh2dk47wq/FF6Installer.part2.rar)
   [3](http://www.mediafire.com/download/x84imkuaxpkaa1b/FF6Installer.part3.rar)
   [4](http://www.mediafire.com/download/s1o3k3dnayeisha/FF6Installer.part4.rar)
   [5](http://www.mediafire.com/download/v243nbhhksynfiz/FF6Installer.part5.rar))
2. [DirectX SDK 8.1](http://www.darwinbots.com/numsgil/dx81sdk_full.exe)
3. [Microsoft Visual Studio 2010 Professional 30 day trial](http://download.microsoft.com/download/D/B/C/DBC11267-9597-46FF-8377-E194A73970D6/vs_proweb.exe)
4. [Windows SDK 7.1](http://www.microsoft.com/en-us/download/confirmation.aspx?id=8279)
5. [Microsoft Visual Studio 2010 Service Pack 1](http://www.microsoft.com/en-us/download/confirmation.aspx?id=23691)
6. [Microsoft Visual C++ 2010 Service Pack 1 Compiler Update for the Windows SDK 7.1](http://www.microsoft.com/en-us/download/confirmation.aspx?id=4422)
7. [Microsoft Visual Studio 2013 Professional 90 day trial](http://go.microsoft.com/?linkid=9832345&clcid=0x409)
8. [WiX Toolset](https://wix.codeplex.com/releases/view/115492)

## Installation Notes

When installing dependencies, keep the following in mind. Installing everything
to their default locations is recommended to guarantee that everything goes
smoothly.

### FreeFalcon 6

 * Install to `C:\FreeFalcon6`. This should not be necessary, but it is; fixing
   this is a priority for the next release.
 * All 4 checkboxes on the installer should be selected.
 * Download the display.dsp and Viper.pop files [here](http://ge.tt/2oS3Hgz1?c)
   and place them in `C:\FreeFalcon6\config` after installation.

### Visual Studio 2010

 * Only the Visual C++ tools are necessary for FreeFalcon.
 * Be sure to install the required SDKs and their updates before Windows Update
   has a chance to do anything.

### Windows SDK 7.1

The following components are unnecessary for FreeFalcon and may be unchecked if
you choose:

 * Samples
 * .NET Development group
 * Microsoft Help System
 * Application Verifier
 * Windows Performance Toolkit

### Visual Studio 2013

 * Only install Microsoft Foundation Classes for C++
 * Windows Phone 8.1 emulators will automatically begin to install; you may
   cancel this if you wish, you don't need it.
 * Don't launch, just exit the installer.

## Downloading the Code

FreeFalcon uses Git for version control and is hosted on GitHub. If you aren't
familiar with this tool, [start here](http://git-scm.com/book/en/Getting-Started).

Our standards for using Git are outlined [here](/management/GitWorkflow.md). If
you choose to use a Git GUI, make sure it complies with the rules, e.g. [GitHub
for Windows](https://windows.github.com/).

## Configuration

1. To setup the DirectX SDK, go to `Control Panel > System > Advanced system
   settings > Advanced > Environment Variables` and create a new variable named
   `DX81_SDK` containing your installation path with the succeeding backslash
   (default `C:\DXSDK\`).
2. Go to `Control Panel > Windows Update` and install all new updates,
   restarting if necessary. Make sure that it doesn't find any more updates.
3. Run Microsoft Visual Studio 2013.
4. Click `Tools > Import and Export Settings`.
  1. Click Import selected environment settings.
  2. Click Next.
  3. Click No, just import new settings, overwriting my current settings.
  4. Click Next.
  5. Click Browse.
  6. Download and select [this file](http://ge.tt/4bGuYq02/v/0?c).
  7. Click Open.
  8. Click Next.
  9. Click Finish.
  10. Click Close.
5. Click `File > Open > Project/Solution`.
  1. Find and open `freefalcon-central/src/FreeFalcon.sln`.
  2. Right click the `FFViper` project in the Solution Explorer and select Set
     as StartUp Project.
6. Click `View > Property Manager`. If you don’t find it, try `View > Other
   Windows > Property Manager`.
7. Expand `FFViper > Debug | Win32` in the Property Manager.
8. Right click `Microsoft.Cpp.Win32.user` and select Properties.
  1. Navigate to `Common Properties > VC++ Directories` in the Properties
     window.
  2. Change the following lines:

     | Field               | Value                             |
     | ------------------- | --------------------------------- |
     | Include Directories | $(IncludePath);$(DX81_SDK)Include |
     | Library Directories | $(LibraryPath);$(DX81_SDK)Lib     |

9. Now select both `Debug | Win32` and `Release | Win32`, right click, and
   click Properties.
  1. Navigate to `Common Properties > Debugging` in the Properties window.
  2. Change the Working Directory to `C:\FreeFalcon6`.
10. You may optionally set the debug target to run in windowed mode by going to
    `Common Properties > Debugging` for the `Debug | Win32` target and add the
    `-window` flag.
11. Click `File > Save All`.

## Building

The code from the `develop` branch should compile without issue. For feature
branches, keep up with the discussion in that branch's GitHub issue.

Any compilation problems on non-feature branches should be immediately brought
up as a GitHub issue.

If you get the message "Requested unavailable resolution 1024x768x16", running
Win8.1's compatibility troubleshooter (in the right-click menu) on
`freefalcon-central/build/x86/debug_win32/ffviper/FFViper.exe` has been tested
to work.
