---
Description: Auf der Seite „Preise und Verfügbarkeit“ des App-Übermittlungsprozesses können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist.
title: Festlegen von Preisen und Verfügbarkeit von Apps
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
---

# Festlegen von Preisen und Verfügbarkeit von Apps


Auf der Seite **Preise und Verfügbarkeit** des [App-Übermittlungsprozesses](app-submissions.md) können Sie festlegen, wie viel Ihre App kosten soll, ob Sie eine kostenlose Testversion anbieten und wie, wann und wo sie für Kunden verfügbar ist. Wir stellen Ihnen hier die auf dieser Seite verfügbaren Optionen vor und informieren Sie darüber, was Sie bei der Eingabe dieser Informationen beachten sollten.

## Grundpreis


Mit der ersten Option auf dieser Seite können Sie einen Grundpreis für Ihre App auswählen. Sie können sie kostenlos anbieten oder eines der verfügbaren Preisniveaus auswählen. Ein Grundpreis muss angegeben werden, damit die App eingereicht werden kann.

Weitere Informationen erhalten Sie unter [Festlegen des Preises und Auswählen der Märkte](define-pricing-and-market-selection.md).

## Kostenlose Testversion


Viele Entwickler bieten Kunden die Möglichkeit, ihre App mithilfe der im Store verfügbaren Testfunktionen kostenlos auszuprobieren. Eine App wird standardmäßig nicht als kostenlose Testversion bereitgestellt. Falls Sie jedoch eine Testversion anbieten möchten, wählen Sie einen Wert aus der Dropdownliste **Kostenlose Testversion** aus.

Wählen Sie **Testversion läuft nie ab**, damit Kunden unbegrenzte Zeit kostenlos auf Ihre App zugreifen dürfen. Da Sie Ihre Kunden zum Kauf der Vollversion animieren möchten, achten Sie darauf, mithilfe des geeigneten Codes [Features in der Testversion auszuschließen oder einzuschränken](https://msdn.microsoft.com/library/windows/apps/mt219685).

Sie können die Nutzung der Testversion auch auf **1**,**7**, **15** oder **30 Tage** begrenzen. Sie können während des Testzeitraums zusätzlich Features einschränken oder Kunden den Zugriff auf den gesamten Funktionsumfang gewähren.

> **Hinweis**  Zeitlich begrenzte Testversionen sind für Kunden mit Windows Phone 8.1 und früheren Versionen nicht sichtbar.

## Märkte und angepasste Preise


Die App wird standardmäßig in allen möglichen Märkten zum Grundpreis eingetragen. Sie können diese Einstellungen im Abschnitt **Märkte und angepasste Preise** ändern, um bestimmte Märkte ein- oder auszuschließen und den App-Preis in Märkten, in denen Sie die App anbieten, zu ändern. Weitere Informationen erhalten Sie unter [Festlegen des Preises und Auswählen der Märkte](define-pricing-and-market-selection.md).

## Sonderangebotsverkaufspreise


Wenn Sie Ihre App zu einem reduzierten Preis für einen begrenzten Zeitraum anbieten möchten, können Sie ein Sonderangebot erstellen und planen. Weitere Informationen finden Sie unter [Anbieten von Apps und IAPs](put-apps-and-iaps-on-sale.md).

## Verteilung und Sichtbarkeit


Im Abschnitt **Verteilung und Sichtbarkeit** können Sie Einschränkungen dafür festlegen, wie die App entdeckt und erworben werden kann.

Die Standardeinstellung ist **Diese App im Store verfügbar machen**. Das heißt, dass Ihre App im Store so eingetragen wird, dass sie von Kunden über den direkten Link zur App und/oder mit anderen Methoden wie z. B. Suchen, Browsen oder Einträgen in zusammengestellten Listen gefunden werden kann.

Wenn Sie Ihre App im Store ausblenden, sie aber dennoch für bestimmte Personen verfügbar machen möchten, wählen Sie eine der folgenden Optionen aus, um die Verfügbarkeit Ihrer App einzuschränken. Beachten Sie, dass Kunden mit Windows 8 und Windows 8.1 bei Auswahl einer dieser Optionen keinen Zugriff auf die App haben.

-   **Diese App ausblenden und den Erwerb verhindern. Kunden mit einem Werbecode können die App auf Windows 10-Geräten weiterhin herunterladen**: Kein Kunde kann Ihre App im Store durch Suchen oder beim Surfen finden, Sie können aber [Werbecodes generieren](generate-promotional-codes.md) und diese an bestimmte Windows 10-Benutzer verteilen. Diese Personen können den Link und den Code verwenden, um Ihre App kostenlos zu erhalten, obwohl Sie sie für alle anderen Kunden nicht anbieten.
-   **Diese App im Store ausblenden. Kunden mit einem direkten Link zum Eintrag der App können sie weiterhin herunterladen, außer unter Windows 8 und Windows 8.1**: Kein Kunde kann Ihre App im Store durch Suchen oder beim Surfen finden, aber alle Kunden mit einem direkten Link zu Ihrem App-Eintrag können die App auf Geräte unter Windows 10 oder Windows Phone 8.1 und früher herunterladen.
-   **Diese App im Store ausblenden und nur für die unten angegebenen Kunden verfügbar machen, die diese App auf Windows Phone 8.x-Geräten herunterladen können. Auf Windows 10-Geräten kann ein Werbecode zum Herunterladen dieser App verwendet werden**: Kein Kunde kann Ihre App im Store durch Suchen oder beim Surfen finden, und nur die Kunden, deren E-Mail-Adressen (die ihren Microsoft-Konten zugeordnet sind) Sie getrennt durch Semikolons in das Feld eingeben, können Ihre App über den direkten Link herunterladen. Sie können auch [Werbecodes generieren](generate-promotional-codes.md) und diese an bestimmte Windows 10-Benutzer verteilen Diese Option wird häufig für [Betatests](beta-testing-and-targeted-distribution.md) unter Windows Phone 8.1 und früher verwendet. Diese Option kann nur ausgewählt werden, wenn Sie die App zuvor noch nie über die Option **Verteilung und Sichtbarkeit** mit der Einstellung **Jeder kann Ihre App im Store finden** veröffentlicht haben.

> **Hinweis**  Wenn Sie eine App überhaupt nicht mehr für Kunden anbieten möchten, klicken Sie in der App-Übersicht auf **Make app unavailable**. Nachdem Sie bestätigt haben, dass die App nicht mehr verfügbar sein soll, wird sie innerhalb weniger Stunden im Store nicht mehr angezeigt, und neue Kunden haben keine Möglichkeit, sie herunterzuladen. Diese Aktion setzt alle hier ausgewählten Optionen außer Kraft: Die App ist für neue Kunden nicht verfügbar. Falls Sie sie für neue Kunden wieder verfügbar machen möchten, können Sie jederzeit in der App-Übersicht auf **Make app available** klicken. Weitere Informationen finden Sie unter [Entfernen einer App aus dem Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## Windows 10-Gerätefamilien

In diesem Abschnitt können Sie angeben, welche Typen von Windows 10-Geräten die Kunden verwenden können, um Ihre App zu erwerben. (Wenn das Paket auf einem bestimmten Gerätetyp nicht ausgeführt werden kann, wird es für diesen Gerätetyp nicht zum Download angeboten.)

> **Wichtig**  Um zu verhindern, dass eine bestimmte Windows 10-Gerätefamilie Ihre App herunterladen kann, müssen Sie das [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)-Element im Appx-Manifest so aktualisieren, dass die App nur auf die Gerätefamilie abzielt, die Sie unterstützen möchten (d. h. **Windows.Mobile** oder **Windows.Desktop**), statt den **Windows.Universal**-Wert zu übernehmen (für die universelle Gerätefamilie), den Microsoft Visual Studio standardmäßig im Appx-Manifest einschließt.

Standardmäßig sind die Kontrollkästchen **Mobil** und **Desktop** aktiviert. Wir empfehlen, diese Kontrollkästchen hier aktiviert zu lassen, es sei denn, Sie möchten aus einem bestimmten Grund die Windows 10-Gerätetypen einschränken, die Ihre App erwerben können. Angenommen Sie haben z. B. universelle Windows-Pakete erstellt, müssen jedoch noch einigen Probleme mit der App auf Mobilgeräten testen. Um zu verhindern, dass neue Kunden die App auf Mobilgeräten mit Windows 10 herunterladen, können Sie hier das Kontrollkästchen **Mobil** deaktivieren. Wenn Sie die App dann später für Mobilgeräte mit Windows 10 anbieten möchten, können Sie eine neue Übermittlung Erstellen, bei der das Kontrollkästchen **Mobil** aktiviert ist.

Wenn Sie Ihre App getestet und sichergestellt haben, dass sie unter Microsoft HoloLens ausgeführt werden kann, können Sie auch das Kontrollkästchen **Hologramm** aktivieren, um die App für HoloLens-Kunden anzubieten. Weitere Informationen zum Erstellen, Testen und Veröffentlichen von Hologramm-Apps finden Sie in der [Entwicklungsübersicht für Windows Holographic](http://dev.windows.com/holographic/development_overview).

Beachten Sie, dass die in diesem Abschnitt getroffene Auswahl unabhängig von der Zielbetriebssystem-Version (Windows 10, Windows 8.x, Windows Phone 8.x usw.) für alle Pakete Ihrer App gilt. Sie wirkt sich jedoch nur auf die Verfügbarkeit Kunden aus, die Windows 10-Geräte (und nicht Windows 8.x- oder Windows Phone 8.x-Geräte) verwenden.

Beachten Sie außerdem, dass die hier getroffene Auswahl nur für neue Verkäufe gilt. Kunden, die Ihre App bereits verwenden, können dies weiterhin tun und erhalten alle zur Verfügung gestellten Updates, selbst wenn Sie diese Gerätefamilie an dieser Stelle entfernen. Dies gilt auch für Kunden, die Ihre App vor dem Upgrade auf Windows 10 erworben haben. Beispiel: Wenn Sie eine App mit Windows Phone 8.1-Paketen veröffentlicht haben und später ein Windows 10 (UWP)-Paket für die gleiche App hinzufügen, das auf die universelle Gerätefamilie abzielt, wird Kunden mit Mobilgeräten unter Windows 10, die bereits über das Windows Phone 8.1-Paket verfügen, ein Update auf dieses Windows 10 (UWP)-Paket angeboten, selbst wenn Sie das Kontrollkästchen für **Mobil** deaktiviert haben (da dies kein neuer Verkauf ist, sondern ein Update). Wenn Sie kein Windows 10 (UWP)-Paket bereitstellen, das auf die universelle oder Mobilgerätefamilie abzielt, bleibt Kunden mit Mobilgeräten mit Windows 10 weiterhin das Windows Phone 8.1-Paket zur Verfügung.

Weitere Informationen über Gerätefamilien finden Sie unter [Anleitung für UWP-Apps (Universal Windows Platform)](https://msdn.microsoft.com/library/windows/apps/dn894631) und [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

> **Hinweis**  Es wird auch ein Kontrollkästchen angezeigt, in dem Sie angeben können, ob Microsoft die App zukünftigen Windows 10-Gerätefamilien zur Verfügung stellen soll. Es wird empfohlen, diese Option aktiviert zu lassen, sodass Ihre App für mehr potenzielle Kunden zur Verfügung gestellt wird, wenn neue Gerätefamilien eingeführt werden.

## Organisationslizenzierung


Standardmäßig kann Ihre App Organisationen für den Volumekauf angeboten werden. Sie können angeben, ob und wie Ihre App in diesem Abschnitt angeboten werden soll.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).

## Veröffentlichungsdatum


Mithilfe der Optionen im Abschnitt **Veröffentlichungsdatum** können Sie angeben, wann die App (oder das Update) veröffentlicht wird.

-   Wählen Sie **Publish this submission as soon as it passes certification**, um die Übermittlung so schnell wie möglich im Store verfügbar zu machen.
-   Wählen Sie **Publish this submission manually**, wenn Sie den Veröffentlichungszeitpunkt Ihrer Übermittlung selbst angeben möchten. Dazu können Sie auf der Seite mit dem Zertifizierungsstatus auf **Jetzt veröffentlichen** klicken oder ein bestimmtes Datum auswählen, wie unten beschrieben.
-   Wählen Sie **Nicht vor \[Datum\]**, um sicherzustellen, dass die Übermittlung nicht vor einem bestimmten Datum veröffentlicht wird. Mit dieser Option wird Ihre Übermittlung möglichst zum oder nach dem angegebenen Datum veröffentlicht. Das Datum muss mindestens 24 Stunden in der Zukunft liegen. Zusammen mit dem Datum können Sie auch die Uhrzeit angeben, zu der die Veröffentlichung der Übermittlung beginnen soll.
    > **Hinweis**  Aufgrund von Verzögerungen bei der Zertifizierung oder Veröffentlichung kann das gewünschte Veröffentlichungsdatum unter Umständen nicht eingehalten werden. Es kann nicht garantiert werden, dass Ihre App (oder das Update) an einem bestimmten Datum im Windows Store zur Verfügung steht.

Sie können das Veröffentlichungsdatum auch nach dem Einreichen der App ändern, solange sie noch nicht in die Phase **Veröffentlichen** eingetreten ist.
 

 






<!--HONumber=Mar16_HO5-->

