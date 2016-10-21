---
author: jnHs
Description: "Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden."
title: "Leitfaden für die Verwaltung von App-Paketen"
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 68ed87147ed3cc3e1155eb1ab6d301867ba1ae55

---

# Leitfaden für die Verwaltung von App-Paketen


Erfahren Sie, wie App-Pakete für Ihre Kunden verfügbar gemacht werden und bestimmte Paketszenarien verwaltet werden.

-   [Betriebssystemversionen und Paketverteilung](#os-versions-and-package-distribution)
-   [Hinzufügen von Paketen für Windows10 zu einer zuvor veröffentlichten App](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Verwalten der Paketkompatibilität für Windows Phone8.1](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [Entfernen einer App aus dem Store](#removing-an-app-from-the-store)
-   [Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie](#removing-packages-for-a-previously-supported-device-family)

## Betriebssystemversionen und Paketverteilung


Auf unterschiedlichen Betriebssystemen können unterschiedliche Pakettypen ausgeführt werden. Wenn mindestens zwei Pakete auf dem Gerät eines Kunden ausgeführt werden können, stellt der Windows Store die beste verfügbare Übereinstimmung bereit.

Im Allgemeinen können höhere Betriebssystemversionen Pakete ausführen, die auf frühere Betriebssystemversionen für dieselbe Gerätefamilie abzielen. Kunden können diese Pakete jedoch nur beziehen, wenn die App kein Paket enthält, das auf die aktuelle Betriebssystemversion abzielt.

Beispielsweise können Geräte unter Windows10 alle zuvor unterstützten Betriebssystemversionen (pro Gerätefamilie) ausführen. Auf Desktopgeräten unter Windows10 können Apps ausgeführt werden, die für Windows8.1 oder Windows8 erstellt wurden. Auf Mobilgeräten unter Windows10 können Apps ausgeführt werden, die für Windows Phone8.1, WindowsPhone8 und sogar Windows Phone7.x erstellt wurden.

In den folgenden Beispielen werden verschiedene Szenarien für eine App veranschaulicht, die das Abzielen auf unterschiedliche Betriebssystemversionen umfasst. In einigen Fällen führen bestimmte Einschränkungen der Pakete dazu, dass sie möglicherweise nicht auf allen hier aufgelisteten Betriebssystemversionen und Gerätetypen (beispielsweise muss die Architektur entsprechend sein) ausgeführt werden. Diese Beispiele sollten Ihnen jedoch beim Nachvollziehen helfen, unter welchen Betriebssystemen die bestimmten Pakete ausgeführt werden können.

### Beispiel-App 1

| Zielbetriebssystem des Pakets | Dieses Paket abrufende Betriebssysteme |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Desktopgeräte unter Windows10, Windows8.1      |
| Windows Phone 8.1                   | Mobilgeräte unter Windows10, Windows Phone8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone7.1                   | Windows Phone7.x                            |

In der Beispiel-App1 verfügt die App noch nicht über UWP-Pakete (Universelle Windows-Plattform), die speziell für Geräte unter Windows10 erstellt wurden. Kunden von Windows10 können die App jedoch weiterhin abrufen. Diese Kunden erhalten in Abhängigkeit des Gerätetyps die besten verfügbaren Pakete.

### Beispiel-App 2

| Zielbetriebssystem des Pakets  | Dieses Paket abrufende Betriebssysteme |
|--------------------------------------|----------------------------------------------|
| Windows10 (universelle Gerätefamilie) | Windows10 (alle Gerätefamilien)             |
| Windows 8.1                          | Windows 8.1                                  |
| WindowsPhone8.1                    | WindowsPhone8.1                            |
| Windows Phone7.1                    | Windows Phone 7.x, Windows Phone 8           |

In der Beispiel-App2 ist kein Paket vorhanden, das unter Windows8 ausgeführt werden kann. Kunden, die andere Betriebssystemversionen ausführen, können die App abrufen.

### Beispiel-App 3

| Zielbetriebssystem des Pakets | Dieses Paket abrufende Betriebssysteme                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows10 (Desktopgerätefamilie)  | Desktopgeräte unter Windows10                                    |
| Windows Phone 8                     | Mobilgeräte unter Windows10, WindowsPhone8, Windows Phone 8.1 |

In der Beispiel-App3 erhalten Kunden von Mobilgeräten unter Windows10 das Windows Phone8-Paket, da kein UWP-Paket auf die Mobilgerätefamilie abzielt. Wenn in dieser App später ein Paket hinzugefügt wird, das auf die Mobilgerätefamilie (oder die universelle Gerätefamilie) abzielt, stehen diese Pakete für Kunden von Mobilgeräten unter Windows10 anstelle des WindowsPhone8-Pakets zur Verfügung.

Beachten Sie zudem, dass diese Beispiel-App kein Paket enthält, das auf Windows7.x ausgeführt werden kann.

### Beispiel-App 4

| Zielbetriebssystem des Pakets  | Dieses Paket abrufende Betriebssysteme |
|--------------------------------------|----------------------------------------------|
| Windows10 (universelle Gerätefamilie) | Windows10 (alle Gerätefamilien)             |

In der Beispiel-App4 können Geräte unter Windows10 die App abrufen. Sie steht jedoch nicht für Kunden einer früheren Betriebssystemversion zur Verfügung. Da das UWP-Paket auf die universelle Gerätefamilie abzielt, steht es für Desktopgeräte und Mobilgeräte unter Windows10 zur Verfügung.

## Hinzufügen von Paketen für Windows10 zu einer zuvor veröffentlichten App


Wenn Sie über eine App im Store verfügen und Sie die App für Windows10 aktualisieren möchten, müssen Sie eine neue Übermittlung erstellen und die UWP-Pakete „.appxupload“ während des Schritts [Pakete](upload-app-packages.md) hochladen. Nachdem Ihre App den Zertifizierungsprozess durchlaufen hat, können Kunden, die Ihre App bereits vor dem Upgrade auf Windows 10 besaßen, die UWP-Pakete als Update aus dem Store abrufen. Das UWP-Paket steht auch für Käufe von Neukunden unter Windows 10 zur Verfügung.

> **Wichtig**  Nachdem ein Kunde unter Windows 10 das UWP-Paket erhalten hat, können Sie für diesen Kunden kein Rollback auf ein Paket für eine frühere Betriebssystemversion mehr ausführen. Stellen Sie sicher, dass Sie Ihre UWP-Pakete unter Windows 10 gründlich getestet haben, bevor Sie sie Ihrer Übermittlung hinzufügen.

Sie können gleichzeitig andere Pakete aktualisieren oder andere Änderungen an der Übermittlung (beispielsweise möchten Sie möglicherweise [plattformspezifische Beschreibungen erstellen](create-platform-specific-descriptions.md), die Kunden von früheren Betriebssystemversionen angezeigt werden sollen) vornehmen. Sie können nach Bedarf auch alles andere unverändert lassen.

> **Hinweis**  Die Versionsnummer des entsprechenden Windows10-Pakets muss höher sein als die der Pakete für Windows8, Windows8.1 und/oder WindowsPhone8.1, die Sie für die gleiche App veröffentlichen (oder der Pakete für diese Betriebssystemversionen, die Sie zuvor veröffentlicht haben). Weitere Informationen über die Versionsnummerierung bei Windows10 finden Sie unter [Paketversionsnummern](package-version-numbering.md).

Nachdem die neue Übermittlung den Zertifizierungsprozess abgeschlossen hat, stehen die UWP-Pakete zusammen mit anderen von Ihnen für diese Kunden, die noch nicht über Windows10 verfügen, zur Verfügung gestellten Pakete zur Verfügung.

Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken universeller Windows-Apps für Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

> **Wichtig**  Beachten Sie beim Bereitstellen von Paketen für die universelle Gerätefamilie, dass jeder Kunde, der Ihre App bereits auf einem früheren Betriebssystem (WindowsPhone8, Windows8.1 usw.) verwendet hat und anschließend ein Upgrade auf Windows10 ausführt, auf Ihr universelles Windows10-Paket aktualisiert wird.
> 
> Dies erfolgt auch dann, wenn Sie eine bestimmte Gerätefamilie im Schritt [Preise und Verfügbarkeit](set-app-pricing-and-availability.md#windows-10-device-families) der Übermittlung ausgeschlossen haben, da die Auswahl **Gerätefamilien** nur auf neue Käufe zutrifft. Wenn Sie nicht möchten, dass jeder vorherige Kunden Ihr neues Windows10-Paket abrufen kann, müssen Sie das [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)-Element in Ihrem APPX-Manifest so aktualisieren, dass nur die bestimmte Gerätefamilie einbezogen wird, die Sie unterstützen möchten.
> 
> Angenommen, Sie möchten, dass Ihre Kunden mit Windows8 und Windows8.1, die ein Upgrade auf Windows10 vorgenommen haben, Ihre UWP-App abrufen können, und Sie möchten, dass Kunden mit WindowsPhone8.1 und früher die von Ihnen zuvor verfügbar gemachten Pakete beibehalten sollen (für WindowsPhone8 oder WindowsPhone8.1). Dafür müssen Sie [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) in Ihrem APPX-Manifest so aktualisieren, dass nur **Windows.Desktop** einbezogen (für die Desktopgerätefamilie) wird und nicht als den Wert **Windows.Universal** belassen (für die universelle Gerätefamilie), den Microsoft Visual Studio standardmäßig im APPX-Manifest enthält. Übermitteln Sie keine UWP-Pakete, die entweder auf universelle oder Mobilgerätefamilien (**Windows.Universal** oder **Windows.Universal**) abzielen. Auf diese Weise erhalten die Kunden von Windows10 überhaupt keine UWP-Pakete.
> 
> Weitere Informationen über Gerätefamilien finden Sie unter [Anleitung für UWP (Universelle Windows-Plattform)-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631).

## Verwalten der Paketkompatibilität für Windows Phone8.1


Beim Aktualisieren von Apps, die zuvor für Windows Phone 8.1 veröffentlicht wurden, gelten bestimmte Anforderungen für Pakettypen.

-   Nachdem eine App über ein veröffentlichtes WindowsPhone8.1-Paket verfügt, müssen alle nachfolgenden Updates ebenfalls über ein WindowsPhone8.1-Paket verfügen.
-   Nachdem eine App über eine veröffentlichte WindowsPhone8.1-XAP-Datei verfügt, müssen nachfolgende Updates über ein WindowsPhone8.1-XAP-, WindowsPhone8.1-APPX- oder ein WindowsPhone8.1-APPXBUNDLE-Format verfügen.
-   Wenn eine App über ein veröffentlichtes WindowsPhone8.1-APPX-Format verfügt, müssen nachfolgende Updates über ein WindowsPhone8.1-APPX- oder WindowsPhone8.1-APPXBUNDLE-Format verfügen. Ein WindowsPhone8.1-XAP-Format ist demnach nicht zulässig. Dies gilt für das Format "appxupload", das auch ein WindowsPhone8.1-APPX-Format enthält.
-   Nachdem eine App über ein veröffentlichtes WindowsPhone8.1-APPXBUNDLE-Format verfügt, müssen nachfolgende Updates über ein WindowsPhone8.1-APPXBUNDLE-Format verfügen. Somit ist weder ein WindowsPhone8.1-XAP- noch ein WindowsPhone8.1-APPX-Format zulässig. Dies gilt für das Format "appxupload", das auch ein WindowsPhone8.1-APPXBUNDLE-Format enthält.

Wenn diese Regeln nicht beachtet werden, führt dies zu Problemen beim Hochladen, die Sie daran hindern, Ihre Übermittlung abzuschließen.

## Entfernen einer App aus dem Store


Manchmal möchten Sie Kunden eine App vielleicht überhaupt nicht mehr anbieten, d.h. Sie heben die Veröffentlichung der App auf. Klicken Sie hierzu in der App-Übersicht auf **Make app unavailable**. Nachdem Sie bestätigt haben, dass die App nicht mehr verfügbar sein soll, wird sie innerhalb weniger Stunden nicht mehr im Store angezeigt, und neue Kunden haben keine Möglichkeit, sie herunterzuladen (auch nicht mit Werbecodes).

> **Wichtig**  Hierdurch werden alle in den Übermittlungen ausgewählten Einstellungen für [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) außer Kraft gesetzt.

Beachten Sie, dass Kunden, die die App bereits besitzen, sie weiterhin verwenden können (und sogar Updates erhalten können, wenn Sie zu einem späteren Zeitpunkt neue Pakete übermitteln).

Nachdem die Bereitstellung der App aufgehoben wurde, wird sie weiterhin in Ihrem Dashboard angezeigt. Wenn Sie die App Kunden erneut anbieten möchten, klicken Sie in der App-Übersicht auf **Make app available**. Nach dem Bestätigen ist die App für Neukunden innerhalb weniger Stunden verfügbar (es sei denn, es liegen Einschränkungen durch Einstellungen in der letzten Übermittlung vor).

> **Hinweis**  Wenn die App verfügbar bleiben, Kunden mit bestimmten Betriebssystemversionen jedoch nicht mehr angeboten werden soll, können Sie eine neue Übermittlung erstellen und alle Pakete für die Betriebssystemversion entfernen, unter der Sie neue Verkäufe verhindern möchten. Wenn Sie beispielsweise bisher Pakete für WindowsPhone8, WindowsPhone8.1 und Windows10 hatten, die App jedoch keinen Neukunden mit WindowsPhone8 mehr anbieten möchten, entfernen Sie Ihre WindowsPhone8-Pakete aus der Übermittlung. Nach der Veröffentlichung des Updates können keine neuen Kunden unter WindowsPhone8 die App mehr erwerben (Kunden, die sie bereits haben, können sie jedoch weiterhin nutzen). Für neue Kunden unter WindowsPhone8.1 und Windows10 ist die App weiterhin verfügbar.

## Entfernen von Paketen für eine zuvor unterstützte Gerätefamilie


Wenn Sie alle Pakete für eine bestimmte Gerätefamilie entfernen, die Ihre App zuvor unterstützt hat, werden Sie aufgefordert zu bestätigen, dass dies beabsichtigt ist, bevor Sie Ihre Änderungen auf der Seite **Pakete** speichern können.

Wenn Sie eine Übermittlung veröffentlichen, die Pakete für eine Gerätefamilie entfernt, die Ihre App bisher unterstützt hat, können neue Kunden die App für diese Gerätefamilie nicht erwerben. Sie können jederzeit ein weiteres Update veröffentlichen, um erneut Pakete für diese Gerätefamilie anzubieten.

Hinweis: Auch wenn Sie alle Pakete entfernen, die eine bestimmte Gerätefamilie unterstützen, können vorhandene Kunden, die die App auf diesem Gerätetyp bereits installiert haben, diese weiterhin verwenden und erhalten alle Updates, die Sie später zur Verfügung stellen.

 

 







<!--HONumber=Aug16_HO3-->


