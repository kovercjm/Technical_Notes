# 1 Setup KMS
## 1.1 Download vlmcsd
The latest vlmcsd is released on [GitHub](https://github.com/Wind4/vlmcsd/releases/tag/svn1112). Download the latest version, like below.
```
wget https://github.com/Wind4/vlmcsd/releases/download/svn1112/binaries.tar.gz
```

## 1.2 Release the binary
Copy the binary according to the system, and run it.
```
scp vlmcsd-x64-musl-static /opt/kms
cd /opt
chmod u+x kms
/opt/kms
```
Check with the below command for kms threads.
```
ps -ef | grep vlmcsd-x64-musl-static
```
If there is thread(s) named as vlmcsd-x64-musl-static running, then the installation is finished.

# 2 Active Windows
## 2.1 Holding proper puclic key
If Widows has the proper public key for activing, run the following commands with Administator CMD.
```
slmgr.vbs /skms [VPS IP]
slmgr.vbs /ato
```

## 2.2 Input proper public key
If there is no proper key holding, choose a proper key from the following table.
Operating system edition | KMS Client Setup Key
-------------------------|---------------------
Windows 10 Pro | W269N-WFGWX-YVC9B-4J6C9-T83GX
Windows 10 Pro Workstations | NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J
Windows 10 Pro Education | 6TP4R-GNPTD-KYYHQ-7B7DP-J447Y
Windows 10 Education | NW6C2-QMPVW-D7KKK-3GKT6-VCFB2
Windows 10 Enterprise | NPPR9-FWDCX-D2C8J-H872K-2YT43
Windows 8.1 Pro | GCRJD-8NW9H-F2CDX-CCM8D-9D6T9
Windows 8.1 Enterprise | MHF9N-XY6XB-WVXMC-BTDCT-MKKG7
Windows 8 Pro | NG4HW-VH26C-733KW-K6F98-J8CK4
Windows 8 Enterprise | 32JNW-9KQ84-P47T8-D8GGY-CWCK7
Windows 7 Professional | FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4
Windows 7 Enterprise | 33PXH-7Y6KF-2VJC9-XBBR8-HVTHH
Update the key and active as chapter 2.1.
```
slmgr.vbs /upk
slmgr.vbs /ipk *****-*****-*****-*****-*****
```

# 3 Active Office 2019
## 3.1 Proper installation version
The Office must be VOL version to active via KMS. The Office 2019 VOL can be installed by [Office Deployment Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49117). The configuration file can be set [online](https://config.office.com/deploymentsettings) and customizes the installation by the following commands.
```
.\setup.exe /download .\configuration.xml
.\setup /configure .\configuration.xml
```

## 3.2 Active with GVLK key
Choose proper GVLK key from the below table and input into an Office product.
Product | GVLK
--------|-----
Office Professional Plus  | NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP 
Office Standard 2019 | 6NWWJ-YQWMR-QKGCB-6TMB3-9D9HK 
Project Professional 2019 Preview | B4NPR-3FKK7-T2MBV-FRQ4W-PKD2B
Project Standard 2019 | C4F7P-NCP8C-6CQPT-MQHV9-JXD2M
Visio Professional 2019 | 9BGNQ-K37YR-RQHF2-38RQ3-7VCBB 
Visio Standard 2019 | 7TQNQ-K3YQQ-3PFH7-CCPPM-X4VQ2
Access 2019 | 9N9PT-27V4Y-VJ2PD-YXFMF-YTFQT 
Excel 2019 | TMJWT-YYNMB-3BKTF-644FC-RVXBD
Outlook 2019 | 7HD7K-N4PVK-BHBCQ-YWQRW-XW4VK 
PowerPoint 2019 | RRNCX-C64HY-W2MM7-MCH9G-TJHMQ 
Publisher 2019 | G2KWX-3NW6P-PY93R-JXK2T-C9Y9V
Skype for Business | NCJ33-JHBBY-HTK98-MYCV8-HMKHJ 
Word 2019 | PBX3G-NWMT6-Q7XBW-PYJGG-WXD33 
Active with the following commands.
```
cd /d "%ProgramFiles%\Microsoft Office\Office16"
cscript ospp.vbs /sethst:[VPS IP]
cscript ospp.vbs /act
cscript ospp.vbs /dstatus
```

# 4 Reference
Official document links:

 - Windows KMS client keys:  https://docs.microsoft.com/zh-cn/windows-server/get-started/kmsclientkeys
 - Office GVLK keys: https://docs.microsoft.com/zh-cn/DeployOffice/vlactivation/gvlks