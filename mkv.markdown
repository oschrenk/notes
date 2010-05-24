# MKV #

Matroska Video (MKV) files

## Installation

	sudo port install mkvtoolnix
	
Which took ages (2+ hours, mainly because of Boost).

## Extract Audio/Video/Subtitles ##

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


