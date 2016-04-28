---
description: Wenn Sie einen Katalog mit Apps und IAPs haben, können Sie mit der Windows Store-Sammlungs-API und der Windows Store-Einkaufs-API Besitzerinformationen abrufen.
title: Umsatzsteuerinfo
ms.assetid: 93834A6B-86D6-4647-AC01-CBCA3CB7A578
---

# Umsatzsteuerinfo


Wenn Sie während des Dev Center-Registrierungsprozesses eine Umsatzsteuer-Identifikationsnummer angeben müssen, finden Sie im Folgenden grundlegende Informationen.

## Grundlagen der Steuernummern


Eine Umsatzsteuernummer ist eine ID, die für Länder oder Regionen in der EU verwendet wird Weitere Informationen erhalten Sie auf der offiziellen [VIES-Website](http://go.microsoft.com/fwlink/p/?LinkId=258372) der EU.

## Gültige Formate für Umsatzsteuernummern


Beachten Sie, dass Microsoft keine Steuertipps gibt und die folgende Tabelle nur als Leitfaden bereitgestellt wird. Informationen zu aktuellen Änderungen erhalten Sie bei Ihrem örtlichen Finanzamt, falls dieser Leitfaden nicht ausreicht, um Microsoft eine Umsatzsteuer-Identifikationsnummer zur Verfügung zu stellen.

<table Responsive="true">
<tr><th>Land/Region</th><th>Umsatzsteuerinfo</th></tr>
<tr><td data-th="Country/region">Österreich
</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 1 Buchstabe und 8 Ziffern</li>
<li>Code für Land/Region: AT</li>
<li>Beispiel: U12345678</li>
<li>Hinweis: Erstes Zeichen lautet immer „U“.
</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Belgien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 10 Ziffern</li>
<li>Code für Land/Region: BE</li>
<li>Beispiel: 1234567890</li>
<li>Hinweis: 9 Ziffern vor dem 1. Januar 2007.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Bulgarien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 oder 10 Ziffern</li>
<li>Code für Land/Region: BG</li>
<li>Beispiel: 123456789 oder 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Kroatien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 2 Buchstaben und 11 Ziffern</li>
<li>Code für Land/Region: HR</li>
<li>Beispiel: HR12345678901</li>
<li>Hinweis: Die ersten Zeichen lauten immer „HR“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Zypern</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 2 Buchstaben, 8 Ziffern und 1 Buchstabe</li>
<li>Code für Land/Region: CY</li>
<li>Beispiel: 12345678, 123456789 oder 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Tschechische Republik</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 8, 9 oder 10 Ziffern</li>
<li>Code für Land/Region: CZ</li>
<li>Beispiel: 12345678, 123456789 oder 0123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Dänemark</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 8 Ziffern</li>
<li>Code für Land/Region: DK</li>
<li>Beispiel: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Estland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 Ziffern</li>
<li>Code für Land/Region: EE</li>
<li>Beispiel: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Finnland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: Ziffern</li>
<li>Code für Land/Region: FI</li>
<li>Beispiel: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Frankreich</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 11 Ziffern</li>
<li>Code für Land/Region: FR</li>
<li>Beispiel: 12345678901, X1234567890, 1X123456789 oder XX123456789</li>
<li>Hinweis: kann beliebige Buchstaben mit Ausnahme von I und Q als erstes oder zweites Zeichen oder als erstes und zweites Zeichen gefolgt von 9 Ziffern enthalten.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Deutschland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 Ziffern</li>
<li>Code für Land/Region: DE</li>
<li>Beispiel: 123456789</li>
<li>Hinweis: muss die neunstellige „Umsatzsteuer-Identifikationsnummer“ (Ust-ID-Nr.) sein</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Griechenland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 Ziffern</li>
<li>Code für Land/Region: EL, GR</li>
<li>Beispiel: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Ungarn</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 8 Ziffern</li>
<li>Code für Land/Region: HU</li>
<li>Beispiel: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Irland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 8 Ziffern</li>
<li>Code für Land/Region: IE</li>
<li>Beispiel: 1234567X oder 1X34567X</li>
<li>Hinweis: enthält 1 oder 2 Buchstaben: an der letzten Stelle oder an der zweiten und der letzten Stelle.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Italien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 11 Ziffern</li>
<li>Code für Land/Region: IT</li>
<li>Beispiel: 12345678901</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Lettland</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 11 Ziffern</li>
<li>Code für Land/Region: LV</li>
<li>Beispiel: 01234567890</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Litauen</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 oder 12 Ziffern</li>
<li>Code für Land/Region: LT</li>
<li>Beispiel: 123456789 oder 012345678901</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Luxemburg</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 8 Ziffern</li>
<li>Code für Land/Region: LU</li>
<li>Beispiel: 12345678</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Malta</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 2 Buchstaben und 8 Ziffern</li>
<li>Code für Land/Region: MT</li>
<li>Beispiel: MT12345678</li>
<li>Hinweis: Die ersten Zeichen lauten immer „MT“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Niederlande</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 11 Ziffern und 1 Buchstabe</li>
<li>Code für Land/Region: NL</li>
<li>Beispiel: 123456789B01</li>
<li>Hinweis: zehntes Zeichen ist immer „B“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Polen</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 10 Ziffern</li>
<li>Code für Land/Region: PL</li>
<li>Beispiel: 1234567890</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Portugal</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 Ziffern</li>
<li>Code für Land/Region: PT</li>
<li>Beispiel: 123456789</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Rumänien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 2 Buchstaben und 8-10 Ziffern</li>
<li>Code für Land/Region: RO</li>
<li>Beispiel: RO12345678, RO123456789 oder RO1234567890</li>
<li>Hinweis: Die ersten Zeichen lauten immer „RO“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Slowakei</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 10 Ziffern</li>
<li>Code für Land/Region: SK</li>
<li>Beispiel: 1234567890</li>
<li>Hinweis: Die ersten Zeichen lauten immer „SI“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Slowenien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 2 Buchstaben und 8 Ziffern</li>
<li>Code für Land/Region: SI</li>
<li>Beispiel: SI12345678</li>
<li>Hinweis: Die ersten Zeichen lauten immer „SI“.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Spanien</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 Ziffern</li>
<li>Code für Land/Region: ES</li>
<li>Beispiel: X12345678, 12345678X oder X1234567X</li>
<li>Hinweis: enthält 1 oder 2 Buchstaben: an der ersten, an der letzten oder an der ersten und der letzten Stelle.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Schweden</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 12 Ziffern</li>
<li>Code für Land/Region: SE</li>
<li>Beispiel: 123456789001</li>
<li>Hinweis: Die letzten beiden Zeichen müssen „01“ sein.</li>
</ul>
</td></tr>
<tr><td data-th="Country/region">Vereinigtes Königreich</td><td data-th="VAT info">
<ul>
<li>Umsatzsteuernummernformat: 9 oder 12 Ziffern</li>
<li>Code für Land/Region: GB</li>
<li>Beispiel: 123456789 oder 123456789001</li>
<li>Hinweis: normalerweise 9 Ziffern, aber 12 Ziffern, wenn die Nummer ein untergeordnetes Unternehmen innerhalb einer Gruppe darstellt.</li>
</ul>
</td></tr>
</table>
                                                                                                                                                                  

 

 






<!--HONumber=Mar16_HO1-->


