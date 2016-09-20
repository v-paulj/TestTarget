---
author: jnHs
Description: "Fügen Sie Ihrem Dev Center-Konto Benutzer hinzu, und weisen Sie diesen Rollen mit bestimmten Berechtigungen zu."
title: Verwalten von Kontobenutzern
ms.assetid: 9245F0D0-7D8F-4741-AFB4-FBA5601D0A9B
ms.sourcegitcommit: 3cfc50e56f3fa65a9dfa2c8b4582c1a53c2b13d1
ms.openlocfilehash: 18e25d0064652089d450eec811a7a5d24b8dc3e8

---

# Verwalten von Kontobenutzern


Sie können mit Azure Active Directory Ihrem Dev Center-Konto Benutzer hinzufügen. Jedem Benutzer wird eine Rolle zugewiesen, mit der er bestimmte Berechtigungen für das Konto erhält. Sie können eine Rolle auch einer Gruppe von Benutzern oder einer Azure AD-App zuweisen.

> 
            **Wichtig**  Zum Hinzufügen und Verwalten von Kontobenutzern müssen Sie zunächst Ihr Dev Center-Konto dem Azure Active Directory Ihres Unternehmens zuordnen. Dazu müssen Sie sich bei Azure AD mit einem [globalen Administratorkonto](http://go.microsoft.com/fwlink/?LinkId=746654) anmelden. Nachdem Sie diese Zuordnung festgelegt haben, können Sie sie erst nach Rücksprache mit dem Support wieder entfernen.

 

## Zuordnen Ihres Dev Center-Kontos zum Azure Active Directory des Unternehmens


Windows Dev Center nutzt Azure Active Directory zum Verwalten mehrerer Benutzer und zum Zuweisen von Rollen. Wenn in Ihrer Organisation bereits mit Office 365 oder Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie schon über Azure AD. Andernfalls können Sie innerhalb von Dev Center ohne zusätzliche Kosten ein neues Azure AD erstellen. 

Beachten Sie, dass einem Azure AD jeweils nur ein Dev Center-Konto zugeordnet werden kann. Ebenso kann einem Dev Center-Konto auch nur ein Azure AD zugeordnet werden.

> 
            **Hinweis**  Sie können Ihrem Dev Center-Konto nur Benutzer hinzufügen, die Teil des Azure AD Ihrer Organisation sind (oder wenn Sie für diese neue Azure AD-Konten erstellen). Es ist nicht möglich, dem Dev Center-Konto Benutzer mithilfe ihrer persönlichen Microsoft-Konten hinzuzufügen.

So ordnen Sie Ihr Dev Center-Konto dem vorhandenen Azure AD Ihrer Organisation zu

1.  Wechseln Sie zu den **Kontoeinstellungen**, und klicken Sie auf **Benutzer verwalten**.
2.  Klicken Sie auf die **Schaltfläche zum Zuordnen Ihres Dev Center-Kontos zu Azure AD**.
3.  Melden Sie sich bei Ihrem AzureAD-Konto an. Dieses Konto muss über Berechtigungen des [globalen Administrators](http://go.microsoft.com/fwlink/?LinkId=746654) verfügen, damit die Zuordnung eingerichtet werden kann.
4.  Überprüfen Sie den Organisations- und den Domänennamen für das Azure AD-Konto. Klicken Sie zum Abschließen der Zuordnung auf **Bestätigen**.
5.  Wenn die Zuordnung erfolgreich abgeschlossen wurde, können Sie nun auf der Seite **Benutzer verwalten** Ihres Kontos Kontobenutzer hinzufügen und verwalten, wie in den folgenden Abschnitten beschrieben.
  
So erstellen Sie ein neues Azure AD, um diesem Ihr Dev Center-Konto zuzuordnen
1.  Wechseln Sie zu den **Kontoeinstellungen**, und klicken Sie auf **Benutzer verwalten**.
2.  Klicken Sie auf die Schaltfläche **Neues Azure AD erstellen**.
3.  Geben Sie die Verzeichnisinformationen für das neue Azure AD ein:
- 
            **Domänenname**: der eindeutige Name, den für Ihre Azure AD-Domäne verwendet wird, zusammen mit „. onmicrosoft.com“. Wenn Sie beispielsweise „beispiel“ eingegeben haben, wäre Ihre Azure AD-Domäne „beispiel.onmicrosoft.com“. 
- 
            **Kontakt-E-Mail-Adresse**: eine E-Mail-Adresse, unter der wir Sie hinsichtlich Ihres Kontos erreichen können, wenn nötig.
- 
            **Benutzerkontoinformationen für den globalen Administrator**: Vorname, Nachname, Benutzername und Kennwort, die Sie für das neue Administratorkonto verwenden möchten. 
4.  Klicken Sie auf **Erstellen**, um die neue Domäne und die Kontoinformationen zu bestätigen.
5.  Melden Sie sich mit dem neuen Azure AD-Benutzernamen und -Kennwort als globaler Administrator an, um auf der Seite **Benutzer verwalten** zusätzliche Kontobenutzer hinzuzufügen und zu verwalten, wie in den folgenden Abschnitten beschrieben.


> 
            **Wichtig**  Nachdem Sie Ihr Dev Center-Konto dem Azure AD zugeordnet haben, müssen Sie sich am Dev Center stets mithilfe des globalen Azure AD-Administratorkontos (und nicht mit einem persönlichen Microsoft-Konto) anmelden, um Kontobenutzer hinzuzufügen und zu verwalten.

## Hinzufügen und Verwalten von Kontobenutzern, Gruppen und Azure AD-Anwendungen


Nachdem Sie die Zuordnung hergestellt haben, können Sie Ihrem Konto Benutzer, Gruppen und Azure AD-Apps hinzufügen. Sie können auch Rollen ändern, Kontodetails bearbeiten und Benutzer entfernen.

> 
            **Hinweis**  Wenn Ihre Organisation die [Verzeichnisintegration](http://go.microsoft.com/fwlink/p/?LinkID=724033) zum Synchronisieren des lokalen Verzeichnisdiensts mit AzureAD verwendet, können Sie in DevCenter keine neuen Benutzer, Gruppen oder AzureAD-Anwendungen erstellen. Sie (oder ein anderer Administrator in Ihrem lokalen Verzeichnis) müssen sie direkt im lokalen Verzeichnis erstellen, bevor sie in DevCenter angezeigt und hinzugefügt werden können.

Beachten Sie beim Verwalten von Benutzern Folgendes:

-   Alle Dev Center-Benutzer müssen über ein aktives Konto im Azure AD Ihrer Organisation verfügen.
-   Beim Erstellen **neuer** Benutzer oder Gruppen in Dev Center werden diese auch dem Azure AD Ihrer Organisation hinzugefügt.
-   Wenn Sie den Namen eines Benutzers oder einer Gruppe in Dev Center ändern, werden diese Änderungen auch im Azure AD der Organisation vorgenommen.
-   Benutzer (einschließlich von Gruppen und Azure AD-Apps) können mit den Berechtigungen für ihre jeweils zugewiesene Rolle auf das gesamte Dev Center-Konto zugreifen. Sie können den Zugriff eines Benutzers nicht beschränken, sodass dieser nur mit bestimmten Apps und/oder IAPs arbeiten kann.
-   Sie können einem Benutzer, einer Gruppe oder einer Azure AD-Anwendung Zugriff auf weitere Funktionen gewähren, die über deren Rolle hinausgehen, indem Sie mehrere Rollen auswählen.
-   Ein Benutzer mit einer bestimmten Rolle kann auch Mitglied einer Gruppe sein, der eine andere Rolle zugewiesen ist. In einem solchen Fall hat der Benutzer Zugriff auf die Funktionen, die beiden Rollen zugewiesen sind.

### Rollen und Berechtigungen

Allen Benutzern, Gruppen oder Azure AD-Anwendungen, die Sie einem Konto hinzufügen, müssen Sie mindestens eine der unten angegebenen Rollen zuweisen. Jede Rolle verfügt über spezifische Berechtigungen, mit denen bestimmte Funktionen innerhalb des Kontos ausgeführt werden können.

> 
            **Hinweis**  Der Besitzer des Kontos ist die Person, die es als erste mit einem Microsoft-Konto erstellt hat (und keiner der Benutzer, die über Azure AD hinzugefügt wurden). Dieser Kontobesitzer ist die einzige Person mit Vollzugriff auf das Konto. Hierzu zählt die Möglichkeit, Apps zu löschen, zu erstellen und zu bearbeiten, alle Kontobenutzer zu bearbeiten sowie sämtliche finanziellen Einstellungen und Kontoeinstellungen zu ändern. Das Microsoft-Konto, das zum Erstellen des Kontos verwendet wurde, muss beim Erstellen von App-Paketen in Microsoft Visual Studio verwendet werden.

| Rolle                 | Beschreibung              |
|----------------------|--------------------------|
| Manager              | Verfügt über vollständigen Zugriff auf das Konto, kann jedoch keine Steuer- und Auszahlungseinstellungen ändern. Dies umfasst das Verwalten von Benutzern in Dev Center. Beachten Sie jedoch, dass die Fähigkeit zum Erstellen und Löschen von Benutzern von den Berechtigungen des Kontos in Azure AD abhängig ist. Das heißt, wenn einem Benutzer die Manager-Rolle zugewiesen ist, er jedoch nicht über Administratorberechtigungen im Azure AD der Organisation verfügt, kann er keine neuen Benutzer erstellen oder Benutzer aus dem Verzeichnis löschen (er kann jedoch die Dev Center-Rolle eines Benutzers ändern). |
| Entwickler            | Kann Pakete hochladen und Apps und IAPs einreichen sowie den [Nutzungsbericht](usage-report.md) für Telemetrie-Details einsehen. Kann keine finanziellen Informationen oder Kontoeinstellungen anzeigen.                                                                                                                                                                                                                                                                                                                     |
| Mitwirkender im Geschäftsbereich | Kann auf finanzielle Informationen zugreifen und Preisdetails festlegen. Kann keine neuen Apps und IAPs erstellen oder einreichen und keine Kontoeinstellungen ändern.                                                                                                                                                                                                                                                                                                                                                              |
| Mitwirkender im Finanzbereich  | Kann [Auszahlungsberichte](payout-summary.md) anzeigen. Kann keine Änderungen an Apps, IAPs und Kontoeinstellungen vornehmen.                                                                                                                                                                                                                                                                                                                                                                                 |
| Händler             | Kann auf [Kundenbewertungen reagieren](respond-to-customer-reviews.md) und nicht finanzbezogene [Analyseberichte](analytics.md) einsehen. Kann keine Änderungen an Apps, IAPs und Kontoeinstellungen vornehmen.                                                                                                                                                                                                                                                                                                            |

> 
            **Hinweis**  Benutzer mit der Rolle „Manager“ oder „Entwickler“ können Apps über das Dashboard übermitteln. Beim Erstellen von App-Paketen in Visual Studio muss anstelle des Azure AD-Kontos das Microsoft-Konto verwendet werden, mit dem das Entwicklerkonto eröffnet wurde.

### Hinzufügen und Verwalten von Kontobenutzern

Klicken Sie zum Angeben von Benutzern, die Sie Ihrem Dev Center-Konto hinzufügen und denen Sie eine Rolle zuweisen möchten, auf **Benutzer hinzufügen**.

Sie können einen oder mehrere Benutzer aus dem Verzeichnis Ihrer Organisation dem Dev Center-Konto hinzufügen. Wenn Sie mehrere Benutzer gleichzeitig hinzufügen, müssen Sie ihnen die gleiche Rolle zuweisen. Wenn Sie Benutzer hinzufügen, ihnen jedoch unterschiedliche Rollen zuweisen möchten, wiederholen Sie die folgenden Schritte für jede Rolle.

**Hinzufügen von Benutzern aus dem Verzeichnis der Organisation**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Benutzer hinzufügen**.
2.  Wählen Sie in der angezeigten Liste einen oder mehrere Benutzer aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
3.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Wählen Sie im Abschnitt **Rollen** eine oder mehrere Rollen aus, die dieser Gruppe von Benutzern zugewiesen werden sollen.
5.  Klicken Sie auf **Speichern**.

Wenn Sie einem völlig neuen Benutzerkonto Zugriff auf Dev Center gewähren möchten, können Sie im Abschnitt **Benutzer verwalten** ein Konto erstellen. Beachten Sie, dass hierdurch nicht nur in Ihrem Dev Center-Konto, sondern auch im Verzeichnis der Organisation ein neues Konto erstellt wird.

**Erstellen eines neuen Benutzerkontos**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Benutzer hinzufügen**.
2.  Klicken Sie auf der nächsten Seite auf **Neuer Benutzer**.
3.  Geben Sie den Vornamen, den Nachnamen und den Benutzernamen für den neuen Benutzer ein.
4.  Wählen Sie im Abschnitt **Rollen** eine oder mehrere Rollen aus, die dem neuen Benutzer zugewiesen werden sollen.
5.  Wählen Sie im Abschnitt **Gruppenmitgliedschaft** alle Gruppen aus, denen der neue Benutzer angehören soll.
6.  Klicken Sie auf **Speichern**.
7.  Auf der Bestätigungsseite werden die Anmeldedaten für den neuen Benutzer angezeigt, z.B. ein temporäres Kennwort. Notieren Sie sich diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.

Änderungen an den Benutzerkonten, die Sie Ihrem Dev Center-Konto hinzugefügt haben, können Sie im Abschnitt **Benutzer verwalten** vornehmen. Beachten Sie, dass Änderungen des Benutzernamens oder der Gruppenmitgliedschaft des Benutzers im Verzeichnis der Organisation und nicht nur im Dev Center-Konto widergespiegelt werden. Änderungen an der Rolle eines Benutzers wirken sich nur auf dessen jeweiligen Dev Center-Zugriff aus.

**Bearbeiten eines Benutzerkontos**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf den Namen des Benutzerkontos, das bearbeitet werden soll.
2.  Sie können die folgenden Änderungen vornehmen:
    -   Bearbeiten Sie den Vornamen, den Nachnamen oder den Benutzernamen des Benutzers. Diese Änderungen werden im Verzeichnis Ihrer Organisation vorgenommen.
    -   Aktivieren bzw. deaktivieren Sie im Abschnitt **Rollen** die Rolle(n), die Sie für diesen Benutzer hinzufügen oder entfernen möchten.
    -   Aktivieren bzw. deaktivieren Sie im Abschnitt **Gruppenmitgliedschaft** die Gruppe(n), denen der Benutzer beitreten bzw. aus denen der Benutzer entfernt werden soll. Diese Änderungen werden im Verzeichnis Ihrer Organisation vorgenommen.

3.  Klicken Sie auf **Speichern**.

Wenn Sie das Kennwort für ein Benutzerkonto ändern müssen, das Sie dem Dev Center-Konto hinzugefügt haben, können Sie die notwendigen Schritte im Abschnitt **Benutzer verwalten** ausführen. Dadurch wird das Verzeichniskennwort des Benutzers und nicht nur sein Kennwort für den Dev Center-Zugriff geändert.

**Ändern des Verzeichniskennworts eines Benutzers**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf den Namen des Benutzerkontos, das bearbeitet werden soll.
2.  Klicken Sie am unteren Rand der Seite auf die Schaltfläche **Kennwort zurücksetzen**.
3.  Auf einer Bestätigungsseite werden die Anmeldeinformationen für den Benutzer angezeigt, einschließlich eines temporären Kennworts.
  > 
            **Wichtig**  Drucken oder kopieren Sie diese Informationen, und teilen Sie sie dem neuen Benutzer mit, da Sie nach dem Verlassen dieser Seite nicht mehr auf das temporäre Kennwort zugreifen können.

### Hinzufügen und Verwalten von Gruppen

Wenn Sie dem Dev Center-Konto eine Gruppe aus dem Verzeichnis der Organisation hinzufügen, kann jeder Benutzer, der Mitglied dieser Gruppe ist, mit den Berechtigungen für die der Gruppe zugewiesenen Rolle darauf zugreifen. Beachten Sie, dass alle an Gruppen vorgenommenen Änderungen (u.a. Änderungen von Name oder Mitgliedschaft) im Verzeichnis der Organisation widergespiegelt werden.

Wenn Sie mehrere Gruppen gleichzeitig hinzufügen, müssen Sie ihnen die gleiche Rolle zuweisen. Wenn Sie Gruppen hinzufügen, ihnen jedoch unterschiedliche Rollen zuweisen möchten, wiederholen Sie die folgenden Schritte für jede Rolle.

**Hinzufügen von Gruppen aus dem Verzeichnis der Organisation**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Gruppen hinzufügen**.
2.  Wählen Sie in der angezeigten Liste eine oder mehrere Gruppen aus. Im Suchfeld können Sie nach bestimmten Gruppen suchen.
3.  Wenn Sie die Gruppen ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Wählen Sie im Abschnitt **Rollen** eine oder mehrere Rollen aus, die diesen Gruppen zugewiesen werden sollen.
5.  Klicken Sie auf **Speichern**.

Wenn Sie einer völlig neuen Gruppe den Zugriff auf Dev Center gestatten möchten, können Sie im Abschnitt **Benutzer verwalten** eine neue Gruppe erstellen. Beachten Sie, dass hierdurch nicht nur in Ihrem Dev Center-Konto, sondern auch im Verzeichnis der Organisation eine neue Gruppe erstellt wird.

**Erstellen eines neuen Gruppenkontos**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Gruppen hinzufügen**.
2.  Klicken Sie auf der nächsten Seite auf **Neue Gruppe**.
3.  Geben Sie den Anzeigenamen für die neue Gruppe ein.
4.  Wählen Sie eine oder mehrere Rollen aus, die der neuen Gruppe zugewiesen werden sollen. Alle Mitglieder der Gruppe können mit den Berechtigungen für die betreffende Rolle auf Ihr Dev Center-Konto zugreifen.
5.  Wählen Sie in der angezeigten Liste einen oder mehrere Benutzer aus. Im Suchfeld können Sie nach bestimmten Benutzern suchen.
6.  Wenn Sie die Benutzer ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
7.  Klicken Sie auf **Speichern**.

Änderungen an den Gruppenkonten, die Sie Ihrem Dev Center-Konto hinzugefügt haben, können Sie im Abschnitt **Benutzer verwalten** vornehmen. Beachten Sie, dass Änderungen des Gruppennamens oder der Gruppenmitgliedschaft im Verzeichnis der Organisation und nicht nur im Dev Center-Konto widergespiegelt werden. Änderungen an der Rolle einer Gruppe wirken sich nur auf den Dev Center-Zugriff der Gruppe aus.

**Bearbeiten eines Gruppenkontos**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf den Namen des Gruppenkontos, das bearbeitet werden soll.
2.  Nehmen Sie zum Bearbeiten der Gruppeninformationen die gewünschten Änderungen am Gruppennamen vor. Diese Änderungen werden im Verzeichnis Ihrer Organisation vorgenommen.
3.  Wenn Sie die Gruppenrolle ändern möchten, aktivieren bzw. deaktivieren Sie die Rolle(n), die auf die Gruppe angewendet werden sollen.
4.  Klicken Sie auf **Speichern**.

### Hinzufügen und Verwalten von Azure AD-Apps

Sie können Anwendungen oder Diensten, die Teil der Azure AD-Instanz Ihrer Organisation sind, den Zugriff auf Ihr Dev Center-Konto gewähren.

Wenn Sie mehrere Azure AD-Apps gleichzeitig hinzufügen, müssen Sie ihnen die gleiche Rolle zuweisen. Wenn Sie Gruppen hinzufügen, ihnen jedoch unterschiedliche Rollen zuweisen möchten, wiederholen Sie die folgenden Schritte für jede Rolle.

**Hinzufügen von Azure AD-Apps aus Verzeichnis der Organisation**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Azure AD-Apps hinzufügen**.
2.  Wählen Sie eine oder Azure AD-Anwendungen aus der angezeigten Liste aus. Mithilfe des Suchfelds können Sie nach bestimmten Azure AD-Apps suchen.
3.  Wenn Sie die Azure AD-Apps ausgewählt haben, klicken Sie auf **Ausgewählte hinzufügen**.
4.  Wählen Sie im Abschnitt **Rollen** eine oder mehrere Rollen aus, die diesen Azure AD-Apps zugewiesen werden sollen.
5.  Klicken Sie auf **Speichern**.

Wenn Sie einem völlig neuen Azure AD-Anwendungskonto den Zugriff auf Dev Center gestatten möchten, können Sie im Abschnitt **Benutzer verwalten** ein neues Konto erstellen. Beachten Sie, dass hierdurch nicht nur in Ihrem Dev Center-Konto, sondern auch im Verzeichnis der Organisation ein neues Konto erstellt wird.

**Erstellen einer neuen Azure AD-Anwendung**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf **Azure AD-Apps hinzufügen**.
2.  Klicken Sie auf der nächsten Seite auf **New Azure AD application**.
3.  Geben Sie die **Antwort-URL** für die neue Azure AD-App ein. Dies ist die URL, mit der sich Benutzer anmelden und Ihre Azure AD-App verwenden können (wird auch als App-URL oder Anmelde-URL bezeichnet). Die **Antwort-URL** darf nicht mehr als 256 Zeichen enthalten.
4.  Geben Sie den **App-ID-URI** für die neue Azure AD-App ein. Dies ist ein logischer Bezeichner für die Azure AD-App, der beim Senden einer Anforderung für einmaliges Anmelden an Azure AD angezeigt wird. Beachten Sie, dass der **App-ID-URI** für jede Azure AD-App im Verzeichnis eindeutig sein muss und nicht mehr als 256 Zeichen enthalten darf.
5.  Wählen Sie im Abschnitt **Rollen** eine oder mehrere Rollen aus, die der neuen Azure AD-App zugewiesen werden sollen.
6.  Klicken Sie auf **Speichern**.

Änderungen an Azure AD-Apps, die Sie Ihrem Dev Center-Konto hinzugefügt haben, können Sie im Abschnitt **Benutzer verwalten** vornehmen. Beachten Sie, dass Änderungen der Antwort-URL und des App-ID-URI im Verzeichnis der Organisation und nicht nur im Dev Center-Konto widergespiegelt werden. Rollenänderungen wirken sich nur auf die Berechtigungen der Azure AD-App in Dev Center aus.

**Bearbeiten einer Azure AD-App**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf den Namen des Azure AD-Anwendungskontos, das bearbeitet werden soll.
2.  Geben Sie zum Ändern der **Antwort-URL** oder des **App-ID-URI** die neuen Werte hier ein. Diese Änderungen werden im Verzeichnis Ihrer Organisation vorgenommen.
3.  Wenn Sie die Rolle einer Azure AD-App ändern möchten, aktivieren bzw. deaktivieren Sie die Rolle(n), die angewendet werden sollen.
4.  Klicken Sie auf **Speichern**.

Wenn die Azure AD-App Daten in Microsoft Azure AD liest und schreibt, benötigt sie einen Schlüssel. Sie können Schlüssel für eine Azure AD-App erstellen, indem Sie ihre Informationen in Dev Center bearbeiten. Sie können auch Schlüssel entfernen, die nicht mehr benötigt werden.

**Verwalten von Schlüsseln für eine Azure AD-App**

1.  Klicken Sie auf der Seite **Benutzer verwalten** auf den Namen der Azure AD-App.

    > 
            **Tipp**  Wenn Sie auf den Namen der Azure AD-App klicken, sehen Sie alle aktiven Schlüssel für die Azure AD-App mit dem jeweiligen Erstellungs- und Ablaufdatum des Schlüssels. Klicken Sie auf **Entfernen**, um einen nicht mehr benötigten Schlüssel zu entfernen.

2.  Klicken Sie auf **Add new key**, um einen neuen Schlüssel hinzuzufügen.

3.  Es wird ein Bildschirm mit den Werten für **Client-ID** und **Schlüssel** angezeigt.

    > 
            **Wichtig**  Drucken oder kopieren Sie diese Informationen, da Sie nach dem Verlassen dieser Seite nicht mehr darauf zugreifen können.

4.  Klicken Sie auf **Add another key**, wenn Sie weitere Schlüssel erstellen möchten.

### Entfernen von Benutzern, Gruppen und Azure AD-Apps

Um einen Benutzer, eine Gruppe oder eine Azure AD-Anwendung aus Ihrem Dev Center-Konto zu entfernen, klicken Sie auf den Link **Entfernen**, der auf der Seite **Benutzer verwalten** neben dem jeweiligen Namen angezeigt wird. Nachdem Sie das Entfernen bestätigt haben, kann der Benutzer, die Gruppe oder die Azure AD-Anwendung nicht mehr auf Ihr Dev Center-Konto zugreifen (es sei denn, Sie fügen das Element später wieder hinzu).

> 
            **Hinweis**  Wenn Sie Benutzer, Gruppen oder Azure AD-Anwendungen entfernen, bedeutet dies, dass diese nicht mehr auf Ihr Dev Center-Konto zugreifen können. Dadurch werden nicht die betreffenden Benutzer, Gruppen oder Azure AD-Apps aus dem Verzeichnis der Organisation gelöscht.

 

 

 







<!--HONumber=Jun16_HO4-->


