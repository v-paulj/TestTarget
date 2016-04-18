---
Description: Wenn Ihre App ein AdMediatorControl- oder ein AdControl-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre Anzeigenfüllrate und Ihren Umsatz steigern, indem Sie in Ihrer App Microsoft-Partneranzeigen anzeigen.
title: Flight-Pakete
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
---

# Flight-Pakete

Mithilfe von Flight-Paketen können Sie Pakete an eine begrenzte Testgruppe verteilen. 

Flight-Pakete ermöglichen Ihnen das Bereitstellen anderer Pakete für eine festgelegte Gruppe von Testern, ohne die Benutzererfahrung Ihrer übrigen Kunden zu unterbrechen. Nur die Pakete unterscheiden sich. Die Details des Store-Eintrags sind für alle Kunden gleich.

Beachten Sie, dass Flight-Pakete ebenso wie eine reguläre Übermittlung ohne Test-Flight den [Zertifizierungsprozess](the-app-certification-process.md) bestehen müssen. Wenn Sie später Pakete aus einem Flight-Paket für alle Ihre Kunden bereitstellen möchten, können Sie diese Pakete wie unten beschrieben in Ihre Übermittlung ohne Test-Flight ziehen.

Beim Einrichten von Flight-Paketen können Sie bestimmte Personen auswählen, die bestimmte Pakete erhalten sollen, indem Sie sie einer Test-Flight-Gruppe hinzufügen. Benutzer in einer Test-Flight-Gruppe, die ein Gerät mit einer Windows 10-Version verwenden, die Flight-Pakete unterstützt (Windows.Desktop Build 10586 oder höher; Windows.Mobile Build 10586.63 oder höher), erhalten Pakete aus dem für die jeweilige Gruppe festlegten Test-Flight. Benutzer, die nicht einer Ihrer Test-Flight-Gruppen hinzugefügt wurden, oder ein Gerät verwenden, das Flight-Pakete nicht unterstützt, erhalten Pakete aus der Übermittlung ohne Test-Flight.

Nachdem Sie eine Übermittlung für Ihre App veröffentlicht haben, ist auf der Seite App-Übersicht der Abschnitt **Flight-Pakete** vorhanden. Klicken Sie auf **New package flight**, um zu beginnen. Wenn Sie noch keine Test-Flight-Gruppen eingerichtet haben, werden Sie aufgefordert, eine zu erstellen, bevor Sie fortfahren können.

## Erstellen einer neuen Test-Flight-Gruppe

In Test-Flight-Gruppen können Sie die Personen festlegen, die Sie in die Gruppe aufnehmen möchten. Um Ihre Pakete mit Test-Flight zu erhalten, muss jede Person über ein Microsoft-Konto im Store authentifiziert werden, das der von Ihnen angegebenen E-Mail-Adresse zugeordnet ist, und ein Windows 10-Gerät (wie oben angegeben) zum Herunterladen der App verwenden.

Wenn Sie eine Test-Flight-Gruppe erstellen, müssen Sie ihr einen Namen geben. Jede Test-Flight-Gruppe muss mindestens eine E-Mail-Adresse und kann höchstens 10.000 E-Mail-Adressen enthalten. Geben Sie E-Mail-Adressen direkt in das Feld ein (getrennt durch Leerzeichen, Kommas oder Semikolons), oder klicken Sie auf den Link **CSV importieren**, um die Test-Flight-Gruppe aus einer Liste von E-Mail-Adressen in einer CSV-Datei zu erstellen.

Klicken Sie auf **Gruppe erstellen**, um die Gruppe zu speichern und mit der Einrichtung des Flight-Pakets fortzufahren.

> **Wichtig** Vergewissern Sie sich, dass Sie alle erforderlichen Einverständniserklärungen von den Personen eingeholt haben, die Sie Ihrer Test-Flight-Gruppe hinzufügen, und dass diese Personen sich bewusst sind, dass sie andere Pakete als die aus Ihrer Übermittlung ohne Test-Flight erhalten. 
> Sie sollten sich auch überlegen, wie die Personen in Ihrem Flight-Paket Ihnen Feedback zur App geben können. Wir empfehlen das [Hinzufügen eines Steuerelements in Ihre App, um den Feedback-Hub zu starten](../monetize/launch-feedback-hub-from-your-app.md). Kunden können auf diese Weise ihre Eingabe direkt bereitstellen. Prüfen Sie das Feedback anschließend im [Feedbackbericht](feedback-report.md)) der App.

Wenn Sie Ihre Test-Flight-Gruppe später bearbeiten möchten, klicken Sie auf **Export .csv**, um die Gruppeninformationen in einer CSV-Datei zu speichern. Nehmen Sie in dieser Datei Ihre Änderungen vor, und klicken Sie dann auf **CSV importieren**, um die Gruppenmitgliedschaft mit der neuen Version zu aktualisieren. Beachten Sie, dass es bis zu 30 Minuten dauern kann, bis Änderungen an der Gruppenmitgliedschaft implementiert werden.

## Erstellen eines neuen Flight-Pakets

Nachdem Sie Ihre erste Test-Flight-Gruppe erstellt haben, wird eine Seite angezeigt, auf der Sie Details hinzufügen können, um die Einrichtung abzuschließen. Sie müssen dem Flight-Paket einen Namen geben und mindestens eine Test-Flight-Gruppe festlegen. Auf dieser Seite haben Sie auch die Möglichkeit, eine neue Gruppe einzurichten.

Klicken Sie auf **Create flight**, nachdem Sie den Namen eingegeben und die Test-Flight-Gruppe(n) ausgewählt haben. Diese Details lassen sich später nicht mehr ändern (Sie können sie jedoch jederzeit löschen, ein neues Flight-Paket erstellen und dieses verwenden).

> Hinweis: Wenn Sie mehrere Flight-Pakete haben, müssen Sie jedem einen Rang zuweisen. Weitere Informationen finden Sie nachfolgend unter „Weitere Flight-Pakete hinzufügen und nach Rang sortieren“.

## Festlegen von Paketen zum Einfügen in Ihr Flight-Paket

Nachdem Sie die Details des Flight-Pakets gespeichert haben, wird die Übersicht dazu angezeigt. Klicken Sie auf **Pakete**, um festzulegen, welche Pakete das Test-Flight enthalten soll.

Sie haben die Option, Pakete auszuwählen, die einer vorherigen Übermittlung zugeordnet waren (entweder einer Übermittlung ohne Test-Flight einem Ihrer anderen Flight-Pakete, falls Sie mehrere haben). Wenn Sie für dieses Flight-Paket neue Pakete verwenden möchten, können Sie diese hier hochladen (mit dem gleichen Verfahren wie beim [Hochladen von App-Paketen](upload-app-packages.md) bei einer regulären Übermittlung ohne Test-Flight). Wenn Sie alle Pakete für dieses Flight-Paket angegeben haben, klicken Sie auf **Speichern**.

Bedenken Sie, dass die Personen in Ihrer Test-Flight-Gruppe nur Pakete abrufen können, die in der Übermittlung eines für diese Gruppe verfügbaren Flight-Pakets enthalten sind. Sie haben keinen Zugriff auf Pakete Ihrer Übermittlung ohne Test-Flight. Achten Sie darauf, Pakete bereitzustellen, die alle Gerätetypen unterstützen, die von den Kunden verwendet werden, die dieses Flight-Paket herunterladen. In den meisten Fällen sollten Sie Pakete bereitstellen, die den gleichen Satz von Gerätefamilien unterstützen, die auch von Ihrer Übermittlung ohne Test-Flight unterstützt werden.

Denken Sie auch daran, dass die Store-Produktinformationen für Ihre Übermittlung ohne Test-Flight einschließlich der Verfügbarkeit für Gerätefamilien für Kunden in einer Test-Flight-Gruppe verwendet wird. Wenn Ihre Übermittlung ohne Test-Flight keine Pakete enthält, die eine bestimmte Gerätefamilie unterstützen, können Kunden in Ihrem Test-Flight diese App also nicht auf Geräte dieses Typs herunterladen, auch wenn Ihr Flight-Paket Pakete enthält, die diese Gerätefamilie unterstützen.

## Optionen für das Konfigurieren zusätzlicher Flight-Pakete

Ihr Flight-Paket wird standardmäßig veröffentlicht und Ihrer Test-Flight-Gruppe zur Verfügung gestellt, sobald der Zertifizierungsprozess abgeschlossen ist. Wenn Sie das [Veröffentlichungsdatum](set-app-pricing-and-availability.md#publish-date) ändern oder [Hinweise für die Zertifizierung](notes-for-certification.md) hinzufügen möchten, haben Sie im Abschnitt **Optionen** die Möglichkeit dazu. Klicken Sie auf **Speichern**, um zur Flight-Paket-Übersicht zurückzukehren. 

## Übermitteln des Flight-Paket an den Store

Wenn Sie Pakete angegeben und alle erforderlichen Optionen konfiguriert haben, klicken Sie auf **An Store übermitteln**. Ihr Flight-Paket durchläuft anschließend den [App-Zertifizierungsprozess](the-app-certification-process.md). Beachten Sie, dass Pakete in Ihrem Flight-Paket wie bei allen Übermittlungen den [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) entsprechen müssen.

Personen, die in Ihren Test-Flight-Gruppen diesem Flight-Paket zugeordnet sind und Ihre App bereits haben, erhalten jetzt ein Update mit den Paketen, die Sie in Ihrem Flight-Paket bereitgestellt haben. Wenn diese Personen Ihre App noch nicht haben, erhalten sie die Pakete aus Ihrem Flight-Paket, wenn sie es installieren. 

> Hinweis: Personen, die ein Paket haben, das nur in einem Flight-Paket verfügbar ist, können die App mit Sternen bewerten und Rezensionen hinterlassen. Ihre Bewertungen und Rezensionen werden jedoch nicht für andere Kunden angezeigt. (Dies schließt ältere 7.x- oder 8.0 XAP-Pakete aus. Bewertungen und Rezensionen von Mitgliedern Ihrer Flight-Gruppen, die diese Pakete verwenden, sind für andere Kunden sichtbar.) Sie können ihr Feedback von allen Kunden, einschließlich derer in Ihrer Flight-Gruppen, in den Bewertungs- und Rezensionsberichten für die App anzeigen.

## Aktualisieren oder Ändern Ihres Flight-Pakets

Zum Erstellen einer neuen Übermittlung für ein vorhandenes Flight-Paket klicken Sie in der App-Übersicht neben dem Test-Flight-Namen auf **Update**. Anschließend können Sie genau wie bei einer Übermittlung ohne Test-Flight neue Pakete hochladen (und nicht benötigte Pakete entfernen). Nehmen Sie alle weiteren erforderlichen Änderungen vor, und klicken Sie dann auf **An Store übermitteln**, um das aktualisierte Flight-Paket an den [App-Zertifizierungsprozess](the-app-certification-process.md) zu senden.

Wenn Sie einen vorhandenen Flight ändern möchten, ohne ein neues Update zu erstellen, klicken Sie neben dem Flight-Namen auf **Modify**. Dadurch können Sie Details wie die Flight-Gruppen, den Name und den Rang ändern, ohne dass das Flight-Paket erneut zertifiziert werden muss.

## Weitere Flight-Pakete hinzufügen und nach Rang sortieren

Sie können mehrere Flight-Pakete für dieselbe App erstellen, um unterschiedliche Pakete an verschiedene Kundengruppen zu verteilen. 

Sobald Sie Ihr erstes Flight-Paket erstellt haben, können Sie mit dem oben beschriebenen Vorgang ein weiteres erstellen. Wenn Sie bereits ein Flight-Paket erstellt haben, besteht der einzige Unterschied darin, dass Sie im Abschnitt **Rang** die Reihenfolge der Priorität aller Flight-Pakete angeben müssen. Anhand dieser kann der Store bestimmen, welche Pakete die einzelnen Kunden erhalten sollen, auch wenn sie mehreren Ihrer Test-Flight-Gruppen angehören. Benutzer in den Flight-Gruppen erhalten immer das Paket mit dem höchsten Rang, das für sie verfügbar ist. Dies gilt selbst dann, wenn ein Flight-Paket mit niedrigerem Rang Pakete mit einer höheren Versionsnummer enthält.

Ihr neues Flight-Paket erhält standardmäßig den höchsten Rang. Wenn Sie den Rang ändern möchten, verschieben Sie das Paket nach unten (oder wieder nach oben), um es zwischen Ihren anderen Flight-Paketen richtig zu positionieren.

Beachten Sie, dass eine Übermittlung ohne Test-Flight immer den niedrigsten Rang erhält. Das heißt, dass Personen, die keiner Ihrer Test-Flight-Gruppen angehören, nur Pakete aus Ihrer Übermittlung ohne Test-Flight aus dem Store abrufen können. Personen in einer Test-Flight-Gruppe erhalten immer Pakete aus dem Flight-Paket mit dem höchsten Rang, das für sie verfügbar ist (jedoch nie aus der Übermittlung ohne Test-Flight). Dadurch sind Sie flexibel beim Bestimmen, wie Ihre Pakete an Benutzer verteilt werden, die u. U. in mehreren Ihrer Test-Flight-Gruppen Mitglied sind.

Nehmen wir beispielsweise an, dass Sie neben Ihrer regulären Übermittlung ohne Test-Flight zwei Flight-Pakete erstellen möchten: eines, das relativ stabil und für den Test mit einer breiten Zielgruppe bereit ist, und eines, bei dem Sie sich nicht so sicher sind und es auf wenige Tester beschränken möchten. Sie erstellen eine Test-Flight-Gruppe mit dem Namen „Tester“ und fügen sie einem Flight-Paket mit dem Namen „Test-Flight für Tester“ hinzu. Anschließend erstellen Sie eine Test-Flight-Gruppe mit dem Namen „Interessierte Benutzer“ und fügen sie einem anderen Flight-Paket mit dem Namen „Test-Flight für Interessierte Benutzer“ hinzu. Wenn Sie „Test-Flight für Tester“ einen höheren Rang als „Test-Flight für Interessierte Benutzer“ zuweisen, können Sie Pakete, bei denen Sie sich relativ sicher sind, in „Test-Flight für Interessierte Benutzer“ verwenden, und Pakete mit höherem Risiko, die nur für Tester vorgesehen sind, in „Test-Flight für Tester“. Mitglieder der Tester-Gruppe erhalten immer die Pakete, die Sie in „Test-Flight für Tester“ bereitstellen, auch wenn sie ebenfalls der Gruppe „Interessierte Benutzer“ angehören. (Wenn sich später dann herausstellt, dass die Pakete in „Test-Flight für Tester“ problemlos ausgeführt werden, können Sie „Test-Flight für Interessierte Benutzer“ so aktualisieren, dass die ursprünglich an „Test-Flight für Tester“ verteilten Pakete verwendet werden. Möglicherweise können Sie diese Pakete irgendwann auch in Ihrer Übermittlung ohne Test-Flight verwenden.

## Pakete aus einem Flight-Paket für alle Kunden zur Verfügung stellen

Wenn Sie eines oder mehrere Pakete in einem veröffentlichten Flight-Paket für Kunden verfügbar machen möchten, die keiner Test-Flight-Gruppe angehören, können Sie Ihre Übermittlung ohne Test-Flight entsprechend aktualisieren, ohne dieselben Pakete erneut hochladen zu müssen. 

Bei der Erstellung Ihrer neuen Übermittlung wird auf der Seite [**Packages**](upload-app-packages.md) eine Dropdownliste mit der Option zum Kopieren von Paketen aus einer vorherigen Übermittlung bzw. einem Flight-Paket angezeigt. Wählen Sie das Flight-Paket Flug mit den Paketen, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete darin auswählen, um sie in die Übermittlung ohne Test-Flight aufzunehmen.

Beachten Sie, dass dieselben Regeln für Paketvalidierung gelten, auch wenn Pakete aus einer zuvor veröffentlichten Übermittlung verwendet werden. 

## Löschen eines Flight-Pakets

Zum Löschen eines Flight-Paket, das Sie nicht mehr unterstützen möchten, klicken Sie in der App-Übersicht auf dessen Namen. Klicken Sie auf der Flight-Paket-Übersichtsseite auf **Delete package flight**. (Wenn Sie gerade eine unveröffentlichte Übermittlung des Flight-Pakets ausführen, müssen Sie diese zunächst löschen.) Dies kann bis zu 30 Minuten dauern.

Hinweis: Wenn Sie ein Test-Flight löschen, erhalten alle Kunden in diesem Test-Flight ein App-Update, wenn ein Paket mit einer höheren Versionsnummer als im Flight-Paket vorhanden ist (bzw. sobald ein solches Paket verfügbar wird). Wenn Benutzer die App deinstallieren und später erneut installieren, wird dies als neuer Erwerb behandelt, und die Benutzer erhalten die höchste derzeit verfügbare Version. 


<!--HONumber=Mar16_HO5-->


