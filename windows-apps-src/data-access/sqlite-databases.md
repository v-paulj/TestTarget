---
author: mcleblanc
ms.assetid: 5A47301A-2291-4FC8-8BA7-55DB2A5C653F
title: SQLite-Datenbanken
description: "SQLite ist ein eingebettetes Datenbankmodul ohne Server. In diesem Artikel wird erläutert, wie die im SDK enthaltene SQLite-Bibliothek verwendet wird und wie Sie Ihre eigene SQLite-Bibliothek in einer Universellen Windows-App verpacken oder aus der Quelle erstellen können."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: dd628d16b3ee230ddc0c56b47fd381a518b8af00

---
# SQLite-Datenbanken

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


SQLite ist ein eingebettetes Datenbankmodul ohne Server. In diesem Artikel wird erläutert, wie die im SDK enthaltene SQLite-Bibliothek verwendet wird und wie Ihre eigene SQLite-Bibliothek in einer universellen Windows-App verpackt oder aus der Quelle erstellt wird.

## Was ist SQLite und wann wird es verwendet?

SQLite ist eine eingebettete Open-Source-Datenbank ohne Server. In den letzten Jahren hat es sich zur dominanten geräteseitigen Technologie für die Speicherung von Daten auf vielen Plattformen und Geräten entwickelt. Universelle Windows-Plattform (UWP) unterstützt und empfiehlt SQLite für den lokalen Speicher in allen Windows 10-Gerätefamilien.

SQLite eignet sich am besten für Telefon-Apps, eingebettete Apps für Windows 10 IoT Core (IoT Core) und als Cache für RDBS-Daten (Relations Database Server) von Unternehmen. Es erfüllt die meisten Anforderungen an den lokalen Datenzugriff, sofern keine umfangreichen gleichzeitigen Schreibvorgänge oder BigData-Szenarios vorliegen, was für die meisten Apps aber unwahrscheinlich ist.

In Anwendungen für Medienwiedergabe und Spiele kann SQLite auch als Dateiformat zum Speichern von Katalogen oder anderen Ressourcen verwendet werden, zum Beispiel für die Level eines Spiels, die unverändert von einem Webserver heruntergeladen werden können.

## Hinzufügen von SQLite zu einem UWP-App-Projekt

Es gibt drei Möglichkeiten zum Hinzufügen von SQLite zu einem UWP-Projekt.

1.  [Verwenden des SDK SQLite](#using-the-sdk-sqlite)
2.  [Einschließen von SQLite in das App-Paket](#including-sqlite-in-the-app-package)
3.  [Erstellen von SQLite aus der Quelle in Visual Studio](#building-sqlite-from-source-in-visual-studio)

### Verwenden des SDK SQLite

Sie können die im UWP-SDK enthaltene SQLite-Bibliothek verwenden, um die Größe des Anwendungspakets zu reduzieren, und sich für die regelmäßige Aktualisierung der Bibliothek auf die Plattform verlassen. Mit dem SDK SQLite können Sie auch Leistungsvorteile erzielen, z. B. kürzere Startzeiten, sofern die SQLite-Bibliothek mit großer Wahrscheinlichkeit bereits in den Arbeitsspeicher für die Verwendung durch Systemkomponenten geladen wurde.

Um auf das SDK SQLite zu verweisen, schließen Sie den folgenden Header in Ihrem Projekt ein. Der Header enthält auch die Version von SQLite, die von der Plattform unterstützt wird.

`#include <winsqlite/winsqlite3.h>`

Konfigurieren Sie das Projekt zur Verknüpfung mit winsqlite3.lib. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**&gt;**Linker**&gt;**Eingabe**. Fügen Sie dann winsqlite3.lib in **Zusätzliche Abhängigkeiten** hinzu.

### 2. Einschließen von SQLite im App-Paket

Manchmal möchten Sie Ihre eigene Bibliothek möglicherweise packen, anstatt die SDK-Version zu verwenden, wenn Sie z. B. eine bestimmte Version für die plattformübergreifenden Clients verwenden möchten, die sich von der im SDK enthaltenen Version von SQLite unterscheidet.

Installieren Sie die SQLite-Bibliothek auf der Visual Studio-Erweiterung der universellen Windows-Plattform über SQLite.org oder über das Tool für Erweiterungen und Updates.

![Bildschirm „Erweiterungen und Updates“](./images/extensions-and-updates.png)

Sobald die Erweiterung installiert ist, verweisen Sie in Ihrem Code auf die folgende Headerdatei.

`#include <sqlite3.h>`

### 3. Erstellen von SQLite aus der Quelle in Visual Studio

Manchmal möchten Sie Ihre eigene SQLite-Binärdatei möglicherweise für die Verwendung [verschiedener Compileroptionen](http://www.sqlite.org/compile.html) kompilieren, um die Dateigröße zu reduzieren, die Leistung der Bibliothek zu optimieren, oder die Funktionen auf Ihre Anwendung anzupassen. SQLite stellt Optionen für die Plattformkonfiguration, für das Festlegen von Standardparameterwerten, für das Festlegen von Größeneinschränkungen, für das Steuern von Betriebsmerkmalen, für das Aktivieren von Funktionen, die normalerweise deaktiviert sind, für das Deaktivieren von Features, die normalerweise aktiviert sind, für das Auslassen von Features, für das Aktivieren der Analyse und des Debuggens und für das Verwalten des Arbeitsspeicher-Zuweisungsverhaltens unter Windows bereit.

*Hinzufügen einer Quelle zu einem Visual Studio-Projekt*

Der SQLite-Quellcode steht zum Download auf der [SQLite.org-Downloadseite](https://www.sqlite.org/download.html) zur Verfügung. Fügen Sie diese Datei dem Visual Studio-Projekt der Anwendung hinzu, in der Sie SQLite verwenden möchten.

*Konfigurieren von Präprozessoren*

Verwenden Sie immer SQLITE\_OS\_WINRT und SQLITE\_API=\_\_declspec(dllexport) zusätzlich zu allen anderen [Kompilierzeitoptionen](http://www.sqlite.org/compile.html).

![Bildschirm mit den SQLite-Eigenschaftsseiten](./images/property-pages.png)

## Verwalten einer SQLite-Datenbank

SQLite-Datenbanken können mit den SQLite-C-APIs erstellt, aktualisiert und gelöscht werden. Weitere Informationen zur SQLite-C-API finden Sie auf der SQLite.org-Seite unter [Einführung in die SQLite-C/C++-Schnittstelle](http://www.sqlite.org/cintro.html).

Um ein fundiertes Verständnis der Funktionsweise von SQLite zu erhalten, arbeiten Sie von der Hauptaufgabe der SQL-Datenbank (das Auswerten von SQL-Anweisungen) aus rückwärts. Zwei Objekte müssen Sie berücksichtigen:

-   [Das Datenbankverbindungs-Handle](https://www.sqlite.org/c3ref/sqlite3.html)
-   [Das vorbereitete Anweisungsobjekt](https://www.sqlite.org/c3ref/stmt.html)

Es gibt sechs Schnittstellen zum Ausführen von Datenbankvorgängen für diese Objekte:

-   [sqlite3\_open()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/open.html)
-   [sqlite3\_prepare()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/prepare.html)
-   [sqlite3\_step()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/step.html)
-   [sqlite3\_column()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/column_blob.html)
-   [sqlite3\_finalize()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/finalize.html)
-   [sqlite3\_close()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/close.html)

 

 







<!--HONumber=Jun16_HO4-->


