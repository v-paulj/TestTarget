---
author: payzer
title: So wird's gemacht - Deaktivieren des Overscans
description: 
area: Xbox
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# So wird's gemacht - Zeichnen der Benutzeroberfläche bis zum Bildschirmrand   
Standardmäßig werden Anwendungen an den Rändern des Viewports mit einem Rahmen versehen. Damit wird dem fernsehsicheren Bereich Rechnung getragen. Weitere Informationen finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area).  Wir empfehlen, diese Option zu deaktivieren und bis zum Bildschirmrand zu zeichnen. Wenn Sie bis zum Bildschirmrand zeichnen möchten, fügen Sie den folgenden Code hinzu, wenn die Anwendung gestartet wird:
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
Hinweis: Für C++-/DirectX-Anwendungen ist dies nicht relevant. Das System rendert die Anwendung immer bis zum Bildschirmrand.



<!--HONumber=Jun16_HO4-->


