---
author: jnHs
Description: "Halten Sie die folgenden Richtlinien ein, wenn Sie die App-Pakete für die Übermittlung an den Windows Store vorbereiten."
title: App-Paketanforderungen
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
translationtype: Human Translation
ms.sourcegitcommit: c15d4153f6ae83cc7bf1ae02d834bd07189e38ab
ms.openlocfilehash: 250e94c2766227cabad791db6d994bcfb1a2ac33

---

# App-Paketanforderungen

Halten Sie die folgenden Richtlinien ein, wenn Sie die App-Pakete für die Übermittlung an den Windows Store vorbereiten.

## Vor dem Erstellen des App-Pakets für den Windows Store

Denken Sie daran, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](https://msdn.microsoft.com/library/windows/apps/mt186449). Außerdem wird empfohlen, Ihre App mit verschiedenen Hardwaretypen zu testen. Beachten Sie, dass Ihre App nur auf Computern mit Entwicklerlizenzen installiert und ausgeführt werden kann, bis wir die App zertifiziert und im Windows Store verfügbar gemacht haben.

## Erstellen des App-Pakets mit Microsoft Visual Studio

Wenn Sie Microsoft Visual Studio als Entwicklungsumgebung verwenden, verfügen Sie bereits über integrierte Tools zum schnellen und einfachen Erstellen eines App-Pakets. Weitere Informationen finden Sie unter [Verpacken von Apps](https://msdn.microsoft.com/library/windows/apps/mt270969).

> **Hinweis**  Achten Sie darauf, für alle Dateinamen ANSI zu verwenden. 


Um Ihr Paket in Visual Studio zu erstellen, müssen Sie sich mit demselben Konto anmelden, das Ihrem Entwicklerkonto zugeordnet ist. Einige Teile des Paketmanifests enthalten spezifische kontobezogene Details. Diese Informationen werden erkannt und automatisch hinzugefügt.

Beim Erstellen der App-Pakete kann Visual Studio eine APPX-Datei oder eine APPXUPLOAD-Datei erstellen (bzw. eine XAP-Datei für Windows Phone 8.1 und frühere Versionen). Bei Apps für Windows 10 laden Sie immer die APPXUPLOAD-Datei auf die Seite [Pakete](upload-app-packages.md) hoch. Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken universeller Windows-Apps für Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

App-Pakete müssen nicht mit einem Stammzertifikat einer vertrauenswürdigen Zertifizierungsstelle signiert werden.

### App-Bündel

Für Apps, die für Windows 8.1, Windows Phone 8.1 und höhere Versionen entwickelt werden, kann Visual Studio ein App-Bündel (.appxbundle) erzeugen, um die Downloadgröße der App für den Benutzer zu reduzieren. Dieser Schritt ist in der Regel sinnvoll, wenn Sie sprachspezifische Ressourcen, mehrere Ressourcen für die Bildgröße oder Ressourcen für bestimmte Versionen von Microsoft DirectX definiert haben.

> **Hinweis**  Ein App Bundle kann Ihre Pakete für alle Architekturen enthalten. Pro Zielbetriebssystem sollte nur ein App-Bündel eingereicht werden.


Bei einem App-Bündel lädt der Benutzer nicht alle vorhandenen Ressourcen, sondern nur relevante Dateien herunter. Weitere Informationen zu App-Bündeln finden Sie unter [Verpacken von Apps](https://msdn.microsoft.com/library/windows/apps/mt270969) und [Verpacken universeller Windows-Apps für Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

## Manuelles Erstellen des App-Pakets

Wenn Sie Ihr Paket nicht mit Visual Studio erstellen, müssen Sie Ihr [Paketmanifest manuell erstellen](https://msdn.microsoft.com/library/windows/apps/br211476).

Ausführliche Informationen und die Anforderungen für Manifeste finden Sie in der Dokumentation zum [App-Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474). Es werden nur Apps zertifiziert, deren Manifest dem Paketmanifestschema entspricht.

Ihr Manifest muss spezifische konto- und App-bezogene Informationen enthalten. Sie finden diese Informationen unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung** der App-Übersichtsseite im Dashboard.

> **Hinweis**  Bei den Werten im Manifest wird die Groß-/Kleinschreibung berücksichtigt. Leerzeichen und Satzzeichen müssen ebenfalls übereinstimmen. Geben Sie die Werte richtig ein, und überprüfen Sie sie anschließend auf ihre Korrektheit.


App-Bündel verwenden ein anderes Manifest. Ausführliche Informationen und die Anforderungen für App-Bündel finden Sie in der Dokumentation zum [Bündelmanifest](https://msdn.microsoft.com/library/windows/apps/dn263089).

> **Tipp**  Führen Sie vor dem Übermitteln Ihrer Pakete unbedingt das [Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/mt186449) aus. So können Sie feststellen, ob es mit Ihrem Manifest Probleme gibt, die Zertifizierungs- oder Einreichungsfehler verursachen können.


Wenn Ihre App mehrere Pakete enthält, müssen die folgenden App-Manifestelemente in allen Paketen gleich sein (pro Zielbetriebssystem):

-   [**Paket/Funktionen**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Paket/Abhängigkeiten**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Paket/Ressourcen**](https://msdn.microsoft.com/library/windows/apps/br211462)

## Paketformatanforderungen

Ihre App-Pakete müssen die folgenden Anforderungen erfüllen:

| App-Paketeigenschaft | Anforderung                                                          |
|----------------------|----------------------------------------------------------------------|
| Paketgröße         | APPX-Bündel: maximal 25GB pro Bündel <br>APPX-Pakete für Windows 8.1: maximal 8 GB pro Paket <br> APPX-Pakete für Windows 8: maximal 2 GB pro Paket <br> APPX-Pakete für WindowsPhone 8.1: maximal 4GB pro Paket <br> XAP-Pakete: maximal 1 GB pro Paket                                                                           |
| Hashes für Blockzuordnung     | SHA2-256-Algorithmus                                                   |
 

## Datei „StoreManifest.xml“

„StoreManifest.xml“ ist eine optionale Konfigurationsdatei, die in App-Pakete aufgenommen werden kann. Sie dient zum Aktivieren von Features, die vom Paketmanifest nicht abgedeckt werden – beispielsweise Features zum Deklarieren Ihrer App als Windows Store-Geräte-App oder zum Deklarieren von Anforderungen, die für ein Paket erfüllt werden müssen, damit es auf ein Gerät angewendet werden kann. „StoreManifest.xml“ wird mit dem App-Paket eingereicht und muss sich im Stammordner des App-Hauptprojekts befinden. Weitere Informationen finden Sie unter [StoreManifest-Schema](https://msdn.microsoft.com/library/windows/apps/mt617325).

 

 







<!--HONumber=Aug16_HO3-->


