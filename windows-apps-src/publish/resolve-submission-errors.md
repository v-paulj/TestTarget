---
Description: Wenn nach der Übermittlung Ihrer App an den Store Fehler auftreten, müssen Sie diese auflösen, um den Zertifizierungsprozess fortsetzen zu können.
title: Fehler bei der Übermittlung
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
---

# Fehler bei der Übermittlung


Wenn nach der Übermittlung Ihrer App an den Store Fehler auftreten, müssen Sie diese auflösen, um den [Zertifizierungsprozess](the-app-certification-process.md) fortsetzen zu können. Die Fehlermeldung weist darauf hin, worin das Problem besteht und was eventuell erforderlich ist, um das Problem zu beheben. Nachfolgend sind einige zusätzliche Informationen aufgeführt, die Ihnen beim Beheben dieser Fehler helfen können.

## UWP-Apps


Wenn Sie eine UWP-App einreichen, wird während der Vorverarbeitung möglicherweise ein Fehler angezeigt, wenn die Paketdatei keine von Visual Studio für den Store generierte „.appxupload“-Datei ist. Achten Sie darauf, dass Sie die Schritte in [Verpacken universeller Windows-Apps für Windows 10](../packaging/packaging-uwp-apps.md) beim Erstellen der Paketdatei der App befolgen und nur die „.appxupload“-Datei auf der Seite [Pakete](upload-app-packages.md) der Übermittlung hochladen, keine „appx“- oder „.appxbundle“-Datei.

Wenn ein Kompilierungsfehler angezeigt wird, stellen Sie sicher, dass Sie die Anwendung erfolgreich im Releasemodus erstellen können. Weitere Informationen finden Sie unter [Systemeigene .NET-Compilerfehler](http://go.microsoft.com/fwlink/p/?LinkID=613098).

## Windows Phone-Apps


Wenn während der Vorverarbeitung Probleme mit Windows Phone-Paketen auftreten, wird möglicherweise **Fehler 2001** angezeigt. In den meisten Fällen müssen Sie das Paket Ihrer App neu erstellen, um den Fehler zu beheben. Sobald Sie damit fertig sind, ersetzen Sie auf der Seite [Pakete](upload-app-packages.md) der Einreichung das alte durch das neue Paket, bevor Sie erneut auf **An Store einreichen** klicken.

Es gibt eine Reihe von Problemen, die diesen Fehler verursachen können. Überprüfen Sie die nachfolgende Liste, um zu ermitteln, welches Problem möglicherweise auf Ihre Pakete zutrifft.

-   **Mindestens eine Assembly im Paket wurde nicht ordnungsgemäß verborgen:** Verwenden Sie ein anderes Tool zum Verbergen, oder verzichten Sie auf das Verbergen. Der Kompilierungsprozess optimiert die verborgenen Assemblys, aber gelegentlich werden jedoch einige Assemblys mit einem Tool verborgen, das die MSIL auf nicht unterstützt Weise verändert und daher einen Fehler auslöst.
-   **Die IL-Größe von mindestens einer Methode in der App beträgt mehr als 256 KB.** Gestalten Sie die betreffende(n) Methode(n) so um, damit kleinere Funktionen entstehen. Die MSIL-Größe für Methoden in einer Assembly kann mit dem ILDASM-Tool ermittelt werden.
-   **Die Prüfung der starken Namenssignatur ist für mindestens eine Assembly fehlgeschlagen:** Dieser Fehler tritt meist auf, wenn für die starke Namenssignatur ein anderer Schlüssel als der in den Assemblymetadaten erwartete verwendet wurde. Verwenden Sie den richtigen Schlüssel zum Signieren, oder entfernen Sie die starke Namenssignatur.
-   **Das Paket enthält Assemblys mit gemischtem Modus (d. h. mit verwaltetem und nativem Code)):** Assemblys mit gemischtem Modus werden unter Windows Phone nicht unterstützt. Entfernen Sie die betreffenden Assemblys aus dem Paket, und reichen Sie die App erneut ein.
-   **Eine Windows Phone 8.1-XAP oder appx-/appxbundle-Assembly ist ungültig:** Stellen Sie sicher, dass die WINMD-Datei mindestens über einen öffentlichen Einstiegspunkt verfügt. Sie können ggf. eine beliebige Dekompilierungsanwendung verwenden, um den Code zu überprüfen und öffentliche Einstiegspunkte zu suchen.

Ein weiterer Fehler, der möglicherweise nach dem Einreichen Ihrer App angezeigt wird, ist **Fehler 1300**. Dieser Fehler tritt auf, wenn mindestens eine Assembly (oder das gesamte Paket) bereits vorkompiliert ist. Um dieses Problem zu beheben, erstellen Sie das App-Paket in Microsoft Visual Studio neu und reichen dann das neu generierte Paket ein.

 

 






<!--HONumber=Mar16_HO1-->


