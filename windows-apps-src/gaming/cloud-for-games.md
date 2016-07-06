---
author: joannaleecy
title: "Nutzen von Clouddiensten für UWP-Spiele"
description: "Erfahren Sie mehr über die Implementierung der Cloud als Back-End für Ihre UWP-Spiele."
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.sourcegitcommit: b25f02dc4ebcf960882e64f66f0306a8e584ebbd
ms.openlocfilehash: d9b252783213f0c6a82944729f98c84e21d56535

---
#  Nutzen von Clouddiensten für UWP-Spiele

Die Universelle Windows-Plattform (UWP) in Windows 10 bietet eine Reihe von APIs, die zur Entwicklung von Spielen für Microsoft-Geräte verwendet werden können. Bei der Entwicklung von Spielen für alle Plattformen und Geräte können Sie ein Cloud-Back-End nutzen, um das Spiel je nach Bedarf zu skalieren.

##  Was ist Cloud Computing?

Beim Cloud Computing werden über das Internet IT-Ressourcen und Anwendungen zum Speichern und Verarbeiten von Daten eingesetzt. Der Begriff _Cloud_ beschreibt die große Menge an nicht-lokalen Ressourcen, auf die Sie von überall aus zugreifen können.
Das Cloud Computing ermöglicht eine neue Art der Ressourcen- und Softwarenutzung. Sie müssen nicht mehr vorab für das komplette Produkt oder die Ressourcen zahlen, sondern können Plattformen, Software und Ressourcen stattdessen als Dienst nutzen. Cloudanbieter rechnen häufig nutzungsbasiert oder über Abonnements für Dienste ab.

##  Vorteile von Clouddiensten

Ein Vorteil beim Einsatz von Clouddiensten für Spiele ist, dass Sie nicht vorab in eigene physische Server investieren müssen. Sie zahlen stattdessen erst später für die Nutzung oder die abonnierten Dienste. So senken Sie Ihre finanziellen Risiken bei der Entwicklung eines neuen Spieles. 

Ein weiterer Vorteil ist, dass Ihr Spiel auf umfangreiche Cloudressourcen bauen kann und so skalierbar ist (Sie können unerwartete Spitzen bei der Spielermenge, bei rechenintensiven Echtzeit-Berechnungen und bei der Datenverarbeitung abfangen). So sorgen Sie dafür, dass die Leistung Ihres Spiels rund um die Uhr stabil bleibt. Darüber hinaus können Cloudressourcen von jedem Gerät auf jeder Plattform weltweit genutzt werden. Sie können Ihr Spiel also global vermarkten.

Für Ihre Spieler ist ein überzeugendes Spielerlebnis das Wichtigste. Da die Game-Server in der Cloud von clientseitigen Updates unabhängig sind, erhalten Sie mehr Kontrolle und sorgen für eine sicherere Umgebung.   Indem Sie Ihre Game-Logik nicht auf dem Client, sondern serverseitig in der Cloud ausführen, können Sie für ein konsistentes Gameplay sorgen. Sie können Verbindungen zwischen Diensten konfigurieren und so für ein besser integriertes Spielerlebnis sorgen – beispielsweise, indem Sie In-Game-Käufe mit verschiedenen Zahlungsanbietern verknüpfen, mehrere Gaming-Netzwerke verbinden und In-Game-Updates über soziale Netzwerke wie Facebook und Twitter bereitstellen. 

Sie können dedizierte Cloudserver nutzen und so große und persistente Spielwelten erstellen, eine Gamer-Community schaffen, Daten von den Spielern sammeln und analysieren und so im Laufe der Zeit das Gameplay verbessern und das Monetisierungsmodell des Spiels optimieren.

Auch Spiele, die umfangreiche Funktionalitäten zur Datenverwaltung benötigen (beispielsweise mit Funktionen für Spiele in sozialen Netzwerken, die mit asynchronen Multiplayer-Mechanismen arbeiten), können über Clouddienste implementiert werden.

##  Nutzung der Cloudtechnologie durch Entwickler

Hier erfahren Sie, wie andere Entwickler Cloudlösungen in ihre Spiele implementiert haben.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header">
        <th>Entwickler</th>
        <th>Beschreibung</th>
        <th>Kerndaten des Spiels</th>
        <th>Weitere Informationen</th>
    </tr>
    <tr>
        <td>[343 Industries](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_ implementierte mit Microsoft Azure DocumentDB [Halo: Spartan Companies](https://www.halowaypoint.com/spartan-companies) als Social-Gaming-Plattform. Microsoft Azure DocumentDB wurde aufgrund der schnellen und flexiblen Funktionalitäten für die automatische Indizierung ausgewählt.</td>
        <td>
            <ul>
                <li>Skalierbare Datenebene zur Verarbeitung der Erstellung und Verwaltung von Gruppen beim Multiplayer-Gameplay <li>Integration des Spiels mit sozialen Medien <li>Echtzeit-Datenabfragen über mehrere Attribute <li>Synchronisierung von Erfolgen und Statistiken im Spiel </ul>
        </td>
        <td>
            <ul>
                <li>[Social-Gaming-Implementierung über Azure DocumentDB](https://azure.microsoft.com/en-us/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>Illyriad Games entwickelte mit _Age of Ascent_ ein herausragendes 3D-MMO (Massively Multiplayer Online), das auf Geräten mit modernen Browsern gespielt werden kann. Das Spiel kann also auf PCs, Laptops, Smartphones und anderen mobilen Geräten ohne Plug-Ins genutzt werden. Es verwendet ASP.NET Core, HTML5, WebGL und Microsoft Azure.</td>
        <td>
            <ul>
                <li>Plattformübergreifendes, browserbasiertes Spiel <li>Eine einzige große und persistente offenen Welt <li>Durchführung von intensiven Gameplay-Berechnungen in Echtzeit <li>Skalierung mit der Anzahl der Spieler </ul>
        </td>
        <td>
            <ul>
                <li>[Verwalten von Spielekomponenten als Microservices mit Azure Service Fabric (Video)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Interview mit den Entwicklern von Age of Ascent (Video)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>Next Games ist der Entwickler von _The Walking Dead: No Man's Land_, das auf der Serie von AMC basiert. Das Spiel nutzt Azure als Back-End. Am Veröffentlichungswochenende gab es 1.000.000 Downloads, und in der ersten Woche schaffte es das Spiel in den USA auf Platz 1 der kostenlosen iPhone- und iPad-Apps, in 12 Ländern auf Platz 1 der kostenlosen Apps und in 13 Ländern auf Platz 1 der kostenlosen Spiele.
        </td>
        <td>
            <ul>
                <li>Plattformübergreifend <li>Rundenbasiertes Multiplayer-Spiel <li>Flexible Leistungsskalierung </ul>
        </td>
        <td>
            <ul>
                <li>[Interview mit Kalle Hiitola, CTO von Next Games (Video)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[The Walking Dead sorgt mit DocumentDB für schnellere Entwicklungszyklen und ein besseres Gameplay](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixel Squad](http://www.crimecoast.com/)</td>
        <td>Pixel Squad entwickelte mit der Unity-Engine und Azure das Spiel _Crime Coast_. _Crime Coast_ ist ein Social-Strategiespiel für Android, iOS und die Windows-Plattformen. Für das Spiel wurden Azure Blob Speicher, Managed Azure Redis Cache und mehrere IIS-VMs mit Lastenausgleich sowie der Microsoft Notification-Hub eingesetzt. Erfahren Sie, wie die Skalierung und Handhabung der Spieler bei 5.000 gleichzeitigen Spielern möglich ist.
        </td>
        <td>
            <ul>
                <li>Plattformübergreifend <li>Multiplayer-Onlinespiel <li>Skalierung mit der Anzahl der Spieler </ul>
        </td>
        <td>
            <ul>
                <li>[So nutzt das Crime Coast-MMO Azure Cloud Services](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### Weitere Links

* [Azure als Geheimwaffe für Hitcents, Game Troopers und InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Spiele-Startups im Bizspark-Programm nutzen Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## Design des Cloud-Back-Ends

Schon während die Spieldesigner und Produzenten über die erforderlichen Features und Funktionalitäten eines neuen Spieles sprechen, sollten Sie das Design der Spieleinfrastruktur berücksichtigen. Wenn Sie Spiele für unterschiedliche Geräte und die wichtigsten Plattformen entwickeln möchten, können Sie die Azure-Cloud als Back-End für das Spiel nutzen.

### Schritt-für-Schritt-Lernanleitungen

* [Build 2016-Codelabs: Einsatz von Microsoft Azure App Service und Microsoft SQL Azure als Back-End zur Speicherung von Spielständen](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [Entwerfen der Mobilstrategie für Ihr Spiel](https://azure.microsoft.com/en-us/documentation/articles/mobile-engagement-gaming-scenario/)
* [Nutzung von Azure Mobile Engagement für die iOS-Bereitstellung mit Unity](https://azure.microsoft.com/en-us/documentation/articles/mobile-engagement-unity-ios-get-started/)

### Grundlegendes zu IaaS, PaaS und SaaS

Zunächst müssen Sie entscheiden, welche Dienstform für Spiel am besten geeignet ist. Um einen Ansatz zur Erstellung Ihres Back-Ends auswählen zu können, sollten Sie die Unterschiede der drei Dienstformen kennen.

* [Infrastructure-as-a-Service (IaaS)](https://azure.microsoft.com/en-us/overview/what-is-iaas/)

    IaaS stellt eine direkt verfügbare Computing-Infrastruktur dar, die über das Internet bereitgestellt und verwaltet wird. Stellen Sie sich vor, welche Möglichkeiten sich Ihnen eröffnen, wenn Sie mithilfe vieler Computer je nach Bedarf nach oben und unten skalieren können. Mit IaaS vermeiden Sie die Kosten und die Komplexität der Anschaffung und Verwaltung Ihrer eigenen Server und anderer Elemente einer Datacenter-Infrastruktur.

* [Platform-as-a-Service (PaaS)](https://azure.microsoft.com/en-us/overview/what-is-paas/)

    Ein PaaS-Dienst ist ähnlich wie ein IaaS-Dienst. Er umfasst jedoch auch die Verwaltung der Infrastruktur (z. B. Server, Storage und Netzwerk). Sie müssen also nicht nur keine physischen Server und die Datacenter-Infrastruktur anschaffen, sondern auch keine Softwarelizenzen kaufen und verwalten. Gleiches gilt für die zugrunde liegende Anwendungsstruktur, die Middleware, die Entwicklungstools und andere Ressourcen.

* Software-as-a-Service (SaaS)

    SaaS-Dienste sind normale Anwendungen, die Ihnen über eine vorhandene Cloudplattform bereitgestellt werden. Diese Dienste vereinfachen Ihre Bereitstellung des Spiels über die entsprechenden Dienste.


### Entwerfen der Spieleinfrastruktur mit Azure

In diesem Abschnitt lernen Sie Möglichkeiten zur Nutzung der Azure Cloudangebote für ein Spiel kennen. Azure arbeitet mit Windows, Linux und bekannten Open-Source-Technologien wie Ruby, Python, Java und PHP. Weitere Informationen finden Sie unter [Azure-Cloud](https://azure.microsoft.com).

| Anforderungen                 | Aktivitätsszenarien                            | Produktangebot                      | Produktfunktionen                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hosten Ihrer Domäne in der Cloud     | Effizientes Beantworten von DNS-Abfragen            | [Azure DNS](https://azure.microsoft.com/services/dns/) | Hosten Ihrer Domäne mit hoher Leistung und Verfügbarkeit  |
| Anmeldung, Identitätsüberprüfung      | Spieler meldet sich an und seine Identität wird authentifiziert  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Einmaliges Anmelden mit mehrstufiger Authentifizierung für alle Cloud-Web-Apps und lokalen Web-Apps            |
| Spiel nutzt IaaS-Modell      | Spiel wird auf virtuellen Computern in der Cloud gehostet       | [Azure VMs](https://azure.microsoft.com/services/virtual-machines/) | Skalierung der Game-Server von einer VM-Instanz bis zu mehreren tausend inkl. Virtual-Networking und Lastenausgleich; hybride Konsistenz mit lokalen Systemen           |
| Web-Spiele oder mobile Spiele nutzen PaaS-Modell            | Spiel wird auf einer verwalteten Plattform gehostet                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | PaaS für Websites oder mobile Spiele (über Azure-VMs mit Middleware, Entwicklungstools, BI, DB-Management)   |
| Cloud-Speicher für Spieldaten       | Aktuelle Spieledaten werden in der Cloud gespeichert und an die Client-Geräte gesendet | [Azure Blob-Speicher](https://azure.microsoft.com/services/storage/blobs/)| Keine Einschränkung hinsichtlich der Art der zu speichernden Datei, Objektspeicher für große Mengen an unstrukturierten Daten wie Bilder, Audioinhalte, Video und mehr.  |
| Temporäre Tabellen zur Datenspeicherung| Spieletransaktionen (Änderungen von Spielzuständen) werden vorübergehend in Tabellen gespeichert | [Azure-Tabellenspeicher](https://azure.microsoft.com/services/storage/tables/)| Spieledaten können in einem flexiblen Schema gemäß den Anforderungen des Spiels gespeichert werden |
| Warteschlange für Spieltransaktionen/-anfragen| Spieletransaktionen werden über eine Warteschlange verarbeitet | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Warteschlangen fangen unerwartete Datenverkehrsspitzen auf und könnten verhindern, dass Server durch eine plötzliche Flut von Anfragen überlastet werden   |
| Skalierbare relationale Spieldatenbank| Strukturierte Speicherung von relationalen Daten wie In-Game-Transaktionen zur Datenbank | [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)| SQL-Datenbank als Dienst ([Vergleich mit SQL auf einem virtuellen Computer](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Skalierbare verteilte Spieldatenbank mit geringer Latenz| Schnelles Lesen, Schreiben und Abfragen von Spiel- und Spielerdaten mit Schemaflexibilität | [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)| NoSQL-Dokumentdatenbank als Dienst mit geringer Latenz   |
| Nutzung eines eigenen Datencenters mit Azure-Diensten | Spiel aus dem eigenen Datacenter abgerufen und an die Client-Geräte gesendet | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Ermöglicht Ihrer Organisation die Bereitstellung von Azure-Diensten über Ihr eigenes Datacenter  |
| Übertragung großer Datenmengen| Große Dateien wie Bilder, Audio- und Videodateien können mit Azure CDN über den nächstgelegenen CDN-Pop-Standort (Content Delivery Network) an die Benutzer gesendet werden  | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Azure CDN basiert auf einer modernen Netzwerktopologie mit großen, zentralen Knoten und verarbeitet auch plötzliche Verkehrsspitzen und hohe Lasten. So steigen die Geschwindigkeit und Verfügbarkeit deutlich, und das Benutzererlebnis wird verbessert  |
| Niedrige Latenz               | Nutzen Sie die Zwischenspeicherung, um schnelle und skalierbare Spiele mit mehr Kontrolle und einer garantierten Isolation der Daten zu erstellen. Verbessern Sie außerdem die Mach-Making-Funktion des Spiels. | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Datenzugriff mit hohem Durchsatz und einer konsistent geringen Latenz für schnelle und skalierbare Azure-Anwendungen  |
| Hohe Skalierbarkeit, geringe Latenz | Verarbeitung von Fluktuationen bei der Anzahl der Benutzer im Spiel mit geringer Latenz für Lese- und Schreibvorgänge | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Für die komplexesten und datenintensiven Szenarien mit geringer Latenz und zuverlässiger Skalierung für viele gleichzeitige Benutzer geeignet. Service Fabric ermöglicht Ihnen die Entwicklung von Spielen, ohne einen für statuslose Apps erforderlichen separaten Speicher oder Zwischenspeicher erstellen zu müssen |
| Sammeln von mehreren Millionen Ereignissen pro Sekunde von Geräten                         | Protokollieren von mehreren Millionen Ereignissen pro Sekunde von Geräten | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | Telemetrieaufnahme von Spielen, Websites, Apps und Geräten auf Cloud-Niveau  |
| Verarbeiten von Spieldaten in Echtzeit  | Durchführen von Echtzeitanalyse von Spielerdaten zur Verbesserung des Gameplays| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Echtzeit-Datenstromverarbeitung in der Cloud  |
| Entwickeln eines vorausschauenden Gameplays         | Erstellen eines angepassten, dynamischen Gameplays auf Basis der Spielerdaten  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Ein vollständig verwalteter Cloud-Dienst, mit dem Sie ganz einfach Predictive-Analytics-Lösungen erstellen, bereitstellen und freigeben können  |
| Sammeln und Analysieren von Spieldaten| Massive Parallelverarbeitung von Daten aus relationalen und nicht-relationalen Datenbanken | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Flexibles Data-Warehouse als Dienst mit Enterprise-Funktionen   |
| Erstellen von Marketingkampagnen zur Steigerung und Erhaltung der Nutzungszahlen  | Senden von Push-Benachrichtigungen an Zielgruppen zur Generierung von Interesse und Förderung bestimmter Spielaktionen gemäß Datenanalysen | [Mobile-Engagement](https://azure.microsoft.com/services/mobile-engagement/) |  Steigerung der Spielzeit und der Benutzerzahlen auf allen wichtigen Plattformen (iOS, Android, Windows, Windows Phone) |


##  Ressourcen für Startups und Entwickler

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    Microsoft BizSpark ist ein globales Programm, das Startups durch einen kostenlosen Zugriff auf Azure-Clouddienste, Software und Support unterstützt. BizSpark.Mitglieder erhalten fünf Visual Studio Enterprise mit MSDN-Abonnements (jeweils mit einer monatlichen Azure-Gutschrift von 150 US-Dollar). Die Unternehmen können so pro Monat insgesamt 750 US-Dollar für Azure-Dienste für alle fünf Entwickler ausgeben. BizSpark steht Startups zur Verfügung, die sich in Privatbesitz befinden, weniger als 5 Jahre alt sind und weniger als 1 Million US-Dollar Umsatz pro Jahr machen. Microsoft möchte durch die Unterstützung von Startups eine für beide Seiten gewinnbringende und langfristige Partnerschaft etablieren.
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    Wenn Sie Xbox Live-Features wie Multiplayer-Gameplay, die plattformübergreifende Spielersuche, den Gamerscore, Erfolge und Bestenlisten für Ihr Spiel unter Windows 10 nutzen möchten, melden Sie sich bei ID@Xbox an. Dort erhalten Sie die Tools und die Unterstützung, die Sie für eine erfolgreiche Implementierung benötigen. Registrieren Sie vor der Bewerbung bei ID@Xbox zunächst ein Entwicklerkonto im [Windows Dev Center](https://developer.microsoft.com/windows/programs/join).

## Software-as-a Service für Game-Back-Ends

Dieser Abschnitt stellt einige Unternehmen vor, die Cloud-Back-Ends über wichtige Cloud-Dienstanbieter für Spiele anbieten, mit denen Sie sich auf die Entwicklung Ihres Spiels konzentrieren können.

* [GameSparks](http://www.gamesparks.com/)

    GameSparks ist eine cloudbasierte Entwicklungsplattform für Spieleentwickler, mit der diese die gesamten serverseitigen Komponenten erstellen können.

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon ist eine unabhängige Netzwerke-Engine und Multiplayer-Plattform für Spiele. Die Photon Cloud stellt SaaS-Dienste bereit und ist daher ein vollständig verwalteter Dienst. Sie können sich vollständig auf Ihren Anwendungsclient konzentrieren. Das Hosting, der Serverbetrieb und die Skalierung werden von Exit Games übernommen.

* [Playfab](https://playfab.com/)

    Playfab bietet eine erstklassige Back-End-Technologie für das Live-Game-Management von Smartphone-, PC- und Konsolenspielen.

## Verwandte Links

* [Handbuch zur Entwicklung von Spielen unter Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Microsoft Azure](https://azure.microsoft.com/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 



<!--HONumber=Jun16_HO4-->


