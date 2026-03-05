# 1nf1ltrat0r-S3r13s

<b>My personal, public repo where I will be posting write-ups on various devices that I infiltrate on various networks, all in an effort to improve on hacking as a whole. This content is for educational consumption and does not in any way encourage hacking devices you do not own without explicit permission.<b>

---

## 📂 Devices

### Smart WiFi Webcam
* **Category:** IoT / Surveillance

### Android TV
* **Category:** Entertainment / Media



## 1. Smart WiFi Webcam Pentesting
<p>The first victim on my pentesting series...quite easy, so let's get into it.</p>
<p>In this section, we will be exploiting the Real Time Streaming Protocol (RTSP) that is mainly used to control servers and/or devices that stream media content over the internet.</p> 
<p>The RTSP protocol primarily uses port 554. So whenever you see IP cameras, webcams or media streaming devices, this service should be the first to pop up in mind.</p>


## Discovery
<p>The first step will be discovery, and since I have quite a number of personal devices on my network that I do not need to expose, we will solely focus on the webcam.</p> 
<p>We will use nmap for network discovery.</p>

![Network Discovery](https://i.ibb.co/5gkFrgmN/network-nmap.png)

<p>Boom. We found it. Next we will narrow down to the specific host and enumerate it further. I searched through nmap scripts to see if there are any rtsp-related scripts. Fortunately, i found two. We will use the first one.</p>
<b>Here I landed on a goldmine of results, so it is split across a number of screenshots.</b>

![Webcam_nmap1](https://i.ibb.co/BHzXsfhS/webcam-nmap1.png)
![Webcam_nmap2](https://i.ibb.co/Kc5P6H2c/webcam-nmap2.png)
![Webcam_nmap3](https://i.ibb.co/6RFZf7vj/webcam-nmap3.png)
![Webcam_nmap4](https://i.ibb.co/ksH1mhng/webcam-nmap4.png)

## Exploitation

### Vulnerability Analysis
The initial Nmap scan revealed that the RTSP service on the target device is misconfigured. Every request sent to the service returned a **200 OK** response. This indicates a **complete lack of authentication**; a secured device would have responded with a **401 Unauthorized** to prompt for credentials before granting access to the media stream.



### Path Discovery and Selection
To avoid manually testing the 100+ discovered paths, I prioritized the most likely candidates based on common industry standards:

* **ONVIF Standard:** `rtsp://192.168.0.134/onvif-media/media.amp`
* **Hikvision Style:** `rtsp://192.168.0.134/Streaming/Channels/101`
* **Dahua Style:** `rtsp://192.168.0.134/cam/realmonitor?channel=1&subtype=0`
* **Axis Style:** `rtsp://192.168.0.134/axis-media/media.amp`

### Proof of Concept (PoC)
I used **ffplay**, a command-line media player included in the FFmpeg suite on Kali Linux, to test the streams directly from the terminal. 

Out of curiosity, I also tested the common industry paths discovered on the nmap rstp-url-brute scan using the ffplay tool and fortunately, **I caught streams using all paths**. 

**Execution Command:**
```bash
ffplay rtsp://192.168.0.134/onvif-media/media.amp
```
![Onvif_terminal](https://i.ibb.co/YTLWpvms/onvif-terminal.png)
![Onvif_stream](https://i.ibb.co/rR6trjpc/onvif.png)

```bash
rtsp://192.168.0.134/Streaming/Channels/101
```
![Hikvision_terminal](https://i.ibb.co/5xfRF68W/hikvision-standard-terminal.png)
![Hikvision_stream](https://i.ibb.co/XZ65jJFr/hikvision-standard.png)

```bash
rtsp://192.168.0.134/cam/realmonitor?channel=1&subtype=0
```
![Dahua_terminal](https://i.ibb.co/1tNGZWnV/dahua-standard-terminal.png)
![Dahua_stream](https://i.ibb.co/SXcjVZr9/dahua-standard-stream.png)

```bash
rtsp://192.168.0.134/axis-media/media.amp
```
![Axis_terminal](https://i.ibb.co/qMwp1YDp/axis-terminal.png)
![Axis_stream](https://i.ibb.co/DHRySjYc/axis-stream.png)

