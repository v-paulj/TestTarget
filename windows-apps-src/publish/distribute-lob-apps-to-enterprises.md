---
description: Über den Windows Store für Unternehmen können Sie branchenspezifische Apps (Line-of-Business-Apps, LOB-Apps) direkt für Unternehmen veröffentlichen, damit diese Volumenlizenzen erwerben können, ohne die Apps im Store allgemein zur Verfügung zu stellen.
title: Verteilen von Branchen-Apps an Unternehmen
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
---

# Verteilen von Branchen-Apps an Unternehmen


Über den Windows Store für Unternehmen können Sie branchenspezifische Apps (Line-of-Business-Apps, LOB-Apps) direkt für Unternehmen veröffentlichen, damit diese Volumenlizenzen erwerben können, ohne die Apps im Store allgemein zur Verfügung zu stellen.

> **Wichtig**  Zurzeit können nur kostenlose Apps über den Windows Store exklusiv an Unternehmen verteilt werden. Wenn Sie eine kostenpflichtige App als branchenspezifische App übermitteln, wird sie später für das Unternehmen verfügbar gemacht, nachdem die Unterstützung für den Volumekauf hinzugefügt wurde. 

## Einrichten der Unternehmenszuordnung


Der erste Schritt beim exklusiven Veröffentlichen von branchenspezifischen Apps für ein Unternehmen besteht darin, eine Zuordnung zwischen Ihrem Konto und dem privaten Store des Unternehmens einzurichten.

> **Wichtig**  Dieser Zuordnungsprozess muss vom Unternehmen initiiert werden. Weitere Informationen finden Sie unter [Arbeiten mit LOB-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Wenn ein Unternehmen Sie zum Veröffentlichen von Apps für die exklusive Nutzung in diesem Unternehmen einlädt, erhalten Sie eine E-Mail mit einem Link, über den Sie die Zuordnung bestätigen können. Sie können diese Zuordnungen auch unter **Unternehmenszusammenschlüsse** in Ihren **Kontoeinstellungen** bestätigen.

Um die Zuordnung zu bestätigen, klicken Sie auf **Akzeptieren**. Über Ihr Konto können dann Apps zur exklusiven Nutzung durch dieses Unternehmen veröffentlicht werden.

## Übermitteln einer branchenspezifischen App


Wenn Sie eine App für die exklusive Nutzung durch ein Unternehmen veröffentlichen möchten, ist das Verfahren vergleichbar mit dem Übermitteln von anderen Apps. Die App durchläuft den gleichen Zertifizierungsprozess und muss allen [Windows Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/dn764944) entsprechen. Dieser Prozess weicht nur bei zwei Schritten ab.

### Verteilung und Sichtbarkeit

Nachdem Sie eine Unternehmenszuordnung eingerichtet haben, wird jedes Mal, wenn Sie eine App übermitteln, im Abschnitt **Verteilung und Sichtbarkeit** auf der Seite **Preise und Verfügbarkeit** der App ein Dropdownfeld angezeigt. Dies ist standardmäßig auf **Einzelhandel – Distribution** festgelegt. Damit die App exklusiv für ein Unternehmen bereitgestellt wird, müssen Sie **Branche – Distribution** auswählen.

Wenn **Branche – Distribution** aktiviert ist, werden die sonst angezeigten Optionen unter **Verteilung und Sichtbarkeit** durch eine Liste der Unternehmen ersetzt, für die Sie exklusive Apps veröffentlichen dürfen.

Wählen Sie die Unternehmen aus, die die App verwenden dürfen. Niemand sonst kann darauf zugreifen.

> **Hinweis**  Sie müssen mindestens ein Unternehmen auswählen, um eine App als branchenspezifische App zu veröffentlichen.

### Organisationslizenzierung

Standardmäßig ist das Kontrollkästchen **App für Organisationen mit vom Store verwalteter (Online-)Volumenlizenzierung verfügbar machen** aktiviert, wenn Sie eine App übermitteln. Bei der Veröffentlichung von branchenspezifischen Apps muss dieses Kontrollkästchen aktiviert sein, damit das Unternehmen Volumenlizenzen für die App erwerben kann. Hierdurch wird die App nicht für andere außerhalb der Unternehmen verfügbar gemacht, die Sie im Abschnitt **Verteilung und Sichtbarkeit** ausgewählt haben.

Wenn Sie die App für das Unternehmen über Offline-Lizenzierung verfügbar machen möchten, können Sie zusätzlich das Kontrollkästchen **Getrennte Lizenzierung (offline) für Käufe von Organisationen zulassen** aktivieren.

Weitere Informationen finden Sie unter [Optionen für die Organisationslizenzierung](organizational-licensing.md).

### Unternehmensbereitstellung von branchenspezifischen Apps

Wenn Sie auf **An Store übermitteln** klicken, durchläuft die App den Zertifizierungsprozess. Danach muss ein Administrator des Unternehmens die App im privaten Store des Unternehmens im Portal des Windows Store für Unternehmen hinzufügen. Das Unternehmen kann die App dann für seine Benutzer bereitstellen.

Weitere Informationen finden Sie unter [Arbeiten mit LOB-Apps](http://go.microsoft.com/fwlink/p/?LinkId=698846) und [Verteilen von Apps über Ihren privaten Store](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### Aktualisieren von branchenspezifischen Apps

Wenn Sie Updates für eine App veröffentlichen möchten, die bereits als branchenspezifische App veröffentlicht wurde, erstellen Sie einfach eine neue Übermittlung. Sie können neue Pakete hochladen oder andere Änderungen vornehmen. Klicken Sie dann auf **An Store übermitteln**, um die aktualisierte Version verfügbar zu machen. Achten Sie darauf, die Auswahl der Unternehmen unter **Verteilung und Sichtbarkeit** nicht zu ändern (es sei denn, Sie möchten sie bewusst ändern und z. B. ein zusätzliches Unternehmen auswählen, das die App erwerben kann, oder eines der zuvor eingerichteten Unternehmen löschen).

Wenn Sie eine App, die Sie zuvor als branchenspezifische App veröffentlicht haben, nicht mehr anbieten möchten und keine neuen Käufe möglich sein sollen, müssen Sie eine neue Übermittlung erstellen. Zunächst müssen Sie die Auswahl unter **Verteilung und Sichtbarkeit** von **Branche – Distribution** in **Einzelhandel – Distribution** ändern. Wählen Sie dann unter **Verteilung und Sichtbarkeit** die Option **Diese App ausblenden und den Verkauf stoppen** aus. Nachdem die Übermittlung den Zertifizierungsprozess durchlaufen hat, kann die App nicht mehr neu erworben werden. Benutzer, die sie bereits erworben haben, können sie jedoch weiter verwenden.

### Verteilen von branchenspezifischen Apps durch Querladen

Wenn Apps über den Store für Unternehmen verfügbar gemacht werden, wird sichergestellt, dass die App vom Store signiert wurde und den Standardrichtlinien des Stores entspricht.

In einigen Fällen möchten Unternehmen möglicherweise aus verschiedenen Gründen nicht, dass ihre branchenspezifischen Apps über das Windows Dev Center übermittelt werden (z. B. aus Compliance-Gründen oder bei Apps, für die weitere Funktionen benötigt werden). In diesem Fall können die Unternehmen Apps durch Querladen direkt auf Computern bereitstellen und müssen nicht den Windows Store für Unternehmen verwenden.

Weitere Informationen finden Sie unter [Querladen von Branchen-Apps in Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 






<!--HONumber=Mar16_HO1-->


