---
author: DelfCo
Description: "Mithilfe der verschiedenen Sprach- und Regionseinstellungen von Windows können Sie die Auswahl von UI-Ressourcen und die Formatierung der UI-Elemente der App steuern."
title: Verwalten von Sprache und Region
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
label: Manage language and region
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 294f087fffeefda67ddacd09636915144bf18ff4

---

# Verwalten von Sprache und Region





**Wichtige APIs**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**WinJS.Resources-Namespace**](https://msdn.microsoft.com/library/windows/apps/br229779)

Mithilfe der verschiedenen Sprach- und Regionseinstellungen von Windows können Sie die Auswahl von UI-Ressourcen und die Formatierung der UI-Elemente der App steuern.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Einführung


Unter [Anwendungsressourcen und Lokalisierung – Beispiel](http://go.microsoft.com/fwlink/p/?linkid=231501) finden Sie eine Beispiel-App, die die Verwaltung der Sprach- und Regionseinstellungen veranschaulicht.

In Windows muss der Benutzer nicht genau eine Sprache aus einer begrenzten Gruppe möglicher Sprachen auswählen. Er kann stattdessen jede beliebige Sprache der Welt angeben, selbst wenn Windows selbst noch nicht in diese Sprache übersetzt wurde. Der Benutzer kann auch angeben, dass er mehrere Sprachen spricht.

Ein Windows-Benutzer kann einen Standort angeben, der sich überall auf der Welt befinden kann. Er kann ortsunabhängig auch angeben, dass er jede beliebige Sprache spricht. Standort und Sprache schränken sich nicht gegenseitig ein. Wenn also ein Benutzer Französisch spricht, muss das nicht heißen, dass er sich auch in Frankreich aufhält. Umgekehrt bedeutet ein Standort in Frankreich nicht, dass der Benutzer Französisch sprechen möchte.

Der Benutzer kann Apps in einer völlig anderen Sprache ausführen als Windows selbst. Er kann beispielsweise eine App in Spanisch und Windows in Englisch ausführen.

Für Windows Store-Apps wird eine Sprache als ein [BCP-47-Sprachtag](http://go.microsoft.com/fwlink/p/?linkid=227302) dargestellt. Die meisten APIs in Windows-Runtime, HTML und XAML können Zeichenfolgendarstellungen dieser BCP-47-Sprachtags zurückgeben oder akzeptieren. Weitere Informationen finden Sie in der [IANA-Liste der Sprachen](http://go.microsoft.com/fwlink/p/?linkid=227303).

Eine Liste mit speziell vom WindowsStore unterstützten Sprachtags finden Sie unter [Unterstützte Sprachen](https://msdn.microsoft.com/library/windows/apps/jj657969).

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Aufgaben


### <span id="Users_can_set_their_language_preferences."></span><span id="users_can_set_their_language_preferences."></span><span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."></span>Benutzer können ihre Spracheinstellungen festlegen.

Die Liste mit den Spracheinstellungen des Benutzers enthält dessen bevorzugte Sprachen in der vom Benutzer angegebenen Reihenfolge.

Benutzer legen die Liste unter **Einstellungen**&gt;**Zeit und Sprache**&gt;**Region und Sprache** fest. Alternativ können sie **Systemsteuerung**&gt;**Zeit, Sprache und Region** verwenden.

Die Liste mit den Spracheinstellungen des Benutzers kann mehrere Sprachen sowie regionale und andere spezielle Varianten enthalten. So bevorzugt der Benutzer beispielsweise vielleicht „fr-CA“, versteht aber auch „en-GB“.

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."></span><span id="specify_the_supported_languages_in_the_app_s_manifest."></span><span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."></span>Geben Sie die unterstützten Sprachen im App-Manifest an.

Geben Sie die Liste mit den unterstützten Sprachen Ihrer App im [**Resources-Element**](https://msdn.microsoft.com/library/windows/apps/dn934770) des App-Manifests (normalerweise „Package.appxmanifest“) an. Andernfalls generiert Visual Studio die Liste mit den Sprachen in der Manifestdatei automatisch auf der Grundlage der im Projekt gefundenen Sprachen. Die unterstützten Sprachen müssen im Manifest genau und mit dem richtigen Granularitätsgrad beschrieben werden. Die im Manifest aufgeführten Sprachen sind die Sprachen, die den Benutzern im Windows Store angezeigt werden.

### <span id="Specify_the_default_language."></span><span id="specify_the_default_language."></span><span id="SPECIFY_THE_DEFAULT_LANGUAGE."></span>Angeben der Standardsprache.

Öffnen Sie „package.appxmanifest“ in Visual Studio, navigieren Sie zur Registerkarte **Anwendung**, und legen Sie Ihre Standardsprache auf die Sprache fest, in der Sie Ihre Anwendung erstellen.

Wenn eine App keine der Sprachen unterstützt, die der Benutzer ausgewählt hat, wird die Standardsprache verwendet. Visual Studio verwendet die Standardsprache, um Ressourcen, die in dieser Sprache gekennzeichnet sind, Metadaten hinzuzufügen, die die Auswahl der richtigen Ressourcen zur Laufzeit ermöglichen.

Die Eigenschaft für die Standardsprache muss auch als erste Sprache im Manifest angegeben werden, damit die App-Sprache korrekt festgelegt werden kann (wie unten im Schritt „Erstellen der Anwendungssprachenliste“ beschrieben). Ressourcen in der Standardsprache müssen trotzdem mit ihrer Sprache gekennzeichnet werden (z. B. „en-US/logo.png“). Die Standardsprache gibt nicht implizit die Sprache nicht gekennzeichneter Ressourcen an. Weitere Informationen finden Sie unter [So wird's gemacht: Benennen von Ressourcen mit Qualifizierern](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

### <span id="Qualify_resources_with_their_language."></span><span id="qualify_resources_with_their_language."></span><span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."></span>Qualifizieren von Ressourcen mit ihrer Sprache.

Berücksichtigen Sie Ihr Zielpublikum sowie Sprache und Standort der Benutzer, die Sie ansprechen möchten. Viele Menschen, die in einer bestimmten Region leben, bevorzugen nicht die Hauptsprache dieser Region. Es gibt z.B. Millionen von Haushalten in den USA. in den hauptsächlich Spanisch gesprochen wird.

Qualifizieren von Ressourcen mit einer Sprache:

-   Binden Sie ein Skript ein, falls für die Sprache kein Wert definiert ist, der besagt, dass Skripte unterdrückt werden sollen. Einzelheiten zu Sprachtags finden Sie unter [IANA Subtag Registry](http://go.microsoft.com/fwlink/p/?linkid=227303). Verwenden Sie z.B. "zh-Hant", "zh-Hant-TW" oder "zh-Hans", aber nicht "zh-CN" oder "zh-TW".
-   Markieren Sie den gesamten sprachlichen Inhalt für eine Sprache. Die Projekteigenschaft "Standardsprache" ist nicht die Sprache der nicht markierten Ressourcen (d.h. eine neutrale Sprache), sondern sie gibt an, welche markierte Sprache ausgewählt werden soll, wenn keine markierte Sprachressource zum Benutzer passt.

Markieren von Ressourcen mit einer genauen Darstellung des Inhalts.

-   Windows führt einen komplexen Abgleich unter Berücksichtigung regionaler Varianten wie „en-US“ und „en-GB“ durch, sodass Anwendungen die Ressourcen mit einer exakten Inhaltsdarstellung markieren und Windows den korrekten Abgleich für jeden Benutzer überlassen können.
-   Windows Store zeigt für Benutzer, die auf die Anwendung blicken, den Inhalt des Manifests an.
-   Berücksichtigen Sie, dass für manche Tools und Komponenten wie Übersetzungsprogramme spezielle Sprachtags wie Informationen zu regionalen Dialekten für das Verständnis der Daten hilfreich sein können.
-   Markieren Sie Ressourcen unbedingt mit allen Einzelheiten, insbesondere dann, wenn mehrere Varianten verfügbar sind. Markieren Sie z.B. "en-GB" und "en-US", wenn beide für die Region spezifisch sind.
-   Bei Sprachen mit nur einem Standarddialekt muss keine Region hinzugefügt werden. Eine Verwendung von allgemeinen Tags ist in einigen Situationen vernünftig, so z.B. das Markieren von Ressourcen mit "ja" anstelle von "ja-JP".

In manchen Situationen müssen nicht alle Ressourcen lokalisiert werden.

-   Markieren Sie Ressourcen wie UI-Zeichenfolgen, die in allen Sprachen bereitgestellt werden, mit ihrer eigenen Sprache, und stellen Sie sicher, dass alle diese Ressourcen in der Standardsprache vorhanden sind. Es muss keine neutrale Ressource angegeben werden (eine nicht mit einer Sprache markierte).
-   Geben Sie bei Ressourcen, die eine Teilmenge der Menge der Sprachen der Anwendung darstellen (partielle Lokalisierung), die Menge der Sprachen an, in denen die Ressourcen vorliegen, und stellen Sie sicher, dass alle diese Ressourcen in der Standardsprache vorliegen. Windows wählt nach Beachtung aller vom Benutzer gesprochenen Sprachen in der bevorzugten Reihenfolge die für den Benutzer bestmögliche Sprache aus. Z.B. muss nicht die ganze UI einer App ins Katalanische lokalisiert werden, wenn die App über einen vollständigen Satz von Ressourcen auf Spanisch verfügt. Wenn ein Benutzer Katalanisch und dann Spanisch spricht, werden die nicht auf Katalanisch verfügbaren Ressourcen auf Spanisch angezeigt.
-   Bei Ressourcen mit spezifischen Ausnahmen für bestimmte Sprachen, bei denen alle anderen Sprachen einer gemeinsamen Ressource zugeordnet werden, muss die für alle Sprachen zu verwendende Ressource mit dem Tag "und" für eine unbestimmte Sprache markiert werden. Windows interpretiert das Sprachtag „und“ ähnlich wie „\*“, d. h., spezifische Entsprechungen haben Vorrang vor der Hauptsprache der Anwendung. Wenn beispielsweise einige Ressourcen (z.B. die Breite eines Elements) für Finnisch verschieden sind, der Rest der Ressourcen aber für alle Sprachen übereinstimmt, sollte die Ressource für Finnisch mit dem Sprachtag "Finnisch" und die übrigen mit "und" markiert werden.
-   Verwenden Sie für Ressourcen, die auf dem Skript für eine Sprache anstatt auf der Sprache basieren, z.B. eine Schriftart oder eine Texthöhe, das Tag für eine unbestimmte Sprache mit einem angegebenen Skript an: 'und-&lt;script&gt;'. Verwenden Sie z.B. für lateinische Schriftarten „und-Latn\\fonts.css“ und für kyrillische Schriftarten „und-Cryl\\fonts.css“.

### <span id="Create_the_application_language_list."></span><span id="create_the_application_language_list."></span><span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."></span>Erstellen Sie die Anwendungssprachenliste.

Das System ermittelt zur Laufzeit die bevorzugten Benutzersprachen, für die die App im Manifest Unterstützung deklariert, und erstellt eine *Anwendungssprachenliste*. Anhand dieser Liste werden die Sprachen bestimmt, in denen die App erscheinen soll. Diese Liste bestimmt die Sprachen, die für die App und Systemressourcen, Datumsangaben, Uhrzeiten, Zahlen und andere Komponenten verwendet werden. Das Ressourcenverwaltungssystem ([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) und [**WinJS.Resources Namespace**](https://msdn.microsoft.com/library/windows/apps/br229779)) lädt beispielsweise UI-Ressourcen gemäß der Anwendungssprache. 
            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) wählt ebenfalls Formate basierend auf der Anwendungssprachenliste aus. Die Anwendungssprachenliste ist über [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396) verfügbar.

Die Zuordnung zwischen Sprachen und Ressourcen ist schwierig. Wir empfehlen, Windows die Zuordnung zu überlassen, da die Priorität einer Zuordnung von vielen optionalen Komponenten eines Sprachtags beeinflusst wird, die in der Praxis tatsächlich auftreten können.

Beispiele für optionale Komponenten in einem Sprachtag:

-   Skript für Sprachen mit Skriptunterdrückung. "en-Latn-US" passt z.B. zu "en-US".
-   Region "en-US" passt z.B. zu %%en".
-   Varianten. "de-DE-1996" passt z.B. zu %%de-DE".
-   "-x" und andere Erweiterungen. "en-US-x-Pirate" passt z.B. zu "en-US".

Es gibt darüber hinaus viele Komponenten von Sprachtags, die nicht die Form "xx" oder "xx-yy" haben, und nicht alle passen zueinander.

-   "zh-Hant" passt nicht zu "zh-Hans".

Windows priorisiert die Zuordnung von Sprachen nach einer exakt definierten Standardmethode. "en-US" passt z.B. in der Reihenfolge der Priorität zu "en-US", "en", "en-GB" usw.

-   Windows führt eine Zuordnung über Regionen durch. „en-US“ passt zu „en-US“, dann zu „en“ und dann zu „en-\*“.
-   Windows verfügt über zusätzliche Daten zur Affinitätszuordnung einer Region, z. B. die primäre Region für eine Sprache. "fr-FR" passt z.B. besser zu "fr-BE" als zu "fr-CA".
-   Sie profitieren kostenlos von allen zukünftigen Verbesserungen der Sprachzuordnung in Windows, wenn Sie sich auf die Windows-APIs verlassen.

Zuerst wird die erste Sprache in einer Liste abgeglichen und dann die zweite Sprache in der Liste (auch bei anderen regionalen Varianten). Eine Ressource für "en-GB" wird z.B. vor einer "fr-CA"-Ressource ausgewählt, wenn "en-US" die Anwendungssprache ist. Nur dann, wenn keine Ressourcen für eine Form von "en" vorhanden sind, wird eine Ressource für "fr-CA" gewählt.

Die Anwendungssprachenliste wird auf die regionale Variante des Benutzers festgelegt, auch wenn diese sich von der regionalen Variante der App unterscheidet. Wenn der Benutzer z.B. "en-GB" spricht, die App jedoch "en-US" unterstützt, schließt die Anwendungssprachenliste "en-GB"ein. Dadurch wird sichergestellt, dass Datumsangaben, Uhrzeiten und Zahlen den Erwartungen des Benutzers entsprechend formatiert werden („en-GB“), die UI-Ressourcen aber wegen des Sprachabgleichs trotzdem in der von der App unterstützten Sprache („en-US“) geladen werden.

Die Anwendungssprachenliste umfasst die folgenden Elemente:

1.  
            **(Optional) Überschreibung der primären Sprache** [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398) ist eine einfache Überschreibungseinstellung für Apps, bei denen Benutzer eine eigene Sprachauswahl treffen können, oder für Apps, bei denen die Standardsprachauswahl aus einem speziellen Grund überschrieben werden sollte. Weitere Informationen dazu finden Sie unter [Anwendungsressourcen und Lokalisierung – Beispiel](http://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Die von der App unterstützten Sprachen des Benutzers.** Hierbei handelt es sich um eine Liste der nach Präferenz geordneten bevorzugten Sprachen des Benutzers. Sie wird anhand der Liste der unterstützten Sprachen im App-Manifest gefiltert. Das Filtern der Benutzersprachen anhand der von der App unterstützten Sprachen sorgt für dauerhafte Konsistenz zwischen den Software Development Kits (SDKs), Klassenbibliotheken, abhängigen Frameworkpaketen und der App.
3.  **Wenn 1 und 2 leer sind, die Standardsprache oder erste von der App unterstützte Sprache.** Wenn der Benutzer keine der von der App unterstützten Anwendungssprachen versteht, wird die erste von der App unterstützte Sprache ausgewählt.

Beispiele finden Sie unten im Abschnitt "Anmerkungen".

### <span id="Set_the_HTTP_Accept_Language_header."></span><span id="set_the_http_accept_language_header."></span><span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."></span>Legen Sie den HTTP-Header „Accept Language“ fest.

HTTP-Anforderungen, die von Windows Store-Apps und Desktop-Apps in typischen Webanforderungen und XHR-Anforderungen (XMLHttpRequest) gesendet werden, verwenden den standardmäßigen HTTP-Header "Accept-Language". Der HTTP-Header ist standardmäßig auf die bevorzugte Sprache des Benutzers festgelegt, entsprechend der bevorzugten Reihenfolge des Benutzers, die unter **Einstellungen**&gt;**Zeit und Sprache**&gt;**Region und Sprache** angegeben ist. Jede Sprache in der Liste wird zudem um die regionsneutralen Varianten der Sprache sowie um eine Gewichtung (q) erweitert. Beispielsweise ergibt eine Benutzersprachenliste, die aus „fr-FR“ und „en-US“ besteht, einen HTTP Accept-Language-Header mit „fr-FR“, „fr“, „en-US“, „en“ („fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3“).

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."></span><span id="use_the_apis_in_the_windows.globalization_namespace."></span><span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."></span>Verwenden Sie die APIs im Windows.Globalization-Namespace.

In der Regel verwenden die API-Elemente im [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)-Namespace die Anwendungssprachenliste zur Bestimmung der Sprache. Hat keine der Sprachen ein übereinstimmendes Format, wird das Benutzergebietsschema verwendet. Hierbei handelt es sich um das von der Systemuhr verwendete Gebietsschema. Das Benutzergebietsschema ist in **Einstellungen**&gt;**Zeit und Sprache**&gt;**Region und Sprache**&gt;**Zusätzliche Datums-, Uhrzeit- und Ländereinstellungen**&gt;**Region: Datums-, Uhrzeit- oder Zahlenformat ändern** verfügbar. Die **Windows.Globalization-APIs** akzeptieren auch eine Überschreibung, wenn Sie eine Liste mit Sprachen angeben möchten, die anstelle der Anwendungssprachenliste verwendet werden soll.


            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) besitzt ebenfalls ein [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)-Objekt, das als Hilfsobjekt bereitgestellt wird. Damit können Apps die Details der Sprache überprüfen, wie das Skript der Sprache, den Anzeigenamen und den systemeigenen Namen.

### <span id="Use_geographic_region_when_appropriate."></span><span id="use_geographic_region_when_appropriate."></span><span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."></span>Verwenden einer geografischen Region, falls erforderlich.

Verwenden Sie zur Auswahl der Inhalte, die dem Benutzer angezeigt werden sollen, nicht die Sprache, sondern die Einstellung für den geografischen Standort des Benutzers. So kann eine Nachrichten-App beispielsweise standardmäßig Inhalte für den Wohnort eines Benutzers anzeigen, der bei der Installation von Windows festgelegt wird und auf der Benutzeroberfläche von Windows unter **Region: Datums-, Uhrzeit- oder Zahlenformat ändern** zu finden ist (wie in der vorherigen Aufgabe beschrieben). Die aktuelle Wohnorteinstellung kann mit [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829) abgerufen werden.


            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) besitzt ebenfalls ein [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795)-Objekt, das als Hilfsobjekt bereitgestellt wird. Damit können Apps Details zu einer bestimmten Region (wie Anzeigename, systemeigener Name und verwendete Währungen) überprüfen.

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Hinweise


Die folgende Tabelle enthält Beispiele für die Elemente, die dem Benutzer unter den verschiedenen Sprach- und Regionseinstellungen auf der App-UI anzeigt werden.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Von der App unterstützte Sprachen (im Manifest definiert)</th>
<th align="left">Spracheinstellungen des Benutzers (in der Systemsteuerung festgelegt)</th>
<th align="left">Überschreibung der primären Sprache (optional)</th>
<th align="left">App-Sprachen</th>
<th align="left">Anzeige in der App</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Englisch (GB) (Standard); Deutsch (Deutschland)</td>
<td align="left">Englisch (GB)</td>
<td align="left">keine</td>
<td align="left">Englisch (GB)</td>
<td align="left">UI: Englisch (GB)<br>Datum/Uhrzeit/Zahlen: Englisch (GB)</td>
</tr>
<tr>
<td align="left">Deutsch (Deutschland) (Standard); Französisch (Frankreich); Italienisch (Italien)</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">keine</td>
<td align="left">Französisch (Österreich)</td>
<td align="left">UI: Französisch (Frankreich) (Fallback aus Französisch (Österreich))<br>Datum/Uhrzeit/Zahlen: Französisch (Österreich)</td>
</tr>
<tr>
<td align="left">Englisch (US) (Standard); Französisch (Frankreich); Englisch (GB)</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">keine</td>
<td align="left">Englisch (Kanada); Französisch (Kanada)</td>
<td align="left">UI: Englisch (USA) (Fallback aus Englisch (Kanada))<br>Datum/Uhrzeit/Zahlen: Englisch (Kanada)</td>
</tr>
<tr>
<td align="left">Spanisch (Spanien) (Standard); Spanisch (Mexiko); Spanisch (Lateinamerika); Portugiesisch (Brasilien)</td>
<td align="left">Englisch (USA)</td>
<td align="left">keine</td>
<td align="left">Spanisch (Spanien)</td>
<td align="left">UI: Spanisch (Spanien) (verwendet „Standard“ da kein Fallback für Englisch verfügbar ist)<br>Datum/Uhrzeit/Zahlen Spanisch (Spanien)</td>
</tr>
<tr>
<td align="left">Katalanisch (Standard); Spanisch (Spanien); Französisch (Frankreich)</td>
<td align="left">Katalanisch; Französisch (Frankreich)</td>
<td align="left">keine</td>
<td align="left">Katalanisch; Französisch (Frankreich)</td>
<td align="left">UI: Vorwiegend Katalanisch, etwas Französisch (Frankreich), da nicht alle Zeichenfolgen in Katalanisch vorhanden sind<br>Datum/Uhrzeit/Zahlen: Katalanisch</td>
</tr>
<tr>
<td align="left">Englisch (GB) (Standard); Französisch (Frankreich); Deutsch (Deutschland)</td>
<td align="left">Deutsch (Deutschland); Englisch (GB)</td>
<td align="left">Englisch (GB) (vom Benutzer in der App-UI ausgewählt)</td>
<td align="left">Englisch (GB); Deutsch (Deutschland)</td>
<td align="left">UI: Englisch (GB) (Sprachüberschreibung)<br>Datum/Uhrzeit/Zahlen Englisch (GB)</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Verwandte Themen


* [BCP-47-Sprachtag](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [IANA-Liste der Sprachen](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Anwendungsressourcen und Lokalisierung – Beispiel](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [Unterstützte Sprachen](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 






<!--HONumber=Jun16_HO4-->


