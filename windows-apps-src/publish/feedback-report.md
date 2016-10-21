---
author: jnHs
Description: "Im Feedbackbericht im Windows Dev Center-Dashboard werden die Probleme, Vorschläge und Zustimmungen angezeigt, die Ihre Windows 10-Kunden über den Feedback-Hub übermittelt haben."
title: Feedbackbericht
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
translationtype: Human Translation
ms.sourcegitcommit: 70020d3c6e0fb0fea321ce1951720803fd25f9c0
ms.openlocfilehash: e6266ff7c45a49b3eece8ffaf3d0603d55a04761

---

# Feedbackbericht

Im **Feedback-Bericht** im Windows Dev Center-Dashboard werden die Probleme, Vorschläge und Zustimmungen angezeigt, die Ihre Windows 10-Kunden über den Feedback-Hub übermittelt haben. Sie können diese Informationen in Ihrem Dashboard anzeigen oder die Daten exportieren, um sie offline anzuzeigen.

Ermuntern Sie Ihre Kunden, Ihnen Feedback zu Ihrer App zu geben. Dies ist eine hervorragende Möglichkeit, um mehr über die Probleme und Funktionen zu erfahren, die ihnen besonders wichtig sind. Wenn Ihre Kunden wissen, dass sie Ihnen ihr Feedback direkt senden können, geben sie mit höherer Wahrscheinlichkeit keine negative Bewertung ab.

> **Hinweis**  Sie können über diesen Bericht auch direkt [auf Feedback reagieren](respond-to-customer-feedback.md), um Kunden zu signalisieren, dass Sie ihr Feedback ernst nehmen.

Verwenden Sie die Feedback-API im [Microsoft Store Services SDK](http://aka.ms/store-em-sdk), um es Ihren Kunden zu ermöglichen, den [Feedback-Hub direkt aus Ihrer App heraus zu starten](../monetize/launch-feedback-hub-from-your-app.md). Bedenken Sie, dass jeder Kunde, der Ihre App auf einem Windows 10-Gerät heruntergeladen hat, das den Feedback-Hub unterstützt, mithilfe der Feedback-Hub-App ein Feedback abgeben kann. Aus diesem Grund wird in diesem Bericht möglicherweise Feedback von Kunden angezeigt, auch wenn Sie in Ihrer App nicht explizit um Feedback gebeten haben.

> **Tipp** Feedback ist besonders wertvoll, wenn Sie [Flight-Pakete](package-flights.md) verwenden, da im Feedbackbericht das spezifische Paket aufgeführt wird, das auf dem Gerät des jeweiligen Kunden installiert war, als er das Feedback abgegeben hat.

## Anzeigen von Feedbackdetails

Im Abschnitt **Details** dieses Berichts finden Sie das jeweilige Feedback von Ihren Kunden. Links neben dem Feedbacktext wird angezeigt, wie oft das Feedback von anderen Kunden im Feedback-Hub bewertet wurde. Sie können das Feedback auf drei Arten sortieren:

- **Zustimmung** (Standard): Zeigt Feedback, das von anderen Kunden bewertet wurde, beginnend mit dem Feedback, das die meisten Zustimmungen erhalten hat.
- **Populär**: Zeigt Feedback, das von anderen Kunden in den letzten sieben Tagen bewertet wurde, beginnend mit dem Feedback, das die letzte Aktivität aufweist.
- **Aktuellste**: Zeigt sämtliches Feedback, beginnend mit dem zuletzt abgegebenen Feedback an.

Neben jedem Kommentar werden der Typ des Feedbacks sowie das Datum angezeigt, an dem das Feedback abgegeben wurde. Außerdem werden der Markt des Kunden, das spezifische Paket Ihrer App, das beim Abgeben des Feedbacks auf dem Gerät installiert war, der Gerätetyp und **Windows-Insider** angezeigt, sofern der Kunde, der das Feedback übermittelt hat, Mitglied des Windows-Insider-Programms ist.


## Anwenden von Filtern

Im oberen Seitenbereich können Sie **Filter anwenden** erweitern, um alle Daten auf dieser Seite zu filtern.

> **Tipp** Wenn auf der Seite kein Feedback zu sehen ist, stellen Sie sicher, dass Sie mit Ihrer Filterauswahl nicht sämtliches Feedback ausgeschlossen haben. Wenn Sie z. B. nach einem **Gerätetyp** filtern, der von Ihrer App nicht unterstützt wird, wird kein Feedback angezeigt.

- **Datum**: Der Standardfilter ist **Immer**. Sie können kürzere Zeiträume zwischen **Letzte 30 Tage** und **Letzte 12 Monate** auswählen.
- **Feedbacktyp**: Die Standardeinstellung ist **Alle**. Sie können **Problem** oder **Vorschlag** auswählen, um nur diese Art von Feedback anzuzeigen.
- **Gerätetyp**: Die Standardeinstellung ist **Alle Geräte**. Sie können **Telefon**, **PC** oder **Tablet** auswählen, um nur Feedback anzuzeigen, das für den entsprechenden Gerätetyp abgegeben wurde.
- **Paketversion**: Die Standardeinstellung ist **Alle Pakete**. Sie können eines Ihrer Pakete auswählen, damit nur Feedback von Kunden angezeigt wird, die dieses Paket verwendet haben, als sie ihr Feedback abgegeben haben.
- **Markt**: Die Standardeinstellung ist **Alle Märkte**. Sie können einen bestimmten Markt auswählen, um nur Feedback von den in diesem Markt ansässigen Kunden anzuzeigen.
- **Gruppe**: Die Standardeinstellung ist **Alle**. Sie können festlegen, dass nur das Feedback angezeigt werden soll, das [Windows-Insider](http://insider.windows.com) abgeben.

## Übersetzen von Feedback

Rezensionen, die nicht in Ihrer bevorzugten Sprache verfasst wurden, werden standardmäßig für Sie übersetzt. Falls gewünscht, können Sie die Übersetzung von Feedback deaktivieren, indem Sie das Kontrollkästchen **Rezensionen übersetzen** oben rechts über der Feedbackliste deaktivieren.

Da das Feedback durch ein automatisches Übersetzungssystem übersetzt wird, sind die resultierenden Übersetzungen u. U. nicht immer exakt. Für den Fall, dass sie ihn mit der Übersetzung vergleichen oder auf andere Weise übersetzen lassen möchten, steht der Originaltext zur Verfügung.

## Starten des Feedback-Hubs direkt in der App

Wie bereits erwähnt, wird empfohlen, direkt in Ihrer App einen Link zum Feedback-Hub einzufügen, um die Benutzer zu ermuntern, Feedback abzugeben. Weitere Informationen finden Sie unter [Feedback-Hub in Ihrer App starten](../monetize/launch-feedback-hub-from-your-app.md).



<!--HONumber=Aug16_HO5-->


