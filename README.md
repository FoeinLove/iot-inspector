# IoT-Inspector
This software was developed to collect WiFi network traffic for research purposes. If you publish any results with data collected using this software, please cite the following paper:

**Keeping the Smart Home Private with Smart(er) IoT Traffic Shaping.** Noah Apthorpe, Danny Yuxing Huang, Dillon Reisman, Arvind Narayanan, Nick Feamster. 2018. [arXiv Preprint.](https://arxiv.org/abs/1812.00955)

## System requirements
This software was developed and tested for use on a Raspberry Pi v3.  It should also work on any Linux machine that has both a WiFi interface that can be used as an access point and a wired connection to the internet.

## Install instructions

#### Connect to network
Connect the Raspberry Pi to the Internet using a wired Ethernet connection.  You can then access the Raspberry Pi from your computer using ssh.

#### Change user password
The default password for the "pi" user (with sudo privileges) is "raspberry". You should change this using the `passwd` command if the pi has a public IP address or is on a LAN open to other users. 

#### Download code
Execute the following commands from the terminal of the Raspberry Pi:

```
$ cd /home/pi
$ git clone https://github.com/NoahApthorpe/iot-inspector.git
$ cd iot-inspector
```

#### Change WiFI SSID and Password
The defualt SSID of the WiFi network created by this software is "Pi3-AP". You may wish to change this, especially if you have multiple Raspberry Pi's running this software in proximity.  

The default password for the WiFi network created by this software is "raspberry".  You should change this password to prevent others from using the WiFi network. 

The SSID and password are set in the file `iot-inspector/config/hostapd.conf`. Open this file in a text editor and change the values of `ssid` and `wpa_passphrase` to new a SSID and password, respectively.  

#### Run install script

From the `iot-inspector` directory, run 
```
$ sudo ./install.sh
```
This will prepare the configure and start the WiFi network and download the required packages for packet capture and analysis.

## Usage Instructions

Execute the following commands from the `iot-inspector` directory to start capturing packets and uploading the resultant pcap files to a remote directory. The `tmux` command opens a new tmux window and allows the packet capture to continue running after you close the ssh session.

```
$ tmux
$ sudo ./start.sh [local_directory] [pcap_filename] [ssh_username] [ssh_server] [remote_directory] [ssh_password]
```

This will start capturing packets on the wireless interface of the Raspberry Pi and saving pcap files to `output_directory/pcap_filename.pcap`. Standard input and standard error will be redirected to `nohup.out`.

Use the command `[ctrl-b] d` to exit the tmux window.

To reattach to an existing session, run `tmux attach`.

To stop data collection, run `ps` to find the PIDs of the `dumpcap` and `python` processes. Then run the following commands to send SIGINT to both

```
$ kill -2 [dumpcap_pid]
$ kill -2 [python_pid]
```
