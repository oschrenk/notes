# The Hit List #

I tried a couple of applications to manage my tasks, like [Things](http://culturedcode.com/things/) or [Midnight Inbox](http://www.midnightbeep.com/). I really like the full GTD approach of Midnight Inbox, but unfortunately it had too many bugs when I tried it.

And then [The Hit List](http://www.potionfactory.com/thehitlist/) came along and blew me away with its keyboard driven interface as it really fits habits as rarely use my mouse anyway and even train to use it less and less.

## The workflow ##

### Initial setup and ideas ###

I use Hit List as container for tasks and ideas, not for notes or anything bigger then a simple mind fart (sorry to be that blunt). If you have to lose a lot more words on a topic write it down anywhere else and reference to it.

THL offers two approaches when it comes to tasks. You can combine a hierarchical structuring of your tasks with tags, allowing for creating graph like dependencies between tasks. I seldom find use for simple tags and have a rather complex structure of folders and lists. The name of the list and its place in the project structure mostly give me enough info so that I often don't need the extra metadata of the tag.

What I do use is contexts, as they give my the ability to aggregate tasks by location (or mode as I would rather call it) rather then projects. I only have four contexts setup right now @home, @office, @university, and @errand.

The Inbox THL offers has no purpose for me in my setup as it mashes up all sorts of tasks. I setup my own smart folders, that show my tasks with regards to context (and without showing future tasks, as they would only add to confusion).

I have several other smart folders helping me with my review process. For a general overview I also have Smart folders setup showing all tasks of a specific context. For my daily, weekly review I also have smart folders showing me tasks without a context, without a tag, without a due date, without a start date respectively.

If there is interest you can take a look at in the appendix at my´project layout and the rules I have setup.

### Collecting ###

Collecting tasks with THL of course only works when I'm working on my Mac, so always have a notebook handy to write things down.

I have setup `Strg + Return` as a Quick entry (as `Cmd + return` is taken up by Quicksilver). I just jot down the task and don't mind about correct spelling or tagging, as I delegate that to the review phase.

### Review ###

I have many different review stages. When I'm working in my office I sometimes review multiple times a day, as there are just so many different tasks pulling you the one way or the other, forcing you to reschedule or reprioritize.

When I do review my tasks, I convert my analog notes to _Quick Entries_. As all my tasks land in the general inbox at first I only have one place to burn through.

When I enter the items I correct not only all spelling mistakes (so that I can find my tasks later) but really try to write them as actionable items. Often I find myself breaking them up in multiple items.

As said above there are only a few tags I use and most of them has to with how to correspond with other people (`/mail`, `/call`, `/wait`, `/delegated` and peoples' names like `/john connor/`).

In the evening or in the next morning during the commute I sift again through my tasks and reorder them, so that I can actually do them. I find that having a more minimal task list for one day makes you more productive. Only schedule one to three things a day!

### Working through tasks ###

When I can I use the note view as it does offer a view with less distractions, and a place to jot some text. As a side note all info that goes in that view should be considered as useless. Sounds weird at first, but if you have anything important to write down THL is not the place, use your companies wiki, a local text file or something else. Notes on a specific task are made to be forgotten in my opinion. Corresponding telephone numbers, if they are important have to go into your address book right away and may be kept as a copy in the note.

## The Appendix ##

### My layout ###


SF
:	Smart Folder

RF
:	Regular Folder

LL
:	List


    SF	All // ANY List is in "Inbox", List is in "Projects"

    `RF	Today w/ context
    		SF @home
    		SF @office
    		SF @university
    		SF @errand`

    `// The Rule for each SF is rather complex:
    		ALL
    			ANY
    				ANY
    					Due today
    					Due within the last 5 years
    				Start within the last 5 years
    			Tag is "@home"`

    `// May be the rule can be setup more easy but basically it lists
    	// all the tasks for a single context, that are due today, overdue
    	// or have been started. Reason is to omit clutter from tasks to be done
    	// in the future`

    `RF	Contexts
    		SF @home
    		SF @office`

    `RF	Review
    		SF @home // All containing Tag "@home"
    		SF @office // All containing Tag "@office"
    		SF @university // All containing Tag "@university"
    		SF No Context // All Title not containing "@"
    		SF Not Tagged // All Title not containing "/"`

    `RF	Projects
    		RF Private`

    `RF Personal
    				LL Correspondence // call x, mail y
    				LL Finances // get current rate of ..., write tax report
    				LL Health // make doctors appointment
    				LL Household // fix chair, ...
    				LL Write // things like this text
    				LL Read // a list of books I want to read,
    				LL Watch // movies and series I want to watch
    				LL Listen // audiobooks, albums to listen too
    				LL Wishes // Things I want to buy, like books, tea kettle`

    `RF Open Source // not restricted to computer stuff
    				LL Ideas // brain farts for future projects
    				LL Project A // build a paper-mâché table
    				LL Project B // build twitter client
    				[...]`

    `RF University
    			LL Class A // do home work
    			LL Class B
    			[...]`

    `RF Work
    			LL Client A // resolve bug 1234 ...
    			LL Client B
    			LL Client C
    			[...]`

    `RF	Archive // old work clients or finished projects
    		[...]`

### Features I don't have a use for (yet): ###

#### Priorities ####

Even if you didn't notice it but The Hit List comes with three different approaches for handling priorities, setting it manually is just the most explicit one. I prefer a more implicit approach.

*   You can rearrange tasks in your list. If it is on top, it is the
    most important one.
*   If your folder/list structure allows for it, you can even use the
    order of the folders/lists as an indicator for the priority of
    your projects.
*   You can set a date for a task. The task with the nearest date is
    the most important one.

#### Timer ####

As said in the text above. When I'm on my personal time there is no need track it, when I'm on company time I have to use their system. Maybe I find a way to combine time timer of THL with our system.

#### Tabs ####

It is a nice idea but doesn't serve my needs in the current implementation. I'd rather have a setting where I can define up to (lets say) 5 tabs that are always available, can't be closed, do have their own color scheme and ideally can be addressed to opened directly with a command like `Cmd + 1`, `Cmd + 2` and so on. That would help me switch contexts or current projects very quickly.

### Current Quirks with THL ###

#### Not all commands usable with foreign keyboard layout ####

I can't move the Due or Start Date via my keyboard as I have a german keyboard layout. For example I have to press `Alt + 6` to get a `]`, but that doesn't postpone the due date.

#### No ability to mass tag via keyboard ####

I already opened a thread for this. It would be nice to select a few tasks and be able to add a tag/context to all tasks when I start typing `/` or `@`. Seems for like this would be in the spirit of THL

#### Doesn't search in archived tasks ####

That is why I stress not to use vital info in tasks. You can't search for it if the task is down. But maybe its a good point as it forces you to rethink where to store information.

#### Doesn't offer the note view for archived tasks ####

Basically what it says. Sometimes I'd like to view at the info of an archived task.

## Settings/Hints ##

    defaults write com.potionfactory.TheHitList commandWClosesTab -bool YES
