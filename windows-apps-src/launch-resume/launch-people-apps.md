---
author: TylerMSFT
title: Starten der Kontakte-App
description: "In diesem Thema wird das ms-people-URI-Schema beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten."
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: fd3c38dd0b6df2f430d7be4c40e7131d4ae98616

---

# Starten der Kontakte-App


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Thema wird das **ms-people:**-URI-Schema beschrieben. Ihre App kann dieses URI-Schema verwenden, um die Kontakte-App für bestimmte Aktionen zu starten.

## ms-people: URI-Schemaverweis


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ergebnisse</th>
<th align="left">URI-Schema</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Damit können andere Apps die Hauptseite der Kontakte-App starten.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Damit können andere Apps die Einstellungsseite der Kontakte-App starten.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Damit können andere Apps einen Suchbegriff bereitstellen, der in der Kontakte-App mit der Ergebnisseite der Suche gestartet wird.
<div class="alert">
**Hinweis**  
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Wenn Sie die Syntax nicht ordnungsgemäß eingeben oder den Wert der Suchzeichenfolge vergessen, wird standardmäßig eine vollständige Liste der Kontakte ohne Filterung zurückgegeben.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Startet mit einer vorhandenen Kontaktkarte, wenn der Kontakt gefunden wurde. Oder startet mit einer temporären Kontaktkarte, wenn kein Kontakt gefunden wird. Wenn kein Eingabeparameter angegeben wird, wird die Kontakte-App mit einer Kontaktliste gestartet.
<div class="alert">
**Hinweis**  
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
<p>Wenn mehr als eine Übereinstimmung vorliegt, wird die erste Übereinstimmung des Kontakts zurückgegeben.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Startet mit einer „Kontakt speichern“-Seite innerhalb der Kontakte-App, um den angegeben Kontakt mit der angegebenen Telefonnummer oder E-Mail-Adresse zu speichern.
<div class="alert">
**Hinweis**  
<p>Bei den Parametern wird die Groß-/Kleinschreibung beachtet.</p>
<p>Die Reihenfolge der Parameter spielt keine Rolle.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## ms-people:search: Parameterverweis


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Suchzeichenfolge**</td>
<td align="left"><p>Optional.</p>
<p>Die Suchzeichenfolge für die Informationen zur Kontaktsuche.</p>
<p>Die Telefonnummer oder den Namen des Kontakts.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## ms-people:viewcontact: Parameterverweis


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**ContactId**</td>
<td align="left"><p>Optional.</p>
<p>Die Kontakt-ID des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**Email**</td>
<td align="left"><p>Optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>Optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>Optional.</p>
<p>Das Kontaktobjekt.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## ms-people:savetocontact: Parameterverweis


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameter</th>
<th align="left">Beschreibung</th>
<th align="left">Beispiel</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Optional.</p>
<p>Die Telefonnummer des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**Email**</td>
<td align="left"><p>Optional.</p>
<p>Die E-Mail-Adresse des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>Optional.</p>
<p>Der Name des Kontakts.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 



<!--HONumber=Jun16_HO5-->


