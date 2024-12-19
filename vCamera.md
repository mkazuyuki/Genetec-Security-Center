# Making RTSP virtual camera

## Versions to be used

- OBS Studio 31.0.0
- OBS RTSPServer v3.1.0
- VLC media player 3.0.21

## Building Server

1. Install [OBS Studio](https://obsproject.com/) and [OBS-RTSPServer](https://github.com/iamscottxu/obs-rtspserver).
1. Open *OBS Studio*
1. Add *Image* as *Source*
1. *Tool* menu > *RTSP Server*  > *Start output*

## Building Client

1. Install [VLC media player](https://www.videolan.org/)
1. Open *VLC media player*
1. *Media* menu > *Open network stream* > specify `rtsp://10.0.0.1/live` > *Play*  

   **NOTE**: Assuming `10.0.0.1` as IP address of the server.
