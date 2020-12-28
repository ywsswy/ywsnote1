autoconf automake libtool gettext



用户：
./configure
make
make check
sudo make install
make installcheck


‘make all’ Build programs, libraries, documentation, etc.(Same as ‘make’.)
‘make clean’ Erase what has been built (the opposite of ‘make all’).

‘make install’ Install what needs to be installed.
‘make install-strip’ Same as ‘make install’, then strip debugging symbols.
‘make uninstall’ The opposite of ‘make install’.

‘make distclean’ Additionally erase anything ‘./configure’ created.
‘make check’ Run the test suite, if any.
‘make installcheck’ Check the installed programs or libraries, if supported.
‘make dist’ Create PACKAGE-VERSION.tar.gz.
