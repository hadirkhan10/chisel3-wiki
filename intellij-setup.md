# How to use the IntelliJ Scala IDE with the chisel3 project.

The chisel3 repo as cloned is not a correctly set up IntelliJ Scala/SBT Project.  To make it one use the following steps that worked on a MacBookPro running El Capitan.  IntelliJ had already been setup with the Scala plugin


* Get into the IntelliJ project directory, which is somewhere in your home directory as IdeaProjects or IntelliJProjects
* Clone the chisel3 repo `git clone https://github.com/ucb-bar/chisel3.git`
* Now import the project by starting IntelliJ (and closing any open projects you have) which gets you to a dialog that has an *Import Project*.  Choose that.  
* Find your cloned repo in the directory chooser dialog.  
* Then under the Import Project dialog, Select *Import project from external model*
* And from the list select SBT as the external model.  Click *Next*
* Next dialog has a bunch of unclicked options (leave them that way).  I used *Project SDK: 1.8*, Click *Finish*

This game me a functional Project with the files where I expected to find them. There may be other ways to do this, but I have not found one, I have many ways that don't work, but have decided not to document those




