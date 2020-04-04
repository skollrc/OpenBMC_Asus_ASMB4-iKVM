# OpenBMC_Asus_ASMB4-iKVM

## Official documentation
READ THIS DOC FIRST https://www.raptorengineering.com/coreboot/kgpe-d16-bmc-port-status.php

## Step by step procedure with some modifications due to problems I have had

#### Setp 1: clone git repos
Use github repos instead of repos mentioned in the doc:
* <code>git clone -b fido https://github.com/raptor-engineering/ast2050-yocto-poky.git ast2050-yocto-poky</code>
* <code>cd ast2050-yocto-poky</code>
* <code>git clone -b fido https://github.com/raptor-engineering/ast2050-yocto-openembedded.git meta-openembedded</code>
* <code>git clone https://github.com/raptor-engineering/ast2050-yocto-openbmc.git meta-openbmc</code>

#### Step 2 : modify/add some files
* local-aspeed.conf --> modify default hashed password ("0penBmc") by your preferd password (clear text)
* For more info, see this page: https://www.yoctoproject.org/docs/1.8/ref-manual/ref-manual.html

*/etc/perl/find.pl --> create the file with this content : https://blog.kempj.co.uk/2015/09/missing-find-pl-compiling-oe/ 
* (or dl it from this repo)

* edit sources/poky/bitbake/lib/bs4/builder/_html5lib.py and replace all instances of "html5lib.treebuilders._base" with "html5lib.treebuilders.base". 
* --> If you're using an html5lib version > 0.9999999 (seven nines), then you'll see this problem.
* more infos here: https://community.nxp.com/thread/455173 (last post)

* edit ast2050-yocto-poky/meta-openbmc/common/recipes-utils/flashrom/flashrom_0.9.8.bb 
* --> replace http by https for the first encouterd link

#### Step 3: prepare build
* <code>export TEMPLATECONF=meta-openbmc/meta-raptor/meta-asus/conf</code>
* <code>source oe-init-build-env</code>
* <code>cp -Rp ../local-aspeed.conf conf/local.conf</code>

#### Step 4: build (take a while)
* <code>bitbake asus-image</code>

#### Setp 5: flash
Use specific asmb4 flashrom version
* <code> git clone https://github.com/raptor-engineering/flashrom </code>
* <code> flashrom --programmer="ast1100:spibus=2,cpu=reset" -c "S25FL128P......0" --verbose -w <location of the flash-asus- ROM file></code>
