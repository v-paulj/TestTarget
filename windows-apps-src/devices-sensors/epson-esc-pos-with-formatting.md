---
author: DBirtolo
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS mit Formatierung
description: "Erfahren Sie, wie Sie die ESC/POS-Befehlssprache zum Formatieren von Text, z. B. in Fett und mit doppelter Größe, für Ihren Point of Service-Drucker verwenden."
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 33231abb1e573d837193daeee21713f09c7d8448

---
# Epson ESC/POS mit Formatierung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** Wichtige APIs **

-   [PointofService-Drucker](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Erfahren Sie, wie Sie die ESC/POS-Befehlssprache zum Formatieren von Text, z. B. in Fett und mit doppelter Größe, für Ihren Point of Service-Drucker verwenden.

## ESC/POS-Verwendung

Windows Point of Service ermöglicht die Verwendung einer Vielzahl von Druckern, z. B. mehrerer Drucker der Epson TM-Reihe. (Eine vollständige Liste der unterstützten Drucker finden Sie auf der Seite [PointofService-Drucker](https://msdn.microsoft.com/library/windows/apps/Mt426652).) Windows unterstützt das Drucken über die ESC/POS-Druckersteuerungssprache, die über effiziente und funktionale Befehle für die Kommunikation mit dem Drucker verfügt.

ESC/POS ist ein von Epson erstelltes Befehlssystem, das für viele POS-Druckersysteme verwendet wird und mit dem inkompatible Befehlssätze vermieden werden sollen, indem für eine universelle Anwendbarkeit gesorgt wird. Die meisten modernen Drucker unterstützen ESC/POS.

Alle Befehle beginnen mit dem ESC-Zeichen (ASCII 27, HEX 1B) oder GS (ASCII 29, HEX 1D), gefolgt von einem weiteren Zeichen, das für den Befehl steht. Normaler Text wird – getrennt durch Zeilenumbrüche – einfach an den Drucker gesendet.

Die [**Windows PointOfService API**](https://msdn.microsoft.com/library/windows/apps/Dn298071) stellt einen Großteil der Funktionalität für Sie über die Methoden **Print()** oder **PrintLine()** bereit. Um bestimmte Formatierungen zu erzielen oder spezielle Befehle zu senden, müssen Sie jedoch ESC/POS-Befehle senden, die wie eine Zeichenfolge aufgebaut sind und an den Drucker gesendet werden.

## Beispiel für die Verwendung von fetten Zeichen und Zeichen mit doppelter Größe

Im folgenden Beispiel wird die Verwendung von ESC/POS-Befehlen zum Drucken von fetten Zeichen und Zeichen mit doppelter Größe veranschaulicht. Beachten Sie, dass jeder Befehl wie eine Zeichenfolge aufgebaut ist und dann in die printJob-Aufrufe eingefügt wird.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to 
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

Weitere Informationen zu ESC/POS, darunter zu den verfügbaren Befehlen, finden Sie unter [Epson ESC/POS – FAQ](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Ausführliche Informationen zu [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) sowie zu allen verfügbaren Funktionen finden Sie auf der MSDN-Website unter [PointofService Printer](https://msdn.microsoft.com/library/windows/apps/Mt426652).





<!--HONumber=Jun16_HO4-->


