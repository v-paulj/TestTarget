---
author: mijacobs
Description: "Hier erfahren Sie, wie Sie lokale, Roaming- und temporäre App-Daten speichern und abrufen können."
title: Speichern und Abrufen von Einstellungen und anderen App-Daten
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 433cda6432d1e695bacb338e68012bcc9c837eb6

---

# Speichern und Abrufen von Einstellungen und anderen App-Daten





*App-Daten* sind änderbare Daten, die für eine bestimmte App spezifisch sind. Dazu zählen der Laufzeitstatus, die Benutzereinstellungen und weitere Einstellungen. App-Daten unterscheiden sich von *Benutzerdaten*, Daten, die der Benutzer beim Verwenden einer App erstellt und verwaltet. Benutzerdaten umfassen Dokument- oder Mediendateien, E-Mail- oder Kommunikationstranskripte oder Datenbankeinträge, mit vom Benutzer erstellte Inhalte enthalten. Benutzerdaten können für mehrere Apps nützlich oder von Bedeutung sein. Häufig handelt es sich dabei um Daten, die der Benutzer als von der App unabhängige Entität ändern oder übertragen möchte (z. B. ein Dokument).

**Wichtiger Hinweis zu App-Daten:  **Die Lebensdauer der App-Daten ist an die Lebensdauer der App gebunden. Wenn die App entfernt wird, gehen auch alle App-Daten verloren. Verwenden Sie App-Daten nicht zum Speichern von Benutzerdaten oder anderen Daten, die Benutzer als wertvoll und unersetzlich betrachten. Solche Informationen sollten in den Bibliotheken des Benutzers und auf Microsoft OneDrive gespeichert werden. App-Daten eignen sich perfekt zum Speichern von App-spezifischen (Benutzer-)Einstellungen und Favoriten.

## <span id="Types_of_app_data"></span><span id="types_of_app_data"></span><span id="TYPES_OF_APP_DATA"></span>App-Datentypen


Es gibt zwei Arten von App-Daten: Einstellungen und Dateien.

-   **Einstellungen**

    Verwenden Sie die Einstellungen zum Speichern von Benutzereinstellungen und Anwendungsstatus-Informationen. Mit der App-Daten-API können Sie ganz einfach Einstellungen erstellen und abrufen (später in diesem Artikel zeigen wir Ihnen einige Beispiele dazu).

    Im Folgenden finden Sie Datentypen, die Sie für App-Einstellungen verwenden können:

    -   **UInt8**, **Int16**, **UInt16**, **Int32**, **UInt32**, **Int64**, **UInt64**, **Single**, **Double**
    -   **Boolescher Wert**
    -   **Char16**, **String**
    -   [
              **DateTime**
            ](https://msdn.microsoft.com/library/windows/apps/br206576), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [
              **ApplicationDataCompositeValue**
            ](https://msdn.microsoft.com/library/windows/apps/br241588): Eine Reihe von verwandten App-Einstellungen, die atomisch serialisiert und deserialisiert werden müssen. Verwenden Sie Verbundeinstellungen, um atomische Aktualisierungen voneinander abhängiger Einstellungen problemlos behandeln zu können. Das System stellt die Integrität von Verbundeinstellungen bei gleichzeitigem Zugriff und Roaming sicher. Verbundeinstellungen sind für kleine Datenmengen vorgesehen. Bei Verwendung für große Datasets kann die Leistung beeinträchtigt werden.
-   **Dateien**

    Verwenden Sie Dateien zum Speichern von Binärdaten oder zum Aktivieren Ihrer eigenen, benutzerdefinierten, serialisierten Typen.

## <span id="Storing_app_data_in_the_app_data_stores"></span><span id="storing_app_data_in_the_app_data_stores"></span><span id="STORING_APP_DATA_IN_THE_APP_DATA_STORES"></span>Speichern von App-Daten in den App-Datenspeichern


Bei der Installation einer App weist das System der App eigene, benutzerspezifische Datenspeicher für Einstellungen und Dateien zu. Sie müssen nicht wissen, wo und wie diese Daten vorhanden ist, da das System für die Verwaltung des physischen Speichers zuständig ist. Damit wird sichergestellt, dass die Daten von anderen Apps und anderen Benutzern isoliert gehalten werden. Das System behält außerdem den Inhalt dieser Datenspeicher bei, wenn der Benutzer ein App-Update installiert, und entfernt den Inhalt dieser Datenspeicher vollständig und sauber, wenn die App deinstalliert wird.

Jede App verfügt in ihrem jeweiligen App-Datenspeicher über systemdefinierte Stammverzeichnisse: eines für lokale Dateien, eines für Roamingdateien und eines für temporäre Dateien. Ihre App kann den Stammverzeichnissen neue Dateien und Container hinzufügen.

## <span id="Local_app_data"></span><span id="local_app_data"></span><span id="LOCAL_APP_DATA"></span>Lokale App-Daten


Lokale App-Daten sollten für alle Informationen verwendet werden, die zwischen App-Sitzungen beibehalten werden müssen und die nicht als App-Roamingdaten geeignet sind. Daten, die auf anderen Geräten nicht verwendet werden können, sollten ebenfalls dort gespeichert werden. Es gibt keine allgemeinen Größenbeschränkungen für lokal gespeicherte Daten. Verwenden Sie den lokalen App-Datenspeicher für Daten, für die ein Roaming nicht sinnvoll ist, sowie für große Datasets.

### <span id="Retrieve_the_local_app_data_store"></span><span id="retrieve_the_local_app_data_store"></span><span id="RETRIEVE_THE_LOCAL_APP_DATA_STORE"></span>Abrufen des lokalen App-Datenspeichers

Bevor Sie lokale App-Daten lesen oder schreiben können, müssen Sie den lokalen App-Datenspeicher abrufen. Verwenden Sie zum Abrufen des lokalen App-Datenspeichers die [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622)-Eigenschaft, um die lokalen Einstellungen der App als [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599)-Objekt abzurufen. Verwenden Sie die [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621)-Eigenschaft, um die Dateien aus einem [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)-Objekt abzurufen. Verwenden Sie die [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825)-Eigenschaft, um den Ordner im lokalen App-Datenspeicher abzurufen, in dem Sie Dateien speichern können, die nicht in der Sicherung und Wiederherstellung enthalten sind.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <span id="Create_and_retrieve_a_simple_local_setting"></span><span id="create_and_retrieve_a_simple_local_setting"></span><span id="CREATE_AND_RETRIEVE_A_SIMPLE_LOCAL_SETTING"></span>Erstellen und Abrufen einer einfachen lokalen Einstellung

Um eine Einstellung zu erstellen oder zu schreiben, verwenden Sie die [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615)-Eigenschaft, um auf die Einstellungen im `localSettings`-Container zuzugreifen, den wir im vorherigen Schritt abgerufen haben. In diesem Beispiel wird eine Einstellung namens `exampleSetting` erstellt.

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

Um die Einstellung abzurufen, verwenden Sie dieselbe [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615)-Eigenschaft, mit der Sie die Einstellung erstellt haben. Dieses Beispiel zeigt, wie Sie die gerade erstellte Einstellung abrufen können.

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <span id="Create_and_retrieve_a_local_composite_value"></span><span id="create_and_retrieve_a_local_composite_value"></span><span id="CREATE_AND_RETRIEVE_A_LOCAL_COMPOSITE_VALUE"></span>Erstellen und Abrufen eines lokalen zusammengesetzten Werts

Erstellen Sie zum Erstellen oder Schreiben eines zusammengesetzten Werts ein [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)-Objekt. In diesem Beispiel wird eine Verbundeinstellung namens `exampleCompositeSetting` erstellt und dem Container `localSettings` hinzugefügt.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

In diesem Beispiel wird veranschaulicht, wie der gerade erstellte, zusammengesetzte Wert abgerufen werden kann.

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <span id="Create_and_read_a_local_file"></span><span id="create_and_read_a_local_file"></span><span id="CREATE_AND_READ_A_LOCAL_FILE"></span>Erstellen und Lesen einer lokalen Datei

Verwenden Sie die Datei-APIs (z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) und [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)), um eine Datei im lokalen App-Datenspeicher zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `localFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.

```CSharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

Verwenden Sie die Datei-APIs (z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) und [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)), um eine Datei im lokalen App-Datenspeicher zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Schritt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```CSharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <span id="Roaming_data"></span><span id="roaming_data"></span><span id="ROAMING_DATA"></span>Roamingdaten


Wenn Sie Roamingdaten in Ihrer App verwenden, können Benutzer die App-Daten der App problemlos für mehrere Geräte synchronisieren. Installiert ein Benutzer eine App auf mehreren Geräten, sorgt das Betriebssystem dafür, dass die App-Daten synchronisiert sind, und verringert so den Einrichtungsaufwand für die App auf weiteren Geräten. Mittels Roaming können Benutzer außerdem eine Aufgabe (beispielsweise das Erstellen einer Liste) an genau dem Punkt fortsetzen, an dem sie die Aufgabe auf einem anderen Gerät unterbrochen haben. Das Betriebssystem repliziert aktualisierte Roamingdaten in der Cloud und synchronisiert die Daten mit den anderen Geräten, auf denen die App installiert ist.

Das Betriebssystem begrenzt für jede App die Menge der App-Daten für das Roaming. Siehe [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625). Erreicht die App diese Grenze, werden keine App-Daten der App mehr in die Cloud repliziert, bis die Gesamtmenge der App-Roamingdaten wieder unter dem Grenzwert liegt. Aus diesem Grund ist es empfehlenswert, Roamingdaten nur für Benutzereinstellungen, Links und kleine Datendateien zu verwenden.

Roamingdaten für Apps sind in der Cloud verfügbar, solange der Benutzer innerhalb des erforderlichen Zeitintervalls mit einem Gerät darauf zugreift. Führt ein Benutzer eine App für eine dieses Zeitintervall nicht überschreitende Spanne aus, werden die Roamingdaten aus der Cloud gelöscht. Bei der Deinstallation einer App werden die entsprechenden Roamingdaten in der Cloud nicht automatisch gelöscht, sondern beibehalten. Installiert der Benutzer die App innerhalb des Zeitintervalls wieder, werden die Roamingdaten über die Cloud synchronisiert.

### Empfohlene und nicht empfohlene Vorgehensweisen für Roamingdaten

-   Verwenden Sie Roaming für Benutzereinstellungen und -anpassungen, Links und kleine Datendateien. Sie können mithilfe von Roaming beispielsweise dafür sorgen, dass die vom Benutzer vorgenommene Einstellung für die Hintergrundfarbe auf allen Geräten angewendet wird.
-   Verwenden Sie Roaming, damit Benutzer eine Aufgabe auf mehreren Geräten fortsetzen können. Sie können das Roaming für Anwendungsdaten beispielsweise implementieren, um den Inhalt eines E-Mail-Entwurfs oder die zuletzt angezeigte Seite in einer Lese-App zu speichern.
-   Behandeln Sie das [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620)-Ereignis, indem Sie App-Daten aktualisieren. Dieses Ereignis tritt unmittelbar nach der Synchronisierung der App-Daten mit der Cloud auf.
-   Führen Sie Roaming für Verweise auf Inhalte durch, anstelle für unformatierte Daten. Das Roaming für eine URL eines Onlineartikel ist beispielsweise sinnvoller als das Roaming für den Inhalt des Artikels.
-   Verwenden Sie für wichtige, zeitkritische Informationen die Einstellung *HighPriority* zusammen mit [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624).
-   Führen Sie kein Roaming für gerätespezifische App-Daten durch. Manche dieser Informationen sind ausschließlich lokal relevant, wie beispielsweise der Pfadname zu einer lokalen Dateiressource. Wenn Sie lokale Informationen als Roamingdaten verwenden möchten, stellen Sie sicher, dass die App wiederhergestellt werden kann, wenn die Informationen auf dem zweiten Gerät nicht gültig sind.
-   Führen Sie kein Roaming für große App-Datenmengen durch. Die Menge der App-Daten für das Roaming ist begrenzt. Verwenden Sie die Eigenschaft [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625), um diese maximale Datenmenge zu implementieren. Wenn eine App diese Grenze erreicht, wird so lange kein Datenroaming durchgeführt, bis die Größe des App-Datenspeichers wieder unter die Grenze fällt. Berücksichtigen Sie beim App-Design, wie eine Grenze für größere Datenmengen festgelegt wird, sodass die Begrenzung nicht überschritten wird. Wenn für das Speichern eines Spielstands beispielsweise jeweils 10 KB erforderlich sind, sollte die App unter Umständen höchstens zulassen, dass 10 Spielstände gespeichert werden.
-   Verwenden Sie Roaming nicht für Daten, die von einer sofortigen Synchronisierung abhängig sind. Windows garantiert keine sofortige Synchronisierung; die Synchronisierung kann erheblich verzögert werden, wenn Benutzer offline oder mit einem Netzwerk mit hoher Latenz verbunden sind. Stellen Sie sicher, dass die UI nicht von einer sofortigen Synchronisierung abhängig ist.
-   Verwenden Sie Roaming nicht für sich häufig ändernde Daten. Wenn Ihre App beispielsweise Informationen nachverfolgt, die sich häufig ändern (z. B. die Position eines Musiktitels in Sekunden), speichern Sie diese Daten nicht als Roaming-App-Daten. Wählen Sie stattdessen eine weniger häufige angepasste Darstellung, die trotzdem eine gute Benutzererfahrung gewährleistet, z. B. den aktuell wiedergegebenen Titel.

### <span id="Roaming_pre-requisites"></span><span id="roaming_pre-requisites"></span><span id="ROAMING_PRE-REQUISITES"></span>Voraussetzungen für Roaming

Jeder Benutzer kann von Roaming-App-Daten profitieren, wenn ein Microsoft-Konto zur Anmeldung am Gerät verwendet wird. Benutzer und Gruppenrichtlinienadministratoren haben jedoch die Möglichkeit, das Roaming von App-Daten auf einem Gerät zu deaktivieren. Benutzer, die kein Microsoft-Konto verwenden oder die Datenroamingfunktion deaktivieren, können Ihre App auch weiterhin verwenden, die App-Daten sind dann jedoch auf jedem Gerät lokal vorhanden.

Daten, die im [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) gespeichert sind, werden nur übertragen, wenn ein Benutzer ein Gerät als "vertrauenswürdig" eingestuft hat. Wird einem Gerät nicht vertraut, werden die in diesem Tresor gespeicherten Daten nicht für das Roaming verwendet.

### <span id="Conflict_resolution"></span><span id="conflict_resolution"></span><span id="CONFLICT_RESOLUTION"></span>Konfliktlösung

Das Roaming von App-Daten ist nicht für eine gleichzeitige Verwendung auf mehreren Geräten vorgesehen Wenn es während der Synchronisierung zu einem Konflikt kommt, weil eine bestimmte Dateneinheit auf beiden Geräten geändert wurde, verwendet das System vorzugsweise immer den zuletzt geschriebenen Wert. Dadurch wird sichergestellt, dass der App immer die aktuellsten Informationen zur Verfügung stehen. Handelt es sich bei der Dateneinheit um eine zusammengesetzte Einstellung, findet die Konfliktlösung trotzdem auf der Ebene der Einstellungseinheit statt, d. h. der zuletzt geänderte Wert der Zusammensetzung wird synchronisiert.

### <span id="When_to_write_data"></span><span id="when_to_write_data"></span><span id="WHEN_TO_WRITE_DATA"></span>Zeitpunkt für das Schreiben von Daten

Je nach erwarteter Lebensdauer der Einstellung sollten Daten zu unterschiedlichen Zeitpunkten geschrieben werden. Selten bzw. langsam geänderte App-Daten sollten sofort geschrieben werden. Häufig geänderte App-Daten sollten dagegen nur in bestimmten Zeiträumen regelmäßig (beispielsweise einmal alle fünf Minuten) und bei angehaltener App geschrieben werden. So kann eine Musik-App beispielsweise die Einstellung für den aktuellen Titel schreiben, wenn ein neuer Titel wiedergegeben wird, aber die aktuelle Position im Titel sollte nur in angehaltenem Zustand der Anwendung geschrieben werden.

### <span id="Excessive_usage_protection"></span><span id="excessive_usage_protection"></span><span id="EXCESSIVE_USAGE_PROTECTION"></span>Schutz vor übermäßiger Nutzung

Das System verfügt über verschiedene Schutzmechanismen, um unangemessene Verwendung der Ressourcen zu verhindern. Falls App-Daten nicht wie erwartet übertragen werden, wurde das Gerät wahrscheinlich vorübergehend beschränkt. Wenn Sie einige Zeit warten, wird dieses Problem normalerweise automatisch behoben, ohne dass eine Aktion erforderlich ist.

### <span id="Versioning"></span><span id="versioning"></span><span id="VERSIONING"></span>Versionsverwaltung

App-Daten können die Versionsinformationen nutzen, um von einer Datenstruktur auf eine andere zu aktualisieren. Die Versionsnummer unterscheidet sich von der App-Version und kann nach Belieben festgelegt werden. Auch wenn es nicht zwingend erforderlich ist, empfiehlt es sich, aufsteigende Versionsnummern zu verwenden, da beim Übergang zu einer kleineren Nummer, die neuere Daten darstellt, unerwünschte Komplikationen (z. B. Datenverluste) entstehen können.

Das Roaming für App-Daten wird nur in installierten Apps mit der gleichen Versionsnummer durchgeführt. Beispiel: Geräte mit der Versionsnummer 2 tauschen Daten untereinander aus. Gleiches gilt für Geräte mit der Version 3. Es erfolgt jedoch kein Roaming zwischen Geräten mit Version 2 und Geräten mit Version 3. Wenn Sie eine neue App installieren, die auf anderen Geräten unterschiedliche Versionsnummern verwendet hat, synchronisiert die neu installierte App die App-Daten, die mit der höchsten Versionsnummer verknüpft sind.

### <span id="Testing_and_tools"></span><span id="testing_and_tools"></span><span id="TESTING_AND_TOOLS"></span>Tests und Tools

Entwickler können ihr Gerät sperren, um eine Synchronisierung von Roaming-App-Daten auszulösen. Wenn die App-Daten scheinbar nicht innerhalb eines bestimmten Zeitrahmens übertragen werden, überprüfen Sie die folgenden Elemente, und stellen Sie sicher, dass:

-   Ihre Roamingdaten nicht die maximal zulässige Größe überschreiten (siehe auch [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)).
-   Ihre Dateien geschlossen und ordnungsgemäß veröffentlicht wurden.
-   Sie über mindestens zwei Geräte mit derselben App-Version verfügen.


### <span id="Register_to_receive_notification_when_roaming_data_changes"></span><span id="register_to_receive_notification_when_roaming_data_changes"></span><span id="REGISTER_TO_RECEIVE_NOTIFICATION_WHEN_ROAMING_DATA_CHANGES"></span>Registrieren, um Benachrichtigungen bei der Änderung von Roamingdaten zu erhalten

Um das Roaming von App-Daten verwenden zu können, müssen Sie die Roamingdatenänderungen registrieren und die Roamingdatencontainer abrufen, damit Sie Einstellungen lesen und schreiben können.

1.  Nehmen Sie eine Registrierung für den Empfang einer Benachrichtigung vor, wenn sich Roamingdaten ändern.

    Das [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620)-Ereignis benachrichtigt Sie, wenn sich Roamingdaten ändern. In diesem Beispiel wird `DataChangeHandler` als Handler für Änderungen an Roamingdaten festgelegt.

```    CSharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  Rufen Sie die Container für die Einstellungen und Dateien der App ab.

    Rufen Sie mit der [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)-Eigenschaft die Einstellungen und mit der [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)-Eigenschaft die Dateien ab.

```    CSharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <span id="Create_and_retrieve_roaming_settings"></span><span id="create_and_retrieve_roaming_settings"></span><span id="CREATE_AND_RETRIEVE_ROAMING_SETTINGS"></span>Erstellen und Abrufen von Roamingeinstellungen

Verwenden Sie die [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615)-Eigenschaft, um auf die Einstellungen im Container `roamingSettings` zuzugreifen, den wir im vorherigen Abschnitt abgerufen haben. In diesem Beispiel wird eine einfache Einstellung namens `exampleSetting` und ein zusammengesetzter Wert namens `composite` erstellt.

```CSharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

Dieses Beispiel ruft die gerade erstellten Einstellungen ab.

```CSharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <span id="Create_and_retrieve_roaming_files"></span><span id="create_and_retrieve_roaming_files"></span><span id="CREATE_AND_RETRIEVE_ROAMING_FILES"></span>Erstellen und Abrufen von Roamingdateien

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) und [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505), um eine Datei im Roamingspeicher für App-Daten zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `roamingFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.

```CSharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) und [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482), um eine Datei im Roamingspeicher für App-Daten zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Abschnitt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```CSharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <span id="Temporary_app_data"></span><span id="temporary_app_data"></span><span id="TEMPORARY_APP_DATA"></span>Temporäre App-Daten


Der temporäre App-Datenspeicher funktioniert wie ein Cache. Für diese Dateien erfolgt kein Roaming, und sie können jederzeit gelöscht werden. Daten, die an diesem Speicherort gespeichert werden, können von der Aufgabe zur Systemverwaltung jederzeit automatisch gelöscht werden. Benutzer können Dateien im temporären Datenspeicher auch mit der Datenträgerbereinigung löschen. Temporäre App-Daten können zum Speichern temporärer Informationen während einer App-Sitzung verwendet werden. Es gibt jedoch keine Garantie dafür, dass diese Daten über die App-Sitzung hinaus beibehalten werden, da der verwendete Speicherplatz ggf. vom System zurückgefordert wird. Der Speicherort ist über die [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629)-Eigenschaft verfügbar.

### <span id="Retrieve_the_temporary_data_container"></span><span id="retrieve_the_temporary_data_container"></span><span id="RETRIEVE_THE_TEMPORARY_DATA_CONTAINER"></span>Abrufen des temporären Datencontainers

Rufen Sie die Dateien mit der [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629)-Eigenschaft ab. In den nächsten Schritten wird die `temporaryFolder`-Variable aus diesem Schritt verwendet.

```CSharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;</code></pre></td>
</tr>
</tbody>
</table>
```

### <span id="Create_and_read_temporary_files"></span><span id="create_and_read_temporary_files"></span><span id="CREATE_AND_READ_TEMPORARY_FILES"></span>Erstellen und Lesen von temporären Dateien

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) und [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505), um eine Datei im temporären App-Datenspeicher zu erstellen und zu aktualisieren. In diesem Beispiel wird im Container `temporaryFolder` die Datei `dataFile.txt` erstellt, in die das aktuelle Datum und die Uhrzeit geschrieben werden. Der Wert **ReplaceExisting** aus der [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631)-Enumeration gibt an, dass die Datei ersetzt werden soll, falls sie bereits vorhanden ist.

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

Verwenden Sie die Datei-APIs, z. B. [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272), [**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) und [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482), um eine Datei im temporären App-Datenspeicher zu öffnen und zu lesen. In diesem Beispiel wird die im vorherigen Schritt erstellte Datei `dataFile.txt` geöffnet und das Datum aus der Datei gelesen. Einzelheiten zum Laden von Dateiressourcen aus verschiedenen Speicherorten finden Sie unter [So wird's gemacht: Laden von Dateiressourcen](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322).

```CSharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <span id="Organize_app_data_with_containers"></span><span id="organize_app_data_with_containers"></span><span id="ORGANIZE_APP_DATA_WITH_CONTAINERS"></span>Organisieren von App-Daten mit Containern


Um Ihre Einstellungen für App-Daten und Dateien strukturiert zu speichern, erstellen Sie Container (dargestellt durch [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599)-Objekte), statt direkt mit Verzeichnissen zu arbeiten. Sie können Container zu den lokalen, Roaming- und temporären App-Datenspeichern hinzufügen. Container können in bis zu 32 Ebenen geschachtelt werden.

Rufen Sie die [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611)-Methode auf, um einen Einstellungscontainer zu erstellen. In diesem Beispiel wird ein lokaler Einstellungscontainer namens `exampleContainer` erstellt und eine Einstellung namens `exampleSetting` hinzugefügt. Der Wert **Always** aus der [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616)-Enumeration gibt an, dass der Container erstellt wird, sofern er noch nicht vorhanden ist.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <span id="Delete_app_settings_and_containers"></span><span id="delete_app_settings_and_containers"></span><span id="DELETE_APP_SETTINGS_AND_CONTAINERS"></span>Löschen von App-Einstellungen und Containern


Verwenden Sie die [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608)-Methode, um eine einfache Einstellung zu löschen, die Ihre App nicht mehr benötigt. In diesem Beispiel wird die zuvor erstellte, lokale `exampleSetting`-Einstellung gelöscht.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

Verwenden Sie die [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597)-Methode, um eine zusammengesetzte Einstellung zu löschen. In diesem Beispiel wird die lokale, zusammengesetzte `exampleCompositeSetting`-Einstellung gelöscht, die in einem früheren Beispiel erstellt wurde.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

Rufen Sie die [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612)-Methode auf, um einen Container zu löschen. In diesem Beispiel wird der zuvor erstellte, lokale `exampleContainer`-Einstellungscontainer gelöscht.

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <span id="Versioning_your_app_data"></span><span id="versioning_your_app_data"></span><span id="VERSIONING_YOUR_APP_DATA"></span>Versionsverwaltung Ihrer App-Daten


Optional können Sie die App-Daten für Ihre App mit einer Versionsnummer versehen. In diesem Fall können Sie neue Versionen der betreffenden App erstellen, mit denen das Format der App-Daten geändert wird, ohne dass es dadurch zu Kompatibilitätsproblemen mit der Vorgängerversion der App kommt. Die App prüft die Version der App-Daten im Dateispeicher. Handelt es sich um eine niedrigere Version als erwartet, aktualisiert die App sowohl die Anwendungsdaten auf das neue Format als auch die Version. Weitere Informationen finden Sie in den Artikeln zur [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630)-Eigenschaft und zur [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429)-Methode.

## Verwandte Artikel

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)





<!--HONumber=Jun16_HO4-->


