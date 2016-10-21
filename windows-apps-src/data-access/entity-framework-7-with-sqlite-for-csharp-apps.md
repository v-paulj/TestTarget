---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) ist eine objektrelationale Zuordnung, die Ihnen über domänenspezifische Objekte die Verwendung relationaler Daten ermöglicht."
title: "Entity Framework7 mit SQLite für C#-Apps"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b453a2a6c3ab0b9418122ae27bf6a3a1c56e5873

---

# Entity Framework7 mit SQLite für C#-Apps

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Entity Framework (EF) ist eine objektrelationale Zuordnung, die Ihnen über domänenspezifische Objekte die Verwendung relationaler Daten ermöglicht. In diesem Artikel wird erläutert, wie Sie Entity Framework7 mit einer SQLite-Datenbank in einer universellen Windows-App verwenden können.

Mit Entity Framework7, das ursprünglich für .NET-Entwickler konzipiert wurde, können Sie mit SQLite auf der universellen Windows-Plattform (UWP) relationale Daten mit bestimmten Domänenobjekten speichern und bearbeiten. Sie können EF-Code aus einer .NET-App zu einer UWP-App migrieren und davon ausgehen, dass die App bei entsprechenden Änderungen der Verbindungszeichenfolge funktioniert.

Derzeit unterstützt EF auf der UWP nur SQLite. Eine ausführliche exemplarische Vorgehensweise zur Installation von Entity Framework7 und zum Erstellen von Modellen finden Sie unter [Getting Started on Universal Windows Platform](http://go.microsoft.com/fwlink/p/?LinkId=735013). Die folgenden Themen werden behandelt:

-   Voraussetzungen
-   Erstellen eines neuen Projekts
-   Installieren von Entity Framework
-   Erstellen Ihres Modells
-   Erstellen Ihrer Datenbank
-   Verwenden Ihres Modells




<!--HONumber=Aug16_HO3-->


