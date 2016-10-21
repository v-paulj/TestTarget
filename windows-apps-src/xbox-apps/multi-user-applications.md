---
author: Mtoepke
title: "Einführung in Anwendungen mit mehreren Benutzern"
description: "Eine einfache allgemeine Einführung in das Xbox-Mehrbenutzermodell."
translationtype: Human Translation
ms.sourcegitcommit: 098683ae1ad594eee3cb3b197431ddfa3998a30d
ms.openlocfilehash: e9696cb20fa53b2fb5fe7f2cb06d4e5585639faf

---

# Einführung in Anwendungen mit mehreren Benutzern

Dieses Thema dient als einfache allgemeine Einführung in das Xbox-Mehrbenutzermodell.

> [!NOTE]
> Mehrbenutzeranwendungen werden zurzeit nicht unterstützt. 

Das Xbox One-Benutzermodell erfüllt die Anforderungen einer Spielkonsole, auf der mehrere Benutzer gemeinsam auf einem einzigen Gerät spielen können. Sie bietet mehreren Benutzer die Möglichkeit, sich mit je einem eigenen Controller gleichzeitig in einer interaktiven Sitzung auf der Konsole anzumelden. Dies unterscheidet sich von anderen Windows-Geräten. Beispiel:
* **Windows-Desktop-PCs** bieten mehreren Benutzern die Möglichkeit, dasselbe Gerät zu verwenden. Dabei hat jedoch jeder Benutzer eine eigene interaktive Sitzung, die von den anderen Sitzungen auf dem Gerät unabhängig ist.
* **Windows-Telefone** bieten nur einem einzelnen Benutzer die Möglichkeit, das Gerät zu verwenden. Dieser Benutzer wird während auf der Windows-Willkommensseite festgelegt. Er kann sich nach der Anmeldung nicht wieder abmelden. Wenn ein anderer Benutzer das Gerät verwenden möchte, muss dieses sogar zurückgesetzt werden. 
* **Xbox One** ermöglicht die zeitgleiche Anmeldung mehrerer Benutzer, die das Gerät gemeinsam in einer interaktiven Sitzung nutzen.

Im Xbox One-Benutzermodell wird für jeden Benutzer ein lokales Benutzerkonto angelegt. Diese lokale Benutzerkonto ist mit einem Xbox Live-Konto (und somit einem Microsoft-Konto) verknüpft. Dadurch ergibt sich eine strenge 1: 1-Zuordnung eines Xbox-Benutzerkontos zu einem Xbox Live-Konto sowie zu einem Microsoft-Konto.

## Einzelbenutzeranwendungen
Standardmäßig werden Apps für die universelle Windows-Plattform (UWP) im Kontext des Benutzers ausgeführt, der die Anwendung gestartet hat. Diese *Einzelbenutzeranwendungen* (Single User Applications, SUAs) erkennen nur diesen Benutzer und werden in einem Modus ausgeführt, der mit dem Benutzermodell auf anderen Windows-Geräten kompatibel ist. Das Xbox-Benutzermodell verwaltet, welcher Benutzer mit der App verknüpft ist. Zudem stellt es sicher, dass beim Start der App ein Benutzer angemeldet ist. In diesem Modell müssen Autoren von UWP-Apps und Spielen nichts Spezielles für die Ausführung auf der Xbox unternehmen. 

## Mehrbenutzeranwendungen
UWP-Spiele können das Xbox One-Mehrbenutzermodell wählen. Diese *Mehrbenutzeranwendungen* (Multi-User Applications, MUAs) werden im Kontext eines Systemkontos (des sogenannten Standardkontos) ausgeführt. Sie können alle Vorteile der Flexibilität und Leistungsfähigkeit des Xbox One-Benutzermodells nutzen. Bei diesen Spielen verwaltet das Xbox-Benutzermodell nicht, welcher Benutzer mit dem Spiel verknüpft ist. Zudem muss beim Start des Spiels kein Benutzer angemeldet sein. Dies bedeutet, dass sie so geschrieben werden müssen, dass sie sich der Benutzeranforderungen explizit bewusst sind und diese verwalten. Hierzu zählt unter anderem, ob ein Benutzer angemeldet sein muss, ob das Konzept des angemeldeten Benutzers berücksichtigt wird und ob die gleichzeitige Eingabe durch mehrere Benutzer zulässig ist.
   
So wählen Sie das Mehrbenutzermodell:   
1. Öffnen Sie das Projekt in Visual Studio.   
2. Wählen Sie die Datei „package.appxmanifest.xml“ aus.   
3. Klicken Sie mit der rechten Maustaste, und wählen Sie **Code anzeigen** aus.   
4. Fügen Sie im Abschnitt `<Properties></Properties>` folgende Zeile hinzu:

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### Identifizieren von Benutzern und Eingaben
Entwickler können KeyRoutedEventArgs.DeviceId verwenden, das von KeyUp- und KeyDown-Routingereignissen verwendet wird, um die aus unterschiedlichen Eingaben generierten Ereignisse zu unterscheiden.
Mithilfe der Methode Windows.System.UserDeviceAssociation.FindUserFromDeviceId können Sie den Benutzer identifizieren, der mit einer bestimmten Eingaben verknüpft ist.

Weitere Informationen finden Sie im Thema [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid).


## Leitfaden zur Modellauswahl
Alle UWP-Apps und die Mehrzahl der Einzelbenutzerspiele können als Einzelbenutzeranwendungen (SUAs) geschrieben werden. Vorzugsweise sollten nur kooperative Multiplayerspiele das Xbox One-Mehrbenutzermodell auswählen können.

## Siehe auch
- [UWP auf XboxOne](index.md)



<!--HONumber=Aug16_HO3-->


