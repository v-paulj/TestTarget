---
author: Karl-Bridge-Microsoft
Description: "Erweitern Sie die grundlegenden Funktionen von Cortana durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen."
title: Cortana-Interaktionen
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d23416ad3344a39c09078b6ba3acc38fa3ba65a0

---

# Cortana-Interaktionen in UWP-Apps




Erweitern Sie die grundlegenden Funktionen von **Cortana** durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen. 


**Weitere Sprachkomponenten**

-   Falls Sie Spracherkennung und Text-zu-Sprache (auch bezeichnet als TTS oder Sprachsynthese) direkt in Ihre App integrieren, finden Sie in den [Richtlinien für den Sprachentwurf](speech-interactions.md) weitere Informationen.

> **Hinweis**  
> Ein Sprachbefehl ist eine einzelne, in einer Datei mit Sprachbefehlsdefinitionen (VCDs) definierte Äußerung mit einem bestimmten Zweck, die über **Cortana** an eine installierte App gerichtet wird.

> Eine VCD-Datei definiert einen oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.

> Eine Sprachbefehlsdefinition kann unterschiedlich komplex sein. Sie kann beliebige Äußerungen unterstützen – von einer einzelnen eingeschränkten Äußerung bis hin zu einer Sammlung flexiblerer Äußerungen in natürlicher Sprache, die alle dem gleichen spezifischen Zweck dienen.


Die Ziel-App kann abhängig von der Komplexität der Interaktion entweder im Vordergrund gestartet (die App erhält den Fokus, und **Cortana** wird geschlossen) oder im Hintergrund aktiviert werden (**Cortana** behält den Fokus und zeigt Ergebnisse aus der App an). Sprachbefehle, die zusätzlichen Kontext oder Benutzereingaben erfordern, werden beispielsweise am besten in einer Vordergrund-App ausgeführt, während einfache Befehle in **Cortana** über eine Hintergrund-App abgewickelt werden können.

 

**Cortana** integriert die grundlegenden Funktionen Ihrer App, bietet einen zentralen Einstiegspunkt, über den der Benutzer die meisten Aufgaben ohne Öffnen der App ausführen kann, und wird somit zum Bindeglied zwischen Ihrer App und dem Benutzer. Durch die Verknüpfung mit App-Funktionen und seltenere App-Wechsel kann der Benutzer viel Zeit sparen und wird erheblich entlastet.


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Artikel</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Entwurfsrichtlinien](cortana-design-guidelines.md)</p></td>
<td align="left"><p>Diese Richtlinien und Empfehlungen beschreiben, wie Ihre App bei der Benutzerinteraktion am besten von **Cortana** profitieren kann, um den Benutzer beim Ausführen einer Aufgabe zu unterstützen und ihm dabei deutlich zu vermitteln, was passiert.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Starten einer Vordergrund-App mit Sprachbefehlen](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Neben der Verwendung von Sprachbefehlen innerhalb von <strong>Cortana</strong> für den Zugriff auf Systemfeatures können Sie mithilfe von Sprachbefehlen über <strong>Cortana</strong> auch eine Vordergrund-App starten und eine Aktion oder einen Befehl angeben, der innerhalb der App ausgeführt werden soll.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Dynamisches Ändern von VCD-Begriffslisten](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (<strong>PhraseList</strong>-Elemente) in einer VCD-Datei zugreifen und diese aktualisieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Starten einer Hintergrund-App mit Sprachbefehlen](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Zusätzlich zur Verwendung von Sprachbefehlen in <strong>Cortana</strong> für den Zugriff auf Systemfeatures können Sie <strong>Cortana</strong> auch um Features und Funktionen einer Hintergrund-App erweitern, indem Sie Sprachbefehle verwenden, mit denen die Ausführung einer Aktion oder eines Befehls in der App angegeben wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interagieren mit einer Hintergrund-App](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>Hier erfahren Sie, wie ein Benutzer während der Ausführung eines Sprachbefehls über die Spracheingabe und die Canvas von <strong>Cortana</strong> mit einer Hintergrund-App interagieren kann.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Deep-Link zu einer Hintergrund-App](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>Stellen Sie Deep-Links aus dem Hintergrund-App-Dienst in <strong>Cortana</strong> bereit, um die App in einem bestimmten Zustand oder Kontext im Vordergrund zu starten.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Unterstützen von Sprachbefehlen in natürlicher Sprache](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Erfahren Sie, wie Sie <strong>Cortana</strong> mit flexibleren Befehlen in natürlicher Sprache erweitern, bei denen der Benutzer den App-Namen an einer beliebigen Stelle im Befehl verwenden kann.</p></td>
</tr>
</tbody>
</table>

 

## Verwandte Artikel


* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designer**
* [Entwurfsrichtlinien für die Spracherkennung](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Cortana-Entwurfsrichtlinien](https://msdn.microsoft.com/library/windows/apps/dn974233)

**Beispiele**
* [Cortana-Sprachbefehlbeispiel](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO5-->


