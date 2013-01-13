# Wine #

	brew install wine

## Games ##

### UFO: Enemy Unknown ###

I had problems with Wine 1.5.20 and had to revert to 1.5.19, which is till in development and not listed as a version in homebrew but available as a development version

	cd /usr/local
	git checkout 13957e1a0f9b8c5b9fc0029ac7c22e719fadb536 /usr/local/Library/Formula/wine.rb

After that some dependencies have to be installed

	winetricks mfc42 xact_jun2010 d3dx11_43 d3dx9_43 d3dcompiler_43 d3dx9_36 vcrun2008 vcrun2010

I go the game installed but it just won't start up properly. Only a black screen.

## FAQ ##

When trying to install software I get

	Runtime Error (at - 1:0):
	Cannot Import dll:C:\users\xxx\Temp\is-00IEG.tmp\isskin.dll

You need to install winetricks


	brew install winetricks

	winetricks mfc42