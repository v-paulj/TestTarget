---
author: Jwmsft
Description: "Beim Master/Details-Muster werden eine Masterliste und die Details für das derzeit ausgewählte Element angezeigt. Dieses Muster wird häufig für E-Mails und Kontaktlisten/Adressbücher verwendet."
title: Master/Details
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: Master/details
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5845aaf69bbcf561164c519f76f93578bf0da6db

---
# Master/Details-Muster

Das Master/Details-Muster verfügt über einen Masterbereich (in der Regel mit einer [Listenansicht](lists.md)) und einen Detailbereich für Inhalte. Wenn ein Element in der Masterliste ausgewählt wird, wird der Detailbereich aktualisiert. Dieses Muster wird häufig für E-Mails und Adressbücher verwendet.

![Beispiel für das Master/Details-Muster](images/HIGSecOne_MasterDetail.png)

## Ist dies das richtige Muster?

Master/Details-Muster eignet sich gut für Folgendes:

-   Erstellen einer E-Mail-App, eines Adressbuchs oder einer anderen App, die auf einem Listen-Details-Layout basiert
-   Suchen und Priorisieren einer großen Sammlung von Inhalten
-   Schnelles Hinzufügen und Entfernen von Elementen aus einer Liste und gleichzeitiges Wechseln zwischen Kontexten

## Auswählen des richtigen Formats

Beim Implementieren des Master/Details-Musters ist es ratsam, je nach Größe der verfügbaren Bildschirmfläche das gestapelte Format oder das Format mit paralleler Anordnung zu verwenden.

| Verfügbare Fensterbreite | Empfohlenes Format |
|------------------------|-------------------|
| 320 Epx - 719 Epx        | Gestapelt           |
| 720 Epx oder breiter       | Nebeneinander      |

 
## Gestapeltes Format

Im gestapelten Format ist jeweils nur ein Bereich sichtbar: die Master- oder der Detailbereich.

![Master/Details im Stapelmodus](images/patterns-md-stacked.png)

Der Benutzer beginnt im Masterbereich und führt einen Drilldown zum Detailbereich durch, indem er ein Element in der Masterliste auswählt. Für den Benutzer sieht es so aus, als ob sich die Masteransicht und die Detailansicht auf zwei getrennten Seiten befinden.

### Erstellen eines gestapelten Master/Details-Musters

Eine Möglichkeit zur Erstellung des gestapelten Master/Details-Musters ist die Verwendung separater Seiten für den Masterbereich und den Detailbereich. Ordnen Sie die Listenansicht, in der die Masterliste bereitgestellt wird, auf einer Seite und das Inhaltselement für den Detailbereich auf einer separaten Seite an.

![Teile der Master/Details-Ansicht im gestapelten Format](images/patterns-md-stacked-parts.png)

Für den Masterbereich eignet sich ein [Listenansicht](lists.md)-Steuerelement gut für die Darstellung von Listen, die Bilder und Text enthalten können.

Verwenden Sie für den Detailbereich das am besten geeignete Inhaltselement. Wenn viele separate Felder vorhanden sind, erwägen Sie die Verwendung eines Rasterlayouts zum Anordnen der Elemente in einem Formular.

## Format mit paralleler Anordnung

Im Format mit paralleler Anordnung sind Master- und Detailbereich gleichzeitig sichtbar.

![Das Master/Details-Muster](images/patterns-masterdetail-400x227.png)

Für die Liste im Masterbereich wird eine visuelle Auswahlmethode genutzt, um das derzeit ausgewählte Element anzugeben. Wenn in der Masterliste ein neues Element ausgewählt wird, wird der Detailbereich aktualisiert.

### Erstellen eines parallelen Master/Details-Musters

Für den Masterbereich eignet sich ein [Listenansicht](lists.md)-Steuerelement gut für die Darstellung von Listen, die Bilder und Text enthalten können.

Verwenden Sie für den Detailbereich das am besten geeignete Inhaltselement. Wenn viele separate Felder vorhanden sind, erwägen Sie die Verwendung eines Rasterlayouts zum Anordnen der Elemente in einem Formular.

## Beispiele

In diesem Entwurf einer App zur Verfolgung der Börse wird ein Master/Details-Muster verwendet. In diesem Beispiel für die Anzeige der App auf Smartphones befindet sich der Masterbereich/die Masterliste auf der linken Seite und der Detailbereich auf der rechten Seite.

![Beispiel für eine App mit dem Master/Details-Muster, auf einem Smartphone](images/uap-finance-phone-masterdetails-600.png)

In diesem Entwurf einer App zur Verfolgung der Börse wird ein Master/Details-Muster verwendet. In diesem Beispiel für die Anzeige der App auf dem Desktop sind sowohl der Masterbereich/die Masterliste als auch der Detailbereich sichtbar und werden im Vollbildmodus angezeigt. Der Masterbereich bietet ein Suchfeld oben und eine Befehlsleiste im unteren Bereich.

![Beispiel für eine App mit dem Master/Details-Muster, auf dem Desktop](images/uap-finance-desktop700.png)



## Verwandte Artikel

- [Listen](lists.md)
- [Suche](search.md)
- [App- und Befehlsleisten](app-bars.md)
- [**ListView-Klasse (XAML)**](https://msdn.microsoft.com/library/windows/apps/br242878)



<!--HONumber=Jun16_HO4-->


