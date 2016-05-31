---
description: In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird.
title: Freigeben von Daten
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
author: awkoren
---

# Freigeben von Daten

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern.

## Einrichten eines Ereignishandlers

Fügen Sie einen [**DataRequested**][DataRequested]-Ereignishandler hinzu, der aufgerufen werden soll, wenn der Benutzer die Teilen-Funktion verwendet. Dies kann durch Tippen auf ein Steuerelement in Ihrer App (etwa eine Schaltfläche oder ein App-Leistenbefehl) oder automatisch in einem bestimmten Szenario geschehen (etwa wenn der Benutzer einen Level mit einer hohen Punktzahl abschließt).

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

Wenn ein [**DataRequested**][DataRequested]-Ereignis eintritt, empfängt Ihre App ein [**DataRequest**][DataRequest]-Objekt. Dieses enthält ein [**DataPackage**][DataPackage], mit dem Sie den Inhalt bereitstellen können, den der Benutzer freigeben möchte. Sie müssen einen Titel und die freizugebenden Daten angeben. Eine Beschreibung ist optional, wird aber empfohlen.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## Wählen von Daten

Sie können verschiedene Arten von Daten freigeben, einschließlich:

-   Nur-Text
-   URIs (Uniform Resource Identifiers)
-   HTML
-   Formatierter Text
-   Bitmaps
-   Nur-Text
-   Dateien
-   Vom Entwickler definierte Daten

Das [**DataPackage**][DataPackage]-Objekt kann mehrere dieser Formate in beliebiger Kombination enthalten. Im folgenden Beispiel wird das Freigeben von Text veranschaulicht.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## Festlegen von Eigenschaften

Beim Verpacken von Daten für die Freigabe können Sie eine Vielzahl von Eigenschaften angeben, die weitere Informationen zum freigegebenen Inhalt enthalten. Mit diesen Eigenschaften kann die Benutzererfahrung für Ziel-Apps verbessert werden. Die Angabe einer Beschreibung kann beispielsweise nützlich sein, wenn der Benutzer Inhalte für mehrere Apps freigibt. Eine Miniaturansicht beim Freigeben eines Bilds oder eines Links für eine Webseite stellt eine visuelle Referenz für den Benutzer dar. Weitere Informationen finden Sie unter [**DataPackage.DataPackagePropertySet**][DataPackagePropertySet].

Alle Eigenschaften mit Ausnahme des Titels sind optional. Die title-Eigenschaft ist erforderlich und muss festgelegt werden.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## Starten der Benutzeroberfläche für das Freigeben

Eine Benutzeroberfläche für das Freigeben wird vom System bereitgestellt. Zum Starten rufen Sie die [**ShowShareUI**][ShowShareUi]-Methode auf.

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## Behandeln von Fehlern

In den meisten Fällen ist das Freigeben von Inhalten ein einfacher Prozess. Es besteht jedoch immer die Möglichkeit, dass unerwartet ein Problem auftritt. Beispielsweise könnte die App davon ausgehen, dass Inhalt ausgewählt wurde, auch wenn der Benutzer nichts ausgewählt hat. Um diese Situationen zu behandeln, verwenden Sie die Methode [**FailWithDisplayText**][FailWithDisplayText], die dem Benutzer eine Meldung anzeigt, wenn ein Fehler auftritt.

## Verzögern der Freigabe mit Delegaten

Manchmal ist es nicht sinnvoll, die Daten, die der Benutzer freigeben möchte, direkt vorzubereiten. Wenn Ihre App zum Beispiel das Senden von großen Bilddateien in verschiedenen Formaten unterstützt, ist es ineffizient, alle diese Bilder zu erstellen, bevor der Benutzer seine Auswahl trifft.

Zur Lösung dieses Problems kann ein [**DataPackage**][DataPackage] einen Delegaten enthalten. Dabei handelt es sich um eine Funktion, die aufgerufen wird, wenn die empfangende App Daten anfordert. Es wird empfohlen, einen Delegaten immer dann zu verwenden, wenn die von einem Benutzer freigegebenen Daten ressourcenintensiv sind.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## Verwandte Themen
* [Empfangen von Daten](receive-data.md)


<!-- LINKS -->
* [DataPackage]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx 
* [DataPackagePropertySet]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx 
* [DataRequest]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx 
* [DataRequested]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx 
* [FailWithDisplayText]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx
* [ShowShareUi]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx
 



<!--HONumber=May16_HO2-->


