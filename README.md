Simple HTTP API to obtain remote video metadata using ffprobe as a backend.

## Installation

### Clone and install

	$ git clone https://github.com/fcingolani/video-metadata-api.git
	$ cd video-metadata-api
	$ npm install

### Having global FFMPEG binaries (Linux)

Install ffmpeg binaries globally using your package manager (*YUM*, *apt*, etc) and copy the default config file:

	$ cp config.sample.json config.json

###	Having local FFMPEG binaries (Windows)

Download binaries from [FFMPEG](https://www.ffmpeg.org/download.html) and place them inside the *bin* directory:

- bin\ffmpeg.exe
- bin\ffprobe.exe

Create a *config.json* file:

```json
{
	"ffmpegDir": "bin",
	"port": 3000,
	"logFormat": "common"
}
```

Install it as a windows service (optional).

	$ npm install -g winser
	$ npm run-script install-windows-service

Configure that service to run as a service account (won't run if you leave it as LocalSystem user).

	$ sc stop "video-metadata-api"
	$ sc config "video-metadata-api" obj= "DOMAIN\User" password= "password"
	$ sc start "video-metadata-api"

## Usage

Make a GET Request specifying the remote video URL in the *video_url* parameter:

	$ curl http://localhost:3000/?video_url=http://media.w3.org/2010/05/sintel/trailer.mp4

You will get a json response:

```json
{
  "streams":[
    {
      "index":0,
      "codec_name":"h264",
      "codec_long_name":"H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10",
      "profile":"High",
      "codec_type":"video",
      "codec_time_base":"1/48",
      "codec_tag_string":"avc1",
      "codec_tag":"0x31637661",
      "width":854,
      "height":480,
      "has_b_frames":2,
      "sample_aspect_ratio":"0:1",
      "display_aspect_ratio":"0:1",
      "pix_fmt":"yuv420p",
      "level":30,
      "timecode":"N/A",
      "id":"N/A",
      "r_frame_rate":"24/1",
      "avg_frame_rate":"24/1",
      "time_base":"1/24",
      "start_pts":0,
      "start_time":0,
      "duration_ts":1253,
      "duration":52.208333,
      "bit_rate":537875,
      "nb_frames":1253,
      "nb_read_frames":"N/A",
      "nb_read_packets":"N/A",
      "tags":{
        "creation_time":"1970-01-01 00:00:00",
        "language":"und",
        "handler_name":"VideoHandler"
      },
      "disposition":{
        "default":1,
        "dub":0,
        "original":0,
        "comment":0,
        "lyrics":0,
        "karaoke":0,
        "forced":0,
        "hearing_impaired":0,
        "visual_impaired":0,
        "clean_effects":0,
        "attached_pic":0
      }
    },
    {
      "index":1,
      "codec_name":"aac",
      "codec_long_name":"AAC (Advanced Audio Coding)",
      "profile":"unknown",
      "codec_type":"audio",
      "codec_time_base":"1/48000",
      "codec_tag_string":"mp4a",
      "codec_tag":"0x6134706d",
      "sample_fmt":"fltp",
      "sample_rate":48000,
      "channels":2,
      "channel_layout":"stereo",
      "bits_per_sample":0,
      "id":"N/A",
      "r_frame_rate":"0/0",
      "avg_frame_rate":"0/0",
      "time_base":"1/48000",
      "start_pts":0,
      "start_time":0,
      "duration_ts":2493440,
      "duration":51.946667,
      "bit_rate":126694,
      "nb_frames":2435,
      "nb_read_frames":"N/A",
      "nb_read_packets":"N/A",
      "tags":{
        "creation_time":"1970-01-01 00:00:00",
        "language":"und",
        "handler_name":"SoundHandler"
      },
      "disposition":{
        "default":1,
        "dub":0,
        "original":0,
        "comment":0,
        "lyrics":0,
        "karaoke":0,
        "forced":0,
        "hearing_impaired":0,
        "visual_impaired":0,
        "clean_effects":0,
        "attached_pic":0
      }
    }
  ],
  "format":{
    "filename":"http://media.w3.org/2010/05/sintel/trailer.mp4",
    "nb_streams":2,
    "nb_programs":0,
    "format_name":"mov,mp4,m4a,3gp,3g2,mj2",
    "format_long_name":"QuickTime / MOV",
    "start_time":0,
    "duration":52.209,
    "size":4372373,
    "bit_rate":669979,
    "probe_score":100,
    "tags":{
      "major_brand":"isom",
      "minor_version":"512",
      "compatible_brands":"isomiso2avc1mp41",
      "creation_time":"1970-01-01 00:00:00",
      "title":"Sintel Trailer",
      "artist":"Durian Open Movie Team",
      "encoder":"Lavf52.62.0",
      "copyright":"(c) copyright Blender Foundation | durian.blender.org",
      "description":"Trailer for the Sintel open movie project"
    }
  }
}
```
