# How to use the IntelliJ Scala IDE with the chisel3 project.

The chisel3 repo is not a valid Chisel3 project.  To make it one locally you must do the following
Get into the IntelliJ project directory, which is somewhere in your home directory as IdeaProjects or IntelliJProjects
Clone the chisel3 repo
`git clone https://github.com/ucb-bar/chisel3.git`
Now import the project by starting IntelliJ (and closing any open projects you have) which gets you to a dialog that has an *Import Project*.  Choose that.  Find your cloned repo in the directory chooser dialog.  
Then under the Import Project dialog, Select *Import project from external model*
And from the list select SBT as the external model.  Click *Next*
Next dialog has a bunch of unclicked options (leave them that way).  I used *Project SDK: 1.8*, Click *Finish*




