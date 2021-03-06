---
author: QuinnRadich
Description: "Entwerfen Sie eine Benutzeroberfläche mit Anleitungen, damit Benutzer lernen können, wie sie Ihre Windows-App nutzen können."
title: "Richtlinien für das Entwerfen von Benutzeroberflächen mit Anleitungen"
label: Instructional UI
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 08b0b88e8ef17c2a8f264df5db4f971c8c49ab2e
ms.openlocfilehash: 6fc4840ce37d05bbaf0e423049d79b4e0c4febfc

---

# Richtlinien für Benutzeroberflächen mit Anleitungen

\ [Aktualisiert für Apps für die universelle Windows-Plattform (UWP) unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \].

Manchmal kann es hilfreich sein, Anleitungen bereitzustellen, um Benutzern die Funktionen in Ihrer App zu demonstrieren, die möglicherweise nicht offensichtlich sind, z.B. bestimmte Touchinteraktionen. In diesen Fällen müssen Sie Anleitungen auf der Benutzeroberfläche für die Benutzer bereitstellen, damit sie diese Funktionen, die sie möglicherweise übersehen haben, finden und nutzen können.

## <span id="when_to_use_instructional_ui"></span><span id="WHEN_TO_USE_INSTRUCTIONAL_UI"></span>Gründe für die Verwendung einer Benutzeroberfläche mit Anleitungen

Benutzeroberflächen mit Anleitungen sollten Sie sparsam einsetzen. Bei übermäßiger Nutzung werden die Anleitungen leicht ignoriert oder verärgern die Benutzer und sind somit wirkungslos.

Benutzeroberflächen mit Anleitungen sollten dazu dienen, dass Benutzer auf wichtige und nicht offensichtliche Funktionen Ihrer App hingewiesen werden, wie z.B. Touchgesten oder Einstellungen, die für sie von Interesse sind. Sie können auch dazu verwendet werden, Benutzer über neue Funktionen und Änderungen in der App aufmerksam zu machen, die sie andernfalls möglicherweise übersehen.

Benuteroberflächen mit Anleitungen sollten nur dann zum Vermitteln grundlegender Funktionen Ihrer App verwendet werden, wenn Ihre App von Touchgesten abhängig ist.

## <span id="writing_instructional_ui"></span><span id="WRITING_INSTRUCTIONAL_UI"></span>Grundsätze für das Entwerfen von Benutzeroberflächen mit Anleitungen

Gute Benutzeroberflächen mit Anleitungen sind hilfreich und lehrreich für Benutzer und optimieren die Benutzerfreundlichkeit. Folgende Merkmale sollten diese Benutzeroberflächen ausmachen:

-   **Einfach:** Benutzer möchten nicht durch komplizierte Informationen unterbrochen werden.
-   **Einprägsam:** Benutzer möchten nicht, dass bei jedem Versuch, eine Aufgabe auszuführen, die gleichen Anleitungen angezeigt werden. Die Anleitungen müssen daher leicht einprägsam sein.
-   **Unmittelbar relevant:** Wenn die Anleitungen auf der Benutzeroberfläche sich nicht auf eine Aktion beziehen, die die Benutzer gerade ausführen möchten, sehen sie keinen Grund, diesen ihre Aufmerksamkeit zu schenken.

Setzen Sie Benutzeroberflächen mit Anleitungen nicht übermäßig ein, und achten Sie darauf, die richtigen Themen auszuwählen. Stellen Sie sicher, dass Folgendes nicht mithilfe von Benutzeroberflächen mit Anleitungen vermittelt wird:

-   **Grundlegende Features:** Wenn Benutzer Anleitungen zum Verwenden Ihrer App benötigen, sollten Sie das App-Design intuitiver gestalten.
-   **Offensichtliche Features:** Wenn eine Funktion für einen Benutzer ohne Anleitung offensichtlich ist, wird ihn eine Benutzeroberfläche mit Anleitungen nur behindern.
-   **Komplexe Features:** Benutzeroberflächen mit Anleitungen müssen präzise sein. Benutzer, für die komplexe Funktionen von Interesse sind, sind in der Regel bereit, selbst nach Anleitungen zu suchen.

Vermeiden Sie es, die Benutzererfahrung durch Ihre Benutzeroberfläche mit Anleitungen zu beeinträchtigen. Nicht empfohlene Vorgehensweise:

-   **Überdecken wichtiger Informationen:** Benutzeroberflächen mit Anleitungen sollten niemals andere Features Ihrer App verdecken.
-   **Nutzungszwang für Benutzer:** Benutzer sollten in der Lage sein, Anleitungen in der Benutzeroberfläche zu ignorieren und mit der Verwendung der App fortzufahren.
-   **Wiederholte Anzeige von Informationen:** Belästigen Sie die Benutzer nicht mit der wiederholten Anzeige von Anleitungen auf der Benutzeroberfläche, auch wenn diese bei der ersten Anzeige ignoriert wurden. Das Hinzufügen einer Einstellung, mit der Anleitungen erneut auf der Benutzeroberfläche angezeigt werden können, ist die bessere Lösung.

## <span id="examples_of_instructional_ui"></span><span id="EXAMPLES_OF_INSTRUCTIONAL_UI"></span>Beispiele für Benutzeroberflächen mit Anleitungen

Hier sehen Sie einige Beispiele für Benutzeroberflächen mit Anleitungen, die für Benutzer sinnvoll sind:

-   **Unterstützen der Benutzer beim Kennenlernen von Touchinteraktionen.** Der folgende Screenshot zeigt eine Benutzeroberfläche mit Anleitungen, mit deren Hilfe ein Spieler lernt, wie er im Spiel *Cut the Rope* Touchgesten verwenden kann.

    ![Screenshot aus dem Spiel, der die Meldung „Slide across to cut the rope“ (Führen Sie zum Trennen des Seils eine Ziehbewegung aus.) der Benutzeroberfläche mit Anweisungen zeigt.](images/in-game-controls-3.png)

-   **Erzielen Sie einen guten ersten Eindruck.** Beim ersten Starten von Movie Moments wird der Benutzer mithilfe von Anleitungen auf der Benutzeroberfläche dazu aufgefordert, mit der Erstellung von Filmen zu beginnen, ohne dass seine Benutzererfahrung beeinträchtig wird.

    ![Startbildschirm für die App „Movie Moments“](images/instructional-ui-movie.png)

-   **Führen der Benutzer zum nächsten Schritt einer komplizierten Aufgabe.** In der Windows-Mail-App werden die Benutzer mithilfe eines Hinweises am unteren Rand des Posteingangs zu den **Einstellungen** für den Zugriff auf ältere Nachrichten geleitet.

    ![Zugeschnittener Screenshot der Windows Mail-App, der eine Meldung einer Benutzeroberfläche mit Anleitungen zeigt](images/instructional-ui-mail-inbox.png)

    Wenn der Benutzer auf die Meldung klickt, wird auf der rechten Seite des Bildschirms das **Einstellungen**-Flyout der App angezeigt, in dem der Benutzer die Aufgabe ausführen kann. Diese Screenshots zeigen die Mail-App vor und nach dem Auswählen der Meldung für die Benutzeroberfläche mit Anleitungen durch den Benutzer.

    | Vorher                                                               | Nachher                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Screenshot der Windows-Mail-App](images/instructional-ui-mail.png) | ![Screenshot der Windows Mail-App mit einem erweiterten Einstellungen-Flyout](images/instructional-ui-mail-flyout.png) |

## <span id="related_topics"></span>Verwandte Artikel

* [Anleitungen für die App-Hilfe](guidelines-for-app-help.md)



<!--HONumber=Aug16_HO3-->


