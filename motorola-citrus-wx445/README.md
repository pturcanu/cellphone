Droidwall modules for Motorola Citrus WX445

Runs on Qualcomm MSM7625 ARMv6 - use kernel/msm


uname -a
Linux localhost 2.6.29-perf #2 PREEMPT Tue Nov 9 15:54:07 CST 2010 armv6l GNU/Linux


sudo apt-get install libncurses5-dev

git clone http://android.git.linaro.org/git/kernel/common.git
cd common/
git br -a
git co -t remotes/origin/archive/android-2.6.29 -b android-2.6.29
cd ..

git clone http://android.git.linaro.org/git/kernel/msm.git
cd msm/
git br -a
git co -t remotes/origin/archive/android-msm-2.6.29 -b android-msm-2.6.29

export ARCH=arm
export SUBARCH=arm
export CROSS_COMPILE=~/android/wx445/prebuilt/linux-x86/toolchain/arm-eabi-4.4.0/bin/arm-eabi-
make distclean
make msm_defconfig OR make ARCH=arm CROSS_COMPILE=/home/dvr/android/wx445/prebuilt/linux-x86/toolchain/arm-eabi-4.4.0/bin/arm-eabi- msm_defconfig
make menuconfig
update .config with 
CONFIG_LOCALVERSION="-perf"
CONFIG_LOCALVERSION_AUTO=n
CONFIG_NETFILTER_XT_MATCH_OWNER=m
CONFIG_IP_NF_TARGET_REJECT=m
CONFIG_IP_NF_TARGET_LOG=m
vi include/config/kernel.release <== 2.6.29-perf
make OR make ARCH=arm CROSS_COMPILE=~/android/wx445/prebuilt/linux-x86/toolchain/arm-eabi-4.4.0/bin/arm-eabi- uImage
make modules (this make target makes the ko files)

output => /net/netfilter/xt_owner.ko
net/ipv4/netfilter/ipt_REJECT.ko
net/ipv4/netfilter/ipt_LOG.ko

copy to phone (root explorer or adb)
insmod xt_owner.ko
insmod ipt_REJECT.ko
insmod ipt_LOG.ko

save to init script

