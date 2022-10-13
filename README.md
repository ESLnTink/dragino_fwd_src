# 1. Introduce.
SX1301/SX1302/SX1308 LoRaWAN concentrator driver. For devices:
- Armbian Platform: HP0A, HP0C, LPS8v2. 
- Raspberry Pi Platform: HP0D
- OpenWrt Platform: LG308, LG308N, LPS8, LPS8N, LIG16, DLOS8, DLOS8N.

# 2. How to Compile

## Requirements:
1. sudo apt install -y libsqlite3-dev
2. sudo apt install -y libftdi1-dev

## 2.1 For Debian Platform

###Compile:
1. Git Clone from: git clone https://github.com/dragino/dragino_fwd_src.git 
2. Enter into dragino_fwd_src/src, type command: ./hp0z-make-deb.sh  c
   the option 'c' mean you will compile for HP0C 
   the option 'd' mean you will compile for HP0D 
3. Wait until you get the *.deb package
4. Run 'dpkg -i' for install 

Reference Link: [Armbian Compile Instruction](http://wiki1.dragino.com/xwiki/bin/view/Main/Armbian%20OS%20instruction/#HHowtorecompileLoRaWANConcentratorDriver28dragino-fwdpackage29.)


## 2.2 For Raspberry Pi Platform

### download the source: git clone https://github.com/dragino/dragino_fwd_src.git

make if with sx1301/sx1308
```
cd dragino_fwd_src
make clean
./hp0z-make-deb.sh  r1
```

make if with sx1302
```
cd dragino_fwd_src
make clean
./hp0z-make-deb.sh  r2
```

### After build succeed, install debian package . 
```
dpkg -i draginofwd-($board)_($version).deb
```
$board will be rasp301 or rasp302
$version will be xxxx-xx-xx , which is current date.

## 2.3 For OpenWrt Platform
See [OpenWrt compile instruction](https://github.com/dragino/openwrt_lede-18.06#how-to-develop-a-c-software-before-build-the-image)



# 3. History Before move to this github
## 2021.12.16
file: fwd/src/pkt_serv.c
remove element from dn_list when size biger than 16
remove element from dn_list when devaddr duplicated 
change the loop time of read_dir 

file: fwd/src/mac-header-decode.c 
fix bug of macMsg elemen

## 2022/02/10
file:fwd/inc/gwcfg.h
change struct serv_net_s addr leng to 256

## 2022/04/24  
version:dragino-fwd-2.3
file:/fwd/src/pkt_serv.c
1.Fix typo for Class C downlink in ABP mode. 
2.Fix FCNT initiate issue for Manually ABP Downlink.

## 2022/05/26  
Move all SX1302 src for different platform (hp0x series, pi series, lgxxx series.)  to the same github
hp0x use new station, 
lg0x series use old station


## 2022/05/26  
Fix MAX definition error in utilities and parson.h

## 2022/05/31
add support sx1301 chips

## 2022/05/31
1. add cfg-301 cfg-302 cfg-308
2. fix sx1301 chips error
3. change compile option: if raspberry, build with sx1301 use option 'r1', build with sx1302 use 'r2'

## 2022/06/06
1. change gps time's  log level to timesync in fwd.c

## 2022/06/08
1. add PATH env in postinst script

## 2022/06/13
1. remove i2c link in postinst script
2. copy sx1302 include file loragw_i2c.h to loragw_i2c.h.rasp when using board hp0d or rasp
3. add config option: td_enabled(ture/false), time_diff(+-number) strings format (3 chars). 

## 2022/06/20  (release: dragino_gw_fwd_2.7.0-1_mips_24kc.ipk )
1. lbt function add
   configure:  
   1. lbt_tty_enabled : true/false
   2. lbt_tty_path :  example /dev/ttyUSB4
   3. lbt_tty_baude(option) : default 9600
   4. lbt_rssi_target(option) : default -85
   5. lbt_scan_time_ms(option) : default 6ms
2. lbt test utily:  lbt_test_utily
   useage: lbt_test_utily /dev/ttyUSB4 923200000
           This will be send AT command to ttyUSB4, 10 loops every 1ms
   run lbt_test_utily display how to useage.

usb module can only make 60ms scan loop.

   
3. localtime zone
    confiure:
    1. td_enabled : true/false
    if td_enabled, will be attach 3 characters to payload ( example: +08 / -08 )

## 2022/07/05  (release: dragino_gw_fwd_2.7.1-1_mips_24kc.ipk )
1. fix bug can't get counter time form concentor when use sx1302

## 2022/07/11  (release: dragino_gw_fwd_2.7.1-1_mips_24kc.ipk )
1. fix bug  rssi and snr not equal when macdecode: change rssi_snr[18] to rssi_snr[32]
2. add cfg-30? configure file to config path

## 2022/07/14  (release: dragino_gw_fwd_2.7.2-0_mips_24kc.ipk )
1. add time section for rxpkt when timer_ref by system  (semtech_serv.c)

## 2022/07/22  (release: dragino_gw_fwd_2.7.3-0_mips_24kc.ipk )
1. fix bug of fport filter  (65536 to 0)

## 2022/09/08 
1. chan fwd VERSION to 2.1.0

## 2022/09/26
1. custom downlink add fport control
2. chan fwd VERSION to 2.7.5

## 2022/00/13
1. basicstaion version upgrade to V2.0.6 (openwrt platform)
