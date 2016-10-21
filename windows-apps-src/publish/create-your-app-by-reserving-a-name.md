---
author: jnHs
Description: "Der erste Schritt beim Erstellen einer neuen App im Windows Dev Center-Dashboard besteht darin, einen App-Namen zu reservieren. In diesem Thema wird erläutert, wie Sie App-Namen reservieren. Es enthält einige Vorschläge zur Wahl aussagekräftiger App-Namen."
title: App-Erstellung durch Reservierung eines Namens
keywords: 
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
translationtype: Human Translation
ms.sourcegitcommit: 3b65bbaf2498dde7484c055ff86ed09e89bf3405
ms.openlocfilehash: 1be7229086e1f2f932e0945334098d89a9978b70

---

# App-Erstellung durch Reservierung eines Namens


Der erste Schritt beim Erstellen einer neuen App im Windows Dev Center-Dashboard besteht darin, einen App-Namen zu reservieren. In diesem Thema wird erläutert, wie Sie App-Namen reservieren. Es enthält einige [Vorschläge zur Wahl aussagekräftiger App-Namen](#choosing-your-app-s-name). Jeder reservierter Name muss im gesamten Store eindeutig sein.

> **Hinweis**  Wenn Sie über eine bereits erstellte Windows Phone-App verfügen, für die noch kein Name reserviert wurde, können Sie diese App trotzdem verwalten und einreichen. Um jedoch APPX-Pakete für die App hochzuladen oder [App-Identitätsdetails](view-app-identity-details.md) speziell für das Erstellen von APPX-Paketen anzuzeigen, müssen Sie anhand der folgenden Schritte einen eindeutigen Namen reservieren. Dadurch wird außerdem verhindert, dass eine andere Person diesen Namen für sich selbst reserviert.

Wenn Sie Ihre [App-Pakete hochladen](upload-app-packages.md), muss der [**Package/Properties/DisplayName**](https://msdn.microsoft.com/library/windows/apps/dn423240)-Wert dem Namen entsprechen, den Sie im **Dashboard** für Ihre App reserviert haben. Wenn Sie das App-Paket mit MicrosoftVisual Studio erstellen, wird dieses Attribut für Sie ausgefüllt.

## App-Erstellung durch Reservierung eines neuen Namens

Das Reservieren eines Namens ist der erste Schritt bei der App-Erstellung im Dashboard. Die Reservierung ist möglich, auch wenn Sie noch nicht mit der App-Erstellung begonnen haben. Wir empfehlen, die Reservierung so früh wie möglich vorzunehmen, damit der Name nicht von jemand anderem verwendet wird.

1.  Klicken Sie in der **Dashboardübersicht** oder auf der Seite **Alle Apps** auf **Neue App erstellen**.
2.  Geben Sie im Textfeld den Namen ein, den Sie verwenden möchten, und klicken Sie dann auf den Link **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt. (Ist der von Ihnen eingegebene Name bereits reserviert oder wird von einem anderen Entwickler verwendet, wird eine Fehlermeldung angezeigt, dass der Name nicht verfügbar ist.)
3.  Klicken Sie auf **App-Namen reservieren**.

Der Name ist jetzt für Sie reserviert, und Sie können mit der [Einreichung](app-submissions.md) beginnen, sobald Sie dazu bereit sind.

> **Hinweis**  Da Namen für ein Jahr reserviert werden können, kann ein bestimmter Name u.U. nicht reserviert werden, obwohl keine Apps mit diesem Namen im Store gelistet sind. Das liegt normalerweise daran, dass ein anderer Entwickler den Namen für seine App reserviert, aber die entsprechende App noch nicht eingereicht hat. Wenn Sie einen Namen, den Sie markenrechtlich oder anderweitig geschützt haben, nicht reservieren können oder im WindowsStore eine andere App mit diesem Namen entdecken, [wenden Sie sich an Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).

Nach dem Reservieren eines Namens haben Sie ein Jahr Zeit, um die App einzureichen. Wenn Sie sie nicht innerhalb eines Jahres einreichen, läuft die Reservierung ab, und ein anderer Entwickler kann den Namen für seine App verwenden. Wenn Sie versuchen, eine App unter einem abgelaufenen Namen einzureichen, tritt ein Fehler auf.

## Auswählen des App-Namens

Es ist sehr wichtig, für Ihre App den richtigen Namen auszuwählen. Wählen Sie einen Namen, der die Aufmerksamkeit von Kunden auf sich zieht und diese dazu animiert, sich ausführlicher über Ihre App zu informieren. Hier sind ein paar Tipps zum Auswählen eines geeigneten App-Namens.

-   **Halten Sie den Namen kurz.** Für die Anzeige des App-Namens ist meist nur wenig Platz – lassen Sie sich also einen möglichst kurzen Namen einfallen. Der Name der App kann bis zu 256Zeichen haben, aber möglicherweise ist das Ende eines sehr langen Namens für Kunden nicht immer sichtbar.

    > **Hinweis**  Die tatsächliche Anzahl von an unterschiedlichen Stellen angezeigten Zeichen kann je nach zugewiesener Länge und im App-Namen verwendetem Zeichentyp variieren. So benötigen beispielsweise in der von Windows verwendeten Schriftart „SegoeUI“ etwa 30I-Zeichen genauso viel Platz wie zehn W-Zeichen. Sie sollten die App aufgrund dieser Abweichungen also auf jeden Fall testen und prüfen, wie der Name auf den Kacheln (falls Sie sich für die Überlagerung mit dem App-Namen entscheiden), in Suchergebnissen und in der App selbst angezeigt wird, bevor Sie die App einreichen. Berücksichtigen Sie auch die Sprachen, in denen Sie die App anbieten möchten. Denken Sie daran, dass ostasiatische Zeichen im Allgemeinen breiter sind als lateinische Zeichen, sodass weniger Zeichen angezeigt werden.

-   **Fügen Sie Informationen zur Unterscheidung nicht am Ende des Namens hinzu.** Wenn Infos zur Unterscheidung mehrerer Apps am Ende eines Namens angefügt werden, werden sie von den Kunden vor allem bei langen Namen vielleicht übersehen, und es scheint, als hätten alle Apps den gleichen Namen. Falls sich dies nicht vermeiden lässt, verwenden Sie unterschiedliche Logos und App-Bilder, um die Unterscheidung zwischen Apps zu erleichtern.
-   **Seien Sie unverwechselbar.** Suchen Sie einen eindeutigen App-Namen aus, damit die App nicht so leicht mit einer anderen App verwechselt werden kann.
-   **Verwenden Sie keine von anderen geschützten Namen.** Stellen Sie sicher, dass sie zum Verwenden des reservierten Namens berechtigt sind. Wurde der Name als Marke eingetragen, kann ein Verstoß gemeldet werden, und Sie dürfen den Namen nicht weiter verwenden. Geschieht dies nach der Veröffentlichung Ihrer App, wird sie aus dem Store entfernt. Sie müssen dann den Namen der App sowie alle Vorkommen des Namens innerhalb Ihrer App und der zugehörigen Inhalte ändern, bevor Sie [die App erneut zur Zertifizierung einreichen](app-submissions.md).

## Verwalten zusätzlicher App-Namen

Auf der Seite **Verwalten von App-Namen** im Abschnitt **App-Verwaltung** für jede der Apps im Windows Dev Center-Dashboard können Sie Namen für die Apps verwalten.

In einigen Fällen möchten Sie u. U. mehrere Namen reservieren, die für dieselbe App verwendet werden sollen; beispielsweise wenn Sie Ihre App unter verschiedenen Namen in mehreren Sprachen anbieten möchten. Sie müssen einen zusätzlichen Namen reservieren, wenn Sie den Namen einer App vollständig ändern möchten.

Auf dieser Seite können Sie auch reservierte Namen, die Sie nicht mehr verwenden, löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

 

 







<!--HONumber=Aug16_HO3-->


