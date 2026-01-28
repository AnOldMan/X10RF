

## Bond Bridge X10 RF

Collection of files used to update the capture/scan of X10 RF commands on the Bond Bridge

1. Adam Davis - CM17A Protocol.txt
> Description/decoding of the transmitted protocol

2. Bond.Local.API.postman_collection.json
> Postman collection for communicating with the Bond Local API.
>
> The full documentation can be found here: [https://docs-local.appbond.com/](https://docs-local.appbond.com/)
>
> Postman can be found here: [https://www.postman.com/](https://www.postman.com/)

3. X-10 RF Protocol.txt
> Description/decoding of the rf protocol

4. X10 RF parser and creator for Bond Bridge.html
> Simple web page to decode (from scans), or encode, X10 RF commands for the Bond Bridge


### Bond Local API
- Download and install Postman
- Import the collection
- Set your {{localhost}} environment variable (the local address of your Bond Bridge).
> You can do this from any method by hovering over the {{localhost}} and pasting it in.  
> You can find your Bond's IP address in the Bond App under **Bond Device -> Advanced Settings -> Network Info**
- Request a token **Getting Started -> Get Bond Token**

> If you do not get a token back, cycle power of your Bridge (this is a simple security measure) and request again within 10 minutes.

#### Get your current scans
- **Getting Started -> Get Device Information**
> This will return a list of your programmed devices as their device ids.
> 
> Note that there is nothing to tell you what each device is.  That's the next command.
>
> At this point, if you have a lot of devices, I recommend a spreadsheet.

- **Devices -> Get Device**
> Hover over the {{device_id}} and paste in one of your device ids.  This will return the device name and commands.

If this is one of the devices you wish to check/update:

- **Devices -> Device Commands**
> This will return the command ids for the device

- **Devices -> Device Command Signal**
> Hover over {{command_id}} and paste in one of the command ids.

The returned "data" field will contain your scan result.

#### Decode your current scan
- Simply open the **X10 RF parser and creator for Bond Bridge.html** in any browser, paste in your scan, and click **Parse**
- If the parsing is successful (the scan has enough valid data), the page will:
1. automatically display the House / Unit code, and On or Off
2. automatically generate a new *clean* RF signal for Bond

I recommend attempting to decode all scans, since the Bond API doesn't seem to tell you which command is ON or OFF.  
(I've found they're returned in On then Off order, but your results may vary)

#### Update your current scan 
- **Devices -> Device Signal Modify**

Simply paste your generated signal into the "data" field and send.
> the other fields should work fine as is

You will see a significant payload reduction for the "data" field compared to scans.  This is because
- the scan is just *guessing* when to start/stop
- repeat[s] are part of the scan

Generated commands are "clean"
- start immediately
- no "noise" : all chars should be F or 0 for this protocol, unless a rising/trailing group requires truncation
- end properly : the last rising signal is required to be followed by a low for a precise interval
- repeatable : scans contain repeats, generated signals are repeatable (timing, see "end properly")

