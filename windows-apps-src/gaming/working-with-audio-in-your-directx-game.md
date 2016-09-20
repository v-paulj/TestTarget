---
author: mtoepke
title: "Audio für Spiele"
description: "Hier erfahren Sie, wie Sie Musik und Sound entwickeln und in Ihr DirectX-Spiel integrieren. Außerdem erfahren Sie, wie Sie Audiosignale verarbeiten, um dynamischen und positionsbezogenen Sound zu erzeugen."
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 98896d4966ee2adb3bf494dcba22656fc8bb6414

---

# Audio für Spiele


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Hier erfahren Sie, wie Sie Musik und Sound entwickeln und in Ihr DirectX-Spiel integrieren. Außerdem erfahren Sie, wie Sie Audiosignale verarbeiten, um dynamischen und positionsbezogenen Sound zu erzeugen.

Für die Audioprogrammierung empfehlen wir die Verwendung der XAudio2-Bibliothek in DirectX, die auch hier zur Anwendung kommt. XAudio2 ist eine untergeordnete Audiobibliothek, die eine grundlegende Signalverarbeitung und -abmischung für Spiele bereitstellt und eine Vielzahl von Formaten unterstützt.

Einfache Sounds sowie eine einfache Musikwiedergabe können auch mithilfe von [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) implementiert werden. Microsoft Media Foundation ist eigentlich für die Wiedergabe von Mediendateien und Datenströmen (sowohl Audio als auch Video) konzipiert, kann aber auch in Spielen eingesetzt werden und ist besonders hilfreich bei Filmszenen oder Spielkomponenten ohne Interaktionsmöglichkeit.

## Konzepte auf einen Blick


Im Anschluss finden Sie einige Konzepte für die Audioprogrammierung, die in diesem Abschnitt zur Anwendung kommen.

-   Signale bilden die Grundeinheit der Soundprogrammierung (vergleichbar mit Pixeln bei der Grafik). Die digitalen Signalprozessoren (Digital Signal Processors, DSPs), von denen sie verarbeitet werden, sind gewissermaßen die Pixelshader für den Sound des Spiels. Sie können Signale transformieren, kombinieren oder filtern. Mithilfe der DSP-Programmierung können Sie die Soundeffekte und die Musik Ihres Spiels ganz flexibel und mit der gewünschten Komplexität verändern.
-   Bei Stimmen handelt es sich um einen Submix aus mindestens zwei Signalen. XAudio2 bietet drei Arten von Stimmobjekten: Quellstimme, Submixstimme und Masterstimme. Quellstimmen verarbeiten die vom Client bereitgestellten Audiodaten. Quell- und Submixstimmen senden ihre Ausgabe an mindestens eine Submix- oder Masterstimme. Submix- und Masterstimmen mischen die Audiodaten aller Stimmen, von denen sie Daten erhalten, und verarbeiten das Ergebnis. Masterstimmen schreiben Audiodaten auf ein Audiogerät.
-   Beim Mixing werden mehrere getrennte Stimmen – beispielsweise die Soundeffekte und die Hintergrundgeräusche einer Szene – in einem einzelnen Stream miteinander kombiniert. Beim Submixing werden mehrere getrennte Signale – beispielsweise die Soundkomponenten eines Motorengeräuschs – zu einer Stimme kombiniert.
-   Audioformate. Musik und Soundeffekte für Ihr Spiel können in vielen unterschiedlichen digitalen Formaten gespeichert werden. Zur Auswahl stehen unkomprimierte Formate wie WAV sowie komprimierte Formate wie MP3 und OGG. Je stärker die Komprimierung eines Audiosamples (üblicherweise abzulesen an der Bitrate), desto schlechter die Klangtreue, da die Verringerung der Bitrate höhere Verluste nach sich zieht. Aufgrund der Klangtreueunterschiede bei verschiedenen Komprimierungsschemas und Bitraten empfiehlt es sich, ein wenig zu experimentieren, um eine möglichst gute Lösung für Ihr Spiel zu finden.
-   Samplingrate und Qualität. Sound kann unterschiedliche Samplingraten haben. Mit abnehmender Samplingrate verschlechtert sich die Klangtreue allerdings erheblich. Die Samplingrate für Sound in CD-Qualität beträgt 44,1kHz (44.100Hz). Wenn es bei Ihrem Sound nicht auf hohe Klangtreue ankommt, können Sie eine geringere Samplingrate wählen. Eine höhere Samplingrate empfiehlt sich unter Umständen für professionelle Audioanwendungen, bei einem Spiel ist sie dagegen nicht unbedingt erforderlich – es sei denn, das Spiel benötigt Sound mit professioneller Klangtreue.
-   Soundquellen. Soundquellen in XAudio2 sind Punkte, von denen Sound ausgeht – ganz gleich, ob es sich dabei um einen Piepton im Hintergrund oder um einen fetzigen Rocksong aus einer Stereoanlage im Spiel handelt. Die Quellen werden anhand von Weltkoordinaten angegeben.
-   Soundempfänger. Beim Soundempfänger handelt es sich häufig um den Spieler, in aufwendigeren Spielen möglicherweise auch um eine Entität mit künstlicher Intelligenz, die den von einer Soundquelle stammenden Sound verarbeitet. Dieser Sound kann per Submixing dem Audiodatenstrom zugeführt und so für den Spieler wiedergegeben werden. Alternativ können Sie den Sound aber auch verwenden, um eine bestimmte Aktion im Spiel (beispielsweise die Alarmierung einer als Empfänger markierten KI-Wache) auszulösen.

## Überlegungen bei der Entwicklung


Audio spielt beim Entwerfen und Entwickeln von Spielen eine immens wichtige Rolle. Viele Spieler erinnern sich noch an eher durchschnittliche Spiele, die allein dank ihres einprägsamen Soundtracks, dank großartiger Sprecher und einer tollen Soundabmischung oder allgemein dank einer herausragenden Audioproduktion Kultstatus erreicht haben. Musik und Sound definieren die Persönlichkeit eines Spiels. Sie liefern auch das Hauptmotiv, das das Spiel definiert und von ähnlichen Spielen abhebt. Der Aufwand, den Sie für die Ausarbeitung und Entwicklung des Audioprofils Ihres Spiel betreiben, zahlt sich in jedem Fall aus.

Mit positionsbezogenem 3D-Sound können Sie die 3D-Grafik Ihres Spiels um ein zusätzliches immersives Element erweitern. Falls Sie ein komplexes Spiel entwickeln, das eine Welt simuliert oder einen filmähnlichen Stil erfordert, sollten Sie die Verwendung von Techniken für positionsbezogenen 3D-Sound in Betracht ziehen, um den Spieler voll und ganz in die Spielwelt eintauchen zu lassen.

## Roadmap für DirectX-Audioentwicklung


### Konzeptionelle Ressourcen zu XAudio2

XAudio2 ist die Audiomixingbibliothek für DirectX, die in erster Linie zum Entwickeln von Audiomodulen mit hoher Leistung für Spiele gedacht ist. Spieleentwicklern, die Soundeffekte und Hintergrundmusik zu ihren modernen Spielen hinzufügen möchten, bietet XAudio2 ein Modul für Audiodiagramme und zum Mischen mit geringer Latenz und Unterstützung für dynamische Puffer, synchrone Wiedergabe genau nach Beispiel und implizite Quellratenumwandlung.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Einführung in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)</p></td>
<td align="left"><p>Dieses Thema enthält eine Liste der von XAudio2 unterstützten Audioprogrammierfeatures.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Erste Schritte mit XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415762)</p></td>
<td align="left"><p>Dieses Thema enthält Informationen zu wichtigen XAudio2-Konzepten, XAudio2-Versionen und zum RIFF-Audioformat.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Allgemeine Konzepte für die Audioprogrammierung](https://msdn.microsoft.com/library/windows/desktop/ee415692)</p></td>
<td align="left"><p>Dieses Thema bietet einen Überblick über allgemeine Audiokonzepte, mit denen ein Audioentwickler vertraut sein sollte.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2-Stimmen](https://msdn.microsoft.com/library/windows/desktop/ee415825)</p></td>
<td align="left"><p>Dieses Thema bietet einen Überblick über XAudio2-Stimmen, die für Submixing und das Verarbeiten und Mastern von Audiodaten verwendet werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2-Rückrufe](https://msdn.microsoft.com/library/windows/desktop/ee415745)</p></td>
<td align="left"><p>In diesem Thema werden die XAudio2-Rückrufe behandelt, mit denen Unterbrechungen bei der Audiowiedergabe verhindert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2-Audiodiagramme](https://msdn.microsoft.com/library/windows/desktop/ee415739)</p></td>
<td align="left"><p>In diesem Thema werden die XAudio2-Audioverarbeitungsdiagramme behandelt. Diese empfangen eine Reihe von Audiostreams vom Client als Eingabe, verarbeiten sie und übergeben das Endergebnis an ein Audiogerät.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[XAudio2-Audioeffekte](https://msdn.microsoft.com/library/windows/desktop/ee415756)</p></td>
<td align="left"><p>In diesem Thema werden XAudio2-Audioeffekte behandelt, die eingehende Audiodaten empfangen und vor der Weitergabe bestimmte Vorgänge für die Daten (z.B. einen Halleffekt) ausführen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Streamen von Audiodaten mit XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415821)</p></td>
<td align="left"><p>In diesem Thema wird das Audiostreaming mit XAudio2 behandelt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[X3DAudio](https://msdn.microsoft.com/library/windows/desktop/ee415714)</p></td>
<td align="left"><p>In diesem Thema wird X3DAudio vorgestellt. Dabei handelt es sich um eine API, die in Verbindung mit XAudio2 verwendet wird, um die Illusion eines 3D-Klangeffekts zu erzeugen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[XAudio2-Programmierreferenz](https://msdn.microsoft.com/library/windows/desktop/ee415899)</p></td>
<td align="left"><p>Dieser Abschnitt enthält die vollständige Referenz für die XAudio2-APIs.</p></td>
</tr>
</tbody>
</table>

 

### XAudio2-Anleitungsressourcen

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Initialisieren von XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie XAudio2 für die Audiowiedergabe initialisieren, indem Sie eine Instanz des XAudio2-Moduls und eine Masterstimme erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Laden von Datendateien in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die zur Wiedergabe von Audiodaten in XAudio2 erforderlichen Strukturen füllen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Wiedergeben von Ton mit XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie zuvor geladene Audiodaten in XAudio2 wiedergeben.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Verwenden von Submixstimmen](https://msdn.microsoft.com/library/windows/desktop/ee415794)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Gruppen von Stimmen festlegen, um ihre Ausgabe an dieselbe Submixstimme zu senden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Verwenden der Rückrufe für Quellstimmen](https://msdn.microsoft.com/library/windows/desktop/ee415769)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die XAudio2-Quellstimmrückrufe verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Verwenden der Modulrückrufe](https://msdn.microsoft.com/library/windows/desktop/ee415774)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die XAudio2-Modulrückrufe verwenden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Erstellen eines grundlegenden Audioverarbeitungsdiagramms](https://msdn.microsoft.com/library/windows/desktop/ee415767)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie ein Audioverarbeitungsdiagramm erstellen, das sich aus einer Masterstimme und einer Quellstimme zusammensetzt.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Dynamisches Hinzufügen und Entfernen von Stimmen zu bzw. aus einem Audiodiagramm](https://msdn.microsoft.com/library/windows/desktop/ee415772)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Submixstimmen zu einem Diagramm hinzufügen oder daraus entfernen, das anhand der Schritte unter [So wird's gemacht: Erstellen eines grundlegenden Audioverarbeitungsdiagramms](https://msdn.microsoft.com/library/windows/desktop/ee415767) erstellt wurde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Erstellen einer Effektkette](https://msdn.microsoft.com/library/windows/desktop/ee415789)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie eine Effektkette auf eine Stimme anwenden, um die benutzerdefinierte Verarbeitung der Audiodaten für diese Stimme zu ermöglichen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Erstellen eines XAPOs](https://msdn.microsoft.com/library/windows/desktop/ee415730)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie [<strong>IXAPO</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415893) zum Erstellen eines XAudio2-Audioverarbeitungsobjekts (XAudio2 Audio Processing Object, XAPO) implementieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Hinzufügen der Laufzeitparameterunterstützung zu einem XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415728)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie einem XAPO durch Implementieren der [<strong>IXAPOParameters</strong>](https://msdn.microsoft.com/library/windows/desktop/ee415896)-Schnittstelle die Laufzeitparameterunterstützung hinzufügen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Verwenden eines XAPOs in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415733)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie einen Effekt verwenden, der als XAPO in einer XAudio2-Effektkette implementiert wurde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Verwenden von XAPOFX in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415723)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie einen der Effekte in XAPOFX in einer XAudio2-Effektkette verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Streamen von Sound von einem Datenträger](https://msdn.microsoft.com/library/windows/desktop/ee415791)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Audiodaten in XAudio2 streamen, indem Sie einen separaten Thread zum Lesen eines Audiopuffers erstellen und diesen Thread mithilfe von Rückrufen steuern.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[So wird's gemacht: Integrieren von X3DAudio in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415798)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie mit X3DAudio die Lautstärke- und Tonhöhenwerte für XAudio2-Stimmen sowie die Parameter für den integrierten XAudio2-Halleffekt bereitstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[So wird's gemacht: Gruppieren von Audiomethoden als Vorgangssatz](https://msdn.microsoft.com/library/windows/desktop/ee415783)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie XAudio2-Vorgangssätze verwenden, damit eine Gruppe von Methodenaufrufen gleichzeitig wirksam wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Debuggen von Audiostörungen in XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415765)</p></td>
<td align="left"><p>Hier erfahren Sie, wie der Debuggingprotokolliergrad für XAudio2 festgelegt wird.</p></td>
</tr>
</tbody>
</table>

 

### MediaFoundation-Ressourcen

Media Foundation (MF) ist eine Medienplattform zum Streamen von Audio- und Videodaten. Mithilfe der MediaFoundation-APIs können Sie mit verschiedenen Algorithmen codierte und komprimierte Audio- und Videodaten streamen. Die Plattform ist nicht für Echtzeitspielszenarien konzipiert. Stattdessen bietet sie leistungsstarke Tools und breite Codec-Unterstützung für eine lineare Aufnahme und Präsentation von Audio- und Videokomponenten.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Info über Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms696274)</p></td>
<td align="left"><p>Dieser Abschnitt enthält allgemeine Informationen zu den MediaFoundation-APIs und die verfügbaren Tools zu ihrer Unterstützung.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[MediaFoundation: Grundlegende Konzepte](https://msdn.microsoft.com/library/windows/desktop/ee663601)</p></td>
<td align="left"><p>In diesem Thema werden einige Konzepte vorgestellt, die Sie vor dem Schreiben einer MediaFoundation-Anwendung kennen müssen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Media Foundation-Architektur](https://msdn.microsoft.com/library/windows/desktop/ms696219)</p></td>
<td align="left"><p>In diesem Abschnitt werden das allgemeine Design von Microsoft Media Foundation sowie die Mediengrundtypen und die verwendete Verarbeitungspipeline beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Audio-/Videoaufzeichnung](https://msdn.microsoft.com/library/windows/desktop/dd317910)</p></td>
<td align="left"><p>In diesem Thema wird die Verwendung von Microsoft Media Foundation zum Aufzeichnen von Audio- und Videodaten beschrieben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Audio-/Videowiedergabe](https://msdn.microsoft.com/library/windows/desktop/dd317914)</p></td>
<td align="left"><p>In diesem Thema wird die Implementierung der Audio- oder Videowiedergabe in Ihrer App beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Unterstützte Medienformate in MediaFoundation](https://msdn.microsoft.com/library/windows/desktop/dd757927)</p></td>
<td align="left"><p>In diesem Thema sind die Medienformate aufgeführt, für die Microsoft Media Foundation systemeigene Unterstützung bietet. (Drittanbieter können zusätzliche Formate durch Erstellung benutzerdefinierter Plug-Ins unterstützen.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Codierung und Dateierstellung](https://msdn.microsoft.com/library/windows/desktop/dd318778)</p></td>
<td align="left"><p>In diesem Thema wird die Verwendung von Microsoft Media Foundation zum Codieren von Audio- und Videodaten und zum Erstellen von Mediendateien beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[WindowsMedia-Codecs](https://msdn.microsoft.com/library/windows/desktop/ff819508)</p></td>
<td align="left"><p>In diesem Thema wird beschrieben, wie Sie mit den Features der WindowsMedia-Codecs für Audio- und Videodaten komprimierte Datenströme erzeugen und wiedergeben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[MediaFoundation-Programmierreferenz](https://msdn.microsoft.com/library/windows/desktop/ms704847)</p></td>
<td align="left"><p>Dieser Abschnitt enthält Referenzinformationen für die MediaFoundation-APIs.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[MediaFoundation-SDK-Beispiele](https://msdn.microsoft.com/library/windows/desktop/aa371827)</p></td>
<td align="left"><p>In diesem Abschnitt sind Beispiel-Apps aufgeführt, die die Verwendung von MediaFoundation veranschaulichen.</p></td>
</tr>
</tbody>
</table>

 

### Medientypen der Windows-Runtime-XAML

Bei Verwendung der [DirectX-XAML-Interoperabilität](https://msdn.microsoft.com/library/windows/apps/hh825871) können die Windows-Runtime-XAML-Medien-APIs für einfachere Spielszenarien mithilfe von DirectX mit C++ in die WindowsStore-Apps integriert werden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[<strong>Windows.UI.Xaml.Controls.MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926)</p></td>
<td align="left"><p>XAML-Element, das ein Objekt darstellt, das Audio- oder Videodaten oder beide Datentypen enthält.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Audio, Video und Kamera](https://msdn.microsoft.com/library/windows/apps/mt203788)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie grundlegende Audio- und Videoinhalte in Ihrer UWP-App (Universelle Windows-Plattform) integrieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie in Ihrer UWP-App eine lokal gespeicherte Mediendatei wiedergeben.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie in Ihrer UWP-App eine Mediendatei mit geringer Wartezeit streamen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Medienumwandlung](https://msdn.microsoft.com/library/windows/apps/mt282143)</p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie mit dem Vertrag für "Wiedergeben auf" Medien aus Ihrer UWP-App auf anderen Geräten streamen.</p></td>
</tr>
</tbody>
</table>

 

## Referenz


-   [XAudio2-Einführung](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [XAudio2-Programmieranleitung](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Übersicht über Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)

> **Hinweis**  
Dieser Artikel ist für Windows10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows8.x oder Windows Phone8.x entwickeln, finden Sie Informationen dazu in der [archivierten Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Verwandte Themen


-   [XAudio2-Programmieranleitung](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 







<!--HONumber=Jun16_HO4-->


