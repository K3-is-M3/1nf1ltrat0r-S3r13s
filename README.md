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
