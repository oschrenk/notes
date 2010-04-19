# Using OSX with Windows Machines

## Windows Domain

As far as I understand can't really work with Windows Domains, at least not in the same way normal Windows machines would. You can still mount network drives though using the login data provided by the admin though. But to have it automounted on startup, you would have to write your own script run at login time.

## Workgroup

I had problems logging into the drives. What helped was using the same workgroup as the windows machines. You can edit the workgroup in

	System Preferences > Network > Ethernet Adapter > More Options > WINS > Workgroup