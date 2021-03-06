# /*******************************************************************
# Copyright (C) 2017 AMPL Optimization, Inc.; written by David M. Gay.
#
# Permission to use, copy, modify, and distribute this software and its
# documentation for any purpose and without fee is hereby granted,
# provided that the above copyright notice appear in all copies and that
# both that the copyright notice and this permission notice and warranty
# disclaimer appear in supporting documentation.
#
# The author and AMPL Optimization, Inc. disclaim all warranties with
# regard to this software, including all implied warranties of
# merchantability and fitness.  In no event shall the author be liable
# for any special, indirect or consequential damages or any damages
# whatsoever resulting from loss of use, data or profits, whether in an
# action of contract, negligence or other tortious action, arising out
# of or in connection with the use or performance of this software.
# *******************************************************************/

# For compiling solvers/examples on Unix and Linux systems.

# Invoke with "make -f makefile.u",
# or "cp makefile.u makefile", edit makefile as necessary,
# and just invoke with "make".

.SUFFIXES: .c .f .o
CC = cc
S = ..
# "S = .." assumes this directory is solvers/examples .

CFLAGS = -g -O -I$S

# If things don't run right, you may need to change -O to -g
# so you can poke around with a debugger.

# See $S/makefile.u for other comments about CFLAGS.

.c.o:
	$(CC) -c $(CFLAGS) $*.c

## On HP systems, add -ldld to the end of the L = line below.
## On linux and Solaris systems, add -ldl to the end of the L = line below.

L = -lm -ldl
Lf2c = libf2c.a -ldl
cport = cport.a

# $(cport) = PORT subroutine library.  For the present examples,
# it suffices to build cport.a from a subset of the port/*.f files in
# http://www.netlib.org/port.tar
# For your possible convenience, solvers/examples/cport.zip contains some
# sample makefiles along with the result of running port.tar through f2c.

# -lf2c is for f2c's version of Fortran I/O: we assume
# $(cport) was compiled by f2c and cc.
Lp = $(cport) $(Lf2c)

FC = fc
FFLAGS =
# Fortran compiler (fc script provided with f2c)

.f.o:
	$(FC) -c $(FFLAGS) $*.f

A = $S/amplsolver.a
Af = $S/funcadd0.o $A

all1 = evalchk lin1 lin2 lin3 linrc dualconv nlcopy qtest et
all = mng1 fmng1 nl21 fnl21 mng mnh nl2 tn v8 ve08

all: $(all1) $(all)

# "make all" requires various routines from netlib's opt and port
# directories; "make all1" requires just files distributed with
# this directory.

all1: $(all1)

evalchk = evalchk.c $A
evalchk: $(evalchk)
	$(CC) $(CFLAGS) -o evalchk $(evalchk) $L

lin1 = lin1.c $(Af)
lin1: $(lin1)
	$(CC) $(CFLAGS) -o lin1 $(lin1)

lin2 = lin2.c $(Af)
lin2: $(lin2)
	$(CC) $(CFLAGS) -o lin2 $(lin2)

lin3 = lin3.c $(Af)
lin3: $(lin3)
	$(CC) $(CFLAGS) -o lin3 $(lin3)

linrc = linrc.c $(Af)
linrc: $(linrc)
	$(CC) $(CFLAGS) -o linrc $(linrc)

mng1 = mng1.c $A
mng1: $(mng1)
	$(CC) $(CFLAGS) -o mng1 $(mng1) $(Lp) $L

nl21 = nl21.c $A
nl21: $(nl21)
	$(CC) $(CFLAGS) -o nl21 $(nl21) $(Lp) $L

mng = mng.c rvmsg.o keywds.o $A
mng: $(mng)
	$(CC) $(CFLAGS) -o mng $(mng) $(Lp) $L

mnh = mnh.c rvmsg.o hkeywds.o $A
mnh: $(mnh)
	$(CC) $(CFLAGS) -o mnh $(mnh) $(Lp) $L

nl2 = nl2.c rvmsg.o keywds.o $A
nl2: $(nl2)
	$(CC) $(CFLAGS) -o nl2 $(nl2) $(Lp) $L

fmng1 = fmng1.o $A
fmng1: $(fmng1)
	$(CC) -o fmng1 $(fmng1) $(Lp) $L

fnl21 = fnl21.o $A
fnl21: $(fnl21)
	$(CC) -o fnl21 $(fnl21) $(Lp) $L

# tn.f and blas.f are available from netlib: ask for "tn from opt".

tn = tnmain.o tn.o blas.o $A
tn: $(tn)
	$(CC) -o tn $(tn) $(Lf2c) $L

# ve08ad.f is available from netlib: ask for "ve08 from opt".
# See comments in README about a possible adjustment to ve08ad.f .

v8.o: ve08.c
v8 = v8.o ve08ad.o $A
v8: $(v8)
	$(CC) $(CFLAGS) -o v8 $(v8) $(Lp) $L

ve08 = ve08.o ve08ad.o $A
ve08: $(ve08)
	$(CC) -o ve08 $(ve08) $(Lf2c) $L

dualconv = dualconv.c $(Af)
dualconv: $(dualconv)
	$(CC) $(CFLAGS) -o dualconv $(dualconv)

nlcopy = nlcopy.c $A
nlcopy: $(nlcopy)
	$(CC) $(CFLAGS) -o nlcopy $(nlcopy) $L

qtest = qtest.c $(Af)
qtest: $(qtest)
	$(CC) $(CFLAGS) -o qtest $(qtest) $L

et: et.c $A
	$(CC) $(CFLAGS) -o et et.c $A $L

$S/amplsolver.a:
	cd $S; make amplsolver.a

clean:
	rm -rf $(all1) $(all) *.o dietd.duw *.mex* t bad bad1 hist

# make xsum.out to check for transmission errors.
# This assumes you have the xsum program, whose source
# you can get by asking research!netlib to
#	send xsum.c from f2c/src

xs0 = README README.gjh amplfun4.c amplfunc.c bspline.c ch3.amp ch3.nl\
 ch3mng.out ch3mng.sol ch3mnh.out ch3mnh.sol ch3nl2.out ch3nl2.sol\
 ch3nl21.out ch3qtest.out ch3tn.out ch3tn.sol cport.zip diet.amp\
 diet.col diet.nl diet.row diet.sol dietd.nl dietd.sol dietl1.out\
 dietl2.out dietl3.out dietu.sol dminos dualconv.c enewt.m et.c et.in\
 et.out1 evalchk.c evalf.m evalg.m evalw.m fmng1.f fnl21.f gjh.c hist.c\
 hkeywds.c hs100.amp hs100.nl hs100.so0 init.m keywds.c lin1.c\
 lin1diet.out lin2.c lin2diet.out lin3.c lin3diet.out linrc.c\
 linrcdie.out makefile.u makefile.vc mng.c\
 mng1.c mnh.c nl2.c nl21.c nlcopy.c qtest.c rvmsg.c rvmsg.h savesol.m\
 spamfun4.c spamfunc.c tnmain.c v8.c v8ch3.out ve08.c ve08ch3.out

xsum.out: $(xs0)
	xsum $(xs0) >xsum1.out
	cmp xsum0.out xsum1.out && mv xsum1.out xsum.out || diff xsum[01].out

### "make test" to compare outputs; equivalent to "make test1 test2".

### "make test1" to compare outputs that do not require things
### from other netlib directories.

### Anything left in directory "bad" or "bad1" may indicate something
### amiss.  Trivial differences in function and gradient values in the
### *.out files (particularly ve08ch3.out) may be due to differences in
### special-function implementations among math libraries.  Other
### differences are cause for concern.

test: test1 test2

test1: all1
	mkdir bad1
	lin1 diet >foo
	cmp foo lin1diet.out || mv foo bad1/lin1diet.out
	lin2 diet >foo
	cmp foo lin2diet.out || mv foo bad1/lin2diet.out
	lin3 diet >foo
	cmp foo lin3diet.out || mv foo bad1/lin3diet.out
	linrc diet >foo
	cmp foo linrcdie.out || mv foo bad1/linrcdie.out
	dualconv diet foo
	cmp foo.nl dietd.nl || mv foo.nl bad1/dietd.nl
	mv foo.duw dietd.duw
	dualconv -u dietd foo
	cmp foo.sol dietu.sol || mv foo.sol bad1/dietu.sol
	rm -f foo.nl foo.sol dietd.duw
	qtest ch3 >foo
	cmp foo ch3qtest.out || mv foo bad1/ch3qtest.out
	mkdir t
	nlcopy -g hs100 t/hs100
	cmp hs100.nl t/hs100.nl
	rm -rf t
	et -ft et.in >foo
	cmp foo et.out1 || mv foo bad1/et.out1
	rmdir bad1

test2: all
	mkdir bad
	mng ch3 >foo 2>&1
	cmp foo ch3mng.out || mv foo bad/ch3mng.out
	mnh ch3 >foo 2>&1
	cmp foo ch3mnh.out || mv foo bad/ch3mnh.out
	nl2 ch3 >foo 2>&1
	cmp foo ch3nl2.out || mv foo bad/ch3nl2.out
	nl21 ch3 >foo 2>&1
	cmp foo ch3nl21.out || mv foo bad/ch3nl21.out
	tn ch3 >foo 2>&1
	cmp foo ch3tn.out || mv foo bad/ch3tn.out
	mng ch3 -AMPL
	cmp ch3.sol ch3mng.sol || mv ch3.sol bad/ch3mng.sol
	mnh ch3 -AMPL
	cmp ch3.sol ch3mnh.sol || mv ch3.sol bad/ch3mnh.sol
	nl2 ch3 -AMPL
	cmp ch3.sol ch3nl2.sol || mv ch3.sol bad/ch3nl2.sol
	tn ch3 -AMPL
	cmp ch3.sol ch3tn.sol || mv ch3.sol bad/ch3tn.sol
	rm -f ch3.sol
	v8 ch3 >foo 2>&1
	if cmp foo v8ch3.out && cmp foo v8ch3nu.out; then mv foo bad/v8ch3.out; fi
	ve08 ch3 >foo 2>&1
	cmp foo ve08ch3.out || mv foo bad/ve08ch3.out
	rmdir bad
	echo tests done.

## For use with MATLAB "make amplfunc.mex" or "make spamfunc.mex".
## Make sure your PATH includes MATLAB's "bin" directory.
## For Octave, change "mex" to "mkoctfile --mex" in the amplfunc.mex
## and spamfunc.mex rules below.  On some (Linux) systems,
## amplsolver.a may need to be compiled with -fPIC .

af = amplfunc.c $S/funcaddr.o $A

amplfunc.mex: $(af)
	mex -I$S $(af)
	touch amplfunc.mex

saf = spamfunc.c $S/funcaddr.o $A

spamfunc.mex: $(saf)
	mex -I$S $(saf)
	touch spamfunc.mex

$S/funcaddr.o: $S/funcadd1.c
	cd $S; make funcaddr.o

## Variants for MATLAB 4.x: "make amplfun4.mex" or "make spamfun4.mex".

af4 = amplfun4.c $A

amplfun4.mex: $(af4)
	cmex -I$S $(af4)
	touch amplfun4.mex

saf4 = spamfun4.c $A

spamfun4.mex: $(saf4)
	cmex -I$S $(saf4)
	touch spamfun4.mex

gjh: gjh.c
	$(CC) $(CFLAGS) -o gjh gjh.o $A $L

# The "hist" program provides bash-style history to any program that reads stdin.
# Prior to 20120214, it was convenient to use "hist" with AMPL.
# On Unix-like systems, including Linux and MacOSX, versions of AMPL starting
# with 20120214 have a builtin history mechanism.

# To make "hist", you must have the GNU history and readline packages installed.
# When using "hist" with "ampl", invoke "hist ampl -vi2 ..." to see prompts.
hist: hist.c
	$(CC) $(CFLAGS) -o hist hist.c -lhistory -lreadline
