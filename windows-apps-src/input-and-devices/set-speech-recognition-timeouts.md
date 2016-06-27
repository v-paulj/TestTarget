---
author: Karl-Bridge-Microsoft
Description: "Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet."
title: "Festlegen von Timeouts für die Spracherkennung"
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: e0dbc43cba4556674a4d96013b65837d95044622

---

# Festlegen von Timeouts für die Spracherkennung
Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.

**Wichtige APIs**

-   [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## <span id="Set_a_timeout"></span><span id="set_a_timeout"></span><span id="SET_A_TIMEOUT"></span>Festlegen eines Timeouts


Hier geben wir verschiedene [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)-Werte an:

-   InitialSilenceTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul Stille erkennt (vor Generierung etwaiger Erkennungsergebnisse) und davon ausgeht, dass keine Spracheingabe erfolgen wird.
-   BabbleTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul weiterhin auf erkennbare Geräusche (Störgeräusche) wartet, bevor davon ausgegangen wird, dass die Spracheingabe beendet ist, und der Erkennungsvorgangs beendet wird.
-   EndSilenceTimeout – Die Zeitspanne, für die das Spracherkennungsmodul Stille erkennt (nach Generierung von Erkennungsergebnissen) und davon ausgeht, dass die Spracheingabe beendet ist.

**Hinweis**  Timeouts können pro Erkennungsmodul festgelegt werden.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <span id="related_topics"></span>Verwandte Artikel


* [Interaktionen mit der Spracherkennung](speech-interactions.md)
            
          
            **Beispiele**
* [Beispiel zu Spracherkennung und Sprachsynthese](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 







<!--HONumber=Jun16_HO3-->


