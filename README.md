What am I looking at?
=====================

* This is the source code of the Debian Linux kernel from backports.org versioned 3.2.23-1~bpo60+2.
* The upstream branch is pristine 3.2.23.
* The debian branch has all debian fixes that are in 3.2.23-1~bpo60+2.
* The Byte branch has grsec enabled and has a couple of Debian patches removed.

How do I use this?
==================

* Take a i386 host.
* Install prerequisites:
  * `apt-get install build-essential gcc-4.4 devscripts fakeroot quilt`
  * `apt-get build-dep linux-2.6`
* Fetch the Linux sources. Switch to branch `byte`.
  * `git clone https://github.com/ByteInternet/linux-3.2.23-grsec`
  * `git checkout byte`
* Apply the Debian patches. This also applies grsec.
  * `QUILT_PATCHES=debian/patches/ quilt push -a`
* Compile the packages that you need
  * Take a look at the [Debian Guide](http://kernel-handbook.alioth.debian.org/ch-common-tasks.html#s4.2.5) if you run into problems
  * Use the following command: `fakeroot make -f debian/rules.gen binary-arch_i386_grsec_686-pae binary-arch_i386_grsec_real`
  * Add -j 4 to the make command if you have multiple CPUs to spend on the compile.
* Install the .deb files that are the result, test them, put them in your repo.

How do I change config options?
===============================

You should change the config of the resulting kernel in the Byte branch.

* The general kernel options are in `debian/config/config`.
* Snippets in `debian/config/` override values or add values.
* The grsec and PAX config options are at the end of `debian/config/config`.
* Edit them there, commit, compile and test.


How do I add a newer version of the kernel?
===========================================

* Import the kernel sources into the upstream branch
* Merge the sources into the debian branch
* Import the new Debian patches into the debian branch
* Rebase the Byte changes onto the new debian branch
* Put the new grsec patch in debian/patches/
* Update debian/patches/series to apply the new grsec patch
  * Make sure to put the grsec patch first, because it is a big one. The small Debian patches are easier to update.

Caveats when updating:
* There are a boatload of patches.
* Most of them are relevant and are needed.
* Some only apply to sparc or armel.
* Some are things that grsec patches as well. Toss the Debian patch.
* If you need to use a featureset (e.g. rt), then update the featureset patches as well. This is currently not done.
* If you do not need to use the real-time featureset, make sure it is disabled in debian/rules.gen: 64f64ab8ec809a6ff857d31fded79bad0067d672

