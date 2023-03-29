# RZ-BSP

Renesas RZ BSP environment setup (experimental).

Please note that Panfrost is not part of the official Renesas BSP package.

## Instructions

Clone this particular branch with the following command:

	git clone --recursive -b dunfell/rzg2l_updated https://github.com/renesas-rz/rz-bsp.git

There are two options to configure and setp the environment afterwards:

### 1. Using the Setup script:
You can use the setup script: bsp_setup.sh:

	USAGE:
	source bsp_setup.sh  [-h] [-b Board] [-v BSP-version] [-p panfrost_enable]
	Set's up the yocto environment for building a BSP Image
	where:
	
	    -h  show this help text

	    -b  board for which you want build the BSP
		rzg2l rzg2lc rzg2ul rzv2l rzg2h rzg2m rzg2n ek874

	    -v  version of BSP (default BSP-3.0.0)
		BSP-3.0.0 BSP-3.0.0-update1 BSP-3.0.0-update2
		BSP-3.0.1 BSP-3.0.2 BSP-3.0.2-update1

	    -p  enable panfrost graphics for supported boards (default disabled)
		Supported boards: rzg2l rzg2lc rzv2l
	
The script will setup the environment for the image build and you can the build the image of your choice with the command:

	bitbake <target>

Common targets are:   
* core-image-minimal  
* core-image-bsp  
* core-image-qt  
* core-image-weston (for graphics)  
	
### 2. Manual Method:  
1. Setup the environment with the following command: 
	source poky/oe-init-build-env  

2. Checkout the tag for the relevant BSP version. The supported versions are:   
   `BSP-3.0.0 BSP-3.0.0-update1 BSP-3.0.0-update2 BSP-3.0.1 BSP-3.0.2 BSP-3.0.2-update1`
   
   Use the following command to update the submodules:  
   `git submodule update --init --recursive`
	
3. Prepare the configuration files for the target board. Run the commands below in build directory. Please replace _**board**_ by the name below:  

	HiHope RZ/G2H board: hihope-rzg2h  
	HiHope RZ/G2M board: hihope-rzg2m  
	HiHope RZ/G2N board: hihope-rzg2n  
	EK874 RZ/G2E board: ek874  
	RZ/G2L Evaluation Board Kit PMIC version: smarc-rzg2l  
	RZ/G2LC Evaluation Board Kit: smarc-rzg2lc  
	RZ/G2UL Evaluation Board Kit: smarc-rzg2ul  
	RZ/V2L Evaluation Board Kit: smarc-rzv2l
	
	`cp ../meta-renesas/docs/template/conf/<board>/*.conf ./conf/`

4. If you want to add support for panfrost, please run the following command within the build directory:  
	`bitbake-layers add-layer meta-rz-panfrost`  
	
   Afterwards, add the followng to build/conf/local.conf:  
   
   	`PACKAGECONFIG_append_pn-mesa = " egl kmsro panfrost"`
	
	`IMAGE_INSTALL_append += " mesa weston kmscube"`

5. Build the image of your choice with the command:  

	`bitbake <target>`
	
   Common targets are:  
	* core-image-minimal  
	* core-image-bsp  
	* core-image-qt  
	* core-image-weston (for graphics)  
