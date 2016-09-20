---
author: DelfCo
Description: "Befolgen Sie diese bewährten Methoden beim Globalisieren Ihrer Apps für eine größere Zielgruppe und wenn Sie Ihre Apps für einen bestimmten Markt lokalisieren."
Search.Refinement.TopicID: 180
title: "Richtlinien für Globalisierung und Lokalisierung"
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: bdbe6b3e319aa90a78660c664f1603bac93399ca

---

# Empfohlene und nicht empfohlene Vorgehensweisen für Globalisierung und Lokalisierung





**Wichtige APIs**

-   [**Globalisierung**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)

Befolgen Sie diese bewährten Methoden beim Globalisieren Ihrer Apps für eine größere Zielgruppe und wenn Sie Ihre Apps für einen bestimmten Markt lokalisieren.



## <span id="guidelines_for_internationalization"></span><span id="GUIDELINES_FOR_INTERNATIONALIZATION"></span>Globalisierung

Bereiten Sie Ihre App für die einfache Anpassung an verschiedene Märkte vor, indem Sie angemessene globale Begriffe und Bilder für Ihre Benutzeroberfläche auswählen, [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)-APIs zum Formatieren von App-Daten verwenden und Annahmen auf Grundlage von Standort und Sprache vermeiden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Empfehlung</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Verwenden Sie die richtigen Formate für Zahlen, Datumsangaben, Uhrzeiten, Adressen und Telefonnummern.</p></td>
<td align="left"><p>Die Formatierung für Zahlen, Datumsangaben, Uhrzeiten und andere Datenformen unterscheidet sich zwischen Kulturen, Regionen, Sprachen und Märkten. Wenn Sie Zahlen, Daten, Uhrzeiten oder andere Daten anzeigen, verwenden Sie [<strong>Globalization</strong>](https://msdn.microsoft.com/library/windows/apps/br206813)-APIs, um das für die bestimmte Zielgruppe angemessene Format zu erhalten.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Unterstützen Sie internationale Papierformate.</p></td>
<td align="left"><p>Die gebräuchlichen Papierformate unterscheiden sich zwischen den Ländern. Unterstützen und testen Sie häufig verwendete internationale Formate, wenn Sie Features einsetzen möchten, bei denen das Papierformat eine Rolle spielt (z.B. Drucken).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Unterstützen Sie internationale Maßeinheiten und Währungen.</p></td>
<td align="left"><p>In den verschiedenen Ländern werden unterschiedlichen Einheiten und Skalen verwendet, wobei das metrische und das imperiale System am verbreitetsten sind. Wenn Sie mit Maßen wie Länge, Temperatur oder Fläche arbeiten, erhalten Sie mit der [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793)-Eigenschaft die richtigen Systemmaße.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Zeigen Sie Text und Schriftarten richtig an.</p></td>
<td align="left"><p>Die ideale Schriftart, Schriftgrad und Textrichtung hängt vom jeweiligen Markt ab.</p>
<p>Weitere Informationen finden Sie unter [<strong>Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Verwenden Sie Unicode zur Zeichencodierung.</p></td>
<td align="left"><p>In den neueren Versionen von Microsoft Visual Studio wird standardmäßig für alle Dokumente die Unicode-Zeichencodierung verwendet. Stellen Sie bei Verwendung eines anderen Editors sicher, dass Sie die Quelldateien in der richtigen Unicode-Zeichencodierung speichern. Alle Windows-Runtime-APIs geben UTF-16-codierte Zeichenfolgen zurück.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Zeichnen Sie die Eingabesprache auf.</p></td>
<td align="left"><p>Wenn die App den Benutzer zum Eingeben von Text auffordert, zeichnen Sie die eingegebene Sprache auf. Dadurch stellen Sie sicher, dass die Eingabe dem Benutzer später in der richtigen Formatierung angezeigt wird. Rufen Sie die aktuelle Eingabesprache mit der [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658)-Eigenschaft ab.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Leiten Sie den Standort des Benutzers nicht von der Sprache ab und umgekehrt nicht die Sprache vom Standort des Benutzers.</p></td>
<td align="left"><p>Sprache und Standort des Benutzers sind in Windows zwei unterschiedliche Konzepte. Ein Benutzer kann eine bestimmte regionale Sprachvariante sprechen, beispielsweise "en-gb" für britisches Englisch, kann sich jedoch in einem ganz anderen Land oder einer anderen Region befinden. Überlegen Sie, ob die Sprache des Benutzers für die App relevant ist, z.B. für UI-Text, und ob der Standort relevant ist, z.B. für Lizenzbelange.</p>
<p>Weitere Informationen finden Sie unter [<strong>Verwalten von Sprache und Region</strong>](manage-language-and-region.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Verwenden Sie keine umgangssprachlichen Ausdrücke und Metaphern.</p></td>
<td align="left"><p>Eine für eine demografische Gruppe (z.B. Kultur oder Alter) spezifische Sprache ist schwer zu übersetzen, da nur Personen in dieser demografischen Gruppe diese Sprache verwenden. Ebenso ergeben Metaphern nicht für alle Personen Sinn. Beispielsweise hat &quot;Bache&quot; (weibliches Wildschwein) für Jäger eine besondere, für andere Personen jedoch keine Bedeutung. Wenn Sie die App lokalisieren möchten und informelle Formulierungen verwenden, erklären Sie den Lokalisierern genau die zu übersetzende Bedeutung und Ausdrucksweise.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Verwenden Sie keinen technischen Jargon, Abkürzungen oder Akronyme.</p></td>
<td align="left"><p>Fachsprache wird von Fachfremden kaum verstanden und ist schwer zu übersetzen. Im alltäglichen Sprachgebrauch ist diese Terminologie unüblich. Fachsprache erscheint oft in Fehlermeldungen, um auf Hardware- und Softwareprobleme hinzuweisen. Manchmal lässt sich das nicht vermeiden. Schreiben Sie die Formulierungen jedoch möglichst allgemein verständlich um.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Verwenden Sie keine potenziell anstößigen Bilder.</p></td>
<td align="left"><p>Bilder, die in Ihrer Kultur angemessen sind, könnten in anderen Kulturen als anstößig empfunden werden. Vermeiden Sie religiöse Symbole, Tiere oder Farbkombinationen, die mit Nationalfahnen oder politischen Bewegungen in Verbindung gebracht werden könnten.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Vermeiden Sie politische Beleidigungen in Karten oder bei Verweisen auf Regionen.</p></td>
<td align="left"><p>Karten können umstrittene regionale oder nationale Grenzen enthalten und erweisen sich als eine häufige Quelle von politischen Beleidigungen. Achten Sie darauf, dass die Benutzeroberfläche für die Auswahl von Nationen die Formulierung &quot;Land/Region&quot; verwendet. Wenn ein umstrittenes Gebiet in einer Liste mit der Bezeichnung &quot;Länder&quot; (z.B. in einem Adressformular) aufgeführt wird, könnte dies zu Problemen führen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Verwenden Sie zum Vergleichen von Sprachtags nicht nur den Zeichenfolgenvergleich.</p></td>
<td align="left"><p>BCP-47-Sprachtags sind komplex. Beim Vergleichen von Sprachtags muss einiges berücksichtigt werden, wie z.B. der Abgleich von Skriptinformationen, Legacytags und mehrfache regionale Varianten. Das Ressourcenverwaltungssystem in Windows übernimmt den Abgleich für Sie. Sie können eine Ressourcengruppe in beliebigen Sprachen angeben, und das System wählt die geeignete Gruppe für den Benutzer und die App aus.</p>
<p>Weitere Informationen zur Ressourcenverwaltung finden Sie unter [<strong>Definition der App-Ressourcen</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Gehen Sie nicht immer von einer alphabetischen Sortierung aus.</p></td>
<td align="left"><p>Bei Sprachen, die keine Lateinschrift verwenden, basiert die Sortierung auf anderen Faktoren wie der Aussprache oder der Anzahl der Schriftzeichen usw. Selbst bei Sprachen mit Lateinschrift wird nicht immer alphabetisch sortiert. In einigen Kulturen enthält ein Telefonbuch vielleicht keine alphabetische Sortierung. Das System kann die Sortierung für Sie übernehmen. Berücksichtigen Sie jedoch beim Erstellen eines eigenen Sortieralgorithmus die Sortiermethoden des Zielmarkts.</p></td>
</tr>
</tbody>
</table>

 

## <span id="guidelines_for_localization"></span><span id="GUIDELINES_FOR_LOCALIZATION"></span>Lokalisierung

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Empfehlung</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Trennen Sie Ressourcen wie Zeichenfolgen und Bilder der Benutzeroberfläche vom Code.</p></td>
<td align="left"><p>Entwickeln Sie Ihre Apps so, dass Ressourcen wie Zeichenfolgen und Bilder vom Code abgegrenzt sind. So können sie unabhängig verwaltet, lokalisiert und für unterschiedliche Skalierungsfaktoren, Barrierefreiheitsoptionen und andere Benutzer- und Computersituationen angepasst werden.</p>
<p>Trennen Sie Zeichenfolgenressourcen vom Code der App, und erstellen Sie eine einzige sprachunabhängige Codebasis. Trennen Sie immer Zeichenfolgen von Code und Markup, und speichern Sie sie in einer Ressourcendatei (z.B. ResW oder ResJSON).</p>
<p>Verwenden Sie die Ressourceninfrastruktur in Windows zur Auswahl der am besten geeigneten Ressourcen, die der Laufzeitumgebung eines bestimmten Benutzers am besten gerecht werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Isolieren Sie andere lokalisierbare Ressourcendateien.</p></td>
<td align="left"><p>Speichern Sie andere Dateien, die lokalisiert werden müssen, in Ordnern mit den entsprechenden Sprachnamen. Dazu gehören z.B. Bilder mit Text, der übersetzt oder aus kulturellen Gründen geändert werden muss.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Legen Sie die Standardsprache fest, und kennzeichnen Sie alle Ressourcen (selbst die in der Standardsprache).</p></td>
<td align="left"><p>Legen Sie immer die am besten geeignete Standardsprache für die Apps im App-Manifest (package.appxmanifest) fest. Die Standardsprache wird verwendet, wenn der Benutzer keine der unterstützten Sprachen der App spricht. Kennzeichnen Sie Ressourcen der Standardsprache (z.B. „en-us/Logo.png“) mit der entsprechenden Sprache, sodass das System die Sprache der Ressource und ihre Verwendung in bestimmten Situationen erkennen kann.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ermitteln Sie die Ressourcen der App, die lokalisiert werden müssen.</p></td>
<td align="left"><p>Was muss bei einer Lokalisierung für andere Märkte geändert werden? Textzeichenfolgen müssen in andere Sprachen übersetzt werden. Bilder müssen gegebenenfalls anderen Kulturen entsprechend angepasst werden. Überlegen Sie, welche anderen Ressourcen von einer Lokalisierung betroffen sein können (wie Audio oder Video).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Verwenden Sie Ressourcenbezeichner in Code und Markup, um auf Ressourcen zu verweisen.</p></td>
<td align="left"><p>Verwenden Sie Verweise auf Ressourcen anstelle von Zeichenfolgenliteralen oder speziellen Dateinamen für Bilder im Markup. Achten Sie auf einen eindeutigen Bezeichner für jede Ressource. Weitere Informationen finden Sie unter [<strong>So wird's gemacht: Benennen von Ressourcen mithilfe von Qualifizierern</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324).</p>
<p>Achten Sie auf Ereignisse, die ausgelöst werden, wenn sich das System ändert und beginnt, einen anderen Satz von Qualifizierern zu verwenden. Verarbeiten Sie das Dokument erneut, damit die richtigen Ressourcen geladen werden können.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Ermöglichen Sie das Vergrößern des Textumfangs.</p></td>
<td align="left"><p>Weisen Sie Textpuffer dynamisch zu, da der Textumfang beim Übersetzen zunehmen kann. Wenn sich statische Puffer nicht vermeiden lassen, verwenden Sie besonders große (vielleicht die doppelte Länge bei einer englischen Zeichenfolge), um die eventuell längere Zeichenfolge nach der Übersetzung zu berücksichtigen. Auch bei der Benutzeroberfläche kann der verfügbare Platz eingeschränkt sein. Stellen Sie für die lokalisierten Sprachen sicher, dass die Zeichenfolgen etwa 40% länger sein können als die englischen Zeichenfolgen. Bei sehr kurzen Zeichenfolgen wie einzelnen Wörtern ist vielleicht sogar 300% mehr Platz nötig. Wenn zudem mehrere Zeilen und Textumbrüche in Steuerelementen unterstützt werden, ist mehr Platz zum Anzeigen von Zeichenfolgen verfügbar.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Unterstützen Sie Spiegelung.</p></td>
<td align="left"><p>Die Textausrichtung und Leserichtung kann von links nach rechts (wie bei Deutsch) oder von links nach rechts (wie bei Arabisch oder Hebräisch) verlaufen. Wenn Sie Ihr Produkt in Sprachen mit einer anderen Leserichtung lokalisieren, achten Sie darauf, dass das Layout der UI-Elemente eine Spiegelung unterstützt. Auch Elemente wie Zurück-Schaltflächen, UI-Übergangseffekte und Bilder müssen gegebenenfalls gespiegelt werden.</p>
<p>Weitere Informationen finden Sie unter [<strong>Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Kommentieren Sie Zeichenfolgen.</p></td>
<td align="left"><p>Stellen Sie sicher, dass Zeichenfolgen ausreichend kommentiert sind und nur zu übersetzende Zeichenfolgen an den Lokalisierer übergeben werden. Eine übermäßige Lokalisierung ist eine häufige Problemquelle.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Verwenden Sie kurze Zeichenfolgen.</p></td>
<td align="left"><p>Kürzere Zeichenfolgen können einfacher übersetzt werden und ermöglichen eine mehrfache Verwendung. Bei mehrfacher Verwendung einer Übersetzung sparen Sie Geld, da dieselbe Zeichenfolge nicht mehrmals an den Lokalisierer gesendet wird.</p>
<p>Zeichenfolgen, die 8192 Zeichen überschreiten, werden von einigen Lokalisierungstools nicht unterstützt. Beschränken Sie die Länge deshalb auf maximal 4000 Zeichen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Geben Sie die Zeichenfolgen als ganze Sätze an.</p></td>
<td align="left"><p>Teilen Sie einen Satz nicht in einzelne Wörter, sondern geben Sie die Zeichenfolgen in ganzen Sätzen an. Denn die Übersetzung der Wörter kann von ihrer Position im Satz abhängen. Gehen Sie bei einem Satz mit mehreren Parametern auch nicht davon aus, dass diese Parametern bei allen Sprachen an derselben Stelle bleiben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Optimieren Sie Bild- und Audiodateien für die Lokalisierung.</p></td>
<td align="left"><p>Vermeiden Sie Text in Bildern oder Sprache in Audiodateien, um die Lokalisierungskosten gering zu halten. Wenn die lokalisierte Sprache in eine andere Leserichtung als die ursprüngliche Sprache verläuft, unterstützen symmetrische Bilder und Effekte die Spiegelung besser.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Wiederholen Sie eine Zeichenfolge nicht in einem unterschiedlichen Kontext.</p></td>
<td align="left"><p>Verwenden Sie Zeichenfolgen nicht erneut in anderen Kontexten, da auch einfache Wörter wie &quot;on&quot; oder &quot;off&quot; je nach Kontext verschieden übersetzt werden.</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Verwandte Artikel


**Beispiele**
* [Anwendungsressourcen und Lokalisierung – Beispiel](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [Beispiel für Globalisierungseinstellungen](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 






<!--HONumber=Jun16_HO4-->


