---
author: jnHs
Description: Sie können Kunden helfen, Ihre App zu entdecken, indem Sie einen Link zum Store-Eintrag Ihrer App einfügen.
title: Erstellen eines Links zu Ihrer App
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
---

# Erstellen eines Links zu Ihrer App


Sie können Kunden helfen, Ihre App zu entdecken, indem Sie einen Link zum Store-Eintrag Ihrer App einfügen.

## Abrufen des Links zum Store-Eintrag Ihrer App


Den Link zum Store-Eintrag Ihrer App finden Sie auf der Seite [App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung** von jeder App im Dashboard.

Dieser Link hat das Format **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Wenn ein Kunde auf diesen Link klickt, wird die webbasierte Eintragsseite für Ihre App geöffnet. Wenn Ihre App für das Gerät des Kunden verfügbar ist, werden die Store-App gestartet und der App-Eintrag angezeigt.

> **Hinweis**  Je nachdem, für welche Betriebssystemversionen Sie Apps anbieten, kann hier mehr als ein Link angezeigt werden. Alle Apps zeigen die URL für Windows 10 an, die für jedes Betriebssystem funktionieren. Es werden möglicherweise zusätzliche Links für Windows 8.1 und frühere Versionen bzw. Windows Phone 8.1 und frühere Versionen angezeigt, die nur auf den angegebenen Betriebssystemversionen funktionieren.

 

## Setzen eines Links zum Store-Eintrag Ihrer App mit dem Windows Store-Badge


Sie können mit einem benutzerdefinierten Badge einen direkten Link zum Eintrag Ihrer App erstellen, um Kunden darüber zu informieren, dass Ihre App im Windows Store vorhanden ist.

Besuchen Sie zum Erstellen Ihres Badges die Seite [Windows Store-Badges](http://go.microsoft.com/fwlink/p/?LinkID=534236). Sie benötigen die Store-ID Ihrer App, um dieses Formular zum Generieren von Badge und Link verwenden zu können. Bei dieser ID handelt es sich um die letzten 12 Zeichen der **URL für Windows 10**, die auf der Seite [App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung** angezeigt wird.

> **Hinweis**  Weitere Informationen über das Verwenden des Windows Store-Badge finden Sie unter [Richtlinien für die App-Vermarktung](app-marketing-guidelines.md).

 

## Erstellen direkter Links zum Windows Store


Sie können unter Verwendung des **ms-windows-store:**-URI-Schemas einen Link erstellen, der den Windows Store öffnet und direkt zur Eintragsseite Ihrer App wechselt.

Diese Links sind hilfreich, wenn Sie wissen, dass die Benutzer ein Windows-Gerät verwenden, und Sie möchten, dass sie direkt zur Eintragsseite im Store gelangen. Sie sollten dieses beispielsweise verwenden, nachdem Sie das Betriebssystem des Benutzers anhand der Zeichenfolge des Benutzer-Agenten in einem Browser überprüft haben oder wenn Sie bereits über eine UWP-App kommunizieren.

Um das Windows Store-Protokoll für einen direkten Link mit dem Store-Eintrag Ihrer App zu verwenden, fügen Sie diesem Link die Store-ID der App hinzu:

`ms-windows-store://pdp/?ProductId=`

Weitere Informationen zur Verwendung des Windows Store-Protokolls finden Sie unter [Starten der Windows Store-App](../launch-resume/launch-store-app.md).

 

 






<!--HONumber=May16_HO2-->


