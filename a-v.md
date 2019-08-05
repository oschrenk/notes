# Audio & Video #

## Audio ##

### Conversions

`wav` to `mp3`

    # qscale range is 0-9 where a lower value is a higher quality
    ffmpeg -i input.wav -codec:a libmp3lame -qscale:a 2 output.mp3

`wav` to `m4a`

    ffmpeg -i joined.wav -acodec alac out.m4a

`aif` to `mp3`

```
# qscale range is 0-9 where a lower value is a higher quality
ffmpeg -i input.aifc -codec:a libmp3lame -qscale:a 0 output.mp3
```

### Splitting Mp3 ###

Install via

	brew install mp3splt

Usage:

	mp3splt -c music.cue music.mp3
	mp3splt file.mp3 0.32 3.56

Which pumps out the named files into the same directory

	mp3splt [OPTIONS] FILE_OR_DIR1 [FILE_OR_DIR2] ... [BEGIN_TIME] [TIME2] ... [END_TIME]

Time format:

	minutes.seconds[.hundredths] or EOF-minutes.seconds[.hundredths]

### Splitting m4a files

  brew install mp4box
  MP4Box -split-chunk 17.5:204.6 input.m4a

### Joining flac files ###

	brew install shntool
	shntool join *.flac
	flac -o out.flac joined.wav

### Extracting audio track ###

#### mp3 from avi ####

Install via

	brew install ffmpeg

Usage:

	ffmpeg -i file.avi -f mp3 file.mp3

#### mp3 from mkv files ####

Install via

	brew install mkvtoolnix

Which took ages (2+ hours, mainly because of Boost).

List contents of mkv file

	mkvmerge -i file.mkv

Output will be something like:

	File 'file.mkv': container: Matroska
	Track ID 1: subtitles (S_TEXT/ASS)
	Track ID 2: audio (A_MPEG/L3)
	Track ID 3: video (V_MPEG4/ISO/AVC)

Extract tracks with `mkvextract`

	mkvextract tracks MovieFile.mkv 1:thesubtitles.srt 2:theaudio.mp3 3:thevideo.mp4

Or just the audio track

	mkvextract tracks MovieFile.mkv 2:theaudio.mp3

#### m4a from mp4 ####

Install via

	brew install ffmpeg

Usage:

	ffmpeg -i input.mp4 -vn -c:a copy output.m4a

## Video ##

### Split Quicktime movies ###

[http://www.3am.pair.com/QTCoffee.html](http://www.3am.pair.com/QTCoffee.html)

To split a movie

	splitmovie video.mov -splitAt 1:06.5 -splitAt 1:32.02 -self-contained -o movie.mov

To join videos

	catmovie movie1.mov movie2.mov movie3.mov ‑self‑contained ‑o bigmovie.mov

### Delete audio track or subtitle or attachment from mkv

First you have to list the content of the container

    mkvmerge -i input.mkv
    Track ID 0: video (MPEG-4p10/AVC/h.264)
    Track ID 1: audio (AAC)
    Track ID 2: audio (AAC)
    Track ID 3: subtitles (SubRip/SRT)
    Track ID 4: audio (AC-3/E-AC-3)
    Track ID 5: audio (AC-3/E-AC-3)
    Track ID 6: subtitles (SubStationAlpha)
    Attachment ID 1: type 'text/plain', size 52 bytes, file name 'foo.txt'
    Attachment ID 2: type 'image/jpeg', size 34832 bytes, file name 'title.jpg'
    Attachment ID 3: type 'application/x-truetype-font', size 46037 bytes, file name 'font1.ttf'
    Attachment ID 4: type 'application/x-truetype-font', size 76703 bytes, file name 'font2.ttf'

You select the audio tracks to keep with a comma separated list

    mkvmerge -o output.mkv --atracks 2,3 input.mkv

You select the subtitles to remove with a comma separated list

    mkvmerge -o output.mkv input.mkv --subtitle-tracks 3,4  # remove tracks 3 and 4
    mkvmerge -o output.mkv input.mkv -s '!3'                # remove all subtitles except 3

You select attachments to keep with a comma separated list

    mkvmerge -o output.mkv input.mkv --attachments 3,4      # keep attachment 3 and 4
    mkvmerge -o output.mkv input.mkv --no-attachments       # remove all attachments

### Split mp4 movie

First `--ss` quick seeks, second one mvoes accurately.

    ffmpeg -ss 01:29:00 -i video.mp4 -ss 00:00:40 -t 00:0l:21 -c copy clip.mp4

## Create gif

    ffmpeg -i clip3.mp4 -s 600x400 -pix_fmt rgb24 -f gif - | gifsicle --optimize=3 --delay=3 > out.gif
