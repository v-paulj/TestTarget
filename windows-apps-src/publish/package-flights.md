---
author: jnHs
Description: "Wenn Ihre App ein AdMediatorControl- oder ein AdControl-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre Anzeigenfüllrate und Ihren Umsatz steigern, indem Sie in Ihrer App Microsoft-Partneranzeigen anzeigen."
title: Flight-Pakete
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
translationtype: Human Translation
ms.sourcegitcommit: baa8212b7ec26be1f50e051fa06bff4bf8095227
ms.openlocfilehash: 34321a5fd3db10833b958049597ebf830599cfb4

---

# Flight-Pakete

Mithilfe von Flight-Paketen können Sie Pakete an eine begrenzte Testgruppe verteilen. 

Flight-Pakete ermöglichen Ihnen das Bereitstellen anderer Pakete für eine festgelegte Gruppe von Testern, ohne die Benutzererfahrung Ihrer übrigen Kunden zu unterbrechen. Nur die Pakete unterscheiden sich. Die Details des Store-Eintrags sind für alle Kunden gleich.

Beachten Sie, dass Flight-Pakete ebenso wie eine reguläre Übermittlung ohne Test-Flight den [Zertifizierungsprozess](the-app-certification-process.md) bestehen müssen. Wenn Sie später Pakete aus einem Flight-Paket für alle Ihre Kunden bereitstellen möchten, können Sie diese Pakete wie unten beschrieben in Ihre Übermittlung ohne Test-Flight ziehen.

Beim Einrichten von Flight-Paketen können Sie bestimmte Personen auswählen, die bestimmte Pakete erhalten sollen, indem Sie sie einer Test-Flight-Gruppe hinzufügen. Benutzer in einer Test-Flight-Gruppe, die ein Gerät mit einer Windows 10-Version verwenden, die Flight-Pakete unterstützt (Windows.Desktop Build 10586 oder höher; Windows.Mobile Build 10586.63 oder höher; oder Xbox One), erhalten die Pakete aus dem für die jeweilige Gruppe festlegten Test-Flight. Benutzer, die nicht einer Ihrer Test-Flight-Gruppen hinzugefügt wurden, oder ein Gerät verwenden, das Flight-Pakete nicht unterstützt, erhalten Pakete aus der Übermittlung ohne Test-Flight.

> **Wichtig** Personen in Ihren Test-Flight-Gruppen erhalten auf Desktops und mobilen Geräten die Pakete in Ihrem Test-Flight jedes Mal automatisch, wenn Sie Updates bereitstellen. **Personen in Ihren Test-Flight-Gruppen, die Xbox-Geräte verwenden, müssen jedoch manuell nach Updates suchen**, um die neuesten Pakete zu erhalten. Dabei müssen sie sicherstellen, dass sie mit ihrem Microsoft-Konto am Gerät angemeldet sind (mit der zugeordneten E-Mail-Adresse, die Sie in Ihre Test-Flight-Gruppe eingefügt haben).

Beachten Sie, dass das Flight-Pakete nicht über den [Windows Store für Unternehmen](https://www.microsoft.com/business-store) verteilt werden. Dies liegt daran, dass die Personen in Ihren Test-Flight-Gruppen mit ihren Microsoft-Konten angemeldet sein müssen, um ein Flight-Paket zu empfangen. Alle Käufe über den Windows Store für Unternehmen empfangen Ihre Pakete, wenn es keine Test-Flight-Pakete sind.

> **Tipp** Flight-Pakete bieten nur den von Ihnen ausgewählten und angegebenen Kunden Pakete an. Um Pakete an eine zufällige Auswahl von Kunden zu einem bestimmten Prozentsatz zu verteilen, können Sie [schrittweise Paketrollouts](gradual-package-rollout.md) verwenden. Sie können das Rollout auch mit Ihren Flight-Paketen kombinieren, wenn Sie ein Update schrittweise auf eine Ihrer Test-Flight-Gruppen verteilen möchten.

> Anders als Flight-Pakete werden schrittweise Paketrollouts auf Kunden angewendet, die Ihre App über den Windows Store für Unternehmen erwerben. 

Nachdem Sie eine Übermittlung für Ihre App veröffentlicht haben, ist auf der Seite App-Übersicht der Abschnitt **Flight-Pakete** vorhanden. Klicken Sie auf **New package flight**, um zu beginnen. Wenn Sie noch keine Test-Flight-Gruppen eingerichtet haben, werden Sie aufgefordert, eine zu erstellen, bevor Sie fortfahren können.

## Erstellen einer neuen Test-Flight-Gruppe

In Test-Flight-Gruppen können Sie die Personen festlegen, die Sie in die Gruppe aufnehmen möchten. Um Ihre Pakete mit Test-Flight zu erhalten, muss jede Person über ein Microsoft-Konto im Store authentifiziert werden, das der von Ihnen angegebenen E-Mail-Adresse zugeordnet ist, und ein Windows10-Gerät (wie oben angegeben) zum Herunterladen der App verwenden.

Wenn Sie eine Test-Flight-Gruppe erstellen, müssen Sie ihr einen Namen geben. Jede Test-Flight-Gruppe muss mindestens eine E-Mail-Adresse und kann höchstens 10.000 E-Mail-Adressen enthalten. Geben Sie E-Mail-Adressen direkt in das Feld ein (getrennt durch Leerzeichen, Kommas oder Semikolons), oder klicken Sie auf den Link **CSV importieren**, um die Test-Flight-Gruppe aus einer Liste von E-Mail-Adressen in einer CSV-Datei zu erstellen.

Klicken Sie auf **Gruppe erstellen**, um die Gruppe zu speichern und mit der Einrichtung des Flight-Pakets fortzufahren.

> **Wichtig** Vergewissern Sie sich, dass Sie alle erforderlichen Einverständniserklärungen von den Personen eingeholt haben, die Sie Ihrer Test-Flight-Gruppe hinzufügen, und dass diese Personen sich bewusst sind, dass sie andere Pakete als die aus Ihrer Übermittlung ohne Test-Flight erhalten. 
> Sie sollten sich auch überlegen, wie die Personen in Ihrem Flight-Paket Ihnen Feedback zur App geben können. Wir empfehlen das [Hinzufügen eines Steuerelements in Ihre App, um den Feedback-Hub zu starten](../monetize/launch-feedback-hub-from-your-app.md). Kunden können auf diese Weise ihre Eingabe direkt bereitstellen. Prüfen Sie das Feedback anschließend im [Feedbackbericht](feedback-report.md)) der App.

Wenn Sie Ihre Test-Flight-Gruppe später bearbeiten möchten, klicken Sie auf **Export .csv**, um die Gruppeninformationen in einer CSV-Datei zu speichern. Nehmen Sie in dieser Datei Ihre Änderungen vor, und klicken Sie dann auf **CSV importieren**, um die Gruppenmitgliedschaft mit der neuen Version zu aktualisieren. Beachten Sie, dass es bis zu 30Minuten dauern kann, bis Änderungen an der Mitgliedschaft der Test-Flight-Gruppe implementiert werden. Wenn Sie nach der Veröffentlichung des zugehörigen Flight-Pakets Personen zu einer Test-Flight-Gruppe hinzufügen, werden die Pakete für die neuen Personen automatisch bereitgestellt. Sie müssen keine neue Übermittlung für das Flight-Paket erstellen und veröffentlichen. 

## Erstellen eines neuen Flight-Pakets

Nachdem Sie Ihre erste Test-Flight-Gruppe erstellt haben, wird eine Seite angezeigt, auf der Sie Details hinzufügen können, um die Einrichtung abzuschließen. Sie müssen dem Flight-Paket einen Namen geben und mindestens eine Test-Flight-Gruppe festlegen. Auf dieser Seite haben Sie auch die Möglichkeit, eine neue Gruppe einzurichten.

Klicken Sie auf **Create flight**, nachdem Sie den Namen eingegeben und die Test-Flight-Gruppe(n) ausgewählt haben. Diese Details lassen sich später nicht mehr ändern (Sie können sie jedoch jederzeit löschen, ein neues Flight-Paket erstellen und dieses verwenden).

> Hinweis: Wenn Sie mehrere Flight-Pakete haben, müssen Sie jedem einen Rang zuweisen. Weitere Informationen finden Sie nachfolgend unter „Weitere Flight-Pakete hinzufügen und nach Rang sortieren“.

## Festlegen von Paketen zum Einfügen in Ihr Flight-Paket

Nachdem Sie die Details des Flight-Pakets gespeichert haben, wird die Übersicht dazu angezeigt. Klicken Sie auf **Pakete**, um festzulegen, welche Pakete das Test-Flight enthalten soll.

Sie haben die Option, Pakete auszuwählen, die einer vorherigen Übermittlung zugeordnet waren (entweder einer Übermittlung ohne Test-Flight einem Ihrer anderen Flight-Pakete, falls Sie mehrere haben). Wenn Sie für dieses Flight-Paket neue Pakete verwenden möchten, können Sie diese hier hochladen (mit dem gleichen Verfahren wie beim [Hochladen von App-Paketen](upload-app-packages.md) bei einer regulären Übermittlung ohne Test-Flight). Wenn Sie alle Pakete für dieses Flight-Paket angegeben haben, klicken Sie auf **Speichern**.

Wenn Ihre App mehrere Gerätefamilien unterstützt, stellen Sie sicher, dass Sie Pakete einschließen, um den gleichen Satz von Gerätefamilien in Ihrem Test-Flight zu unterstützen. Die Mitglieder Ihrer Test-Flight-Gruppen können **nur** Pakete aus diesem Test-Flight erhalten. Sie können nicht auf Pakete aus anderen Test-Flights oder aus Ihren Übermittlungen ohne Test-Flights zugreifen. 

Denken Sie auch daran, dass die Informationen zu Ihren Store-Einträgen aus Ihren Übermittlungen ohne Test-Flights stammen, einschließlich Informationen zu den von Ihrer App unterstützten Gerätefamilien. Die Kunden in Ihren Test-Flight-Gruppen können die App nur für Gerätefamilien herunterladen, die von Ihren Übermittlungen ohne Test-Flights unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung für Gerätefamilien](#device-family-support). 

## Schrittweises Paketrollout

Standardmäßig werden die Pakete in Ihrer Übermittlung für alle Personen in Ihrer Test-Flight-Gruppe gleichzeitig zur Verfügung gestellt. Um dies zu ändern, können Sie das Kontrollkästchen **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows10-Kunden)** aktivieren. Sie können einen Prozentsatz von Personen in Ihrer Test-Flight-Gruppe wählen, die die Pakete aus der neuen Übermittlung erhalten, sodass Sie das Feedback und die Analysedaten überwachen können, um sicherzustellen, dass Sie das Update zuverlässig veröffentlichen können, bevor Sie es umfassender für den Rest der Test-Flight-Gruppe bereitstellen. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung für Ihr Flight-Paket erstellen zu müssen. 

> **Wichtig** Beim schrittweisen Einführen von Paketen in einem Flight-Paket erhalten die Personen, die nicht in dem Prozentsatz enthalten sind, der Ihre neuen Pakete enthält, die Pakete der vorherigen Flight-Paket-Übermittlung (wenn kein Test-Flight mit höherem Rang für sie verfügbar ist).

Weitere Informationen finden Sie unter [Schrittweises Paketrollout](gradual-package-rollout.md).

## Optionen für das Konfigurieren zusätzlicher Flight-Pakete

Ihr Flight-Paket wird standardmäßig veröffentlicht und Ihrer Test-Flight-Gruppe zur Verfügung gestellt, sobald der Zertifizierungsprozess abgeschlossen ist. Wenn Sie das [Veröffentlichungsdatum](set-app-pricing-and-availability.md#publish-date) ändern oder [Hinweise für die Zertifizierung](notes-for-certification.md) hinzufügen möchten, haben Sie im Abschnitt **Optionen** die Möglichkeit dazu. Klicken Sie auf **Speichern**, um zur Flight-Paket-Übersicht zurückzukehren. 

## Übermitteln des Flight-Paket an den Store

Wenn Sie Pakete angegeben und alle erforderlichen Optionen konfiguriert haben, klicken Sie auf **An Store übermitteln**. Ihr Flight-Paket durchläuft anschließend den [App-Zertifizierungsprozess](the-app-certification-process.md). Beachten Sie, dass Pakete in Ihrem Flight-Paket wie bei allen Übermittlungen den [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) entsprechen müssen.

Personen, die in Ihren Test-Flight-Gruppen diesem Flight-Paket zugeordnet sind und Ihre App bereits haben, erhalten jetzt ein Update mit den Paketen, die Sie in Ihrem Flight-Paket bereitgestellt haben. Wenn diese Personen Ihre App noch nicht haben, erhalten sie die Pakete aus Ihrem Flight-Paket, wenn sie es installieren. 

> Hinweis: Personen, die ein Paket haben, das nur in einem Flight-Paket verfügbar ist, können die App mit Sternen bewerten und Rezensionen hinterlassen. Ihre Bewertungen und Rezensionen werden jedoch nicht für andere Kunden angezeigt. (Dies schließt ältere 7.x- oder 8.0 XAP-Pakete aus. Bewertungen und Rezensionen von Mitgliedern Ihrer Flight-Gruppen, die diese Pakete verwenden, sind für andere Kunden sichtbar.) Sie können ihr Feedback von allen Kunden, einschließlich derer in Ihrer Flight-Gruppen, in den Bewertungs- und Rezensionsberichten für die App anzeigen.

## Unterstützung für Gerätefamilien

In den meisten Fällen sollten Sie Pakete bereitstellen, die den gleichen Satz von Gerätefamilien unterstützen, die auch von Ihrer Übermittlung ohne Test-Flight unterstützt werden. Die Gerätefamilienverfügbarkeit für eine App basiert stets auf der Übermittlung ohne Test-Flight, unabhängig davon, ob ein Kunde Mitglied einer Test-Flight-Gruppe ist oder nicht.

**Wenn Ihre Übermittlung ohne Test-Flight eine Gerätefamilie unterstützt, die Ihr Flight-Paket nicht unterstützt**, können die Mitglieder Ihrer Test-Flight-Gruppe die App nicht auf diese Gerätefamilie herunterladen. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise Mobilgerät- und Desktoppakete umfasst und Sie anschließend ein Flight-Paket erstellen, das nur ein Mobilgerätpaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nur auf Mobilgeräte herunterladen, auch wenn Kunden, die nicht Mitglied der Test-Flight-Gruppe sind, ein Desktoppaket herunterladen können. Auch wenn Sie das Flight-Paket nur verwenden, um Änderungen in Ihrem Mobilgerätpaket zu testen, sollten Sie das Desktoppaket aus Ihrer Übermittlung ohne Test-Flight in das Flight-Paket integrieren, damit Kunden in der Test-Flight-Gruppe Ihre App auf Desktopgeräte herunterladen können.

**Wenn Ihr Flight-Paket eine Gerätefamilie unterstützt, die Ihre Übermittlung ohne Test-Flight nicht unterstützt**, kann niemand die App auf diese Gerätefamilie herunterladen, unabhängig davon, ob der betreffende Kunde Mitglied Ihrer Test-Flight-Gruppe ist oder nicht. Wenn Ihre Übermittlung ohne Test-Flight beispielsweise nur ein Mobilgerätpaket enthält und Sie anschließend ein Flight-Paket erstellen, das ein Mobilgerät- und ein Desktoppaket enthält, können die Mitglieder Ihrer Test-Flight-Gruppe die App nach wie vor nur auf Mobilgeräte herunterladen. Das Desktoppaket wird niemandem angeboten, auch keinem Mitglied Ihrer Test-Flight-Gruppe. Wenn Sie Mitgliedern Ihrer Test-Flight-Gruppe ein Desktoppaket zur Verfügung stellen möchten, müssen Sie zunächst Ihre Übermittlung ohne Test-Flight aktualisieren, sodass diese ein Desktoppaket enthält. Um allen Kunden Ihrer App eine optimale Erfahrung zu bieten, sollte Ihre Übermittlung ohne Test-Flight die gleichen Gerätefamilien wie Ihr Flight-Paket unterstützen. 

**Hinweis**  Pakete, die Ihren Flight-Paketen hinzugefügt werden, können jede Betriebssystemversion (oder jeden Build von Windows10) unterstützen. Wie jedoch bereits gesagt, müssen die Mitglieder von Test-Flight-Gruppen ein Gerät verwenden, auf dem eine Version von Windows10 ausgeführt wird, die Flight-Pakete unterstützt (Windows.Desktop-Build 10586 oder höher; Windows.Mobile-Build 10586.63 oder höher), um Pakete aus dem Flight-Paket zu erhalten.

## Aktualisieren oder Ändern Ihres Flight-Pakets

Zum Erstellen einer neuen Übermittlung für ein vorhandenes Flight-Paket klicken Sie in der App-Übersicht neben dem Test-Flight-Namen auf **Update**. Anschließend können Sie genau wie bei einer Übermittlung ohne Test-Flight neue Pakete hochladen (und nicht benötigte Pakete entfernen). Nehmen Sie alle weiteren erforderlichen Änderungen vor, und klicken Sie dann auf **An Store übermitteln**, um das aktualisierte Flight-Paket an den [App-Zertifizierungsprozess](the-app-certification-process.md) zu senden.

Wenn Sie einen vorhandenen Flight ändern möchten, ohne ein neues Update zu erstellen, klicken Sie neben dem Flight-Namen auf **Modify**. Dadurch können Sie Details wie die Flight-Gruppen, den Name und den Rang ändern, ohne dass das Flight-Paket erneut zertifiziert werden muss.

## Weitere Flight-Pakete hinzufügen und nach Rang sortieren

Sie können mehrere Flight-Pakete für dieselbe App erstellen, um unterschiedliche Pakete an verschiedene Kundengruppen zu verteilen. 

Sobald Sie Ihr erstes Flight-Paket erstellt haben, können Sie mit dem oben beschriebenen Vorgang ein weiteres erstellen. Wenn Sie bereits ein Flight-Paket erstellt haben, besteht der einzige Unterschied darin, dass Sie im Abschnitt **Rang** die Reihenfolge der Priorität aller Flight-Pakete angeben müssen. Anhand dieser kann der Store bestimmen, welche Pakete die einzelnen Kunden erhalten sollen, auch wenn sie mehreren Ihrer Test-Flight-Gruppen angehören. Benutzer in den Flight-Gruppen erhalten immer das Paket mit dem höchsten Rang, das für sie verfügbar ist. Dies gilt selbst dann, wenn ein Flight-Paket mit niedrigerem Rang Pakete mit einer höheren Versionsnummer enthält.

Ihr neues Flight-Paket erhält standardmäßig den höchsten Rang. Wenn Sie den Rang ändern möchten, verschieben Sie das Paket nach unten (oder wieder nach oben), um es zwischen Ihren anderen Flight-Paketen richtig zu positionieren.

Beachten Sie, dass eine Übermittlung ohne Test-Flight immer den niedrigsten Rang erhält. Das heißt, dass Personen, die keiner Ihrer Test-Flight-Gruppen angehören, nur Pakete aus Ihrer Übermittlung ohne Test-Flight aus dem Store abrufen können. Personen in einer Test-Flight-Gruppe erhalten immer Pakete aus dem Flight-Paket mit dem höchsten Rang, das für sie verfügbar ist (jedoch nie aus der Übermittlung ohne Test-Flight). Dadurch sind Sie flexibel beim Bestimmen, wie Ihre Pakete an Benutzer verteilt werden, die u.U. in mehreren Ihrer Test-Flight-Gruppen Mitglied sind.

Nehmen wir beispielsweise an, dass Sie neben Ihrer regulären Übermittlung ohne Test-Flight zwei Flight-Pakete erstellen möchten: eines, das relativ stabil und für den Test mit einer breiten Zielgruppe bereit ist, und eines, bei dem Sie sich nicht so sicher sind und es auf wenige Tester beschränken möchten. Sie erstellen eine Test-Flight-Gruppe mit dem Namen „Tester“ und fügen sie einem Flight-Paket mit dem Namen „Test-Flight für Tester“ hinzu. Anschließend erstellen Sie eine Test-Flight-Gruppe mit dem Namen „Interessierte Benutzer“ und fügen sie einem anderen Flight-Paket mit dem Namen „Test-Flight für Interessierte Benutzer“ hinzu. Wenn Sie „Test-Flight für Tester“ einen höheren Rang als „Test-Flight für Interessierte Benutzer“ zuweisen, können Sie Pakete, bei denen Sie sich relativ sicher sind, in „Test-Flight für Interessierte Benutzer“ verwenden, und Pakete mit höherem Risiko, die nur für Tester vorgesehen sind, in „Test-Flight für Tester“. Mitglieder der Tester-Gruppe erhalten immer die Pakete, die Sie in „Test-Flight für Tester“ bereitstellen, auch wenn sie ebenfalls der Gruppe „Interessierte Benutzer“ angehören. (Wenn sich später dann herausstellt, dass die Pakete in „Test-Flight für Tester“ problemlos ausgeführt werden, können Sie „Test-Flight für Interessierte Benutzer“ so aktualisieren, dass die ursprünglich an „Test-Flight für Tester“ verteilten Pakete verwendet werden. Möglicherweise können Sie diese Pakete irgendwann auch in Ihrer Übermittlung ohne Test-Flight verwenden.

## Pakete aus einem Flight-Paket für alle Kunden zur Verfügung stellen

Wenn Sie eines oder mehrere Pakete in einem veröffentlichten Flight-Paket für Kunden verfügbar machen möchten, die keiner Test-Flight-Gruppe angehören, können Sie Ihre Übermittlung ohne Test-Flight entsprechend aktualisieren, ohne dieselben Pakete erneut hochladen zu müssen. 

Bei der Erstellung Ihrer neuen Übermittlung wird auf der Seite [**Packages**](upload-app-packages.md) eine Dropdownliste mit der Option zum Kopieren von Paketen aus einer vorherigen Übermittlung bzw. einem Flight-Paket angezeigt. Wählen Sie das Flight-Paket mit den Paketen aus, die Sie übertragen möchten. Anschließend können Sie einige oder alle der Pakete darin auswählen, um sie in die Übermittlung ohne Test-Flight aufzunehmen.

Beachten Sie, dass dieselben Regeln für Paketvalidierung gelten, auch wenn Pakete aus einer zuvor veröffentlichten Übermittlung verwendet werden. 

## Löschen eines Flight-Pakets

Zum Löschen eines Flight-Pakets, das Sie nicht mehr unterstützen möchten, klicken Sie in der App-Übersicht auf dessen Namen. Klicken Sie auf der Übersichtsseite für Test-Flights **Ändern** und anschließend auf den Link **Löschen**, um das Flight-Paket zu löschen. (Wenn Sie eine unveröffentlichte Übermittlung des Flight-Pakets ausführen, müssen Sie zunächst diese Übermittlung löschen.) Dies kann bis zu 30 Minuten dauern.

Wenn Sie ein Flight-Paket löschen, erhalten alle Kunden mit Paketen, die Sie in diesem Flight-Paket verteilt haben, ein App-Update, wenn es ein Paket mit einer höheren Versionsnummer gibt (oder sobald ein solches Paket verfügbar ist). Wenn Benutzer die App deinstallieren und später erneut installieren, wird dies als neuer Kauf behandelt, und die Benutzer erhalten die höchste aktuell verfügbare Version. 



<!--HONumber=Aug16_HO5-->


