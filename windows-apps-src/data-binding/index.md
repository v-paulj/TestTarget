---
author: mcleblanc
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Datenbindung
description: "Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d1fbc7376b3505d39e757a1b65ae0f0890948078

---

# Datenbindung

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt. Im Markup können Sie entweder die [{X:Bind}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204783) oder die [{Binding}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt204782) verwenden. Sie können sogar eine Mischung aus den beiden in derselben App verwenden – sogar im gleichen Benutzeroberflächenelement. {x:Bind} ist neu in Windows 10 und bietet eine bessere Leistung. {Binding} bietet mehr Features.

| Thema | Beschreibung |
|-------|-------------|
| [Übersicht über Datenbindung](data-binding-quickstart.md) | In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden. Darüber hinaus wird erläutert, wie Sie die Anzeige von Elementen steuern, eine Detailansicht auf Grundlage einer Auswahl implementieren und Daten für die Anzeige umwandeln. Ausführliche Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md). | 
| [Datenbindung im Detail](data-binding-in-depth.md) | Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt. |
| [Beispieldaten für die Entwurfsoberfläche und Prototyperstellung](displaying-data-in-the-designer.md) | Möglicherweise ist es nicht möglich oder nicht erwünscht (z. B. aus Gründen des Datenschutzes oder der Leistung), dass Ihre App Livedaten auf der Entwurfsoberfläche von Microsoft Visual Studio oder Blend für Visual Studio anzeigt. Es gibt mehrere Möglichkeiten, Entwurfszeit-Beispieldaten zu verwenden, damit die Steuerelemente mit Daten aufgefüllt werden (sodass Sie das Layout, die Vorlagen und andere visuelle Eigenschaften der App bearbeiten können). Beispieldaten können auch hilfreich sein und Zeit sparen, wenn Sie eine App als Skizze (oder Prototyp) erstellen. Sie können zur Laufzeit Beispieldaten in der Skizze oder im Prototyp verwenden, um Ihre Ideen zu veranschaulichen, ohne echte Livedaten nutzen zu müssen. |
| [Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | Sie können eine Master/Details-Ansicht mit mehreren Ebenen (auch bekannt als Listen-Details-Ansicht) von hierarchischen Daten erstellen, indem Sie Elementsteuerelemente an [<strong>CollectionViewSource</strong>](https://msdn.microsoft.com/library/windows/apps/BR209833)-Instanzen binden, die in einer Kette verbunden sind. |




<!--HONumber=Jun16_HO4-->


