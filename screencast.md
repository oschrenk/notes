# Screencast

```shell
brew install cytopia/tap/ffscreencast
```

Do a screencast on the default screen (without explicitly choosing the monitor)

```shell
$ ffscreencast
```

List monitors and record on monitor 2 (`Capture screen 0`)

```shell
$ ffscreencast --slist
Available screen recording devices (monitors):

[2] Capture screen 0    Color LCD: Resolution: 2880 x 1800 Retina
[3] Capture screen 1    S2431W: Resolution: 1920 x 1200
[4] Capture screen 2    Thunderbolt Display: Resolution: 2560 x 1440


$ ffscreencast -s2
```

List cameras

```shell
$ ffscreencast --clist
Available camera recording devices:

[0] FaceTime HD Camera (Display) (160x120@29.97 160x120@25 160x120@23.999981 160x120@14.999993 176x144@29.97 176x144@25 176x144@23.999981 176x144@14.999993 320x240@29.97 320x240@25 320x240@23.999981 320x240@14.999993 352x288@29.97 352x288@25 352x288@23.999981 352x288@14.999993 640x480@29.97 640x480@25 640x480@23.999981 640x480@14.999993 960x540@29.97 960x540@25 960x540@23.999981 960x540@14.999993 1024x576@29.97 1024x576@25 1024x576@23.999981 1024x576@14.999993 1280x720@29.97 1280x720@25 1280x720@23.999981 1280x720@14.999993)

[1] FaceTime HD Camera (1280x720@30 640x480@30 320x240@30)

```

Start a screencast with camera overlay (only one camera present)

```shell
$ ffscreencast -c
```

or select the camera device

```shell
$ ffscreencast -c0
```

Show the ffmpeg command for camera recording

```shell
$ ffscreencast -c --dry

ffmpeg -hide_banner -loglevel info -f avfoundation   -i "1" -f avfoundation  -i "0" -c:v libx264 -crf 0 -preset ultrafast -filter_complex 'overlay=main_w-overlay_w-10:main_h-overlay_h-10' "/Users/cytopia/Desktop/Screencast 2015-10-06 at 21.28.01.mkv"

```
