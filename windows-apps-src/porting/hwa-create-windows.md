---
author: seksenov
title: "Gehostete Web Apps – Konvertieren Ihrer Webanwendung in eine Windows-App mithilfe von Visual Studio"
description: "Verwenden Sie Visual Studio, um Ihre Website in eine App für die Universelle Windows-Plattform (UWP) für Windows10 zu konvertieren."
kw: Hosted Web Apps tutorial, Porting to Windows 10 with Visual Studio, How to convert website to Windows, How to add website to Windows Store, Packaging web application for Microsoft Store, Test Windows 10 native features and runtime APIs with CodePen, How to use Windows Cortana Live Tiles Built-in Camera on my Website with remote JavaScript
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e6dfda4a0f4f8b5760f08cfeb3562d9e42ba8a10

---

# Konvertieren Ihrer Webanwendung in eine App für die Universelle Windows-Plattform (UWP)

Erfahren Sie, wie Sie schnell eine App für die Universelle Windows-Plattform für Windows10 mithilfe einer Website-URL erstellen. 

> [!NOTE]
> Die folgenden Anweisungen gelten für die Verwendung mit einer Windows-Entwicklungsplattform. Mac-Benutzer sollten sich in den [Anweisungen zur Verwendung einer Mac-Entwicklungsplattform](/hwa-create-mac.md) informieren.

## Voraussetzungen für die Entwicklung auf einem Windows-Computer

- [Visual Studio2015.](https://www.visualstudio.com/) Die kostenlose Visual Studio Community2015-Anwendung besitzt sämtliche Funktionen und enthält Windows10-Entwicklertools, Vorlagen für universelle Apps, einen Code-Editor, einen leistungsstarken Debugger, Windows Mobile-Emulatoren, eine umfassende Sprachunterstützung und vieles mehr – alles bereit für die Verwendung in der Produktion.
- (Optional) [Eigenständiges WindowsSDK für Windows10.](https://dev.windows.com/downloads/windows-10-sdk) Wenn Sie eine andere Entwicklungsumgebung als Visual Studio2015 verwenden, können Sie ein eigenständiges WindowsSDK für das Windows10-Installationsprogramm herunterladen. Beachten Sie, dass Sie dieses SDK nicht installieren müssen, wenn Sie Visual Studio2015 verwenden. In diesem Fall ist es bereits enthalten.

## Schritt1: Wählen Sie eine Website-URL.
Wählen Sie eine vorhandene Website, die als Einzelseiten-App gut funktioniert. Wir empfehlen nachdrücklich, dass Sie der Besitzer oder Entwickler der Website sind. So können Sie alle erforderlichen Änderungen durchführen. Wenn Ihnen keine URL einfällt, versuchen Sie es mit diesem [Codepen-Beispiel](http://codepen.io/seksenov/pen/wBbVyb/?editors=101) als Website. Kopieren Sie die URL oder die Codepen-URL, um sie während des Lernprogramms zu verwenden. 

![Schritt Nr.1: Wählen Sie eine Website-URL.](images/hwa-to-uwp/windows_step1.png)

## Schritt2: Erstellen Sie eine leere JavaScript-App.

Starten Sie Visual Studio.
1. Klicken Sie auf **Datei**.
2. Klicken Sie auf **Neues Projekt**.
3. Klicken Sie unter **JavaScript** und dann **Windows Universal** auf **Leere App (Windows Universal)**.

![Schritt Nr.2: Erstellen Sie eine leere JavaScript-App.](images/hwa-to-uwp/windows_step2.png)

## Schritt3: Löschen Sie jeden Paketcode.

Da es sich um eine gehostete Web-App handelt, deren Inhalt von einem Remoteserver bereitgestellt wird, benötigen Sie die meisten lokalen App-Dateien nicht, die standardmäßig mit der JavaScript-Vorlage bereitgestellt werden. Löschen Sie alle lokalen HTML-, JavaScript- oder CSS-Ressourcen. Es sollte lediglich die Datei `package.appxmanifest` übrig bleiben, in der Sie die App und die Bildressourcen konfigurieren.

![Schritt Nr.3: Löschen Sie jeden Paketcode.](images/hwa-to-uwp/windows_step3.png)

## Schritt4: Legen Sie die URL der Startseite fest.

1. Öffnen Sie die Datei `package.appxmanifest`.
2. Suchen Sie auf der Registerkarte **Anwendung** das Textfeld **Startseite**.
3. Ersetzen Sie `default.html` durch die URL Ihrer Website.

![Schritt Nr.4: Legen Sie die URL der Startseite fest.](images/hwa-to-uwp/windows_step4.png)

## Schritt5: Definieren Sie die Grenzen Ihrer Web-app.

Regeln für den Anwendungsinhalt-URI (ACURs) geben an, welche Remote-URLs auf Ihre App und die universellen Windows-APIs zugreifen dürfen. Als Minimum benötigen Sie eine ACUR für die Startseite und alle Webressourcen, die von dieser Seite genutzt werden. Um weitere Informationen zu ACURs zu erhalten, [klicken Sie hier](./hwa-access-features.md#keep-your-app-secure-setting-application-content-uri-rules-acurs).
1. Öffnen Sie die Datei `package.appxmanifest`.
2. Klicken Sie auf die Registerkarte **Inhalts-URIs**.
3. Fügen Sie alle URIs hinzu, die für Ihre Startseite erforderlich sind.

Beispiel:
```
1. http://codepen.io/seksenov/pen/wBbVyb/?editors=101
2. http://*.codepen.io/
```
4. Legen Sie den **WinRT-Zugriff** für jeden hinzugefügten URI auf **Alle** fest.

![Schritt Nr.5: Definieren Sie die Grenzen Ihrer Web-app.](images/hwa-to-uwp/windows_step5.png)

## Schritt6: Führen Sie die App aus.

Nun besitzen Sie eine voll funktionsfähige Windows10-App, die auf die universellen Windows-APIs zugreifen kann.

Wenn Sie das Codepen-Beispiel verwenden, klicken Sie auf die Schaltfläche **Popupbenachrichtigung**, um über das gehostete Skript eine Windows-API aufzurufen.

![Schritt Nr.6: Führen Sie die App aus.](images/hwa-to-uwp/windows_step6.png)

## Bonus: Fügen Sie eine Kameraaufnahme hinzu

Kopieren Sie den JavaScript-Code unten und fügen ihn ein, um die Kameraaufnahme zu aktivieren. Wenn Sie Ihre eigene Website verwenden, erstellen Sie eine Schaltfläche, um die Methode `cameraCapture()` aufzurufen. Wenn Sie das Codepen-Beispiel verwenden, ist bereits eine Schaltfläche in HTML vorhanden. Klicken Sie auf die Schaltfläche, und nehmen Sie ein Bild auf.

```JavaScript
function cameraCapture() {
  if(typeof Windows != 'undefined') {
   var captureUI = new Windows.Media.Capture.CameraCaptureUI();
   //Set the format of the picture that's going to be captured (.png, .jpg, ...)
   captureUI.photoSettings.format = Windows.Media.Capture.CameraCaptureUIPhotoFormat.png;
   //Pop up the camera UI to take a picture
   captureUI.captureFileAsync(Windows.Media.Capture.CameraCaptureUIMode.photo).then(function (capturedItem) {
      // Do something with the picture
   });
  }
}
```

## Verwandte Themen

- [Optimieren Ihrer Web-App durch den Zugriff auf Features für die Universelle Windows-Plattform (UWP)](hwa-access-features.md)
- [Anleitung für Apps für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Herunterladen von Designressourcen für Windows Store-Apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


