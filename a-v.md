# Audio & Video #

## Audio ##

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

### Joining flac files ###

	brew install shntool
	shntool join *.flac
	flac -o out.flac joined.wav

### convert wav to m4a

	ffmpeg -i joined.wav -acodec alac out.m4a

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
