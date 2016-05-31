---
author: jnHs
Description: Das Windows Dev Center-Dashboard bietet Ihnen die Möglichkeit, Ihre App nur für bestimmte Personen bereitzustellen. Auf diese Weise können Tester die App ausprobieren, bevor Sie sie für die Allgemeinheit anbieten.
title: Betatests und zielgerichtete Verteilung
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
---

# Betatests und zielgerichtete Verteilung


Wie sorgfältig Sie Ihre App auch testen: Es geht nichts über einen Praxistest, bei dem die App von anderen Benutzern verwendet wird. Das Windows Dev Center-Dashboard bietet Ihnen die Möglichkeit, Ihre App nur für bestimmten Personen bereitzustellen. Auf diese Weise können Tester die App ausprobieren, bevor Sie sie für die Öffentlichkeit anbieten. Die Betatester finden möglicherweise Probleme, die Sie übersehen haben, wie z. B. Rechtschreibfehler, unübersichtliche Benutzerführungen der App und sogar Fehler, die dazu führen können, dass die App abstürzt. Sie haben dann die Möglichkeit, diese Probleme zu beheben, bevor Sie die App für die Allgemeinheit verfügbar machen. So erhalten Sie ein hochwertigeres Endprodukt.

Wir bieten verschiedene Möglichkeiten, um die Verteilung Ihrer Apps auf Ihre Tester zu beschränken, ohne eine separate Version Ihrer App mit eigenem Namen und eigener Paketidentität erstellen zu müssen. (Selbstverständlich können Sie bei Bedarf auch eine separate App zum Testen erstellen. In diesem Fall muss sich der Name der App allerdings vom endgültigen öffentlichen Namen der App unterscheiden.)

Mit welcher Methode Sie Ihre App an die Tester verteilen, hängt davon ab, auf welche Betriebssysteme Ihre App ausgerichtet ist. Im Anschluss finden Sie Optionen für Windows 10 und für Windows Phone 8.1 (und ältere Versionen).

## Bereitstellen Ihrer App für Tester auf Windows 10-Geräten

Wir bieten zwei Optionen, mit denen Sie die Verteilung Ihrer Apps auf bestimmte Benutzer von Windows 10-Geräten beschränken können.

### Flight-Pakete

Falls Sie bereits eine Version Ihrer App veröffentlicht haben, können Sie Flight-Pakete erstellen, um den gewünschten Benutzern einen anderen Satz von Paketen bereitzustellen. Sie können für die gleiche App mehrere Flight-Pakete erstellen und jeweils für unterschiedliche Benutzergruppen verwenden. So können Sie komfortabel mehrere Pakte parallel testen und Pakete aus einem Flight in die Übermittlung ohne Test-Flight überführen, falls Sie zu dem Schluss kommen, dass sie für die allgemeine Bereitstellung bereit sind.

Weitere Informationen finden Sie unter [Flight-Pakete](package-flights.md).

### Ausblenden der App im Store und Verwenden von Werbecodes

Wenn Sie eine App nur an eine bestimmte Gruppe von Testern verteilen möchten, ohne vorher eine allgemein verfügbare Übermittlung zu veröffentlichen, können Sie den gleichen [App-Übermittlungsprozess](app-submissions.md) verwenden wie bei allen anderen Apps. Gehen Sie wie folgt vor, wenn Sie möchten, dass die App nur von bestimmten Benutzern kostenlos bezogen werden kann und andere Kunden weder den App-Eintrag sehen noch die App herunterladen können:

-   Wählen Sie bei der Übermittlung auf der Seite **Preise und Verfügbarkeit** im Abschnitt [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) die Option **Diese App ausblenden und den Erwerb verhindern. Kunden mit einem Werbecode können die App auf Windows 10-Geräten weiterhin herunterladen** aus. Dadurch wird verhindert, dass Benutzer Ihre App über die Suche oder beim Stöbern im Store finden. Dadurch wird verhindert, dass Benutzer Ihre App über die Suche oder durch Browsen im Store finden.
-   Nach der Zertifizierung der App [generieren Sie Angebotscodes](generate-promotional-codes.md) für die App, und verteilen Sie sie dann an Ihre Tester. Sie können für einen Zeitraum von sechs Monaten bis zu 250 Angebotscodes für eine einzelne App generieren. Diese Codes bieten Ihren Testern einen direkten Link zum Eintrag der App, damit sie die App kostenlos herunterladen können, auch wenn Sie bei der Einreichung einen Preis für die App festgelegt haben.

Nachdem Sie die Angebotscodelinks an Ihre Tester verteilt haben, können sie Ihre App kostenlos herunterladen, testen und Ihnen Feedback bieten, um Sie bei der Optimierung der App zu unterstützen. Wenn Sie dann bereit sind, Ihre App für die Allgemeinheit zur Verfügung zu stellen, können Sie eine neue Übermittlung erstellen und die Option **Verteilung und Sichtbarkeit** in **Jeder kann Ihre App im Store finden** ändern (sowie ggf. andere gewünschte Änderungen vornehmen).

Hierbei sind einige Dinge zu beachten:

-   Sie können Ihren Testern jederzeit eine aktualisierte Version Ihrer App bereitstellen, indem Sie eine neue Übermittlung erstellen. Wichtig ist, dass Sie für die Option **Verteilung und Sichtbarkeit** die Einstellung **Diese App ausblenden und den Erwerb verhindern. Kunden mit einem Werbecode können die App auf Windows 10-Geräten weiterhin herunterladen** beibehalten. Die Tester erhalten das Update, nachdem es den Zertifizierungsprozess durchlaufen ist. Andere Personen haben keine Möglichkeit, darauf zuzugreifen.
-   Ihre Tester benötigen ein Windows 10-Gerät, auf dem sie die App installieren können. (Ihre App muss jedoch keine Windows 10-Pakete umfassen, damit diese Methode für Testzwecke verwendet werden kann.)
-   Sie können jederzeit weitere [Angebotscodes](generate-promotional-codes.md) für die Verteilung erstellen (bis zu 250 Codes für einen Zeitraum von sechs Monaten).
-   Sie können den Zugriff auf die App nicht widerrufen, nachdem sie von den Testern heruntergeladen wurde. Nachdem sie die App heruntergeladen haben, können sie diese weiterhin verwenden, und sie erhalten sämtliche Updates, die Sie anschließend veröffentlichen.
-   Sie müssen festlegen, auf welche Weise Sie Feedback von Ihren Testern einholen möchten. Beispielsweise können Sie eine E-Mail-Adresse oder einen Websitelink in die Beta-App integrieren, über die sie problemlos Kommentare abgeben können.
-   Sie können [Analyseberichte](analytics.md) für Ihre App überprüfen, einschließlich der von Ihren Testern abgegebenen Bewertungen und Kommentare.
-   Sie können In-App-Produkte (IAPs) einbeziehen, wenn Sie Ihre App an die Tester verteilen. Da Sie wahrscheinlich keine Kosten erheben möchten, müssen Sie sicherstellen, dass Sie für den Preis der In-App-Produkte (IAP) „Kostenlos“ festlegen, während Sie die Tests durchführen. Wenn Sie die App dann für andere Kunden verfügbar machen, können Sie für jedes IAP eine neue Übermittlung erstellen, um den Preis zu ändern.

## Andere Methoden zum Verteilen von Apps an Tester

Die Verteilung Ihrer App kann auch mithilfe der zusätzlichen Optionen im Abschnitt [Verteilung und die Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) im Abschnitt der App-Übermittlungsseite **Verfügbarkeit und Preise** auf eine bestimmte Gruppe von Benutzern beschränkt werden. Bedenken Sie, dass diese Optionen nicht für Kunden aller Betriebssysteme geeignet sind. Die oben angegebenen Optionen werden empfohlen, wenn Sie die App auf Windows 10-Geräten testen.

Wenn Sie eine der obigen Optionen auswählen, können Sie jederzeit ein Update einreichen und für die Option [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) den Wert **Make this app available in the Store** festlegen, wenn Sie bereit sind, die Testphase zu beenden und die App allgemein zur Verfügung zu stellen. Sie müssen den Namen der App nicht ändern und auch keine vollständig separate App erstellen (sofern dies nicht gewünscht ist).

### Zielgerichtete Verteilung an Kunden mit einem Link zum App-Eintrag

Bei dieser Option können nur Personen mit einem direkten Link zum Eintrag Ihrer App gelangen, um sie herunterzuladen. Sie finden diesen Link im Dashboard auf der Seite [App-Identität](view-app-identity-details.md) (verwenden Sie den **URL für Windows Phone** oder den **URL für Windows 10**). Kein Kunde ist in der Lage, die App durch Suchen oder Browsen im Store zu finden. Aber jeder Kunde mit dem entsprechenden Link kann die App herunterladen. (Beachten Sie, dass für Ihre App der Preis **Kostenlos** festgelegt werden muss, damit sie von den Testern kostenlos heruntergeladen werden kann.)

Um diese Option zu verwenden, aktivieren Sie beim Einrichten der App auf der Seite **Preise und Verfügbarkeit** im Abschnitt [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) die Option **Diese App im Store ausblenden. Kunden mit einem direkten Link zum Eintrag der App können sie weiterhin herunterladen, außer unter Windows 8 und Windows 8.1:**.

> **Wichtig**  Diese Option funktioniert nicht für Tester unter Windows 8 oder Windows 8.1.

### Zielgerichtete Verteilung an Kunden mit angegebenen E-Mail-Adressen

Für Tests auf Windows Phone 8.1 und früher ist es mit dieser Option möglich, die Verteilung der App zu beschränken. Nur Benutzer, deren (mit dem jeweiligen Microsoft-Konto verknüpfte) E-Mail-Adresse Sie in das Feld eingegeben haben, können die App über einen direkten Link zum App-Eintrag herunterladen.

> **Wichtig:**  Benutzer mit den eingegebenen E-Mail-Adressen können die App nur auf Geräten unter Windows Phone 8.1 (oder einer älteren Version) herunterladen.
 
Den direkten Link zur App finden Sie im Dashboard auf der Seite [App-Identität](view-app-identity-details.md). (Verwenden Sie die **URL für Windows Phone**.) Kein Kunde ist in der Lage, die App durch Suchen oder Browsen im Store zu finden. Auch wenn sie über den Links zum App-Eintrag verfügen, können sie die App nicht herunterladen, sofern sie nicht ein Microsoft-Konto verwenden, das einer E-Mail-Adresse zugeordnet ist, die Sie beim Einreichen dieser App bereitgestellt haben.

> **Hinweis**  Wenn Sie diese Option verwenden, können Sie die App trotzdem für Tester auf Windows 10-Geräten verfügbar machen, indem Sie wie oben beschrieben [Angebotscodes generieren](generate-promotional-codes.md). Jeder Benutzer mit einem der Angebotscodes Ihrer App kann diese auf ein Windows 10-Gerät herunterladen, auch wenn Sie dessen E-Mail-Adresse hier nicht eingegeben haben.

Um diese Option zu verwenden, aktivieren Sie beim Übermitteln Ihrer App auf der Seite **Preise und Verfügbarkeit** im Abschnitt [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) den Wert **Diese App im Store ausblenden und nur für die unten angegebenen Kunden verfügbar machen, die diese App auf Windows Phone 8.x-Geräten herunterladen können. Auf Windows 10-Geräten kann ein Werbecode zum Herunterladen dieser App verwendet werden**.

Wenn Sie diese Option auswählen, beachten Sie Folgendes:

-   Diese Option kann nur ausgewählt werden, wenn Sie die App zuvor noch nie über die Option [Verteilung und Sichtbarkeit](set-app-pricing-and-availability.md#distribution-and-visibility) mit der Einstellung **Jeder kann Ihre App im Store finden** veröffentlicht haben.
-   Für Ihre App muss der Preis **Kostenlos** festgelegt sein, damit sie von den Testern kostenlos heruntergeladen werden kann.
-   Ihre Tester können die App nur unter Windows Phone 8.1 und früheren Versionen herunterladen. Tester müssen zur Verwendung der App über ein Windows Phone-Einzelhandelsgerät verfügen, das jedoch nicht entsperrt oder registriert sein muss.
-   Ihre Tester benötigen ein Microsoft-Konto, um auf den Windows Store zuzugreifen und die App herunterzuladen. Sie benötigen von jedem Tester die E-Mail-Adresse seines Microsoft-Kontos, um ihn in Ihre Liste aufzunehmen. Unter [Konto erstellen](http://go.microsoft.com/fwlink/p/?LinkId=618945) können Tester ein neues Microsoft-Konto erstellen.
-   Sie können in dem Textfeld bis zu 10.000 E-Mail-Adressen bereitstellen.
-   E-Mail-Adressen müssen durch Semikolons getrennt sein.
-   Sie können später weitere Adressen hinzufügen, aber Sie müssen dazu eine neue Einreichung erstellen.
-   Sie können den Zugriff auf die App nicht widerrufen, nachdem sie von den Testern heruntergeladen wurde. Nachdem sie die App heruntergeladen haben, können sie diese weiterhin verwenden, und sie erhalten sämtliche Updates, die Sie übermitteln.


<!--HONumber=May16_HO2-->


