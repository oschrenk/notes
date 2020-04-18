TLDR: Give it time, trust every player, communicate a lot and learn the quirks

You probably are already using [Gloomhaven - Fantasy Setup (Scripted UI)](https://steamcommunity.com/sharedfiles/filedetails/?id=1301493206&searchtext=fantasy) - it is the quasi default and the basis for the Frosthaven Demo.

**What were the struggles that you had? Are you new to Gloomhaven? Are you new to TTS?**

My group was playing Gloomhaven on a monthly basis for two years and we needed to get used to TTS (Tabletop Simulator) and then we we had to get used to GFS (Gloomhaven Fantasy Setup). It can be be very time consuming and finicky.

I first played a game Solo and then I scheduled "training sessions" with every player one-on-one, so that everybody understood the mechanics.  Each of those "training " lessons were 60 to 90 minutes. The players learned to move the camera, use the hands and move the player.

And with each different player I learned more about the quirks. And there are a lot.

**The host should:**
* Disable Physics. It helps moving game pieces around.
* Disable Flip Table (IMO that is the most stupid UI element - right next to the menu button... and it crashes the game for me)

**Every player should**
* Set Height to lowest setting. It helps move game pieces around
* Set Rotation to 90. Helps tapping card if you consume them.
* Learn shortcuts:
	* `F` Flip a card/object
	* `Q`,`E` to rotate an object.
	* `Alt`. Pressing it while hovering over a card, allows glancing at it
	* `Shift+Alt`. Glance at the backside
	* `Tab`. Drops an arrow for a short while as an indicator.
	* `Space`. Reset camera position.

**The group should**
* Use the other cursors as visual indicators where they are looking
* Communicate clearly. Not everyone is used to talk with the others not being able to see. You have to talk much more than in real life.
* It helps if all players keep the same camera orientation. At least then left/right/up/down are unambigous
* Trust each other. I don't look at the other players cards so much if at all. They will do their thing.
* Help out. Everyone helps and does so with clear communication. "I will take care of the elements this round". "I will set the damage".

**Advanced Techniques**
I haven't done this a lot but it might help other people.
* Save Camera positions. You can Save camera positions `Ctrl + #` with `#` being a number. to load camera positions, press `Shift + #`





To change Physics (I believe you can only do it as host):
Options > Server > Physics > Choose "Locked"

To change "Lift Height", click on the sticky guy lifting weights, its next to the rotation setting. A slider will appear. Drag it to a value you like (I prefer a very low setting). It will define how high you will lift up things.

> If you have a 2nd monitor, you can save a camera for the 2nd monitor to look at monsters or board while your primary does whatever you want. This can be used to help run monsters but is less necessary after the update when they put stats on the standees and in the initiative tracker. Ideal if you have a 2nd person to run the other side's monsters.
> 1. Open the TTS Console ( ` ) by default
> 2. Type and enter "spectator_screen 1". 2nd monitor should now mirror your primary monitor
> 3. Move your screen to what you want to save for later, CTRL+1-9 to save the camera
> 4. Do this for both sides or pick which side you and your fellow monster AI buddy are doing
> 5. Open the Console ( ` ), type and enter "spectator_camera_load X", where X is the camera number you saved and want to show.
> 6. For example for saved cameras 4 & 5, you can enter "spectator_camera_load 4", do the monster's turn while still able to interact with the board, and then use "spectator_camera_load 5" to do the other side's monsters. Switch as needed. Tab autocomplete works in the Console.

> The newest version of GFS has a lot of quality of life improvements. A lot of extraneous items were removed, greatly improving performance. The monsters now show their base stats above the figures, and their action cards can be viewed in the initiative tracker. Best of all, you can now make a coin by setting an enemy’s HP to zero and clicking “defeat.”
> You don’t need to keep track of waning elements. Just make them when they need to be made and consume them when they need to be consumed.
> I suggest that people be designated certain upkeep tasks. For instance, have one and only person click the “start round” and “end round” buttons after checking in that everyone is ready to go on. Other people could be assigned other upkeep tasks as appropriate.
> There is a useful feature where you can click a character in the initiative tracker to turn their name red. This can be used to indicate that their turn is done. It’s also useful to announce out loud that your turn is done, and to let the next player know that it’s their turn.

> Also: pressing a number when hovering on a deck will draw x number of cards, where x is the number pressed. Thus, for my battle goals, I hover, press R to randomize, then press 2 and it draws 2 (not using keypad).

> [» Camera States](https://berserk-games.com/knowledgebase/camera-states/)
- dungeon
- Play Area Board
- Character


* For each player, count all the loot tokens on the player's character mat, multiply by the right amount based on scenario difficulty, add it to your sheet. Delete the loot tokens.
* For each player, take all the experience on the blue tracker, add it to your sheet.
* For each player, reset Exp and HP trackers to 0 and Max HP, respectively. You can do this by clicking - on the Player Mat health and exp buttons until they are at the right values.
* For each player, return all lost and Discarded cards to hand
* For each player, remove all Scenario Modifier cards from deck (bless, curse, etc) and put them back in right pile on the right side of board.
* For each player, remove all battle goal cards from hand and put them back in pile on the right side of the board.
* For each player, flip all item cards so that they're face up and un-tapped.
* Delete all the scenario objects (map tiles, monsters, scenario book, etc) from the playing area.

To signify exhaustion
> You end up also having to long rest them every turn if I recall correctly, or you get an error message.


last resort to recover things:
> Load your game, and then at the top of the screen, click the "Game" button. In the row that says "Workshop", you should see Gloomhaven - Fantasy Setup. Hover your mouse over the upper right corner of the icon, and three dots should appear. Click those dots, and then click on the "expand" option.

expand hp:
> Click the HP bar of your character’s mini, and it should show “Max” and a pair of arrows above the HP bar. Just use the arrows to increase or decrease the maximum health of your character, then click the HP bar again to hide the max control.

> One other tip. Sometimes it can be hard to get people’s attention toward a specific place or card or whatever. If you click and hold down the Tab key and circle around it in wide loops, you’ll basically make a “radar” circle so people can more easily find it.

rotating party
> At the end of each session, repack your characters into their envelopes (by clicking Remove Player above the player mat). The person running the server can then save each envelope as a saved object. They’ll always be able to drop a copy of the most recent saved version of the character into the game, including the character sheet, items, and enhancements. That should handle rotating parties, unless there’s another issue I’m missing.

remove status
> Just click on the status above the mini or standee.
