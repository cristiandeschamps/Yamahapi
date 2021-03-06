# Yamaha AV Receiver .NET Core WebAPI

## Why?
I have a Yamaha RX-V775 AV receiver. It's a couple of years old now so isn't likely to see much integration with any home automation systems. Since "smart-home" use is never likely to come to the likes of an old Yamaha receiver, I wanted to see if I could make it smart using code and smarts. The intention is to use Google Home with IFTTT (If This Then That) and its Google Assistant and Maker Webhooks to allow me to give instructions to have it turn on and off, up and down, using only my voice.

## How?
The receiver offers a web-based interface which is presented as a typical GUI with buttons and what not to perform actions that are reflected on the receiver. Pressing a button sends a string of XML data in the body using an HTTP POST to the receiver's web interface. It isn't password protected in any way so opening it up to the Internet is a Bad Idea™. Since I've done the leg work with PowerShell already (I have another project that uses PowerShell to control the receiver), it's a simple (ha!) matter of working out how to use .NET Core WebAPI to perform the same action when it receives an HTTP call.

This (clicking the button):

![Yamaha Web UI](http://www.lewisroberts.com/wp-content/uploads/2017/07/YamahaWebGUI-e1499356368140.png)

Becomes: (a bit pointless, but you get my meaning...)
```bash
curl http://server.com/api/power -X PUT
```
or (after IFTTT/Maker Webhooks integration)
```bash
"OK Google, turn on the receiver."
```

This WebAPI is my first actual use of any "respected" programming language so it is likely to be very rough and very verbose. I stumbled on to .NET Core by accident - the fact that it is technically cross-platform is good news and the capability to implement an "API" is pretty simple - something I did need truth be told.

## Warning
There is no authentication/authorisation performed when accessing the API.

There are 4 controllers.

## Power
``` 
PUT http://server.com/api/power - turns the receiver on.
DELETE http://server.com/api/power - turns the receiver off.
```

## Mute
```html
PUT http://server.com/api/mute - mutes the receiver.
DELETE http://server.com/api/mute - unmutes the receiver.
```

## Volume
This assumes that the receiver is configured to use the dB (decibel) setting for reporting volume level.
```html
PUT http://server.com/api/volume/40 - sets the volume of the receiver to -40.0dB
GET http://server.com/api/volume - gets the current volume setting of the receiver.
```

## Input/Inputs
```html
GET http://server.com/api/inputs - gets a list of all inputs available on the receiver.
GET http://server.com/api/input - gets the current input setting on the receiver.
PUT http://server.com/api/input/hdmi1 - sets the input of the receiver to HDMI1
```

I do hope to add further controllers as my knowledge improves but again, this is very much a personal project. The intention is for me to integrate with Google Home (via IFTTT using Maker Webhooks).

To make it a little easier for anyone (brave enough) to use this, I've added a configuration item for the Receiver IP address to make it simple for someone to change if they're using a "built" version. See the `appsettings.json` file.

# How do I use it?
Good question! I'll answer this because it makes me commit some of the information I've absorbed over the last couple of days to memory.

  1. Install Visual Studio Code - this is a lightweight code editor - not the full Visual Studio!
  2. Install the .NET Core SDK latest version (as I write this, it's 1.1.2)
  3. Install the C# extension in to Visual Studio Code.
  4. Clone this repo to a folder on your computer.
  5. In Visual Studio Code, open the Folder.
  6. Open Program.cs.
  7. Press F5 to start debugging and test.
  8. When you want to build a version for yourself, at the command prompt inside your folder, use the following command:
    
`dotnet publish -o "D:\Yamahapi" -c Release`

  9. Use the release build in the folder to deploy to your chosen platform. I use IIS and [used this article to fill in my knowledge gaps - which are significant!](https://weblog.west-wind.com/posts/2016/Jun/06/Publishing-and-Running-ASPNET-Core-Applications-with-IIS)