---
Description: Erweitern Sie die grundlegenden Funktionen von Cortana durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen.
title: Cortana-Interaktionen
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
---

# Cortana-Interaktionen in UWP-Apps


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Erweitern Sie die grundlegenden Funktionen von **Cortana** durch Sprachbefehle, die eine einzelne Aktion in einer externen Anwendung starten und ausführen. 


**Weitere Sprachkomponenten**

-   Falls Sie Spracherkennung und Text-zu-Sprache (auch bezeichnet als TTS oder Sprachsynthese) direkt in Ihre App integrieren, finden Sie in den [Richtlinien für den Sprachentwurf](speech-interactions.md) weitere Informationen.

> **Hinweis**  
> Ein Sprachbefehl ist eine einzelne, in einer Datei mit Sprachbefehlsdefinitionen (VCDs) definierte Äußerung mit einem bestimmten Zweck, die über **Cortana** an eine installierte App gerichtet wird.

> Eine VCD-Datei definiert einzelne oder mehrere Sprachbefehle, die jeweils einem bestimmten Zweck dienen.

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
<td align="left"><p>[Diese Richtlinien und Empfehlungen beschreiben, wie Ihre App bei der Benutzerinteraktion am besten von **Cortana** profitieren kann, um den Benutzer beim Ausführen einer Aufgabe zu unterstützen und ihm dabei deutlich zu vermitteln, was passiert.](cortana-design-guidelines.md)</p></td>
<td align="left"><p>Neben der Verwendung von Sprachbefehlen innerhalb von <strong>Cortana</strong> für den Zugriff auf Systemfeatures können Sie mithilfe von Sprachbefehlen über <strong>Cortana</strong> auch eine Vordergrund-App starten und eine Aktion oder einen Befehl angeben, der innerhalb der App ausgeführt werden soll.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Hier erfahren Sie, wie Sie mithilfe des Spracherkennungsergebnisses zur Laufzeit auf die Liste der unterstützten Begriffe (<strong>PhraseList</strong>-Elemente) in einer VCD-Datei zugreifen und diese aktualisieren.](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Zusätzlich zur Verwendung von Sprachbefehlen in <strong>Cortana</strong> für den Zugriff auf Systemfeatures können Sie <strong>Cortana</strong> auch um Features und Funktionen einer Hintergrund-App erweitern, indem Sie Sprachbefehle verwenden, mit denen die Ausführung einer Aktion oder eines Befehls in der App angegeben wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Hier erfahren Sie, wie ein Benutzer während der Ausführung eines Sprachbefehls über die Spracheingabe und die Canvas von <strong>Cortana</strong> mit einer Hintergrund-App interagieren kann.](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>Stellen Sie Deep-Links aus dem Hintergrund-App-Dienst in <strong>Cortana</strong> bereit, um eine Vordergrund-App in einem bestimmten Zustand oder Kontext zu starten.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Erfahren Sie, wie Sie <strong>Cortana</strong> mit flexibleren Befehlen in natürlicher Sprache erweitern, bei denen der Benutzer den App-Namen an einer beliebigen Stelle im Befehl verwenden kann.](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p><span id="related_topics"></span>Verwandte Artikel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[VCD elements and attributes v1.2](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>Designer</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Entwurfsrichtlinien für die Spracherkennung](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>Cortana-Entwurfsrichtlinien</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Beispiele](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Cortana-Sprachbefehlbeispiel</p></td>
</tr>
</tbody>
</table>

 

## <ph id="ph1">&lt;span id="related_topics"&gt;</ph><ph id="ph2">&lt;/span&gt;</ph>Related articles


* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Speech design guidelines](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Cortana design guidelines](https://msdn.microsoft.com/library/windows/apps/dn974233)

**Samples**
* [Cortana voice command sample](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Apr16_HO3-->


