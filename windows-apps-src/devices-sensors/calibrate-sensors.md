---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Kalibrieren von Sensoren
description: Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein.
---
# Kalibrieren von Sensoren

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

** Wichtige APIs **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

Für Sensoren in einem auf dem Magnetometer – Kompass, Neigungsmesser und Ausrichtungssensor – basierenden Gerät kann aufgrund von Umweltfaktoren eine Kalibrierung erforderlich sein. Falls für Ihr Gerät eine Kalibrierung erforderlich ist, kann die [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552)-Aufzählung helfen, die nächsten Schritte festzulegen.

## Zeitpunkt für die Kalibrierung des Magnetometers

Die [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552)-Aufzählung bietet vier Werte, mit deren Hilfe Sie bestimmen können, ob das Gerät, auf dem Ihre App ausgeführt wird, kalibriert werden muss. Wenn ein Gerät kalibriert werden muss, sollten Sie den Benutzer über die Notwendigkeit einer Kalibrierung informieren. Fordern Sie den Benutzer jedoch nicht zu häufig auf, eine Kalibrierung durchzuführen. Dies sollte nicht öfter als einmal alle 10 Minuten erfolgen.

| Wert           | Beschreibung                                                                                                                                                      |-----------------|-------------------|                                                                                                                                              | **Unknown**     | Der Sensortreiber konnte die aktuelle Genauigkeit nicht ermitteln. Dies bedeutet nicht notwendigerweise, dass das Gerät falsch kalibriert ist. Es ist Aufgabe Ihrer App, die geeigneten Schritte festzulegen, wenn **Unknown** zurückgegeben wird. Falls Ihre App von exakten Sensorwerten abhängig ist, sollten Sie den Benutzer auffordern, das Gerät zu kalibrieren. |
| **Unreliable**  | Das Magnetometer ist aktuell hochgradig ungenau. Wenn dieser Wert zuerst zurückgegeben wird, sollten Apps immer zu einer Kalibrierung durch den Benutzer auffordern. |
| **Approximate** | Die Daten sind für bestimmt Anwendungen genau genug. Eine Virtual-Reality-App, die lediglich wissen muss, ob der Benutzer das Gerät nach oben/unten oder links/rechts bewegt hat, kann ohne Kalibrierung fortgesetzt werden. Apps, die einen absoluten Kurs benötigen, z. B. eine Navigations-App, die wissen muss, in welche Richtung Sie fahren, um Sie zu führen, müssen eine Kalibrierung anfordern. |
| **High**        | Die Daten sind genau. Es ist keine Kalibrierung erforderlich. Dies gilt auch für Apps, die einen absoluten Kurs benötigen, wie Augmented-Reality- oder Navigations-Apps. |

## So wird's gemacht: Kalibrieren des Magnetometers

Dieses kurze Video enthält eine Übersicht zu den Kalibrierungsverfahren für das Magnetometer.<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">One Dev Minute – Sensorkalibrierung</iframe>





<!--HONumber=Mar16_HO1-->


