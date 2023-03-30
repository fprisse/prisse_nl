---
layout: page
title: HDHomeRun HTTP Development
permalink: /hd-homerun/
date: 2014-04-07 00:00:01 +0100
---

# HDHomeRun HTTP Development Guide
HDHomeRun Software release 20140319 contains enhancements and improvements used in this guide.
The latest HDHomeRun drivers, code, and firmware can be found on the Silicondust website: http://www.silicondust.com/downloads

## I. Channel List
The list of available channels can be queried using the following URLs:http://<device ip>/lineup.jsonhttp://<device ip>/lineup.xmlThe following information is returned for each program:•“GuideNumber” (virtual channel number). For ATSC the number will be “n.n” format. For all other systems the number will be “n” format.•“GuideName” in UTF-8 format. Client must support UTF-8 character encoding.•“Tags” in comma-separated-value format. The following tags are supported:◦“favorite” indicates the user has marked this channel as a favorite channel.◦“drm” indicates this channel requires the use of end-to-end copy protection.
 - “URL” for sourcing the video for this channel.II.Streaming VideoThe HTTP request will result in the following sequence:
 - A tuner will be allocated for this HTTP operation.
 - The channel will be authorized and tuned.
 - The PID filter will be set automatically.
 - The video stream will be streamed in MPEG-TS format over the HTTP connection.

The stream will continue until the TCP connection is closed by the client or the specified duration is reached, at which point the tuner will become available for other client requests.

## II. Optional Parameters:
Optional parameters may be specified by adding ?<paramater>&<paramater>&<...> to the end of the URL.
 - duration=<n> sets a duration limit for the http transfer. Once <n> seconds has elasped the stream will be closed by the HDHomeRun.
 - transcode=<profile> enables transocding of the video/audio following the specified profile (PLUS models only)

### Transcode Profiles:
 - native: works too !
 - heavy: transcode to AVC with the same resolution, frame-rate, and interlacing as the original stream. For example 1080i60   AVC 1080i60, 720p60   AVC 720p60.→→
 - mobile: trancode to AVC progressive not exceeding 1280x720 30fps.
 - internet720: transcode to low bitrate AVC progressive not exceeding 1280x720 30fps.
 - internet480: transcode to low bitrate AVC progressive not exceeding 848x480 30fps for 16:9 content, not exceeding 640x480 30fps for 4:3 content.
 - internet360: transcode to low bitrate AVC progressive not exceeding 640x360 30fps for 16:9 content, not exceeding 480x360 30fps for 4:3 content.
 - internet240: transcode to low bitrate AVC progressive not exceeding 432x240 30fps for 16:9 content, not exceeding 320x240 30fps for 4:3 content.

### Examples:
 For the URL http://192.168.0.100:5004/auto/v5.1”http://192.168.0.100:5004/auto/v5.1?duration=120http://192.168.0.100:5004/auto/v5.1?transcode=mobilehttp://192.168.0.100:5004/auto/v5.1?transcode=mobile&duration=120Errors
 - If the virtual channel number is not known the tuner will return “404 Not Found”
 - If the request cannot be completed at this time (for example all tuners are in use) the tuner will return “503 Service Unavailable”.
 - If the program cannot be not found in the stream within 5 seconds or the program cannot be authorized within 5 seconds the tuner will return “503 Service Unavailable”.
 - If the program requires content-protection not requested by the client the tuner will return “503 Service Unavailable” after 5 seconds. This error code may change in the future to return a more relevant error code.

## III.Client requirements for live TV.
 - The stream is delivered to the client in real time as it arrives from the broadcast source.
 - The client will have a slightly different concept of how long one second is due to slight differences in the client clock timebase vs the broadcaster clock timebase. The client must adjust its concept of real time to match the incoming video stream. For example, if the client is displaying frames slightly faster than the incoming stream rate it will slowly empty the input jitter buffer. Alternatively if the client is displaying frames slightly slower than the incoming stream rate it will slowly fill up and overflow the input jitter buffer.
 - For live TV the client must adjust its concept of real time by increasing or decreasing the frame rate slightly to match the rate at which data is arriving.
 - The initial buffering of data by the client must be based on time, not size. The stream is sent in real time which means the amount of data received in one second will be lower on SD channels vs HD channels.
 - There may be a delay between the HTTP request and data being returned. The initial buffering of data by the client must be based on time, starting from the first data arriving.
 - The client input jitter buffer max-size must be larger than the worst case amount of data that can be sent in the initial buffering time. This allows for 1) detecting if the client is running slower than the stream rate (see above), and 2) if the client takes time to set up the video rendering components after detecting the stream content it must continue to buffer (not drop data) during this setup time.

### IV.TODO
 - Documentation TODO: Document HTTP streaming video with DTCP-IP content protection.
