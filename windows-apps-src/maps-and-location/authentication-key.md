---
title: Anfordern eines Kartenauthentifizierungsschlüssels
description: Ihre universelle Windows-App muss authentifiziert werden, bevor sie die MapControl-Klasse und Kartendienste im Windows.Services.Maps-Namespace verwenden kann.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
---

# Anfordern eines Kartenauthentifizierungsschlüssels


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]


Ihre [universelle Windows-App](https://msdn.microsoft.com/library/windows/apps/dn894631) muss authentifiziert werden, bevor sie die [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse und Kartendienste im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Thema wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel vom [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen.

**Tipp** Um mehr über das Verwenden von Karten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die Universelle Windows-Plattform](http://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter:

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Abrufen eines Schlüssels


Erstellen und verwalten Sie Kartenauthentifizierungsschlüssel für Ihre universellen Windows-Apps im [Bing Maps Developer Center](https://www.bingmapsportal.com/).

So erstellen Sie einen neuen Schlüssel

1.  Navigieren Sie in Ihrem Browser zum Bing Maps Developer Center ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Wenn Sie zum Anmelden aufgefordert werden, geben Sie Ihre Microsoft-Kontoinformationen ein, und klicken Sie auf **Anmelden**.

3.  Wählen Sie das Konto, das mit Ihrem Bing Karten-Konto verknüpft werden soll. Wenn Sie Ihr Microsoft-Konto verwenden möchten, klicken Sie auf **Ja**. Andernfalls klicken Sie auf die Option zum **Anmelden mit einem anderen Konto**.

4.  Wenn Sie noch kein Bing Karten-Konto besitzen, erstellen Sie ein neues Bing Karten-Konto. Geben Sie **Kontoname**, **Kontaktname**, **Firmenname**, **E-Mail-Adresse** und **Telefonnummer** ein. Akzeptieren Sie die Nutzungsbedingungen, und klicken Sie auf **Erstellen**.

5.  Klicken Sie unter dem Menü **Mein Konto** auf **Create or view keys** (Schlüssel erstellen oder anzeigen).

6.  Klicken Sie auf den Link zum **Erstellen eines neuen Schlüssels**.

7.  Füllen Sie das Formular **Schlüssel erstellen** aus, und klicken Sie dann auf **Erstellen**.

    -   **Anwendungsname:** Der Name Ihrer Anwendung.
    -   **Anwendungs-URL (optional):** Die URL Ihrer Anwendung.
    -   **Schlüsseltyp:** Wählen Sie **Basic** oder **Enterprise** aus.
    -   **Anwendungstyp:** Wählen Sie **Universelle Windows-App** für die Verwendung in Ihrer universellen Windows-App aus.

    So sieht das Formular aus.

    ![Beispiel des Formulars „Schlüssel erstellen“.](images/createkeydialog.png)

8.  Nachdem Sie auf **Erstellen** geklickt haben, wird der neue Schlüssel unterhalb des Formulars **Schlüssel erstellen** angezeigt. Kopieren Sie ihn an einen sicheren Ort, oder fügen Sie ihn sofort wie im nächsten Schritt beschrieben Ihrer App hinzu.

## Hinzufügen des Schlüssels zur App


Der Kartenauthentifizierungsschlüssel ist erforderlich, um die [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse und Kartendienste ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)) in Ihrer universellen Windows-App zu verwenden. Fügen Sie ihn ggf. dem Kartensteuerelement und Kartendienstobjekten hinzu.

### So fügen Sie den Schlüssel einem Kartensteuerelement hinzu

Setzen Sie zum Authentifizieren der [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004)-Klasse die [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Sie können diese Eigenschaft je nach Ihren Einstellungen im Code oder im XAML-Markup festlegen. Weitere Informationen zur Verwendung der **MapControl**-Klasse finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im XAML-Markup gesetzt.

    ```xaml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### So fügen Sie den Schlüssel Kartendiensten hinzu

Um Dienste im [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)-Namespace zu verwenden, setzen Sie die [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Weitere Informationen zur Verwendung von Kartendiensten finden Sie unter [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md) und [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md).

-   In diesem Beispiel wird die **ServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## Verwandte Themen

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Beispiel zu UWP-Karten](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Richtlinien für Karten](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](http://go.microsoft.com/fwlink/p/?LinkId=619982)




<!--HONumber=Mar16_HO1-->


