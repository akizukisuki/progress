參考：https://github.com/cturra/docker-ntp
"NTP Server running in a Docker container"
```shell
root@raspberrypi:/home/pi/docker-ntp# docker pull cturra/ntp
Using default tag: latest
latest: Pulling from cturra/ntp
fb5dbe6f601f: Pull complete 
3e3f42a98e9e: Pull complete 
b9b11971468b: Pull complete 
b0a0ba8c5b3b: Pull complete 
98499cef5a73: Pull complete 
9e8a9c65b732: Pull complete 
Digest: sha256:f7fc025d729090fb9c8a6b2c9e6c04f3dda6543699c56dfa663827f0f2542409
Status: Downloaded newer image for cturra/ntp:latest
```
```shell
root@raspberrypi:/home/pi/docker-ntp# docker run --name=ntp             \
>               --restart=always       \
>               --detach=true          \
>               --publish=123:123/udp  \
>               --cap-add=SYS_NICE     \
>               --cap-add=SYS_RESOURCE \
>               --cap-add=SYS_TIME     \
>               cturra/ntp
WARNING: Your kernel does not support memory swappiness capabilities, memory swappiness discarded.
7519c167f62081fd72e9f13d213ff7db1af26a835dfd7807780b9b82557a004b
Error response from daemon: Cannot start container 7519c167f62081fd72e9f13d213ff7db1af26a835dfd7807780b9b82557a004b: Error starting userland proxy: listen udp 0.0.0.0:123: bind: address already in use
```
用了以下辦法處理swap memory 的問題  參考： https://github.com/moby/moby/issues/4250
```shell
TL;DR: add cgroup_enable=memory swapaccount=1 to kernel boot options to enable both swap and memory accounting
/etc/default/grub:

GRUB_CMDLINE_LINUX_DEFAULT="cgroup_enable=memory swapaccount=1"
then sudo grub-update && sudo reboot
```
發現grub並不存在，因此在此安裝  參考： https://www.gnu.org/software/grub/grub-download.html
```shell
pi@raspberrypi:~ $ git clone git://git.savannah.gnu.org/grub.git
Cloning into 'grub'...
remote: Counting objects: 92806, done.
remote: Compressing objects: 100% (21328/21328), done.
remote: Total 92806 (delta 69176), reused 92806 (delta 69176)
Receiving objects: 100% (92806/92806), 69.78 MiB | 349.00 KiB/s, done.
Resolving deltas: 100% (69176/69176), done.

pi@raspberrypi:~/grub $ npm install
npm WARN @google-cloud/logging-winston@0.9.0 requires a peer of winston@^2.x but none is installed. You must install peer dependencies yourself.
npm WARN pi@1.0.0 No description
npm WARN pi@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: xpc-connection@0.1.4 (node_modules/xpc-connection):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for xpc-connection@0.1.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"arm"})

audited 3389 packages in 20.446s
found 1 low severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
pi@raspberrypi:~/grub $ npm audit fix
npm WARN @google-cloud/logging-winston@0.9.0 requires a peer of winston@^2.x but none is installed. You must install peer dependencies yourself.
npm WARN pi@1.0.0 No description
npm WARN pi@1.0.0 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: xpc-connection@0.1.4 (node_modules/xpc-connection):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for xpc-connection@0.1.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"arm"})

up to date in 15.264s
fixed 0 of 1 vulnerability in 3389 scanned packages
  1 vulnerability required manual review and could not be updated

```
由於上述可發現會有warning的產生，因此參考http://cjworld1208.pixnet.net/blog/post/9567741-grub%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E8%AA%AA%E6%98%8E
的安裝模式，然而資源取的方法
```wget ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz```
```wget ftp://ftp.gnu.org/gnu/grub/grub-2.02.tar.xz.sig```
after entering the grub file
```shell
pi@raspberrypi:/usr/local/src/grub-2.02 $ ./configure
checking build system type... armv7l-unknown-linux-gnueabihf
checking host system type... armv7l-unknown-linux-gnueabihf
checking target system type... armv7l-unknown-linux-gnueabihf
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking for cmp... cmp
checking for bison... no
configure: error: bison is not found
```
error: bison is not found
解決辦法：
參考：https://geeksww.com/tutorials/miscellaneous/bison_gnu_parser_generator/installation/installing_bison_gnu_parser_generator_ubuntu_linux.php
```shell
pi@raspberrypi:~ $ wget http://ftp.gnu.org/gnu/bison/bison-2.3.tar.gz
--2018-11-04 17:47:58--  http://ftp.gnu.org/gnu/bison/bison-2.3.tar.gz
正在查找主機 ftp.gnu.org (ftp.gnu.org)... 208.118.235.20, 2001:4830:134:3::b
正在連接 ftp.gnu.org (ftp.gnu.org)|208.118.235.20|:80... 連上了。
已送出 HTTP 要求，正在等候回應... 200 OK
長度: 1386694 (1.3M) [application/x-gzip]
Saving to: ‘bison-2.3.tar.gz’

bison-2.3.tar.gz    100%[===================>]   1.32M   237KB/s    in 7.2s    

2018-11-04 17:48:09 (188 KB/s) - ‘bison-2.3.tar.gz’ saved [1386694/1386694]

pi@raspberrypi:~ $ tar -xvzf bison-2.3.tar.gz
bison-2.3/
bison-2.3/djgpp/
bison-2.3/djgpp/Makefile.maint
bison-2.3/djgpp/README.in
bison-2.3/djgpp/config.bat
bison-2.3/djgpp/config.sed
bison-2.3/djgpp/config.site
bison-2.3/djgpp/config_h.sed
bison-2.3/djgpp/subpipe.c
bison-2.3/djgpp/subpipe.h
bison-2.3/djgpp/djunpack.bat
bison-2.3/djgpp/fnchange.lst
bison-2.3/m4/
bison-2.3/m4/bison-i18n.m4
bison-2.3/m4/c-working.m4
bison-2.3/m4/cxx.m4
bison-2.3/m4/dirname.m4
bison-2.3/m4/dmalloc.m4
bison-2.3/m4/dos.m4
bison-2.3/m4/error.m4
bison-2.3/m4/exitfail.m4
bison-2.3/m4/extensions.m4
bison-2.3/m4/getopt.m4
bison-2.3/m4/gettext_gl.m4
bison-2.3/m4/gnulib.m4
bison-2.3/m4/hard-locale.m4
bison-2.3/m4/hash.m4
bison-2.3/m4/iconv.m4
bison-2.3/m4/inttypes_h_gl.m4
bison-2.3/m4/lib-ld_gl.m4
bison-2.3/m4/lib-link.m4
bison-2.3/m4/lib-prefix_gl.m4
bison-2.3/m4/m4.m4
bison-2.3/m4/mbrtowc.m4
bison-2.3/m4/mbstate_t.m4
bison-2.3/m4/mbswidth.m4
bison-2.3/m4/nls.m4
bison-2.3/m4/obstack.m4
bison-2.3/m4/onceonly.m4
bison-2.3/m4/po_gl.m4
bison-2.3/m4/progtest.m4
bison-2.3/m4/quote.m4
bison-2.3/m4/quotearg.m4
bison-2.3/m4/stdbool.m4
bison-2.3/m4/stdint_h_gl.m4
bison-2.3/m4/stdio-safer.m4
bison-2.3/m4/stpcpy.m4
bison-2.3/m4/strdup.m4
bison-2.3/m4/strerror.m4
bison-2.3/m4/strndup.m4
bison-2.3/m4/strnlen.m4
bison-2.3/m4/strtol.m4
bison-2.3/m4/strtoul.m4
bison-2.3/m4/strverscmp.m4
bison-2.3/m4/subpipe.m4
bison-2.3/m4/timevar.m4
bison-2.3/m4/uintmax_t_gl.m4
bison-2.3/m4/ulonglong_gl.m4
bison-2.3/m4/unistd-safer.m4
bison-2.3/m4/unistd_h.m4
bison-2.3/m4/unlocked-io.m4
bison-2.3/m4/warning.m4
bison-2.3/m4/xalloc.m4
bison-2.3/m4/xstrndup.m4
bison-2.3/po/
bison-2.3/po/Makefile.in.in
bison-2.3/po/remove-potcdate.sin
bison-2.3/po/quot.sed
bison-2.3/po/boldquot.sed
bison-2.3/po/en@quot.header
bison-2.3/po/en@boldquot.header
bison-2.3/po/insert-header.sin
bison-2.3/po/Rules-quot
bison-2.3/po/Makevars
bison-2.3/po/POTFILES.in
bison-2.3/po/bison.pot
bison-2.3/po/stamp-po
bison-2.3/po/da.po
bison-2.3/po/de.po
bison-2.3/po/es.po
bison-2.3/po/et.po
bison-2.3/po/fr.po
bison-2.3/po/ga.po
bison-2.3/po/hr.po
bison-2.3/po/id.po
bison-2.3/po/it.po
bison-2.3/po/ja.po
bison-2.3/po/ms.po
bison-2.3/po/nb.po
bison-2.3/po/nl.po
bison-2.3/po/pl.po
bison-2.3/po/pt_BR.po
bison-2.3/po/ro.po
bison-2.3/po/ru.po
bison-2.3/po/rw.po
bison-2.3/po/sv.po
bison-2.3/po/tr.po
bison-2.3/po/vi.po
bison-2.3/po/da.gmo
bison-2.3/po/de.gmo
bison-2.3/po/es.gmo
bison-2.3/po/et.gmo
bison-2.3/po/fr.gmo
bison-2.3/po/ga.gmo
bison-2.3/po/hr.gmo
bison-2.3/po/id.gmo
bison-2.3/po/it.gmo
bison-2.3/po/ja.gmo
bison-2.3/po/ms.gmo
bison-2.3/po/nb.gmo
bison-2.3/po/nl.gmo
bison-2.3/po/pl.gmo
bison-2.3/po/pt_BR.gmo
bison-2.3/po/ro.gmo
bison-2.3/po/ru.gmo
bison-2.3/po/rw.gmo
bison-2.3/po/sv.gmo
bison-2.3/po/tr.gmo
bison-2.3/po/vi.gmo
bison-2.3/po/LINGUAS
bison-2.3/runtime-po/
bison-2.3/runtime-po/Makefile.in.in
bison-2.3/runtime-po/remove-potcdate.sin
bison-2.3/runtime-po/quot.sed
bison-2.3/runtime-po/boldquot.sed
bison-2.3/runtime-po/en@quot.header
bison-2.3/runtime-po/en@boldquot.header
bison-2.3/runtime-po/insert-header.sin
bison-2.3/runtime-po/Rules-quot
bison-2.3/runtime-po/Makevars
bison-2.3/runtime-po/POTFILES.in
bison-2.3/runtime-po/bison-runtime.pot
bison-2.3/runtime-po/stamp-po
bison-2.3/runtime-po/da.po
bison-2.3/runtime-po/de.po
bison-2.3/runtime-po/es.po
bison-2.3/runtime-po/et.po
bison-2.3/runtime-po/fr.po
bison-2.3/runtime-po/ga.po
bison-2.3/runtime-po/hr.po
bison-2.3/runtime-po/id.po
bison-2.3/runtime-po/it.po
bison-2.3/runtime-po/ja.po
bison-2.3/runtime-po/ms.po
bison-2.3/runtime-po/nb.po
bison-2.3/runtime-po/nl.po
bison-2.3/runtime-po/pl.po
bison-2.3/runtime-po/pt_BR.po
bison-2.3/runtime-po/ro.po
bison-2.3/runtime-po/ru.po
bison-2.3/runtime-po/rw.po
bison-2.3/runtime-po/sl.po
bison-2.3/runtime-po/sv.po
bison-2.3/runtime-po/tr.po
bison-2.3/runtime-po/vi.po
bison-2.3/runtime-po/zh_TW.po
bison-2.3/runtime-po/da.gmo
bison-2.3/runtime-po/de.gmo
bison-2.3/runtime-po/es.gmo
bison-2.3/runtime-po/et.gmo
bison-2.3/runtime-po/fr.gmo
bison-2.3/runtime-po/ga.gmo
bison-2.3/runtime-po/hr.gmo
bison-2.3/runtime-po/id.gmo
bison-2.3/runtime-po/it.gmo
bison-2.3/runtime-po/ja.gmo
bison-2.3/runtime-po/ms.gmo
bison-2.3/runtime-po/nb.gmo
bison-2.3/runtime-po/nl.gmo
bison-2.3/runtime-po/pl.gmo
bison-2.3/runtime-po/pt_BR.gmo
bison-2.3/runtime-po/ro.gmo
bison-2.3/runtime-po/ru.gmo
bison-2.3/runtime-po/rw.gmo
bison-2.3/runtime-po/sl.gmo
bison-2.3/runtime-po/sv.gmo
bison-2.3/runtime-po/tr.gmo
bison-2.3/runtime-po/vi.gmo
bison-2.3/runtime-po/zh_TW.gmo
bison-2.3/runtime-po/LINGUAS
bison-2.3/tests/
bison-2.3/tests/Makefile.am
bison-2.3/tests/Makefile.in
bison-2.3/tests/atlocal.in
bison-2.3/tests/bison.in
bison-2.3/tests/local.at
bison-2.3/tests/testsuite.at
bison-2.3/tests/input.at
bison-2.3/tests/output.at
bison-2.3/tests/sets.at
bison-2.3/tests/reduce.at
bison-2.3/tests/synclines.at
bison-2.3/tests/headers.at
bison-2.3/tests/actions.at
bison-2.3/tests/conflicts.at
bison-2.3/tests/calc.at
bison-2.3/tests/torture.at
bison-2.3/tests/existing.at
bison-2.3/tests/regression.at
bison-2.3/tests/c++.at
bison-2.3/tests/cxx-type.at
bison-2.3/tests/glr-regression.at
bison-2.3/tests/testsuite
bison-2.3/tests/package.m4
bison-2.3/README
bison-2.3/configure.ac
bison-2.3/aclocal.m4
bison-2.3/Makefile.am
bison-2.3/Makefile.in
bison-2.3/config.hin
bison-2.3/configure
bison-2.3/ABOUT-NLS
bison-2.3/AUTHORS
bison-2.3/COPYING
bison-2.3/ChangeLog
bison-2.3/INSTALL
bison-2.3/NEWS
bison-2.3/THANKS
bison-2.3/TODO
bison-2.3/GNUmakefile
bison-2.3/Makefile.cfg
bison-2.3/Makefile.maint
bison-2.3/OChangeLog
bison-2.3/PACKAGING
bison-2.3/build-aux/
bison-2.3/build-aux/Makefile.am
bison-2.3/build-aux/Makefile.in
bison-2.3/build-aux/config.guess
bison-2.3/build-aux/config.rpath
bison-2.3/build-aux/config.sub
bison-2.3/build-aux/depcomp
bison-2.3/build-aux/install-sh
bison-2.3/build-aux/mdate-sh
bison-2.3/build-aux/missing
bison-2.3/build-aux/mkinstalldirs
bison-2.3/build-aux/texinfo.tex
bison-2.3/build-aux/ylwrap
bison-2.3/build-aux/prev-version.txt
bison-2.3/lib/
bison-2.3/lib/Makefile.am
bison-2.3/lib/Makefile.in
bison-2.3/lib/gnulib.mk
bison-2.3/lib/dirname.c
bison-2.3/lib/dirname.h
bison-2.3/lib/dup-safer.c
bison-2.3/lib/error.c
bison-2.3/lib/error.h
bison-2.3/lib/exitfail.c
bison-2.3/lib/exitfail.h
bison-2.3/lib/fd-safer.c
bison-2.3/lib/fopen-safer.c
bison-2.3/lib/getopt.c
bison-2.3/lib/getopt1.c
bison-2.3/lib/hard-locale.c
bison-2.3/lib/hard-locale.h
bison-2.3/lib/hash.c
bison-2.3/lib/hash.h
bison-2.3/lib/malloc.c
bison-2.3/lib/obstack.c
bison-2.3/lib/obstack.h
bison-2.3/lib/pipe-safer.c
bison-2.3/lib/quote.c
bison-2.3/lib/quote.h
bison-2.3/lib/quotearg.c
bison-2.3/lib/quotearg.h
bison-2.3/lib/stdio--.h
bison-2.3/lib/stdio-safer.h
bison-2.3/lib/stpcpy.c
bison-2.3/lib/strdup.c
bison-2.3/lib/strdup.h
bison-2.3/lib/strerror.c
bison-2.3/lib/strndup.c
bison-2.3/lib/strndup.h
bison-2.3/lib/strnlen.c
bison-2.3/lib/strnlen.h
bison-2.3/lib/strtol.c
bison-2.3/lib/strtoul.c
bison-2.3/lib/strverscmp.c
bison-2.3/lib/strverscmp.h
bison-2.3/lib/unistd--.h
bison-2.3/lib/unistd-safer.h
bison-2.3/lib/unlocked-io.h
bison-2.3/lib/xalloc.h
bison-2.3/lib/xmalloc.c
bison-2.3/lib/get-errno.h
bison-2.3/lib/get-errno.c
bison-2.3/lib/subpipe.h
bison-2.3/lib/subpipe.c
bison-2.3/lib/abitset.c
bison-2.3/lib/abitset.h
bison-2.3/lib/bbitset.h
bison-2.3/lib/bitset.c
bison-2.3/lib/bitset.h
bison-2.3/lib/bitset_stats.c
bison-2.3/lib/bitset_stats.h
bison-2.3/lib/bitsetv.c
bison-2.3/lib/bitsetv.h
bison-2.3/lib/ebitset.c
bison-2.3/lib/ebitset.h
bison-2.3/lib/lbitset.c
bison-2.3/lib/lbitset.h
bison-2.3/lib/libiberty.h
bison-2.3/lib/vbitset.c
bison-2.3/lib/vbitset.h
bison-2.3/lib/bitsetv-print.h
bison-2.3/lib/bitsetv-print.c
bison-2.3/lib/timevar.h
bison-2.3/lib/timevar.c
bison-2.3/lib/timevar.def
bison-2.3/lib/argmatch.h
bison-2.3/lib/argmatch.c
bison-2.3/lib/basename.c
bison-2.3/lib/stripslash.c
bison-2.3/lib/exit.h
bison-2.3/lib/gettext.h
bison-2.3/lib/mbswidth.h
bison-2.3/lib/mbswidth.c
bison-2.3/lib/stpcpy.h
bison-2.3/lib/verify.h
bison-2.3/lib/xalloc-die.c
bison-2.3/lib/xstrndup.h
bison-2.3/lib/xstrndup.c
bison-2.3/lib/main.c
bison-2.3/lib/yyerror.c
bison-2.3/lib/getopt_.h
bison-2.3/lib/getopt_int.h
bison-2.3/lib/stdbool_.h
bison-2.3/data/
bison-2.3/data/m4sugar/
bison-2.3/data/m4sugar/m4sugar.m4
bison-2.3/data/README
bison-2.3/data/c.m4
bison-2.3/data/yacc.c
bison-2.3/data/glr.c
bison-2.3/data/c++.m4
bison-2.3/data/location.cc
bison-2.3/data/lalr1.cc
bison-2.3/data/glr.cc
bison-2.3/data/Makefile.am
bison-2.3/data/Makefile.in
bison-2.3/src/
bison-2.3/src/Makefile.am
bison-2.3/src/Makefile.in
bison-2.3/src/parse-gram.c
bison-2.3/src/scan-gram.c
bison-2.3/src/scan-skel.c
bison-2.3/src/LR0.c
bison-2.3/src/LR0.h
bison-2.3/src/assoc.c
bison-2.3/src/assoc.h
bison-2.3/src/closure.c
bison-2.3/src/closure.h
bison-2.3/src/complain.c
bison-2.3/src/complain.h
bison-2.3/src/conflicts.c
bison-2.3/src/conflicts.h
bison-2.3/src/derives.c
bison-2.3/src/derives.h
bison-2.3/src/files.c
bison-2.3/src/files.h
bison-2.3/src/getargs.c
bison-2.3/src/getargs.h
bison-2.3/src/gram.c
bison-2.3/src/gram.h
bison-2.3/src/lalr.h
bison-2.3/src/lalr.c
bison-2.3/src/location.c
bison-2.3/src/location.h
bison-2.3/src/main.c
bison-2.3/src/muscle_tab.c
bison-2.3/src/muscle_tab.h
bison-2.3/src/nullable.c
bison-2.3/src/nullable.h
bison-2.3/src/output.c
bison-2.3/src/output.h
bison-2.3/src/parse-gram.h
bison-2.3/src/parse-gram.y
bison-2.3/src/print.c
bison-2.3/src/print.h
bison-2.3/src/print_graph.c
bison-2.3/src/print_graph.h
bison-2.3/src/reader.c
bison-2.3/src/reader.h
bison-2.3/src/reduce.c
bison-2.3/src/reduce.h
bison-2.3/src/relation.c
bison-2.3/src/relation.h
bison-2.3/src/scan-gram-c.c
bison-2.3/src/scan-skel-c.c
bison-2.3/src/scan-skel.h
bison-2.3/src/state.c
bison-2.3/src/state.h
bison-2.3/src/symlist.c
bison-2.3/src/symlist.h
bison-2.3/src/symtab.c
bison-2.3/src/symtab.h
bison-2.3/src/system.h
bison-2.3/src/tables.h
bison-2.3/src/tables.c
bison-2.3/src/uniqstr.c
bison-2.3/src/uniqstr.h
bison-2.3/src/vcg.c
bison-2.3/src/vcg.h
bison-2.3/src/vcg_defaults.h
bison-2.3/src/scan-skel.l
bison-2.3/src/scan-gram.l
bison-2.3/doc/
bison-2.3/doc/gpl.texi
bison-2.3/doc/fdl.texi
bison-2.3/doc/Makefile.am
bison-2.3/doc/Makefile.in
bison-2.3/doc/stamp-vti
bison-2.3/doc/version.texi
bison-2.3/doc/bison.texinfo
bison-2.3/doc/bison.1
bison-2.3/doc/refcard.tex
bison-2.3/doc/bison.info
bison-2.3/examples/
bison-2.3/examples/extexi
bison-2.3/examples/Makefile.am
bison-2.3/examples/Makefile.in
bison-2.3/examples/calc++/
bison-2.3/examples/calc++/Makefile.am
bison-2.3/examples/calc++/Makefile.in
bison-2.3/examples/calc++/calc++-scanner.cc
bison-2.3/examples/calc++/calc++-scanner.ll
bison-2.3/examples/calc++/calc++.cc
bison-2.3/examples/calc++/calc++-driver.hh
bison-2.3/examples/calc++/calc++-driver.cc
bison-2.3/examples/calc++/stack.hh
bison-2.3/examples/calc++/position.hh
bison-2.3/examples/calc++/location.hh
bison-2.3/examples/calc++/calc++-parser.hh
bison-2.3/examples/calc++/calc++-parser.cc
bison-2.3/examples/calc++/calc++-parser.stamp
bison-2.3/examples/calc++/calc++-parser.yy
bison-2.3/examples/calc++/test
pi@raspberrypi:~ $ cd bison-2.3
pi@raspberrypi:~/bison-2.3 $ PATH=$PATH:/usr/local/m4/bin/
pi@raspberrypi:~/bison-2.3 $ ./configure --prefix=/usr/local/bison --with-libiconv-prefix=/usr/local/libiconv/
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking for style of include used by make... GNU
checking for gcc... gcc
checking for C compiler default output file name... a.out
checking whether the C compiler works... yes
checking whether we are cross compiling... no
checking for suffix of executables... 
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ANSI C... none needed
checking dependency style of gcc... gcc3
checking how to run the C preprocessor... gcc -E
checking for egrep... grep -E
checking for AIX... no
checking for ANSI C header files... yes
checking for sys/types.h... yes
checking for sys/stat.h... yes
checking for stdlib.h... yes
checking for string.h... yes
checking for memory.h... yes
checking for strings.h... yes
checking for inttypes.h... yes
checking for stdint.h... yes
checking for unistd.h... yes
checking minix/config.h usability... no
checking minix/config.h presence... no
checking for minix/config.h... no
checking whether it is safe to define __EXTENSIONS__... yes
checking for gcc... (cached) gcc
checking whether we are using the GNU C compiler... (cached) yes
checking whether gcc accepts -g... (cached) yes
checking for gcc option to accept ANSI C... (cached) none needed
checking dependency style of gcc... (cached) gcc3
checking for gcc... (cached) gcc
checking whether we are using the GNU C compiler... (cached) yes
checking whether gcc accepts -g... (cached) yes
checking for gcc option to accept ANSI C... (cached) none needed
checking dependency style of gcc... (cached) gcc3
checking for flex... no
checking for lex... no
checking for yywrap in -lfl... no
checking for yywrap in -ll... no
checking for bison... no
checking for byacc... no
checking for ranlib... ranlib
checking for gm4... no
checking for gnum4... no
checking for m4... no
checking whether m4 supports frozen files... no
configure: error: GNU M4 1.4 is required

```
