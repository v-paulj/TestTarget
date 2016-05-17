---
author: drewbatgit
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: In diesem Artikel werden das Lesen und Schreiben von Eigenschaften von Bildmetadaten und das Hinzufügen von Geomarkierungen zu Dateien mithilfe der GeotagHelper-Hilfsklasse erläutert.
title: Bildmetadaten
---

# Bildmetadaten

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


In diesem Artikel werden das Lesen und Schreiben von Eigenschaften von Bildmetadaten und das Hinzufügen von Geomarkierungen zu Dateien mithilfe der [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683)-Hilfsklasse erläutert.

## Bildeigenschaften

Die [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)-Eigenschaft gibt ein [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642)-Objekt zurück, das Zugriff auf inhaltsbezogene Informationen zur Datei bietet. Rufen Sie die bildspezifischen Eigenschaften durch Aufruf von [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646) ab. Das zurückgegebene [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718)-Objekt macht Member verfügbar, die Felder mit grundlegenden Bildmetadaten enthalten, z. B. den Titel des Bilds und das Aufnahmedatum.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Verwenden Sie für den Zugriff auf eine größere Menge von Dateimetadaten das Windows-Eigenschaftensystem, eine Reihe von Eigenschaften von Dateimetadaten, die mit einem eindeutigen Zeichenfolgenbezeichner abgerufen werden können. Erstellen Sie eine Liste mit Zeichenfolgen, und fügen Sie den Bezeichner für jede Eigenschaft hinzu, die Sie abrufen möchten. Die [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732)-Methode verwendet diese Liste mit Zeichenfolgen und gibt ein Wörterbuch von Schlüssel-Wert-Paaren zurück, bei denen der Schlüssel der Eigenschaftsbezeichner und der Wert der Eigenschaftswert ist.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Eine vollständige Liste der Windows-Eigenschaften einschließlich Bezeichnern und Typ jeder Eigenschaft finden Sie unter [Windows-Eigenschaften](https://msdn.microsoft.com/library/windows/desktop/dd561977).

-   Einige Eigenschaften werden nur für bestimmte Dateicontainer und Bildcodecs unterstützt. Eine Liste der für die einzelnen Bildtypen unterstützten Bildmetadaten finden Sie unter [Richtlinien zu Fotometadaten](https://msdn.microsoft.com/library/windows/desktop/ee872003).

-   Da nicht unterstützte Eigenschaften beim Abrufen möglicherweise einen NULL-Wert zurückgeben, prüfen Sie immer auf NULL, bevor Sie einen zurückgegebenen Metadatenwert verwenden.

## GeotagHelper

„GeotagHelper“ ist eine Hilfsklasse, die das problemlose und direkte Markieren von Bildern mit geografischen Daten unter Verwendung der [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603)-APIs ermöglicht, ohne dass das Metadatenformat manuell analysiert oder konstruiert werden muss.

Wenn Sie bereits über ein [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675)-Objekt verfügen, das die Position darstellt, die Sie im Bild markieren möchten (entweder aus einer früheren Verwendung der Geolocation-APIs oder aus einer anderen Quelle), können Sie die Geomarkierungsdaten festlegen, indem Sie [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685) aufrufen und eine [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) und einen **Geopoint** übergeben.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Um die Geomarkierungsdaten mithilfe der aktuellen Position des Geräts festzulegen, erstellen Sie ein neues [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)-Objekt, und rufen Sie [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) auf. Übergeben Sie dabei die **Geolocator**-Klasse und die zu markierende Datei.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Sie müssen die **location**-Gerätefunktion in das App-Manifest einschließen, um die [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686)-API verwenden zu können.

-   Sie müssen [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) vor [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) aufrufen, um sicherzustellen, dass der Benutzer Ihrer App die Berechtigung zur Verwendung seiner Position gewährt hat.

-   Weitere Informationen zu den Geolocation-APIs finden Sie unter [Karten und Position](https://msdn.microsoft.com/library/windows/apps/mt219699).

Um einen GeoPoint abzurufen, der die geomarkierte Position einer Bilddatei darstellt, rufen Sie [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684) auf.

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## Decodieren und Codieren von Bildmetadaten

Die fortschrittlichste Methode zum Arbeiten mit Bilddaten ist das Lesen und Schreiben der Eigenschaften auf Datenstromebene mithilfe von [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) oder [BitmapEncoder](bitmapencoder-options-reference.md). Für diese Vorgänge können Sie Windows-Eigenschaften verwenden, um die gelesenen oder geschriebenen Daten anzugeben. Sie können aber auch die von der Windows-Bilderstellungskomponente (WIC) bereitgestellte Metadatenabfragesprache verwenden, um den Pfad zu einer angeforderten Eigenschaft anzugeben.

Um Bildmetadaten mit diesem Verfahren lesen zu können, benötigen Sie einen [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176), der mit dem Dateidatenstrom des Quellbilds erstellt wurde. Informationen zur Vorgehensweise finden Sie unter [Bildverarbeitung](imaging.md).

Nachdem Sie über den Decoder verfügen, erstellen Sie eine Liste mit Zeichenfolgen, und fügen Sie für jede abzurufende Metadateneigenschaft einen neuen Eintrag hinzu; verwenden Sie dabei entweder die ID-Zeichenfolge der Windows-Eigenschaft oder eine WIC-Metadatenabfrage. Rufen Sie die [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250)-Methode für den [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248)-Member des Decoders auf, um die angegebenen Eigenschaften anzufordern. Die Eigenschaften werden in einem Wörterbuch mit Schlüssel-Wert-Paaren zurückgegeben, die den Eigenschaftsnamen oder den Pfad und den Eigenschaftswert enthalten.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Informationen über die WIC-Metadatenabfragesprache und die unterstützten Eigenschaften finden Sie unter [WIC-Metadatenabfragen für native Bildformate](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   Viele Metadateneigenschaften werden nur von einer Teilmenge aller Bildtypen unterstützt. [
              Bei Ausführung von **GetPropertiesAsync**
            ](https://msdn.microsoft.com/library/windows/apps/br226250) tritt ein Fehler mit Fehlercode 0x88982F41 auf, wenn eine der angeforderten Eigenschaften von dem dem Decoder zugeordneten Bild nicht unterstützt wird, und mit Fehlercode 0x88982F81, wenn das Bild überhaupt keine Metadaten unterstützt. Die diesen Fehlercodes zugeordneten Konstanten sind WINCODEC\_ERR\_PROPERTYNOTSUPPORTED und WINCODEC\_ERR\_UNSUPPORTEDOPERATION und werden in der Headerdatei „winerror.h“ definiert.
-   Da ein Bild nicht unbedingt einen Wert für eine bestimmte Eigenschaft enthält, überprüfen Sie mithilfe von **IDictionary.ContainsKey**, ob eine Eigenschaft in den Ergebnissen vorhanden ist, bevor Sie versuchen, darauf zuzugreifen.

Um Bildmetadaten in den Datenstrom zu schreiben, muss der Bildausgabedatei ein **BitmapEncoder** zugeordnet sein.

Erstellen Sie ein [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338)-Objekt als Container für die festzulegenden Eigenschaftswerte. Erstellen Sie ein [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687)-Objekt für den Eigenschaftswert. Dieses Objekt verwendet **object** als Wert und Member der [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)-Enumeration, die den Typ des Werts definiert. Fügen Sie den **BitmapTypedValue** dem **BitmapPropertySet** hinzu, und rufen Sie dann [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) auf, damit der Encoder die Eigenschaften in den Datenstrom schreibt.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Ausführliche Informationen dazu, welche Eigenschaften für welche Bilddateitypen unterstützt werden, finden Sie unter [Windows-Eigenschaften](https://msdn.microsoft.com/library/windows/desktop/dd561977), [Richtlinien zu Fotometadaten](https://msdn.microsoft.com/library/windows/desktop/ee872003) und [WIC-Metadatenabfragen für systemeigene Bildformate](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   [
              Für **SetPropertiesAsync**
            ](https://msdn.microsoft.com/library/windows/apps/br226252) tritt ein Fehler mit dem Fehlercode 0x88982F41 auf, wenn das dem Encoder zugeordnete Bild eine der angeforderten Eigenschaften nicht unterstützt.

## Verwandte Themen

* [Bildverarbeitung](imaging.md)
 

 






<!--HONumber=May16_HO2-->


