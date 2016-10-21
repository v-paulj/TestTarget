---
author: seksenov
title: "Gehostete Web Apps – Konvertieren Ihrer Webanwendung in eine Windows-App mithilfe eines Mac-Computers"
description: "Verwenden Sie einen Mac-Computer, um Ihrer Website in eine App für die Universelle Windows-Plattform (UWP) für Windows10 zu konvertieren."
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
translationtype: Human Translation
ms.sourcegitcommit: 0458dcd2aab862ccdecf1ebbc51e883405a929a6
ms.openlocfilehash: 3ba820e2ec8a3556874c0c7c7e328831bab783ca

---

# Erstellen Ihrer gehosteten Web-App mithilfe eines Mac-Computers

Erstellen Sie schnell eine App für die Universelle Windows-Plattform für Windows10 mithilfe einer Website-URL. 

> [!NOTE]
> Die folgenden Anweisungen gelten für die Verwendung mit einer Mac-Entwicklungsplattform. Windows-Benutzer sollten sich in den [Anweisungen zur Verwendung einer Windows-Entwicklungsplattform](/hwa-create-windows.md) informieren.

## Voraussetzungen für die Entwicklung auf einem Mac-Computer

- Ein Webbrowser.
- Eine Eingabeaufforderung.

## Option1: ManifoldJS

[ManifoldJS](http://manifoldjs.com/) ist eine Node.js-App, die problemlos über NPM installiert werden kann. Sie verwendet die Metadaten über Ihre Website und generiert systemeigene gehostete Apps für Android, iOS und Windows. Wenn Ihre Website kein [Web-App-Manifest](https://www.w3.org/TR/appmanifest/) besitzt, wird ein Manifest automatisch für Sie generiert.

1. Installieren Sie [NodeJS](https://nodejs.org/), das NPM (Node Package Manager) enthält. <br>

2. Öffnen Sie eine Eingabeaufforderung, und die NPM-Installation von ManifoldJS:
```
npm install -g manifoldjs
```

3. Führen Sie den Befehl `manifoldjs` auf Ihrer Website-URL aus:
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. Befolgen Sie die im Video unten angezeigten Schritte aus, um die Verpackung durchzuführen und Ihre gehostete Web-App im Windows Store zu veröffentlichen.

[![Veröffentlichen einer UWP-Web-App auf einem Mac-Computer mit ManifoldJS](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "Veröffentlichen einer UWP-Web-App auf einem Mac-Computer mit ManifoldJS")

## Option2: App Studio

[App Studio](http://appstudio.windows.com/) ist ein kostenloses Onlinetool, mit dem Sie Windows10-Apps rasch erstellen können.

1. Öffnen Sie [App Studio](http://appstudio.windows.com/) in Ihrem Webbrowser.

2. Klicken Sie auf **Start now!** (Jetzt starten).

3. Klicken Sie in **Web app templates** (Vorlagen für Web-Apps) auf **Hosted Web App** (Gehostete Web App).

4. Befolgen Sie die Anweisungen auf dem Bildschirm, um ein Paket zu generieren, das Sie im Windows Store veröffentlichen können.

## Verwandte Themen

- [Optimieren Ihrer Web-App durch den Zugriff auf Features für die Universelle Windows-Plattform (UWP)](/hwa-access-features.md)
- [Anleitung für Apps für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Herunterladen von Designressourcen für Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


