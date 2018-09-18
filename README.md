# PROCHECK 3.5.4 legacy code and gfortran >= 4.8.5 

symptoms: the fortran executables fail with "Fortran runtime error: Sequential READ or WRITE not allowed after EOF marker, possibly use REWIND or BACKSPACE"

```
...
     ..................................................................
      
     Phi-psi and chi1-chi2 distributions
      
      
      Main Ramachandran plot          * File: my_file_01.ps
      All-residue Ramachandran plots  * File: my_file_02.ps
      All-residue chi1-chi2 plots     * File: my_file_03.ps
      * Program complete
      
     ..................................................................
      
     Stereochemical quality plots and residue-by-residue listing
      
     At line 2629 of file pplot.f (unit = 14, file = 'my_file.sum')
     Fortran runtime error: Sequential READ or WRITE not allowed after EOF
     marker, possibly use REWIND or BACKSPACE
      
      Main-chain parameters           * File: my_file_04.ps
      
     ..................................................................
      
     Main-chain bond-lengths and angles, and planar groups
      
     At line 1588 of file bplot.f (unit = 14, file = 'my_file.sum')
     Fortran runtime error: Sequential READ or WRITE not allowed after EOF
     marker, possibly use REWIND or BACKSPACE
      
      Main-chain bond lengths         * File: my_file_04.ps
...
```

procheck has been compiled and ran successfully on our CentOS-3/5/6 and only
showed this behaviour on CentOS-7. Same issue with the newer gfortran versions
gcc/4.9.2-devtoolset-3 gcc/6.3.1-devtoolset-6 gcc/5.3.1-devtoolset-4 gcc/7.2.1-devtoolset-7.

The fix was found on http://gcc.gnu.org/bugzilla/show_bug.cgi?id=59513

Makefile line 10:
```
 FOPTS = -O2 -g   # Compile/link options for FORTRAN
```
needs to be changed to:
```
 FOPTS = -O2 -g  -std=legacy # Compile/link options for FORTRAN

```

YMMV,

Tru
