---
author: TylerMSFT
title: Starten der Windows Store-App
description: In diesem Thema wird das ms-windows-store-URI-Schema beschrieben. Ihre App kann mit diesem URI-Schema die Windows Store-App mit bestimmten Seiten des Store starten.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 9b48aeddb5ddc912fccd07149980655a06535470

---

# Starten der Windows Store-App


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Thema wird das **ms-windows-store:**-URI-Schema beschrieben. Ihre App kann mit diesem URI-Schema die Windows Store-App mit bestimmten Seiten des Store starten.

## ms-windows-store: URI-Schemaverweis

<table>
<tr><th>Beschreibung</th><th></th><th>URI-Schema</th></tr>
<tr><td>Startet die Startseite des Store.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Startet eine der obersten Ebenen im Store.<p>Hinweis: Nicht alle Benutzer haben Zugriff auf allen Ebenen.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Startet die Seite mit Produktdetails für ein Produkt. <p>Store-ID wird für Kunden mit Windows 10 empfohlen und funktioniert für alle Betriebssystemversionen. Die früheren Verfahren hierfür (Beispiel: PFN) werden jedoch weiterhin unterstützt.</p>
<p>Diese Werte finden Sie im Windows Dev Center-Dashboard auf der Seite <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">App-Identität</a> im Abschnitt zur App-Verwaltung für die einzelnen Apps.</p>
</td>
<td>
Store-ID <p>(Empfohlen)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>Paketfamilienname (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Produkt-ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>Produkt-ID (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Startet das Schreiben einer Bewertung für ein Produkt.</td>
<td>Store-ID <p>(Empfohlen)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>Paketfamilienname (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Produkt-ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>Produkt-ID (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die einer Dateierweiterung zugeordnet sind. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die einem Protokoll zugeordnet sind.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten, die mindestens einem Tag zugeordnet sind. Tags müssen durch Kommas getrennt werden.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
Startet eine Suche für die angegebene Abfrage. Leerzeichen in der Abfrage sind zulässig.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten in einer Kategorie.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>Startet eine Suche nach Produkten eines angegebenen Herausgebers. Leerzeichen in der Abfrage sind zulässig.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>Startet die Downloads- und Updateseite.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>Startet die Store-Einstellungsseite.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 



<!--HONumber=Jun16_HO4-->


