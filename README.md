weimarnetz - build mesh-networks _without_ pain
==========================================

* community: http://wireless.subsignal.org
* monitoring: http://intercity-vpn.de/networks/dhfleesensee/
* documentation: [API](http://wireless.subsignal.org/index.php?title=Firmware-Dokumentation_API)

needing support?
join the [club](http://blog.maschinenraum.tk) or ask for [consulting](http://bittorf-wireless.de)


how to build this from scratch on a debian server
-------------------------------------------------

	sudo apt-get update
	LIST="build-essential libncurses5-dev m4 flex git git-core zlib1g-dev unzip subversion gawk python libssl-dev quilt"
	for PACKAGE in $LIST; do sudo apt-get install $PACKAGE; done
	
	git clone git://nbd.name/openwrt.git
	git clone git://nbd.name/packages.git
	cd openwrt
	git clone git://github.com/andibraeu/weimarnetz.git
	
	make menuconfig				# simply select exit, (just for init)
	make package/symlinks
	
	weimarnetz/openwrt-build/mybuild.sh gitpull
	weimarnetz/openwrt-build/mybuild.sh select_hardware_model

	weimarnetz/openwrt-build/mybuild.sh set_build_openwrtconfig
	make menuconfig
	make kernel_menuconfig

	weimarnetz/openwrt-build/mybuild.sh set_build_kernelconfig
	weimarnetz/openwrt-build/mybuild.sh applymystuff "ffweimar" "adhoc" "42"
	weimarnetz/openwrt-build/mybuild.sh make 		# needs some hours
	
	# flash your image via TFTP
	FW="/path/to/your/baked/firmware_file"
	IP="your.own.router.ip"
	while :; do atftp --trace --option "timeout 1" --option "mode octet" --put --local-file $FW $IP && break; sleep 1; done
