---
title: "Gehostete Web-Apps – Konvertieren Ihrer Webanwendung in eine App für die Universelle Windows-Plattform (UWP) und Zugriff auf systemeigene Windows10-Features"
description: "Erstellen Sie eine App für die Universelle Windows-Plattform (UWP) über die URL Ihrer Website. Greifen Sie über Code innerhalb Ihrer Web-App auf systemeigene Windows10-Gerätefeatures zu. Mithilfe von Microsoft Windows-Brücken für gehostete Web-Apps, zuvor Projekt Westminster, können Sie Ihre Web-Apps schnell und einfach Sie dem Windows Store hinzufügen."
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 7fe6e240e4ef221b49f9b103cf30192449ce4502
ms.openlocfilehash: 95d50aa37f349f494f260ea3af97211a085623a9

---

# Gehostete Web Apps – Zugriff auf Windows10-Features über Ihre Web-App

Ihre Webanwendung kann vollständigen Zugriff auf die Universelle Windows-Plattform (UWP) erhalten, einschließlich des Aufrufens von Windows-Runtime-APIs direkt über ein auf einem Server gehostetes Skript, der Nutzung der Cortana-Integration und der Verwendung eines Onlineauthentifizierungsanbieters. Hybrid-Apps werden ebenfalls unterstützt, da Sie lokalen Code einfügen können, der aus dem gehosteten Skript aufgerufen werden soll, und die App-Navigation zwischen Remote- und lokalen Seiten verwalten können.

## Erste Schritte

Unabhängig davon, ob Sie auf einem Mac oder einen PC arbeiten, können Sie in wenigen Minuten Ihre eigenen gehosteten Web-App erstellen. Für die ersten Schritte sollten Sie die kostenlose Anwendung [Visual Studio Community2015](https://www.visualstudio.com/) verwenden, die über sämtliche Features verfügt, besonders dann, wenn Sie auf einem Windows-Gerät arbeiten. Wenn Sie keinen Zugriff auf Visual Studio haben, gibt es einige Optionen, aus denen Sie wählen können. Wenn Sie mit Hilfsprogrammen für die Befehlszeilenschnittstelle (CLI) vertraut sind, lesen Sie [ManifoldJS](http://manifoldjs.com/). Sie können auch [App Studio](http://appstudio.windows.com/) verwenden, ein kostenloses Onlinetool für das Erstellen von Apps, das keinen Code erfordert und mit dem Sie schnell Windows10-Apps erstellen können.

- [Schritt-für-Schritt-Anweisungen für die Konvertierung Ihrer Webanwendung in eine App für die Universelle Windows-Plattform (UWP) auf einem Windows-Computer](hwa-create-windows.md)

- [Schritt-für-Schritt-Anweisungen für die Konvertierung Ihrer Webanwendung in eine App für die Universelle Windows-Plattform (UWP) auf einem Mac-Computer](hwa-create-mac.md)

## Optimieren Sie Ihre App

- Heben Sie Ihre App durch den [Zugriff auf systemeigene Windows-Features](hwa-access-features.md) in JavaScript aus der Windows-Runtime hervor.

- Schützen Sie Ihre App, indem Sie mithilfe des Content Security Policy (CSP)-Modells Regeln für den Anwendungsinhalt-URI (ACURs) festlegen.
- Führen Sie Ihre App durch die Integration mit Cortana-Sprachbefehlen mithilfe von Sprachbefehlen aus.

- Gewähren Sie programmgesteuerten Zugriff auf Benutzerressourcen und Gerätefunktionen, indem Sie App-Funktionen deklarieren.

- Erstellen Sie einen einfachen und reibungslosen Anmeldungsablauf für Ihre Benutzer, indem ihre Identität mit OpenID und OAuth überprüft wird.

- Beenden Sie den Zustand, dass Sie sich zwischen einer verpackten und einer gehosteten Web-App entscheiden müssen. Durch die Erstellung einer Hybrid-App können Sie die Vorteile beider Modelle nutzen.

## Konvertieren Ihrer vorhandenen Chrome-App

Wir haben das [Konvertieren Ihrer vorhandenen gehosteten Chrome-App](hwa-chrome-conversion.md) in eine gehostete Windows-Web-App einfach gemacht. [ManifoldJS](http://manifoldjs.com/) akzeptiert nun Chrome-Manifeste als Eingaben. Es wurde außerdem ein [CLI-Tool](https://github.com/MicrosoftEdge/hwa-cli) entwickelt, das ein `.appx`-Paket aus Ihren vorhandenen `.zip` oder `.crx` Dateien generiert.

## Demos

- [Contoso-Reise-App](http://contosotravel.azurewebsites.net/)

- [Windows-Runtime-API: JavaScript-Code-Beispiele](http://rjs.azurewebsites.net/)



<!--HONumber=Aug16_HO3-->


