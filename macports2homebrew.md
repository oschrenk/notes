# Macports #

First, uninstall MacPorts:

	sudo port -f uninstall installed

Second step: remove everything that is left from MacPorts:

	sudo rm -rf /opt/local /Applications/DarwinPorts /Applications/MacPorts /Library/LaunchDaemons/org.macports.* /Library/Receipts/DarwinPorts*.pkg /Library/Receipts/MacPorts*.pkg /Library/StartupItems/DarwinPortsStartup /Library/Tcl/darwinports1.0 /Library/Tcl/macports1.0 ~/.macports
	
It's recommended to also remove the contents of the following directories as they could contain libs and header of other compilations

	rm -rf /usr/local/include/* 
	rm -rf /usr/local/lib/*
	
Install homebrew

	ruby -e "$(curl -fsS http://gist.github.com/raw/323731/install_homebrew.rb)"
	
Get the packages	
	
	brew install wget
	brew install git