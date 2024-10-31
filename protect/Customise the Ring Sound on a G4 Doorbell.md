## Customise the Ring Sound on a G4 Doorbell


Ubiquiti don’t offer the option to customise the ring sound that a visitor hears when they push the button on the G4 doorbell. 
Other competitor video doorbells available on the market allow you to change the sound for seasonal holidays or events. Needless to say, that with a little bit of poking around in the underlying *nix based operating system, 
someone determined enough is able to implement this themselves!

And if you happen to come across this Ubiquiti… perhaps it may be a feature you consider including, given that your products are favourable among so-called prosumers, so often find themselves in residential installations.

## Prerequisites
You’ll need an SSH and a SCP client. I made use of PuTTY and WinSCP as my tools of choice as this was done on a Windows PC. 
Other operating systems may already have the necessary tools built in so adjust the instructions to your particular setup.

You’ll also need your custom audio file; based on the other sound files available on the doorbell, it must meet the following specification:

  - wav format
  - 16-bit
  - Mono
  - 44100 or 48000Hz

If you’re stuck for inspiration, there are plenty of free royalty free sound-effect websites that allow you to download sound effects as mp3 files, including seasonal doorbells for free. 
You can use audio editing software such as Audacity to convert them to the required wav file.  (https://orangefreesounds.com)

## Step 1 – Enable SSH on the Console
Open up a web browser and navigate to your UniFi console, either using its local IP address or through unifi.com.
By default, SSH is not enabled on a UniFi console. If you haven’t previously enabled it, turn it on and set a password.

## Step 2 – UniFi Protect Login Details
UniFi protect devices have a separate password that you will need when trying to log into them outside of the UniFi Protect application. 
Follow these instructions to retrieve or set the password, referred to in UniFi Protect as the Recovery Code.

Open the UniFi Protect application on your console, Click the Cog icon on the left hand side and make sure you are on the General tab.
Under the Other Configurations header at the bottom of the page you will see the Recovery Code. Click the Reveal button to show the current recovery code 
(i.e. the password that you will need to log into the UniFi web interface on the protect camera). 
Make a note of the existing code or set it to something that you are able to remember.

Whilst you are in the Protect application, make a note of your Doorbell’s IP address by clicking on the UniFi Devices menu option and clicking on your doorbell in the list of cameras. 
The current IP will is listed under Host IP Address.

## Step 3 – Enabling SSH on UniFi Protect Devices
There is no option within the GUI to enable SSH on UniFi Protect devices. It is therefore necessary to modify the UniFi Protect configuration file that resides on the console. This in turn enables SSH on the connected cameras.
Open your preferred SSH client, PuTTY is a common choice for many people.

Connect to your UniFi console using its IP address and make sure you’re connecting via SSH on port 22. When prompted to login, enter root as the username, and the password you set in Step 1.

Type `unifi-os shell` and hit enter
To install the nano text editor to allow the config file to be edited type in `apt-get install nano` and hit enter. Confirm that you wish to install it by typing y and then enter.
To edit the configuration file, type `nano /srv/unifi-protect/config.json` and press enter

t is quite possible that the file is currently empty, if it is add the following:
```
{
    "enableSsh": true
}
```
If the file already contains settings, just add the line ` "enableSsh": true `towards the bottom of the file before the closing bracket, remembering to add a comma at the end of the previous line.

To save the changes, press Ctrl + x. You will be asked if you want to “Save modified buffer”, press y and press enter to confirm the file name.
A restart of the UniFi Protect service is required for the cameras to now pick up this new setting.

To do this, enter `service unifi-protect restart`

The service may take a minute or two to restart.

## Step 4 – Transferring the New Sound File to the Doorbell


Using your SCP program of choice (such as WinSCP), open a new connection to the Doorbell.

In WinSCP, make sure you’ve selected SCP as the File Protocol, and enter the IP address of the Doorbell in the Host name field. If you’ve selected the correct protocol above, Port number will automatically be set to 22.
Once connected to the doorbell, navigate to `/var/etc/sounds` and upload your custom doorbell wav file. Keep the name something simple.

## Step 5 – Editing the Doorbell Configuration File

Using the WinSCP GUI, navigate to /var/etc/persistent and open the file ubnt_sounds_leds.conf

Find the part of the file with the customSounds entry:








