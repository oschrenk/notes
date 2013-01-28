# gitattributes or How to deal with EOL #

Based on Tim Clem's blog entry [Mind the end of your line](http://timclem.wordpress.com/2012/03/01/mind-the-end-of-your-line/).

The root of the problem is that Unix, Linux and OS X use `LF` (Linefeed) and Windows uses `CRLF` (Carriage Return, Linefeed) to denote the end of a line. Prior to OS X, the Macintosh Platform used `CR`.

Sharing code or text between these platforms is a problem and a problem you have to deal with everytime you interact with other peoples textual data even if you just copy text from the browser or open a a file in a zip file. These differences, albeit small, make your diffs messy.

Git solution is to define `LF` as the best way to store line endings for _text files_ in a Git repository's object database. It doesn't force you to use this but it's quasi standard.

Git filters files and their line encoding in two different phases.

1. Writing to the object database
2. Reading from the object database and writing to the working directory (eg. `git checkout`)

When Git needs to change line endings to write a file in your working directory it will use the `core.eol` settings.

- `core.eol = native` Default. Git uses default line ending on your platform.
- `core.eol = crlf` Git will always use `crlf` as line ending.
- `core.eol = lf` Git will always use `lf` as line ending.

#### What is a text file? ####

Git has some internal heuristics to determine if a file is binary or not. A file is deemed text if it is not binary.

#### The Old System ####

Git has a configuration setting called `core.autocrlf` which is specifically designed to make sure that when a text file is written to the repository’s object database that all line endings in that text file are normalized to `LF`.

- `core.autocrlf = false` Default. Most people are encouraged to change this immediately though. Git doesn’t ever mess with line endings on your file. You can check in files with `LF` or `CRLF` or `CR` and Git does not care. This can make diffs harder to read and merges more difficult.
- `core.autocrlf = true` Git will process all text files and make sure that `CRLF` is replaced with `LF` when writing that file to the object database and turn all `LF` back into `CRLF` when writing out into the working directory. This is the _recommended setting on Windows_ because it ensures that your repository can be used on other platforms while retaining CRLF in your working directory.
- `core.autocrlf = input` Git will process all text files and make sure that `CRLF` is replaced with `LF` when writing that file to the object database. It will **not**, however, do the reverse. When you read files back out of the object database and write them into the working directory they will still have `LF`s to denote the end of line. This setting is _generally used on Unix/Linux/OS X_ to prevent CRLFs from getting written into the repository. The idea being that if you pasted code from a web browser and accidentally got `CRLF`s into one of your files, Git would make sure they were replaced with LFs when you wrote to the object database.

Sometimes Git can be wrong in determinig if a file is text or not. For these cases `core.safecrlf` was introduced, to prevent Git from changing line endings on a file that really should just be left alone.

- `core.safecrlf = true` When getting ready to run this operation of replacing `CRLF` with `LF` before writing to the object database, Git will make sure that it can actually successfully back out of the operation. It will verify that the reverse can happen (`LF` to `CRLF`) and if not the operation will be aborted.
- `core.safecrlf = warn` Same as above, but instead of aborting the operation, Git will just warn you that something bad might happen.

One final layer on all this is that you can create a file called `.gitattributes` in the root of your repository and add rules for specific files. These rules allow you to control things like `autocrlf` on a per file basis. So you could, for instance, put this in that file to tell Git to always replace `CRLF` with `LF` in txt files:

	*.txt crlf

Or you could do this to tell Git to never replace `CRLF` with LF for txt files like this:

	*.txt -crlf

Or you could do this to tell Git to only replace `CRLF` with LF when writing, but to read back `LF` when writing the working directory for txt files like this:

	*.txt crlf=input

#### The New System ####

The new system moves to defining all of this in the `.gitattributes` file that you keep with your repository and is available in Git 1.7.2 and above.

You are in charge of telling git which files you would like `CRLF` to `LF` replacement to be done on. This is done with a text attribute in your repository’s  `.gitattributes` file.

- `*.txt text` Set all files matching the filter `*.txt` to be text. Git will run `CRLF` to `LF` replacement on these files every time they are written to the object database and the reverse replacement will be run when writing out to the working directory.
- `*.txt -text` Unset all files matching the filter. These files will never run through the `CRLF` to `LF` replacement.
- `*.txt text=auto` Set all files matching the filter to be converted (`CRLF` to `LF`) if those files are determined by Git to be text and not binary. This relies on Git’s built in binary detection heuristics.

If a file is unspecified then Git falls back to the `core.autocrlf` setting, though it is highly recommended (especially for Windows developers) that you explicitly create a `.gitattributes` file.