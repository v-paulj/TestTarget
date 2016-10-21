---
author: TylerMSFT
title: "Schema „ms-tonepicker“"
description: "In diesem Thema wird URI-Schema „ms-tonepicker“ beschrieben und wie Sie dieses verwenden können, um eine Tonauswahl anzuzeigen und Töne auszuwählen, zu speichern und den Anzeigenamen für Töne abzurufen."
translationtype: Human Translation
ms.sourcegitcommit: 4c7037cc91603af97a64285fd6610445de0523d6
ms.openlocfilehash: ef605f9d749148240ecee5e0ecfd473f8440ca25

---

# Auswählen und Speichern von Tönen mithilfe des URI-Schemas „ms-tonepicker“

In diesem Thema wird die Verwendung des URI-Schemas **ms-tonepicker:** beschrieben. Dieses URI-Schema kann verwendet werden, um:
- Zu ermitteln, ob die Tonauswahl auf dem Gerät verfügbar ist;
- Die Tonauswahl anzuzeigen, um die verfügbaren Klingeltöne, Systemtöne, SMS-Klingeltöne und Alarmtöne aufzulisten, und ein Tontoken zu erhalten, das den vom Benutzer ausgewählten Ton darstellt;
- Das Tool zum von Tönen anzuzeigen, das ein Sounddateitoken als Eingabe erhält und es auf dem Gerät speichert. Gespeicherte Töne stehen anschließend über die Tonauswahl zur Verfügung. Benutzer können dem Ton einen Anzeigenamen zuweisen.
- Konvertieren Sie ein Tontoken in dessen Anzeigenamen.

## ms-tonepicker: URI-Schemareferenz

Dieses URI-Schema übergibt keine Argumente über die URI-Schema-Zeichenfolge, sondern über einen [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx). Alle Zeichenfolgen beachten die Groß- und Kleinschreibung.

In den folgenden Abschnitten wird angegeben, welche Argumente übergeben werden müssen, um die angegebene Aufgabe auszuführen.

## Aufgabe: Ermitteln, ob die Tonauswahl auf dem Gerät verfügbar ist
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## Aufgabe: Anzeige der Tonauswahl

Die Argumente, die Sie zum Anzeigen der Tonauswahl übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Action | string | Ja | "PickRingtone" | Öffnet die Tonauswahl. |
| CurrentToneFilePath | string | Nein | Ein vorhandenes Tontoken. | Der Ton, der in der Tonauswahl als aktueller Ton angezeigt werden soll. Wenn dieser Wert nicht festgelegt ist, ist der erste Ton in der Liste standardmäßig aktiviert.<br>Streng genommen, ist dies kein Dateipfad. Sie können einen geeigneten Wert für `CurrenttoneFilePath` aus dem Wert `ToneToken` erhalten, der von der Tonauswahl zurückgegeben wird.  |
| TypeFilter | string | Nein | "Ringtones", "Notifications", "Alarms", "None" | Wählt die Töne aus, die der Auswahl hinzugefügt werden sollen. Wenn kein Filter angegeben ist, werden alle Töne angezeigt. |

<br>Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewerte | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Erfolg. <br> 1 – Abgebrochen. <br> 7 – Ungültige Parameter. <br> 8 – Es gibt keine Töne, die den Filterkriterien entsprechen. <br> 255 – Die angegebene Aktion ist nicht implementiert. | Das Ergebnis des Auswahlvorgangs. |
| ToneToken | string | Das Token des ausgewählten Tons. <br> Die Zeichenfolge ist leer, wenn der Benutzer in der Auswahl **Standard** auswählt. | Dieses Token kann in einer Popupbenachrichtigungs-Nutzlast verwendet werden oder einem Kontakt als Klingelton oder SMS-Klingelton zugewiesen werden. Der Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |
| DisplayName | string | Der Anzeigename des angegebenen Tons. | Eine Zeichenfolge, die dem Benutzer angezeigt werden kann, um den ausgewählten Ton darzustellen. Der Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |
<br>
**Beispiel: Öffnen der Tonauswahl, damit der Benutzer einen Ton auswählen kann**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## Aufgabe: Anzeige des Tools für das Speichern von Tönen

Die Argumente, die Sie zum Anzeigen des Tools für das Speichern von Tönen übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Action | string | Ja | "SaveRingtone" | Öffnet die Auswahl, um einen Klingelton zu speichern. |
| ToneFileSharingToken | string | Ja | [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx)-Dateifreigabetoken für die Klingeltondatei, die gespeichert werden soll. | Speichert eine bestimmte Audiodatei als Klingelton. Die unterstützten Inhaltstypen für die Datei sind „mpeg audio“ und „x-ms-wma audio“. |
| DisplayName | string | Nein | Der Anzeigename des angegebenen Tons. | Legt den Anzeigenamen fest, der beim Speichern des angegebenen Klingeltons verwendet werden soll. |

<br>Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewerte | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Erfolg.<br>1 – Vom Benutzer abgebrochen.<br>2 – Ungültige Datei.<br>3 – Ungültiger Inhaltstyp.<br>4 – Datei überschreitet die maximal zulässige Größe für Klingeltöne (1MB in Windows10).<br>5 – Datei überschreitet die Begrenzung auf 40Sekunden.<br>6 – Datei wird durch Digital Rights Management geschützt.<br>7 – Ungültige Parameter. | Das Ergebnis des Auswahlvorgangs. |
<br>
**Beispiel: Speichern einer lokalen Musikdatei als Klingelton**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## Aufgabe: Konvertieren eines Tontokens in dessen Anzeigenamen

Die Argumente, die Sie zum Abrufen des Anzeigenamens eines Tons übergeben können, sind:

| Parameter | Typ | Erforderlich | Mögliche Werte | Beschreibung |
|-----------|------|----------|-------|-------------|
| Action | string | Ja | "GetToneName" | Gibt an, dass Sie den Anzeigenamen eines Tons abrufen möchten. |
| ToneToken | string | Ja | Das Tontoken. | Das Tontoken, aus dem ein Anzeigename abgerufen werden soll. |

<br>Die Werte, die in [LaunchUriResults.Result](https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.launchuriresult.result.aspx) zurückgegeben werden:

| Rückgabewert | Typ | Mögliche Werte | Beschreibung |
|--------------|------|-------|-------------|
| Ergebnis | Int32 | 0 – Der Auswahlvorgang war erfolgreich.<br>7 – Falscher Parameter (wenn beispielsweise kein Tontoken angegeben wurde).<br>9 – Fehler beim Lesen des Namens für das angegebene Token.<br>10 – Das angegebene Tontoken kann nicht gefunden werden. | Das Ergebnis des Auswahlvorgangs.
| DisplayName | string | Der Anzeigename des Tons. | Gibt den Anzeigenamen des ausgewählten Tons zurück. Dieser Parameter wird im ValueSet nur zurückgegeben, wenn **Result** 0 ist. |
<br>
**Beispiel: Abrufen eines Tontokens aus Contact.RingToneToken und Anzeigen des Anzeigenamens auf der Kontaktkarte.**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```



<!--HONumber=Aug16_HO4-->


