Copyright (c) 2014 Jason Masker <jason@masker.net>
All rights reserved. 

This file is part of wordMerge.

wordMerge is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 2 of the License, or
(at your option) any later version.

wordMerge is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with wordMerge.  If not, see <http://www.gnu.org/licenses/>.

Version 2014-02-08

wordMerge
============================

Overview
--------
wordMerge is a git merge driver for OSX/darwin which can be used to merge Microsoft Word
documents using the commercial software Microsoft Office for Mac 2011.

'wordMerge' is the primary driver, a shell script wrapper which passes applescript
commands to the 'osascript' command-line tool, included with OS X.

Usage
-----
Copy wordMerge into your path and make it executable.

    cp wordMerge /usr/local/bin
    chmod 755 /usr/local/bin/wordMerge

Copy the configuration from the included 'gitconfig' into your '~/.gitconfig' global git
configuration file. Alternatively, you can issue the following commands:

    git config --global merge.wordMerge.name wordMerge
    git config --global merge.wordMerge.driver 'wordMerge "$PWD/%O" "$PWD/%A" "$PWD/%B"'
    git config --global merge.wordMerge.recursive binary

You will also need to create or modify the 'attributes' file in the 'info' directory of
your git repositories to identify the file types that should be merged using the new
driver. An example 'attributes' file has been included which includes the following:

    *.docx binary
    *.dotx binary
    *.docx merge=wordMerge
    *.dotx merge=wordMerge

Add any other file extensions you would like to merge using Microsoft Word.

When a merge of two files with the identified extensions is required, it will be performed
using the newly defined wordMerge driver. Microsoft Word will open the common ancestor and
merge the local and remote branches, tracking changes.

The process will pause for the user to accept or reject the tracked changes using the 
Review panel in Word. 

Make any final edits and select 'Yes' or 'No' from the script prompt to indicate if the
merge was successful or not. Marking the merge as unsuccessful gives the user an
opportunity to pause or cancel the merge process.

Caveats
-------
If there are formatting changes, Microsoft Word cannot merge them and will prompt for
which document to use as a base for formatting. Formatting changes to tables, etc. will
need to be done manually. When prompted, it is almost always best to choose the formatting
for the 'Current Document' and not the 'Target Document.' This is because the merge action
in Word is actually performed twice per merge. The common ancestor is opened and first the
local branch changes are merged in, followed by the remote branch changes. Though you may
be prompted twice, by choosing 'Current Document' each time, you are effectively keeping
the formatting of the common ancestor.

Due to the interactive nature of the script, it works best for a regular merge. If you opt
to rebase using this merge driver, it will work, but there will be a manual, interactive
merge step for each change, which can be a tedious and redundant process.

The script requires tracking the windows open in Microsoft Word and, in order to
accomplish this, begins by closing all open documents and saving changes. It is best to
close all open documents, or ensure that Microsoft Word is not running prior to beginning
a merge. It is a future objective to eliminate this requirement and behavior.

TODO
----
*   Modify the script to work reliably without first closing all open Word documents.
*   Modify driver for use as a diff driver.

