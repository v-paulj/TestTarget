---
author: mcleblanc
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: "Microsoft Visual Studio ist für Windows gleichbedeutend mit Xcode für iOS und Mac OS. In dieser exemplarischen Vorgehensweise möchten wir Ihnen helfen, sich mit der Verwendung von Visual Studio vertraut zu machen."
title: Erstellen eines Projekts in Visual Studio
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 23d9ed066e2909a15b3106fd19bf6ce5ab09e7a9

---

# Erste Schritte: Erstellen eines Projekts

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Erstellen eines Projekts

Microsoft Visual Studio ist für Windows gleichbedeutend mit Xcode für iOS und Mac OS. In dieser exemplarischen Vorgehensweise möchten wir Ihnen helfen, sich mit der Verwendung von Visual Studio vertraut zu machen. Sie lernen die Grundlagen kennen, die Sie beherrschen müssen, um mit der Entwicklung zu beginnen. Jedes Mal, wenn Sie eine App erstellen, führen Sie Schritte aus, die sich an dieser Beschreibung orientieren.

Im folgenden Video werden Xcode und Visual Studio verglichen.

<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/5b7bd91f-6a2f-40b6-9b19-eb2994931d0a/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">One Dev Minute – Vergleich zwischen Xcode und Visual Studio</iframe>

Sie werden außerdem den [Blogbeitrag zum Erstellen von Apps für Windows](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/) sehr hilfreich finden.

Das Erstellen einer App für Windows 10 (formeller als UWP-App bezeichnet) entspricht in etwa dem Erstellen einer iOS-App mit Storyboards. Die Windows 10-App wird (ähnlich einer Website) häufig über mehrere Seiten konstruiert, die jeweils einen anderen Teil der Benutzeroberfläche enthalten. Jeder Seite sind normalerweise zwei Quelldateien zugeordnet: eine zum Speichern der Benutzeroberfläche im unter [Übersicht über XAML](https://msdn.microsoft.com/library/windows/apps/mt185595) definierten Format und eine für den Quellcode, häufig C#. Beim Interagieren mit der App navigiert der Benutzer zwischen diesen Seiten. In dieser exemplarischen Vorgehensweise wird eine App mit zwei Seiten erstellt.

**Hinweis**  Ein wichtiges Feature von Windows 10-Apps ist, dass unabhängig von der Plattform der gleiche Quellcode und der gleiche API-Satz zur Verfügung steht. Wie Sie wissen, können Sie beim Schreiben einer universellen iOS-App für iPhone und iPad zur Laufzeit feststellen, auf welcher Plattform Ihre App ausgeführt wird, und entsprechende Maßnahmen ergreifen. Auf ähnliche Weise können Windows 10-Apps zur Laufzeit mitteilen, auf welchem Gerät sie ausgeführt werden. Bei einer UWP-App entfallen die \#ifdefs in Ihrem Quellcode zum Erstellen von Telefon- bzw. Desktopbuilds. Windows 10-Apps setzen auch ihre Benutzeroberflächen-Steuerelemente abhängig vom Gerät ein: Wenn Ihre App beispielsweise auf ein Datumsauswahl-Steuerelement verweist, weist dieses je nachdem, ob die App auf einem Desktop- oder Telefonbildschirm ausgeführt wird, unterschiedliches Aussehen und unterschiedliche Funktionsweisen auf. Der Quellcode bleibt jedoch unverändert.

Sehen wir uns an, wie eine Windows 10-App erstellt wird. Führen Sie zuerst Visual Studio 2013 aus. Bei der ersten Ausführung des Programms werden Sie zum Anfordern einer Entwicklerlizenz aufgefordert. Mithilfe einer Entwicklerlizenz können Sie Windows Store-Apps auf Ihrem lokalen Computer installieren und testen, bevor Sie sie an den Windows Store senden. Befolgen Sie zum Anfordern einer Lizenz die Bildschirmanweisungen, um sich mit einem Microsoft-Konto anzumelden. Wenn Sie kein Microsoft-Konto haben, klicken Sie im Dialogfeld **Entwicklerlizenz** auf den Link **Registrieren** und befolgen Sie die Bildschirmanweisungen.

Zum Vergleich: beim Starten von Xcode wird zunächst der Bildschirm **Willkommen zu Xcode** angezeigt, der der folgenden Abbildung ähnelt.

![Xcode-Willkommensbildschirm](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio ist sehr ähnlich aufgebaut. Es wird eine **Startseite** ähnlich der Seite in der folgenden Abbildung angezeigt.

![Visual Studio-Startbildschirm](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

Zum Erstellen einer neuen App müssen Sie zunächst ein Projekt anlegen. Gehen Sie hierzu folgendermaßen vor:

-   Tippen Sie im Bereich **Start** auf **Neues Projekt**.
-   Tippen Sie auf das Menü **Datei** und dann auf **Neues Projekt**.

Zum Vergleich: beim Erstellen eines neuen Projekts in Xcode wird eine Liste mit Projektvorlagen angezeigt, siehe folgende Abbildung.

![Xcode-Dialogfeld für neues Projekt](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

In Visual Studio können Sie ebenfalls aus einer Vielzahl von Projektvorlagen wählen, wie in der folgenden Abbildung dargestellt.

![Visual Studio-Dialogfeld für neues Projekt](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) Für diese exemplarische Vorgehensweise tippen Sie auf **Visual C#**, und tippen Sie dann auf **Windows**, **Windows Universal** und auf **Leere App (universelle Windows-App)**. Geben Sie „MyApp“ in das Feld **Name** ein, und tippen Sie dann auf **OK**. Daraufhin erstellt Visual Studio Ihr erstes Projekt und zeigt es an. Nun können Sie mit dem Entwerfen der App beginnen und Code hinzufügen.

## Nächster Schritt

[Erste Schritte: Auswählen einer Programmiersprache](getting-started-choosing-a-programming-language.md)



<!--HONumber=Jun16_HO4-->


