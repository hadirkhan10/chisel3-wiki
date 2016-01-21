# Tips and Tricks
### How to find chisel's intermediate files (on a *nix style machine)
Chisel typically runs a simulation in the users $TMPDIR directory under a directory named for the Scala-Chisel source files and with a unique hash number.  The following shell function will take you to the latest temporary directory of this sort.
I have added this to my bash startup files
```   
# Usage goto_chisel_temp <pattern>
# Example:
# >> goto_chisel_temp Router
Changing to latest tempdir for Small SmallOdds3Tester7903104241751344739/
# will place the user in latest temp directory associated with a chisel simulation
# >>>
function goto_chisel_temp {
      cd $TMPDIR
      dir=`ls -ltr | awk "/$1/"' { print $NF }' | tail -1`
      if test -n "4dir"; then
        echo "Changing to latest tempdir for $1 $dir"
        cd $dir
      else
        echo "Could not find latest tempdir for $1"
      fi
    }
```