---
Description: Description: Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.
title: title: Festlegen von Timeouts für die Spracherkennung
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
---

# ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Timeouts für die Spracherkennung

template: detail.hbs Festlegen von Timeouts für die Spracherkennung


**Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.**

-   [**\[ Aktualisiert für UWP-Apps unter Windows 10.**](https://msdn.microsoft.com/library/windows/apps/dn653253)
-   [**Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]**](https://msdn.microsoft.com/library/windows/apps/dn653230)


## Wichtige APIs


Timeouts

-   SpeechRecognizerTimeouts
-   <span id="Set_a_timeout"></span><span id="set_a_timeout"></span><span id="SET_A_TIMEOUT"></span>Festlegen eines Timeouts
-   Hier geben wir verschiedene [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253)-Werte an:

InitialSilenceTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul Stille erkennt (vor Generierung etwaiger Erkennungsergebnisse) und davon ausgeht, dass keine Spracheingabe erfolgen wird.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## BabbleTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul weiterhin auf erkennbare Geräusche (Störgeräusche) wartet, bevor davon ausgegangen wird, dass die Spracheingabe beendet ist, und der Erkennungsvorgangs beendet wird.


* [EndSilenceTimeout – Die Zeitspanne, für die das Spracherkennungsmodul Stille erkennt (nach Generierung von Erkennungsergebnissen) und davon ausgeht, dass die Spracheingabe beendet ist.](speech-interactions.md)
****Hinweis**  Timeouts können pro Erkennungsmodul festgelegt werden.**
* [<span id="related_topics"></span>Verwandte Artikel](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 






<!--HONumber=Apr16_HO3-->

