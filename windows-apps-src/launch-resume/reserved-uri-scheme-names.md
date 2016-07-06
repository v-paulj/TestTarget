---
author: TylerMSFT
title: Reservierte Datei- und URI-Schemanamen
description: "Sie können URI-Zuordnungen verwenden, um Ihre App automatisch zu starten, wenn eine andere App ein bestimmtes URI-Schema startet."
ms.assetid: 7428C4A2-1380-4EBB-9C2A-7DF7B5C468AE
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 2736267bf72b1cadd2c635b15daf6975a70937df

---
# Reservierte Datei- und URI-Schemanamen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Sie können URI-Zuordnungen verwenden, um Ihre App automatisch zu starten, wenn eine andere App ein bestimmtes URI-Schema startet. Es gibt aber einige URI-Zuordnungen, die Sie nicht verwenden können, Zuordnungen, die reserviert sind. Wenn Ihre App für eine reservierte Zuordnung registriert wird, wird diese Registrierung ignoriert. Dieses Thema listet die reservierten Datei- und URI-Schemanamen auf, die für Ihre App nicht verfügbar sind.

## Reservierte Dateitypen


Es gibt zwei Arten von reservierten Dateitypen: Dateitypen, die für integrierte Apps reserviert sind, und Dateitypen, die für das Betriebssystem reserviert sind. Wenn Sie einen Dateityp starten, der für eine integrierte App reserviert ist, wird nur die integrierte App gestartet. Jeder Versuch, die App für diesen Dateityp zu registrieren, wird ignoriert. Entsprechend wird auch jeder Versuch, Ihre App für einen Dateityp zu registrieren, der für das Betriebssystem reserviert ist, ignoriert.

Dateitypen, die für integrierte Apps reserviert sind

| .aac  | .icon    | .pem  | .wdp   |
|-------|----------|-------|--------|
| .aetx | .jpeg    | .png  | .wmv   |
| .asf  | .jxr     | .pptm | .xap   |
| .bmp  | .m4a     | .pptx | .xht   |
| .cer  | .m4r     | .qcp  | .xhtml |
| .dotm | .m4v     | .rtf  | .xltm  |
| .dotx | .mov     | .tif  | .xltx  |
| .gif  | .mp3     | .tiff | .xml   |
| .hdp  | .mp4     | .txt  | .xsl   |
| .htm  | .one     | .url  | .zip   |
| .html | .onetoc2 | .vcf  |        |
| .ico  | .p7b     | .wav  |        |
 

## Dateitypen, die für das Betriebssystem reserviert sind


Die folgenden Dateitypen sind für das Betriebssystem reserviert:

| .accountpicture-ms | .its      | .ops           | .url      |
|--------------------|----------|----------------|-----------|
| .ade               | .jar     | .pcd           | .vb       |
| .adp               | .js      | .pif           | .vbe      |
| .app               | .jse     | .pl            | .vbp      |
| .appx              | .ksh     | .plg           | .vbs      |
| .application       | .lnk     | .plsc          | .vhd      |
| .appref-ms         | .mad     | .prf           | .vhdx     |
| .asp               | .maf     | .prg           | .vsmacros |
| .bas               | .mag     | .printerexport | .vsw      |
| .bat               | .mam     | .provxml       | .webpnp   |
| .cab               | .maq     | .ps1           | .ws       |
| .chm               | .mar     | .ps1xml        | .wsc      |
| .cmd               | .mas     | .ps2           | .wsf      |
| .cnt               | .mat     | .ps2xml        | .wsh      |
| .com               | .mau     | .psc1          | .xaml     |
| .cpf               | .mav     | .psc2          | .xdp      |
| .cpl               | .maw     | .psm1          | .xip      |
| .crd               | .mcf     | .pst           | .xnk      |
| .crds              | .mda     | .pvw           |           |
| .crt               | .mdb     | .py            |           |
| .csh               | .mde     | .pyc           |           |
| .der               | .mdt     | .pyo           |           |
| .dll               | .mdw     | .rb            |           |
| .drv               | .mdz     | .rbw           |           |
| .exe               | .msc     | .rdp           |           |
| .fxp               | .msh     | .reg           |           |
| .fon               | .msh1    | .rgu           |           |
| .gadget            | .msh1xml | .scf           |           |
| .grp               | .msh2    | .scr           |           |
| .hlp               | .msh2xml | .shb           |           |
| .hme               | .mshxml  | .shs           |           |
| .hpj               | .msi     | .sys           |           |
| .hta               | .msp     | .theme         |           |
| .inf               | .mst     | .tmp           |           |
| .ins               | .msu     | .tsk           |           |
| .isp               | .ocx     | .ttf           |           |
 

## Reservierte URI-Schemanamen


| application.manifest                        | internetshortcut                      | ms-settings:network-mobilehotspot | shbfile                 |
|---------------------------------------------|---------------------------------------|-----------------------------------|-------------------------|
| application.reference                       | javascript                            | ms-settings:network-proxy         | shcmdfile               |
| batfile                                     | jscript                               | ms-settings:network-wifi          | shsfile                 |
| bing                                        | jsefile                               | ms-settings:nfctransactions       | smb                     |
| blob                                        | ldap                                  | ms-settings:notifications         | stickynotes             |
| callto                                      | lnkfile                               | ms-settings:personalization       | sysfile                 |
| cerfile                                     | mailto                                | ms-settings:privacy-calendar      | tel                     |
| chm.filecmdfilecomfile                      | maps                                  | ms-settings:privacy-contacts      | telnet                  |
| cplfile                                     | microsoft.powershellscript.1          | ms-settings:privacy-customdevices | tn3270                  |
| dllfile                                     | ms-accountpictureprovider             | ms-settings:privacy-feedback      | ttffile                 |
| drvfile                                     | ms-appdata                            | ms-settings:privacy-location      | unknown                 |
| dtmf                                        | ms-appx                               | ms-settings:privacy-messaging     | usertileprovider        |
| exefile                                     | ms-autoplay                           | ms-settings:privacy-microphone    | vbefile                 |
| explorer.assocactionid.burnselection        | ms-excel                              | ms-settings:privacy-speechtyping  | vbscript                |
| explorer.assocactionid.closesession         | msi.package                           | ms-settings:privacy-webcam        | vbsfile                 |
| explorer.assocactionid.erasedisc            | msi.patch                             | ms-settings:proximity             | wallet                  |
| explorer.assocactionid.zipselection         | ms-powerpoint                         | ms-settings:regionlanguage        | windows.gadget          |
| explorer.assocprotocol.search-ms            | ms-settings                           | ms-settings:screenrotation        | windowsmediacenterapp   |
| explorer.burnselection                      | ms-settings:batterysaver              | ms-settings:speech                | windowsmediacenterssl   |
| explorer.burnselectionexplorer.closesession | ms-settings:batterysaver-settings     | ms-settings:storagesense          | windowsmediacenterweb   |
| explorer.closesession                       | ms-settings:batterysaver-usagedetails | ms-settings:windowsupdate         | wmp11.assocprotocol.mms |
| explorer.erasedisc                          | ms-settings:bluetooth                 | ms-settings:workplace             | wsffile                 |
| explorer.zipselection                       | ms-settings:connecteddevices          | ms-windows-store                  | wsfile                  |
| file                                        | ms-settings:cortanasearch             | ms-word                           | wshfile                 |
| fonfile                                     | ms-settings:datasense                 | ocxfile                           | xbls                    |
| hlpfile                                     | ms-settings:dateandtime               | office                            | zune                    |
| htafile                                     | ms-settings:display                   | onenote                           |                         |
| http                                        | ms-settings:lockscreen                | piffile                           |                         |
| https                                       | ms-settings:maps                      | regfile                           |                         |
| iehistory                                   | ms-settings:network-airplanemode      | res                               |                         |
| ierss                                       | ms-settings:network-cellular          | rlogin                            |                         |
| inffile                                     | ms-settings:network-dialup            | scrfile                           |                         |
| insfile                                     | ms-settings:network-ethernet          | scriptletfile                     |                         |

 

 

 



<!--HONumber=Jun16_HO5-->


