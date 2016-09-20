---
author: mcleblanc
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: Tipps zu MVVM und Sprachleistung
description: "In diesem Thema werden einige Leistungsaspekte in Bezug auf die Wahl von Softwaredesignmustern und Programmiersprachen erläutert."
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 77a162076e14b4726e1fb29673b14be65be37a16

---
# Tipps zu MVVM und Sprachleistung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Thema werden einige Leistungsaspekte in Bezug auf die Wahl von Softwaredesignmustern und Programmiersprachen erläutert.

## Das Model-View-ViewModel(MVVM)-Muster

Das Model-View-ViewModel(MVVM)-Muster kommt in zahlreichen XAML-Apps zur Anwendung. (MVVM ähnelt sehr stark dem von Fowler beschriebenen Model-View-Presenter-Muster, ist aber speziell auf XAML zugeschnitten.) Das Problem mit dem MVVM-Muster: Es kann zu Apps mit zu vielen Ebenen und Zuordnungen führen. Vorteile von MVVM:

-   
            **Aufgabenteilung**. Es ist immer hilfreich, ein Problem in kleinere Teile zu zerlegen. Mit einem Muster wie MVVM oder MVC können Sie eine App (und sogar ein einzelnes Steuerelement) in die eigentliche Ansicht, ein logisches Modell der Ansicht (Ansichtsmodell) und die von der Ansicht unabhängige App-Logik (das Modell) unterteilen. Dabei hat es sich bewährt, dass sich die Designer mit einem Tool um die Ansicht, die Entwickler mit einem anderen Tool um das Modell und die Designintegratoren mit beiden Tools um Ansicht und Modell kümmern.
-   
            **Modultests**. Für das Ansichtsmodell (und letztlich auch für das Modell) können Modultests durchgeführt werden, die von der Ansicht und somit von Fenstererstellung, Eingaben usw. unabhängig sind. Dank einer klein gehaltenen Ansicht können Sie einen großen Teil Ihrer App testen, ohne jemals ein Fenster erstellen zu müssen.
-   
            **Flexibilität beim Ändern der Benutzererfahrung**. Die Ansicht wird in der Regel besonders häufig und meistens zuletzt geändert, wenn die Benutzeroberfläche auf der Grundlage von Endbenutzerfeedback optimiert wird. Durch die getrennte Behandlung der Ansicht lassen sich diese Änderungen schneller und mit geringeren Auswirkungen auf die App implementieren.

Es gibt mehrere konkrete Definitionen des MVVM-Musters sowie Drittanbieter-Frameworks, die Sie bei der Implementierung unterstützen. Die strikte Einhaltung jeglicher Variante des Musters kann jedoch dazu führen, dass der Zusatzaufwand für eine App jedes vernünftige Maß übersteigt.

-   Die XAML-Datenbindung ({Binding}-Markuperweiterung) wurde unter anderem für die Verwendung von Modell/Ansicht-Mustern entwickelt. {Binding} ist jedoch mit einer hohen Arbeitssatz- und CPU-Auslastung verbunden. Die Erstellung einer Bindung führt zu einer Reihe von Zuordnungen, und die Aktualisierung eines Bindungsziels kann zu Reflektion und Boxing führen. Diese Probleme werden mit der {x:Bind}-Markuperweiterung behandelt, die die Bindung zur Erstellungszeit kompiliert. 
            **Empfehlung:** Verwenden Sie {x:Bind}.
-   Bei MVVM wird „Button.Click“ gerne mithilfe eines gängigen ICommand-Hilfsbefehls wie „DelegateCommand“ oder „RelayCommand“ mit dem Ansichtsmodell verknüpft. Bei diesen Befehlen handelt es sich jedoch um zusätzliche Zuordnungen (einschließlich des CanExecuteChanged-Ereignislisteners), die den Arbeitssatz vergrößern und die Start-/Navigationszeiten für die Seite erhöhen. 
            **Empfehlung:** Ziehen Sie als Alternative zur Verwendung der praktischen ICommand-Schnittstelle die Verwendung von Ereignishandlern im CodeBehind in Betracht. Diese können Sie dann mit den Ansichtsereignissen verknüpfen und bei deren Auslösung einen Befehl für Ihr Ansichtsmodell aufrufen. Darüber hinaus müssen Sie zusätzlichen Code hinzufügen, um die Schaltfläche zu deaktivieren, wenn der Befehl nicht verfügbar ist.
-   Bei Verwendung von MVVM erstellen Entwickler gerne eine Seite mit allen möglichen UI-Konfigurationen und reduzieren dann Teile der Struktur, indem sie die Visibility-Eigenschaft an Eigenschaften des Ansichtsmodells binden. Dadurch erhöht sich unnötig die Startzeit, und auch der Arbeitssatz kann sich unnötig vergrößern, da einige Teile der Struktur möglicherweise gar nicht angezeigt werden. 
            **Empfehlung:** Verwenden Sie das x:DeferLoadStrategy-Feature, um unnötige Teile der Struktur aus dem Startvorgang zu verbannen. Erstellen Sie außerdem separate Benutzersteuerelemente für die verschiedenen Modi der Seite, und sorgen Sie mithilfe des CodeBehind dafür, dass nur die benötigten Steuerelemente geladen bleiben.

## Empfehlungen für C++/CX

-   
            **Verwenden Sie die jeweils aktuelle Version**. Die Leistung des C++/CX-Compilers wird kontinuierlich optimiert. Verwenden Sie für die App-Erstellung das neueste Toolset.
-   
            **Deaktivieren Sie RTTI (/GR-)**. RTTI ist im Compiler standardmäßig aktiviert. Sofern die Option also nicht durch Ihre Buildumgebung deaktiviert wird, verwenden Sie sie wahrscheinlich. RTTI verursacht einen erheblichen Mehraufwand und sollte deaktiviert werden, sofern Ihr Code nicht davon abhängig ist. Das XAML-Framework erfordert keine Verwendung von RTTI in Ihrem Code.
-   
            **Setzen Sie PPL-Aufgaben sparsam ein**. PPL-Aufgaben sind beim Aufrufen asynchroner WinRT-APIs äußerst praktisch, vergrößern aber erheblich den Codeumfang. Das C++/CX-Team arbeitet an einem Programmiersprachenfeature namens „await“, das die Leistung deutlich verbessert. Vorerst empfiehlt es sich jedoch, PPL-Aufgaben an den langsamsten Pfaden Ihres Codes nur sehr sparsam einzusetzen.
-   
            **Vermeiden Sie die Verwendung von C++/CX in der Geschäftslogik Ihrer App**. C++/CX ist für den komfortablen Zugriff auf WinRT-APIs in C++-Apps konzipiert. Die dabei verwendeten Wrapper bedeuten einen Zusatzaufwand. Daher empfiehlt es sich, innerhalb der Geschäftslogik bzw. des Modells Ihrer Klasse auf den Einsatz von C++/CX zu verzichten und es nur an der Grenze zwischen Ihrem Code und WinRT zu verwenden.

 

 







<!--HONumber=Jun16_HO4-->


