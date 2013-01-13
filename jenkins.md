# Jenkins #

	brew install jenkins

Installation notes

	If this is your first install, automatically load on login with:
	  mkdir -p ~/Library/LaunchAgents
	  ln -nfs /usr/local/Cellar/jenkins/1.490/homebrew.mxcl.jenkins.plist ~/Library/LaunchAgents/
	  launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist

	If this is an upgrade and you already have the homebrew.mxcl.jenkins.plist loaded:
	  launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist
	  launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist

	Or start it manually:
	  java -jar /usr/local/Cellar/jenkins/1.490/libexec/jenkins.war
