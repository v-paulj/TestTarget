---
author: mijacobs
Description: Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen.
title: "Übersicht über unformatierte Benachrichtigungen"
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 4b487e44c7acd882a86c0b24dd9994092d976b06

---

# Übersicht über unformatierte Benachrichtigungen





Unformatierte Benachrichtigungen sind kurze, allgemeine Pushbenachrichtigungen. Sie dienen ausschließlich zu Anweisungszwecken und enthalten keine UI-Komponente. Wie bei anderen Pushbenachrichtigungen übermittelt das Feature für den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS) unformatierte Benachrichtigungen von Ihrem Clouddienst an Ihre App.

Unformatierte Benachrichtigungen können für verschiedenste Zwecke verwendet werden, z. B. zum Auslösen einer Hintergrundaufgabe in der App, wenn der Benutzer der App die entsprechende Berechtigung erteilt hat. Durch die Verwendung von WNS für die Kommunikation mit der App entfällt der Verarbeitungsaufwand für das Erstellen dauerhafter Socketverbindungen, Senden von HTTP GET-Nachrichten und andere Verbindungen zwischen Dienst und App.

**Wichtig**   Um die Funktionsweise von unformatierten Benachrichtigungen verstehen zu können, sollten Sie mit den in [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](tiles-and-notifications-windows-push-notification-services--wns--overview.md) erörterten Konzepten vertraut sein.

 

Wie bei Popup-, Kachel- und Signalpushbenachrichtigungen wird eine unformatierte Benachrichtigung vom Clouddienst Ihrer App über einen zugewiesenen Kanal-URI (Uniform Resource Identifier) an WNS gesendet. WNS wiederum übermittelt die Benachrichtigung an das Gerät und das Benutzerkonto, das diesem Kanal zugeordnet ist. Im Gegensatz zu anderen Pushbenachrichtigungen weisen unformatierte Benachrichtigungen kein bestimmtes Format auf. Der Inhalt der Nutzlast wird ausschließlich durch die App definiert.

Als Beispiel für eine App, die von der Verwendung unformatierter Benachrichtigungen profitieren könnte, nehmen wir hier eine theoretische Zusammenarbeits-App für Dokumente. Angenommen, zwei Benutzer bearbeiten gleichzeitig das gleiche Dokument. Der Clouddienst, der das freigegebene Dokument hostet, könnte jeden Benutzer mithilfe von unformatierten Benachrichtigungen informieren, wenn Änderungen vom anderen Benutzer vorgenommen werden. Die unformatierten Benachrichtigungen müssen dabei nicht die Änderungen am Dokument enthalten. Stattdessen weisen sie die App-Instanzen der beiden Benutzer an, eine Verbindung mit dem zentralen Speicherort herzustellen und die verfügbaren Änderungen zu synchronisieren. Durch die Verwendung von unformatierten Benachrichtigungen entfällt für die App und ihren Clouddienst der Mehraufwand, während der gesamten Zeit, in der das Dokument geöffnet ist, dauerhafte Verbindungen aufrecht zu erhalten.

## <span id="How_raw_notifications_work"></span><span id="how_raw_notifications_work"></span><span id="HOW_RAW_NOTIFICATIONS_WORK"></span>Funktionsweise von unformatierten Benachrichtigungen


Alle unformatierten Benachrichtigungen sind Pushbenachrichtigungen. Daher gilt die zum Senden und Empfangen von Pushbenachrichtigungen erforderliche Konfiguration auch für unformatierte Benachrichtigungen:

-   Sie benötigen einen gültigen WNS-Kanal zum Senden von unformatierten Benachrichtigungen. Weitere Informationen zum Anfordern eines Pushbenachrichtigungskanals finden Sie unter [Anfordern, Erstellen und Speichern eines Benachrichtigungskanals](https://msdn.microsoft.com/library/windows/apps/hh465412).
-   Die **Internet**-Funktion muss im App-Manifest hinzugefügt werden. Im Manifest-Editor von Microsoft Visual Studio befindet sich diese Option auf der Registerkarte **Funktionen** unter **Internet (Client)**. Weitere Informationen finden Sie unter [**Funktionen**](https://msdn.microsoft.com/library/windows/apps/br211422).

Für den Textkörper der Benachrichtigung wird ein von der App definiertes Format verwendet. Der Client empfängt die Daten als eine mit Null beendete Zeichenfolge (**HSTRING**), die nur von der App verstanden werden muss.

Wenn der Client offline ist, werden unformatierte Benachrichtigungen nur dann von WNS zwischengespeichert, wenn der [X-WNS-Cache-Policy](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache)-Header in der Benachrichtigung enthalten ist. Es wird jedoch jeweils nur eine unformatierte Benachrichtigung zwischengespeichert und übermittelt, sobald das Gerät wieder online ist.

Auf dem Client gibt es nur drei Möglichkeiten für die Verarbeitung einer unformatierten Benachrichtigung: Sie wird durch ein Benachrichtigungsübermittlungsereignis an die aktive App übermittelt, an eine Hintergrundaufgabe gesendet oder verworfen. Ist der Client offline, wenn WNS versucht, eine unformatierte Benachrichtigung zu übermitteln, wird die Benachrichtigung daher verworfen.

## <span id="Creating_a_raw_notification"></span><span id="creating_a_raw_notification"></span><span id="CREATING_A_RAW_NOTIFICATION"></span>Erstellen einer unformatierten Benachrichtigung


Mit Ausnahme der folgenden Unterschiede ist das Senden einer unformatierten Benachrichtigung mit dem einer Kachel-, Popup- oder Signalpushbenachrichtigung identisch:

-   Der HTTP Content-Type-Header muss auf "application/octet-stream" festgelegt werden.
-   Der HTTP [X-WNS-Type](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_type)-Header muss auf "wns/raw" festgelegt werden.
-   Der Textkörper der Benachrichtigung kann eine beliebige Nutzlast kleiner 5 KB enthalten.

Unformatierte Benachrichtigungen sollen als Kurznachrichten verwendet werden, die eine Aktion der App auslösen (z. B. Herstellen einer direkten Verbindung mit dem Dienst, um eine größere Datenmenge zu synchronisieren, oder Durchführen einer lokalen Statusänderung auf Grundlage des Benachrichtigungsinhalts). Beachten Sie, dass die Übermittlung von WNS-Pushbenachrichtigungen nicht garantiert werden kann. In der App und dem Clouddienst muss daher die Möglichkeit berücksichtigt werden, dass die unformatierte Benachrichtigung den Client nicht erreicht, z. B. wenn dieser offline ist.

Weitere Informationen zum Senden von Pushbenachrichtigungen finden Sie unter [Schnellstart: Senden einer Pushbenachrichtigung](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## <span id="Receiving_a_raw_notification"></span><span id="receiving_a_raw_notification"></span><span id="RECEIVING_A_RAW_NOTIFICATION"></span>Empfangen einer unformatierten Benachrichtigung


Es sind zwei Mechanismen verfügbar, über die Ihre App unformatierte Benachrichtigungen empfangen kann:

-   Durch [Benachrichtigungsübermittlungsereignisse](#notification_delivery_events) während die App ausgeführt wird.
-   Durch [von der unformatierten Benachrichtigung ausgelöste Hintergrundaufgaben](#bg_tasks), wenn die App Hintergrundaufgaben ausführen kann.

Eine App kann beide Mechanismen zum Empfangen von unformatierten Benachrichtigungen verwenden. Bei einer App, die sowohl den Benachrichtigungsübermittlungs-Ereignishandler als auch von unformatierten Benachrichtigungen ausgelöste Hintergrundaufgaben implementiert, hat das Benachrichtigungsübermittlungsereignis Priorität, wenn die App ausgeführt wird.

-   Ist die App aktiv, hat das Benachrichtigungsübermittlungsereignis Vorrang vor der Hintergrundaufgabe, und die App erhält als Erstes die Gelegenheit, die Benachrichtigung zu verarbeiten.
-   Der Benachrichtigungsübermittlungs-Ereignishandler kann festlegen, dass die unformatierte Benachrichtigung bei der Beendigung des Handlers nicht an die Hintergrundaufgabe übergeben werden soll, indem er die [**PushNotificationReceivedEventArgs.Cancel**](https://msdn.microsoft.com/library/windows/apps/br241297)-Eigenschaft des Ereignisses auf **true** festlegt. Wenn die **Cancel**-Eigenschaft auf **false** oder nicht festgelegt ist (der Standardwert ist **false**), löst die unformatierte Benachrichtigung die Hintergrundaufgabe aus, nachdem der Benachrichtigungsübermittlungs-Ereignishandler beendet wurde.

### <span id="notification_delivery_events"></span><span id="NOTIFICATION_DELIVERY_EVENTS"></span>Benachrichtigungsübermittlungsereignisse

Ihre App kann über ein Benachrichtigungsübermittlungsereignis ([**PushNotificationReceived**](https://msdn.microsoft.com/library/windows/apps/br241292)) unformatierte Benachrichtigungen erhalten, während die App verwendet wird. Wenn der Clouddienst eine unformatierte Benachrichtigung sendet, kann die aktive App die Benachrichtigung empfangen, indem sie das Benachrichtigungsübermittlungsereignis für den Kanal-URI behandelt.

Falls die App nicht aktiv ist und keine [Hintergrundaufgaben](#bg_tasks) verwendet, werden alle an sie gesendeten unformatierten Benachrichtigungen beim Empfang von WNS verworfen. Um zu verhindern, dass Ressourcen des Clouddiensts vergeudet werden, sollten Sie ggf. eine Logik implementieren, mit der der Dienst nachverfolgen kann, ob die App aktiv ist. Für diese Informationen kommen zwei Quellen in Frage: Eine App kann dem Dienst explizit mitteilen, dass sie für den Empfang von Benachrichtigungen bereit ist, und WNS kann dem Dienst mitteilen, wann er die Übermittlung beenden soll.

-   **Die App benachrichtigt den Clouddienst**: Die App kann eine Verbindung mit dem Dienst herstellen, um ihm mitzuteilen, dass sie im Vordergrund ausgeführt wird. Der Nachteil bei diesem Ansatz ist, dass die App u. U. sehr oft eine Verbindung mit dem Dienst herstellt. Der Vorteil ist jedoch, dass der Dienst immer weiß, wann die App für den Empfang eingehender unformatierter Benachrichtigungen bereit ist. Ein weiterer Vorteil: Wenn die App eine Verbindung mit dem Dienst herstellt, kann der Dienst unformatierte Benachrichtigungen an die spezifische Instanz der App senden, anstatt einen Broadcast zu senden.
-   **Der Clouddienst antwortet auf WNS-Antwortnachrichten**: Der App-Dienst kann anhand der von WNS zurückgegebenen Informationen [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) und [X-WNS-DeviceConnectionStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_dcs) feststellen, wann keine unformatierten Benachrichtigungen mehr an die App gesendet werden sollen. Wenn Ihr Dienst eine Benachrichtigung als HTTP-POST an einen Kanal sendet, kann er als Antwort eine der folgenden Nachrichten erhalten:

    -   **X-WNS-NotificationStatus: dropped**: Gibt an, dass die Benachrichtigung nicht vom Client empfangen wurde. Die **dropped**-Antwort ist in der Regel darauf zurückzuführen, dass die App auf dem Gerät des Benutzers nicht mehr im Vordergrund ausgeführt wird.
    -   **X-WNS-DeviceConnectionStatus: disconnected** oder **X-WNS-DeviceConnectionStatus: tempconnected**: Gibt an, dass der Windows-Client nicht mehr mit WNS verbunden ist. Diese Nachricht von WNS kann nur empfangen werden, wenn Sie sie durch Festlegen des [X-WNS-RequestForStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_request)-Headers in der HTTP-POST-Nachricht der Benachrichtigung anfordern.

    Der Clouddienst Ihrer App kann die Informationen in diesen Statusnachrichten verwenden, um Kommunikationsversuche durch unformatierte Benachrichtigungen zu beenden. Der Dienst kann das Senden von unformatierten Benachrichtigungen fortsetzen, wenn die App wieder im Vordergrund ausgeführt wird und eine Verbindung mit ihm herstellt.

    Sie sollten sich nicht auf [X-WNS-NotificationStatus](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_notification) verlassen, um festzustellen, ob die Benachrichtigung erfolgreich an den Client übermittelt wurde.

    Weitere Informationen finden Sie unter [Anforderungs- und Antwortheader des Pushbenachrichtigungsdiensts](https://msdn.microsoft.com/library/windows/apps/hh465435).

### <span id="bg_tasks"></span><span id="BG_TASKS"></span>Von unformatierten Benachrichtigungen ausgelöste Hintergrundaufgaben

**Wichtig**   Vor der Verwendung von Hintergrundaufgaben für unformatierte Benachrichtigungen muss einer App über [**BackgroundExecutionManager.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) die Berechtigung zum Ausführen von Hintergrundaufgaben erteilt werden.

 

Die Hintergrundaufgabe muss mit einem [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) registriert sein. Ist die Aufgabe nicht registriert, wird sie beim Empfang einer unformatierten Benachrichtigung nicht ausgeführt.

Eine von einer unformatierten Benachrichtigung ausgelöste Hintergrundaufgabe ermöglicht es dem Clouddienst Ihrer App, eine Verbindung mit der App herzustellen, auch wenn die App nicht ausgeführt wird (sie kann die Ausführung jedoch auslösen). Dies ist möglich, ohne dass die App eine dauerhafte Verbindung aufrecht erhalten muss. Unformatierte Benachrichtigungen sind der einzige Benachrichtigungstyp, der Hintergrundaufgaben auslösen kann. Doch obwohl Popup-, Kachel- und Signalpushbenachrichtigungen keine Hintergrundaufgaben auslösen können, können durch unformatierte Benachrichtigungen ausgelöste Hintergrundaufgaben Kacheln aktualisieren und Popupbenachrichtigungen über lokale API-Aufrufe aufrufen.

Zur Veranschaulichung der Funktionsweise von Hintergrundaufgaben, die durch unformatierte Benachrichtigungen ausgelöst werden, nehmen wir als Beispiel eine App zum Lesen von E-Books. Als Erstes kauft ein Benutzer (möglicherweise auf einem anderen Gerät) online ein Buch. Als Reaktion kann der Clouddienst der App eine unformatierte Benachrichtigung, deren Nutzlast besagt, dass das Buch gekauft wurde und von der App heruntergeladen werden soll, an jedes Gerät des Benutzers senden. Die App stellt dann eine direkte Verbindung mit dem Clouddienst der App her, um einen Hintergrunddownload des neuen Buchs zu starten, damit das Buch später, wenn der Benutzer die App startet, schon da ist und gelesen werden kann.

Um eine Hintergrundaufgabe mit einer unformatierten Benachrichtigung auszulösen, muss Ihre App:

1.  die Berechtigung zum Ausführen von Hintergrundaufgaben (die der Benutzer jederzeit widerrufen kann) über [**BackgroundExecutionManager.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) anfordern.
2.  die Hintergrundaufgabe implementieren. Weitere Informationen finden Sie unter [Unterstützen von Apps durch Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/hh977046).

Die Hintergrundaufgabe wird dann jedes Mal als Reaktion auf den [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) aufgerufen, wenn eine unformatierte Benachrichtigung für die App empfangen wird. Die Hintergrundaufgabe interpretiert die App-spezifische Nutzlast der unformatierten Benachrichtigung und führt die entsprechenden Aktionen aus.

Für jede App kann jeweils nur eine Hintergrundaufgabe ausgeführt werden. Wird eine Hintergrundaufgabe für eine App ausgelöst, für die bereits eine Hintergrundaufgabe ausgeführt wird, muss die erste Hintergrundaufgabe abgeschlossen werden, bevor die neue Aufgabe ausgeführt wird.

## <span id="Other_resources"></span><span id="other_resources"></span><span id="OTHER_RESOURCES"></span>Weitere Ressourcen


Weitere Informationen erhalten Sie, indem Sie das [Beispiel für unformatierte Benachrichtigungen](http://go.microsoft.com/fwlink/p/?linkid=241553) für Windows 8.1 und das [Beispiel für Pushbenachrichtigungen und regelmäßige Benachrichtigungen](http://go.microsoft.com/fwlink/p/?LinkId=231476) für Windows 8.1 herunterladen und den Quellcode in Ihrer Windows 10-App wiederverwenden.

## <span id="related_topics"></span>Verwandte Themen


* [Richtlinien für unformatierte Benachrichtigungen](https://msdn.microsoft.com/library/windows/apps/hh761463)
* [Schnellstart: Erstellen und Registrieren einer Hintergrundaufgabe, für die unformatierte Benachrichtigungen verwendet werden](https://msdn.microsoft.com/library/windows/apps/jj676800)
* [Schnellstart: Abfangen von Pushbenachrichtigungen für aktive Apps](https://msdn.microsoft.com/library/windows/apps/jj709908)
* [**RawNotification**](https://msdn.microsoft.com/library/windows/apps/br241304)
* [**BackgroundExecutionManager.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)
 

 







<!--HONumber=Jun16_HO4-->


