# Tips and Tricks
### How to find chisel's intermediate files (on a *nix style machine)
Chisel typically runs a simulation in the users $TMPDIR directory under a directory named for the Scala-Chisel source files and with a unique hash number.  The following shell function will take you to the latest temporary directory of this sort.
`    function cdnt {
      cd $TMPDIR
      echo "now with awk"
      #ls -ltr | awk "/$1/"' { print $NF}'
      dir=`ls -ltr | awk "/$1/"' { print $NF }' | tail -1`
      #echo "got dir $dir"
      if test -n "4dir"; then
        echo "Changing to latest tempdir for $1 $dir"
        cd $dir
      else
        echo "Could not find latest tempdir for $1"
      fi
    }
`