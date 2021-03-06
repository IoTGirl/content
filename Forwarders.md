---
layout: code
title: Forwarders and Stubs
permalink: /Forwarders.htm
---

#Win32 API
The version of Windows for the Intel Galileo is based on a mobile version of Windows. It is designed to run in a smaller memory and storage footprint. Because of this, much of the Win32 API is not available for applications which target the Desktop version of Windows.

Win32 is a vast API, consisting of hundreds of thousands of application programming interfaces, spanning many technology stacks and language bindings. As the API set grew in size over the generations, it also grew in complexity and interconnectedness.

Over the last few releases of Windows, the Win32 API has been refactored to enable smaller versions of Windows to run on new form factors. As the API set was refactored, APIs critical across the board were included in the smaller versions of Windows, while more and more APIs added to the larger and larger versions of Windows - culminating in the desktop version which contains the whole API set.

If you want to run an application targeted for Desktop on a smaller device - like the Galileo, the APIs it needs from the operating system have to be made available - or the app won't run.

To make them available, there are a few options:

1. If the API is in Windows, but has moved, implement a Forwarder from the traditional API exposure to the new one.
1. If the API is missing, implement a sub which emulates the API as best as possible.
1. If you have access to the sources for the app you'd like to run, Remove the offending API and rebuild for the target version of Windows.

## API Sets
Windows on Galileo is derived from Windows Phone 8.1. This mobile edition of Windows uses the [Windows 8.1 API Set](http://msdn.microsoft.com/en-us/library/windows/desktop/hh802935(v=vs.85).aspx){:target="_blank"}.

##Diagnosing a failing application
When an application fails to load due to a missing API, you can diagnose it by turning on Windows Loader Snaps - a loader tool which shows which APIs are missing. 

To do this: 

1. Telnet to your board
1. Determine the executable name for the binary you'd like to diagnose. 
1. Enter the following command, replacing the executable name:

{% highlight bash %}
reg add "HKLM\software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\node.exe" /v GlobalFlag /t REG_DWORD /d 0x2
{% endhighlight %}

In this case, we've enabled loader snaps for node.exe. Running this under visual studio, you will be told which APIs fail.

{% highlight bash %}
0744:00c8 @ 14911546 - LdrpNameToOrdinal - WARNING: Procedure "GetProcessWindowStation" could not be located in DLL at base 0x75A20000.

0744:00c8 @ 14911562 - LdrpReportError - ERROR: Locating export "GetProcessWindowStation" for DLL "c:\node\node.exe" failed with status: 0xc0000139.

First-chance exception at 0x7758342A (ntdll.dll) in node.exe: 0xC0000139: Entry Point Not Found.
{% endhighlight %}

Once you are done diagnosing a loader error, you can remove the loader snaps logging:

{% highlight bash %}
reg delete "HKLM\software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\node.exe"
{% endhighlight %}

##Forwarders
When building a DLL, you can specify that an export API exists in another DLL, but modifying it's def file. Referring to the [Windows 8.1 API Set](http://msdn.microsoft.com/en-us/library/windows/desktop/hh802935(v=vs.85).aspx){:target="_blank"} 

{% highlight bash %}
LIBRARY User32
EXPORTS
   ...
   CharLowerA = ms-win-downlevel-user32-l1-1-0.CharLowerA
   ...
{% endhighlight %}

## Stubs
Some APIs are missing from the Windows image as they make no sense in that context. In this case, you would add a stub which allows linkage, but does something appropriate for the API:

{% highlight c++ %}
HWINSTA
WINAPI
GetProcessWindowStation(
VOID)
{
    return NULL;

}
{% endhighlight %}

## Forwarder and Stub Repository
To help with porting, a [Forwarder and Stub repository](http://github.com/ms-iot/forwarders) has been created. Please feel commit only those APIs needed to run specific tasks. 

## Building and Deploying the Forwarder and Stubs
Do use the forwarder and stubs:

1. [download the forwarders project zip file](https://github.com/ms-iot/forwarders/archive/master.zip). 
1. Extract all of the files from downloaded zip.
1. Open the solution for the forwarder you require.
1. Build the solution
1. Right click on the project, select 'Open in File Explorer'
1. Navigate up to the Release directory
1. Copy the binary - such as `User32.dll` to `\\mygalileo\c$\test'






---
[&laquo; Return to Samples](SampleApps.htm){: .btn .btn-default} 
