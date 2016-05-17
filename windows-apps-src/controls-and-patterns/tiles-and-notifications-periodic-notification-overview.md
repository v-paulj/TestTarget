---
author: mijacobs
Description: Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen.
title: Übersicht über regelmäßige Benachrichtigungen
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
label: TBD
template: detail.hbs
---

# Übersicht über regelmäßige Benachrichtigungen





Regelmäßige Benachrichtigungen – auch als abgerufene Benachrichtigungen bezeichnet – aktualisieren Kacheln und Signale in festgelegten Intervallen, indem sie Inhalte aus einem Clouddienst herunterladen. Zur Verwendung von regelmäßigen Benachrichtigungen muss Ihr Client-App-Code zwei Informationen bereitstellen:

-   Den URI (Uniform Resource Identifier) eines Webspeicherorts, von dem Windows Kachel- oder Signalupdates für Ihre App abfragt
-   Die Häufigkeit der URI-Abfrage

Regelmäßige Benachrichtigungen bieten Ihnen Live-Kachelaktualisierungen mit minimaler Investition in Clouddienst und Client. Sie stellen auch eine gute Methode zum Verteilen desselben Inhalts an eine große Zielgruppe dar.

**Hinweis**   Weitere Informationen finden Sie durch Herunterladen des [Beispiels für Pushbenachrichtigungen und regelmäßige Benachrichtigungen](http://go.microsoft.com/fwlink/p/?linkid=231476) für Windows 8.1 und die Wiederverwendung des Quellcodes in Ihrer App für Windows 10.

 

## <span id="How_it_works"></span><span id="how_it_works"></span><span id="HOW_IT_WORKS"></span>Funktionsweise


Regelmäßige Benachrichtigungen erfordern einen von Ihrer App gehosteten Clouddienst. Der Dienst wird regelmäßig von allen Benutzern abgefragt, die die App installiert haben. Bei jedem Abfrageintervall, wie z. B. nach einer Stunde, sendet Windows eine HTTP GET-Anforderung an den URI, lädt die angeforderten Inhalte, die als Reaktion auf die Anforderung zurückgegeben werden, für die Kachel oder das Signal herunter (als XML) und zeigt die Inhalte auf der App-Kachel an.

Beachten Sie, dass regelmäßige Benachrichtigungen nicht mit Popupbenachrichtigungen verwendet werden können. Popups werden am besten per [geplanter](https://msdn.microsoft.com/library/windows/apps/hh465417) oder per [Push](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)benachrichtigung übermittelt.

## <span id="URI_location_and_XML_content"></span><span id="uri_location_and_xml_content"></span><span id="URI_LOCATION_AND_XML_CONTENT"></span>URI-Speicherort und XML-Inhalt


Jede gültige HTTP- oder HTTPS-Webadresse kann als abzufragender URI verwendet werden.

Die Antwort des Cloudservers enthält die heruntergeladenen Inhalte. Die von dem URI zurückgegebenen Inhalte müssen mit den XML-Schemaspezifikationen für [Kacheln](tiles-and-notifications-adaptive-tiles-schema.md) oder [Signale](https://msdn.microsoft.com/library/windows/apps/br212851) übereinstimmen, und sie müssen in UTF-8 codiert sein. Sie können definierte HTTP-Header verwenden, um [Ablaufzeit](#expiry) oder [Tag](#taggo) für die Benachrichtigung anzugeben.

## <span id="Polling_Behavior"></span><span id="polling_behavior"></span><span id="POLLING_BEHAVIOR"></span>Abfrageverhalten


Rufen Sie eine der folgenden Methoden auf, um die Abfrage zu starten:

-   [
              **StartPeriodicUpdate**
            ](https://msdn.microsoft.com/library/windows/apps/hh701684) (Kachel)
-   [
              **StartPeriodicUpdate**
            ](https://msdn.microsoft.com/library/windows/apps/hh701611) (Badge)
-   [
              **StartPeriodicUpdateBatch**
            ](https://msdn.microsoft.com/library/windows/apps/hh967945) (Kachel)

Wenn Sie eine dieser Methoden aufrufen, wird der URI sofort abgefragt, und die Kachel oder das Signal wird mit den empfangenen Inhalten aktualisiert. Nach der ersten Abfrage stellt Windows weiter im angegebenen Intervall Updates bereit. Die Abfrage wird fortgeführt, bis Sie sie ausdrücklich (mit [**TileUpdater.StopPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701697)) anhalten, die App deinstalliert wird oder, im Falle einer sekundären Kachel, die Kachel entfernt wird. Andernfalls fragt Windows weiterhin Updates für die Kachel oder das Signal ab, auch wenn die App nie wieder gestartet wird.

### <span id="The_recurrence_interval"></span><span id="the_recurrence_interval"></span><span id="THE_RECURRENCE_INTERVAL"></span>Das Wiederholungsintervall

Das Wiederholungsintervall wird als Parameter der oben aufgeführten Methoden angegeben. Obwohl Windows versucht, den Abruf wie angefordert durchzuführen, kann das Intervall nicht immer genau eingehalten werden. Das angeforderte Abfrageintervall kann um bis zu 15 Minuten verzögert werden, wobei hierfür Windows zuständig ist.

### <span id="The_start_time"></span><span id="the_start_time"></span><span id="THE_START_TIME"></span>Die Startzeit

Optional können Sie eine bestimmte Uhrzeit angeben, zu der mit der Abfrage begonnen werden soll. Gehen wir von einer App aus, deren Kachelinhalt nur einmal täglich geändert wird. In diesem Fall empfehlen wir eine Abfrage nahe dem Zeitpunkt, zu dem auch Ihr Cloud-Dienst aktualisiert wird. Wenn z. B. auf einer Shoppingwebsite mit täglichen Angeboten die Tagesangebote um 08:00 Uhr veröffentlicht werden, sollten neue Kachelinhalte kurz nach 08:00 Uhr abgefragt werden.

Wenn Sie eine Startzeit angeben, werden beim ersten Aufruf der Methode sofort Inhalte abgefragt. Anschließend beginnt der regelmäßige Abruf innerhalb von 15 Minuten nach der angegebenen Startzeit.

### <span id="Automatic_retry_behavior"></span><span id="automatic_retry_behavior"></span><span id="AUTOMATIC_RETRY_BEHAVIOR"></span>Automatisches Wiederholungsverhalten

Der URI wird nur abgefragt, wenn das Geräte online ist. Wenn das Netzwerk verfügbar ist, jedoch keine Verbindung mit dem URI hergestellt werden kann, wird diese Iteration des Abfrageintervalls übersprungen und der URI beim nächsten Intervall erneut abgefragt. Falls ein Abfrageintervall erreicht wird, während das Gerät ausgeschaltet ist oder sich im Standbymodus oder Ruhezustand befindet, wird der URI abgefragt, wenn das Gerät wieder verfügbar ist.

## <span id="expiry"></span><span id="EXPIRY"></span>Ablauf der Kachel- und Signalbenachrichtigungen


Standardmäßig laufen die regelmäßigen Kachel- und Signalbenachrichtigungen drei Tage, nachdem sie heruntergeladen wurden, ab. Wenn eine Benachrichtigung abläuft, wird der Inhalt aus dem Signal, der Kachel oder der Warteschlange entfernt und nicht mehr angezeigt. Es wird empfohlen, eine explizite Ablaufzeit für alle regelmäßigen Kachel- und Signalbenachrichtigungen festzulegen. Verwenden Sie dabei eine für Ihre App sinnvolle Zeit, durch die sichergestellt wird, dass der Inhalt nur so lange beibehalten wird, wie er relevant ist. Eine explizite Ablaufzeit ist für Inhalte mit definierter Lebensdauer von großer Bedeutung. Durch sie wird weiterhin sichergestellt, dass veraltete Inhalte entfernt werden, wenn Ihr Clouddienst nicht verfügbar ist oder der Benutzer die Verbindung mit dem Netzwerk für längere Zeit trennt.

Ihr Cloud-Dienst legt ein Ablaufdatum und eine Ablaufzeit für eine Benachrichtigung fest, indem der Antwortnutzlast der HTTP-Header "X-WNS-Expires" hinzugefügt wird. Der HTTP-Header „X-WNS-Expires” entspricht dem [HTTP-Datumsformat](http://go.microsoft.com/fwlink/p/?linkid=253706). Weitere Informationen finden Sie unter [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) oder [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945).

Beispielsweise können Sie während eines aktiven Börsenhandelstags die Gültigkeitsdauer für eine Aktienpreisaktualisierung gegenüber dem Abfrageintervall verdoppeln (z. B. auf eine Stunde nach Empfang bei einer Abfrage zu jeder halben Stunde). Als weiteres Beispiel dient eine News-App, bei der festgestellt wird, dass ein Intervall von einem Tag für eine tägliche Kachelaktualisierung angemessen ist.

## <span id="taggo"></span><span id="TAGGO"></span>Regelmäßige Benachrichtigungen in der Benachrichtigungswarteschlange


Sie können regelmäßige Benachrichtigungen mit [Benachrichtigungszyklen](https://msdn.microsoft.com/library/windows/apps/hh781199) verwenden. Eine Kachel auf dem Startbildschirm zeigt standardmäßig den Inhalt einer einzelnen Benachrichtigung an, bis die aktuelle Benachrichtigung durch eine neue ersetzt wird. Bei aktivierten Benachrichtigungszyklen verbleiben bis zu fünf Benachrichtigungen in einer Warteschlange und werden nacheinander auf der Kachel angezeigt.

Hat die Warteschlange ihre maximale Kapazität von fünf Benachrichtigungen erreicht, ersetzt die nächste neue Benachrichtigung die älteste Benachrichtigung in der Warteschlange. Durch Festlegen von Tags für Ihre Benachrichtigungen können Sie die Ersetzungsrichtlinie der Warteschlange jedoch beeinflussen. Ein Tag ist eine app-spezifische Zeichenfolge von bis zu 16 alphanumerischen Zeichen (ohne Beachtung der Groß-/Kleinschreibung), die in der Antwortnutzlast im HTTP-Header [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) angegeben wird. Windows vergleicht das Tag einer eingehenden Benachrichtigung mit den Tags aller bereits in der Warteschlange vorhandenen Benachrichtigungen. Wird eine Übereinstimmung gefunden, ersetzt die neue Benachrichtigung die Benachrichtigung in der Warteschlange mit demselben Tag. Wird keine Übereinstimmung gefunden, wird die Standardersetzungsregel angewendet und die neue Benachrichtigung ersetzt die älteste Benachrichtigung in der Warteschlange.

Sie können die Benachrichtigungswarteschlange und Tags verwenden, um eine Vielzahl von Benachrichtigungsszenarien mit großem Funktionsumfang zu implementieren. So kann beispielsweise eine Aktien-App fünf Benachrichtigungen senden – jede für eine andere Aktie und markiert mit dem Aktiennamen. Dadurch enthält die Warteschlange niemals zwei Benachrichtigungen für die gleiche Aktie, wovon eine zwangsläufig nicht mehr aktuell wäre.

Weitere Informationen finden Sie unter [Verwenden der Benachrichtigungswarteschlange](https://msdn.microsoft.com/library/windows/apps/hh781199).

### <span id="Enabling_the_notification_queue"></span><span id="enabling_the_notification_queue"></span><span id="ENABLING_THE_NOTIFICATION_QUEUE"></span>Aktivieren der Benachrichtigungswarteschlange

Um eine Benachrichtigungswarteschlange zu implementieren, aktivieren Sie zunächst die Warteschlange für die Kachel (siehe [Verwendung der Benachrichtigungswarteschlange mit lokalen Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh465429)). Der Aufruf zum Aktivieren der Warteschlange muss nur einmal in der Lebensdauer Ihrer App ausgeführt werden, es schadet jedoch nicht, den Aufruf bei jedem Start der App auszuführen.

### <span id="Polling_for_more_than_one_notification_at_a_time"></span><span id="polling_for_more_than_one_notification_at_a_time"></span><span id="POLLING_FOR_MORE_THAN_ONE_NOTIFICATION_AT_A_TIME"></span>Abfragen von mehreren Benachrichtigungen gleichzeitig

Sie müssen einen eindeutigen URI für jede Benachrichtigung angeben, die Windows für Ihre Kachel herunterladen soll. Mit der [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945)-Methode können Sie bis zu fünf URIs gleichzeitig zur Verwendung mit der Benachrichtigungswarteschlange angeben. Jeder URI wird zur gleichen oder fast zur gleichen Zeit für eine einzige Benachrichtigungsnutzlast abgefragt. Jeder abgefragte URI kann seine eigene Ablaufzeit sowie einen Tag-Wert zurückgeben.

## <span id="related_topics"></span>Verwandte Themen


* [Richtlinien für regelmäßige Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [So wird's gemacht: Einrichten regelmäßiger Benachrichtigungen für Signale](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [So wird's gemacht: Einrichten regelmäßiger Benachrichtigungen für Kacheln](https://msdn.microsoft.com/library/windows/apps/hh761476)
 

 






<!--HONumber=May16_HO2-->


