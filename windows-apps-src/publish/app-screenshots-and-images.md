---
author: jnHs
Description: Ihre App muss verschiedene Logos, Screenshots und Bilder enthalten.
title: App-Screenshots und -Bilder
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.sourcegitcommit: ecb030b7c529f765eded46e4e3e9db99ad0c27e8
ms.openlocfilehash: 9eac5658e2ac04b2abc1bf06abf5c73b16260bc7

---

# App-Screenshots und -Bilder


Ihre App muss verschiedene Logos, Screenshots und Bilder enthalten. Einige dieser Elemente sind erforderlich, während andere optional sind. Beachten Sie, dass die Bilder eine der wichtigsten Möglichkeiten zur Darstellung Ihrer App sind. Durch aussagekräftige Bilder kann die Aufmerksamkeit der Kunden auf Ihre App gelenkt werden.

Während der [App-Einreichung](app-submissions.md) geben Sie Ihre [Screenshots](#screenshots) und [Werbebilder](#promotional-artwork) im Schritt [Beschreibungen](create-app-descriptions.md) an. Diese Bilder werden verwendet, um Ihre App im Store zu präsentieren.

Der Store verwendet außerdem Ihre App-Kachel und andere Bilder, die Sie in Ihr App-Paket einschließen. Führen Sie das [Zertifizierungskit für Windows-Apps](https://msdn.microsoft.com/library/windows/apps/mt186449) aus, um zu ermitteln, ob erforderliche Bilder fehlen. Anleitungen und Empfehlungen zu diesen Bildern finden Sie unter [Ressourcen für Kacheln und Symbole](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Hinweis**  Es hängt vom Betriebssystem des Kunden und weiteren Faktoren ab, wie Bilder im Store, auf dem Startbildschirm des Kunden und in der App selbst angezeigt werden.


## Während der Einreichung bereitgestellte Bilder

Bei der Eingabe der App-Beschreibungsinfos haben Sie die Möglichkeit, mehrere Screenshots (mindestens einer ist erforderlich) und Werbebilder bereitzustellen. Diese Bilder werden nicht aus Ihrem App-Paket übernommen, sondern müssen im Schritt **Beschreibung** für jede Sprache bereitgestellt werden.

In der folgenden Tabelle sind die verschiedenen Bilder aufgeführt, die Sie hochladen können, und ihre Verwendung wird erläutert. Weitere Details finden Sie in den folgenden Abschnitten.

| Bild                                                       | Größe in Pixel                           | Verwendung                                                                                                                                                                           | Erforderlichkeit                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Desktop-Screenshots](#screenshots)                         | 1366 x 768 oder größer                 | Wird im Store-Eintrag Ihrer App auf einem Desktopgerät angezeigt.                                                                                                          | Empfohlen für alle Apps, insbesondere wenn Ihre App für die Verwendung auf Desktopgeräten vorgesehen ist. Es ist mindestens ein Screenshot (für jede Gerätefamilie) erforderlich. |
| [Mobilgerät-Screenshots](#screenshots)                          | 768 x 1280, 720 x 1280 oder 480 x 800 | Wird im Store-Eintrag Ihrer App auf einem Mobilgerät angezeigt.                                                                                                           | Empfohlen für alle Apps, insbesondere wenn Ihre App für die Verwendung auf Mobilgeräten vorgesehen ist. Es ist mindestens ein Screenshot (für jede Gerätefamilie) erforderlich. |
| [Hologramm-Screenshots](#screenshots)                          | 1268 x 720 oder größer                 | Wird im Store-Eintrag Ihrer App auf einem Gerät für Hologramme angezeigt.                                                                                                           | Wird empfohlen, wenn Ihre App für eine Microsoft HoloLens-Verwendung vorgesehen ist. Es ist mindestens ein Screenshot (für jede Gerätefamilie) erforderlich. |
| [Symbol für App-Kachel](#app-tile-icon)                             | 300 x 300                            | Wird als Symbol Ihrer App im Store für Windows Phone 8.1 und ältere Versionen angezeigt (wenn Sie nur Pakete für Windows Phone 8.1 oder frühere Versionen haben, auch im Store unter Windows 10). | Erforderlich für die ordnungsgemäße Anzeige im Store, wenn Ihre App für Windows Phone 8.1 oder frühere Versionen vorgesehen ist.                                                                 |
| [Werbebild: Windows 10-Spotlight](#promotional-artwork) | 2400 x 1200                          | Für Werbelayouts im Store unter Windows 10 verwendet.                                                                                                                        | Empfohlen für alle Apps, insbesondere Apps mit UWP-Paketen, die für Windows 10-Kunden vorgesehen sind.                                                               |
| [Werbebilder: Windows Phone 8.1 und frühere Versionen](#promotional-artwork) | 1000 x 800, 358 x 358                | Für Werbelayouts im Store unter Windows Phone 8.1 und früheren Versionen verwendet.                                                                                                     | Empfohlen für alle Apps, die für Windows Phone 8.1 oder früher vorgesehen sind.                                                                                           |
| [Werbebild: Windows 8.1 und frühere Versionen](#promotional-artwork)        | 414 x 180                            | Für Werbelayouts im Store unter Windows 8.1 verwendet.                                                                                                                       | Empfohlen für alle Apps, die für Windows 8.1 oder früher vorgesehen sind.                                                                                                 |
 

## Screenshots

Screenshots sind die Bilder Ihrer App, die Ihren Kunden im Store-Eintrag der App angezeigt werden.

Sie sehen mehrere Felder auf der Seite **Beschreibung**. Dort können Sie Screenshots für verschiedene Gerätefamilien bereitstellen. Die Anzeige erfolgt, wenn ein Kunde den Store-Eintrag Ihrer App auf dem entsprechenden Gerätetyp aufruft.

Für die Übermittlung ist nur ein Screenshot erforderlich. Sie können jedoch bis zu neun Desktop-Screenshots und bis zu acht Screenshots für mobile Geräte und Hologramm-Geräte bereitstellen. Es ist nicht erforderlich, separate Screenshots für jede Gerätefamilie bereitzustellen. Wir empfehlen jedoch die Bereitstellung von Screenshots für jeden Gerätetyp, den Ihre App unterstützt, damit die Kunden sehen, wie die App auf ihrem Gerät aussieht.

> **Hinweis**  Microsoft Visual Studio enthält ein [Tool zum Erstellen von Screenshots](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Jeder Screenshot muss einer PNG-Datei im Quer- oder Hochformat entsprechen, und die Datei darf nicht größer als 2 MB sein.

Die Größenanforderungen hängen von der Gerätefamilie ab:
- Mobile Geräte: 768 x 1280, 720 x 1280 oder 480 x 800 Pixel
- Desktopgerät: 1366 x 768 Pixel oder größer
- Geräte für Hologramme: 1268 x 720 Pixel oder größer

Sie können eine Kurzbeschreibung von maximal 200 Zeichen für die einzelnen Screenshots angeben.

> **Hinweis**  Wenn Sie Beschreibungen für [mehrere Sprachen](supported-languages.md) erstellen, erhalten Sie für jede Sprache eine Seite vom Typ **Beschreibung**. Sie müssen Bilder für jede Sprache separat hochladen (auch wenn Sie dieselben Bilder verwenden), einschließlich Beschriftungen, die für die einzelnen Sprachen verwendet werden.


## Symbol für App-Kachel

Dies ist nicht für alle Übermittlungen erforderlich, wird jedoch dringend empfohlen, wenn Ihre App unter Windows Phone 8.1 oder früheren Versionen ausgeführt wird. Das Symbol für die App-Kachel wird verwendet, wenn Ihr App-Eintrag für Kunden unter Windows Phone 8.1 und früheren Versionen angezeigt wird. Wenn Sie dieses Bild nicht bereitstellen, sehen Kunden unter Windows Phone 8.1 und früheren Versionen im Eintrag für Ihre App ein leeres Symbol. (Dies gilt auch für Kunden unter Windows 10, wenn Ihre App nur über Pakete für Windows Phone 8.1 oder frühere Versionen verfügt.)

Das Symbol für die App-Kachel muss eine PNG-Datei mit 300 x 300 Pixel sein.

## Werbebilder

Die Windows Store-Redaktion verwendet verschiedene Bilder, um Apps im Store zu bewerben. Wenn Sie Werbebilder übermitteln, zieht das Windows Store-Team Ihre App für Werbelayouts in Betracht.

> **Wichtig**  Das Bereitstellen von Werbebildern für Ihre App garantiert nicht, dass die App präsentiert wird. Ohne Bilder kann die App jedoch nicht für Werbemöglichkeiten in Betracht gezogen werden. (Weitere Informationen finden Sie unter [Einfaches Bewerben Ihrer App](make-your-app-easier-to-promote.md).)

Es empfiehlt sich, Werbebilder in verschiedenen Größen bereitzustellen, je nachdem auf welches Betriebssystemversion(en) Ihre App ausgerichtet ist. Bilder müssen für alle Größen im PNG-Format vorliegen.

Im Folgenden finden Sie einige Tipps, die Sie beim Entwerfen Ihrer Werbebilder beachten sollten:

-   Wählen Sie dynamische Bilder, die mit der App zusammenhängen und den Wiedererkennungswert sowie die Differenzierung zu fördern. Vermeiden Sie Stockfotografien oder generische visuelle Elemente.
-   Fügen Sie keinen Text hinzu (abgesehen von Ihrem Branding).
-   Halten Sie leeren Platz im Bild möglichst gering.
-   Vermeiden Sie das Anzeigen der Benutzeroberfläche Ihrer App, und verwenden Sie keine Bilder für bestimmte Geräte.
-   Vermeiden Sie politische und nationale Designs, Flaggen oder religiöse Symbole.
-   Fügen Sie keine Bilder von taktlosen Gesten, Nacktheit, Glücksspiel, Währung, Drogen, Tabakprodukte oder Alkohol hinzu.
-   Verwenden Sie keine Bilder mit Waffen, die auf den Benutzer gerichtet sind, oder übermäßiger Gewalt und Blut.

### Für Windows 10: 2400 x 1200

Im Store unter Windows 10 befindet sich oben auf den Kategorieseiten von „Apps und Spiele“ ein sich drehendes Spotlight-Bild, um Inhalte zu bewerben. Achten Sie darauf, ein Bild der Größe 2400 x 1200 zu übermitteln, damit Ihre App für diese Spotlight-Platzierung ausgewählt werden kann.

Bedenken Sie beim Entwerfen Ihres Bilds, dass im Fall einer Verwendung für Spotlight im unteren Bilddrittel ein Farbverlauf angewendet wird, damit wir Marketingtext lesbar über dem Bild anzeigen können. Stellen Sie daher sicher, dass Sie im unteren Drittel keinen Text oder wichtige visuelle Elemente verwenden. Außerdem schneiden wir Ihr Bild u. U. zu; platzieren Sie das Branding und die wichtigsten Informationen zu Ihrer App daher in der Bildmitte.

Die folgende Abbildung zeigt, welche wichtigen Proportionen zu berücksichtigen sind. Die „sichere Zone“ in der Mitte ist auch dann hervorgehoben, wenn wir das Bild zuschneiden. Im „dynamischen Textbereich“ können Text und ein Farbverlauf angezeigt werden.

![Richtlinien für Spotlight-Bilder](images/spotlight1.jpg) Das folgende Beispiel zeigt ein gut entworfenes Spotlight-Bild, bei dem diese Richtlinien beachtet wurden. (Die Linien auf dem Bild sollen veranschaulichen, wie die Grafik in die dafür vorgesehenen Bereichen passt; auf dem endgültigen Bild sind sie nicht zu sehen.)

![Gut gestaltetes Spotlight-Bild](images/spotlight2.jpg)
### Für Windows Phone 8.1 und frühere Versionen: 1000 x 800, 358 x 358

Im Store unter Windows Phone 8.1 und früheren Versionen können zwei Bildgrößen in Werbelayouts verwendet werden: 1000 x 800 Pixel und 358 x 358 Pixel. Wenn Ihre App unter Windows Phone 8.1 oder einer früheren Version ausgeführt wird, wird die Bereitstellung von Bildern in diesen beiden Größen empfohlen, damit sie für Werbung in Frage kommen.

> **Tipp**  Achten Sie auch darauf, ein [App-Kachel-Symbolbild](#app-tile-icon) mit 300 x 300 Pixel bereitzustellen, damit Ihre App im Store nicht mit einem leeren Symbol angezeigt wird.

### Für Windows 8.1 und frühere Versionen: 414 x 180

Im Store unter Windows 8.1 und früheren Versionen wird für Werbelayouts möglicherweise ein Bild in der Pixelgröße 414 x 180 verwendet. Wenn Ihre App unter Windows 8.1 oder früheren Versionen ausgeführt wird, wird das Bereitstellen eines Bilds in dieser Größe empfohlen, damit sie für Werbung in Frage kommt.



<!--HONumber=Jun16_HO4-->


