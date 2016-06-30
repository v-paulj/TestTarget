---
author: mcleanbyron
Description: "Sie können Ihre Kunden dazu ermutigen, Feedback zu geben, indem Sie den Feedback-Hub über Ihre App starten."
title: "Starten des Feedback-Hubs über Ihre App"
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
translationtype: Human Translation
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: ccda01d9bfa4ffdff2bbce5d6c60c78e026270e5

---

# Starten des Feedback-Hubs über Ihre App

Sie können Ihre Kunden dazu ermutigen, Feedback zu geben, indem Sie ein Steuerelement (wie beispielsweise eine Schaltfläche) zu Ihrer UWP-App (Universelle Windows-Plattform) hinzufügen, durch das der Feedback-Hub gestartet wird. Beim Feedback-Hub handelt es sich um eine vorinstallierte App, in der an einem zentralen Ort Feedback zu Windows und den installierten Apps gesammelt werden kann. Das gesamte über den Feedback-Hub eingereichte Kundenfeedback für Ihre App wird gesammelt und Ihnen im [Feedbackbericht](../publish/feedback-report.md) im Windows Dev Center-Dashboard angezeigt, sodass Sie sich die Probleme, Vorschläge und Upvotes ansehen können, die Ihre Kunden in einem Bericht übermittelt haben.

>**Hinweis** Der **Feedback**-Bericht ist zurzeit nur für Entwicklerkonten verfügbar, die Mitglied des [Dev Center-Insider-Programms](../publish/dev-center-insider-program.md) sind. 

Um den Feedback-Hub über Ihre App zu starten, verwenden Sie eine API, die vom [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk) bereitgestellt wird. Wir empfehlen die Verwendung dieser API zum Starten des Feedback-Hubs über ein Benutzeroberflächenelement in Ihrer App, das unseren Designrichtlinien entspricht.

>**Hinweis** Der Feedback-Hub ist nur auf Geräten verfügbar, auf denen Windows 10, Version 10.0.14271 oder höher ausgeführt wird. Wir empfehlen Ihnen, ein Feedback-Steuerelement in Ihrer App nur dann einzublenden, wenn der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Mithilfe des Codes in diesem Thema wird die Vorgehensweise veranschaulicht.

## So starten Sie den Feedback-Hub über Ihre App

So starten Sie den Feedback-Hub über Ihre App:

1. Installieren Sie das [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk). Neben der API zum Starten des Feedback-Hubs bietet dieses SDK auch APIs für andere Features, wie beispielsweise das Durchführen von Experimenten in Ihren Apps mit A/B-Tests und das Einblenden von Anzeigen. Weitere Informationen zu diesem SDK finden Sie unter [Monetarisierung Ihrer App und Stärkung der Kundenbindung mit dem Microsoft Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).
2. Öffnen Sie das Projekt in Visual Studio.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verweise** für Ihr Projekt, und wählen Sie anschließend **Verweis hinzufügen** aus.
4. Erweitern Sie im **Verweis-Manager** die Option **Universelle Windows-App**, und klicken Sie auf **Erweiterungen**.
5. Klicken Sie in der Liste der SDKs auf das Kontrollkästchen neben **Microsoft Store Engagement SDK** und anschließend auf **OK**.
6. Fügen Sie in Ihrem Projekt das Steuerelement hinzu, das Sie für Benutzer anzeigen lassen möchten, um den Feedback-Hub zu starten, z. B. eine Schaltfläche. Wir empfehlen Ihnen folgende Konfiguration für das Steuerelement:
  * Legen Sie die Schriftart der Inhalte im Steuerelement auf **Segoe MDL2 Assets** fest.
  * Legen Sie den Text im Steuerelement auf das hexadezimale Unicode-Zeichen E939 fest. Dabei handelt es sich um den Zeichencode für das empfohlene Feedbacksymbol in der Schriftart **Segoe MDL2 Assets**.
  * Legen Sie bezüglich der Sichtbarkeit des Steuerelements fest, dass es ausgeblendet wird.

    > **Hinweis**  Der Feedback-Hub ist nur auf Geräten verfügbar, auf denen Windows 10, Version 10.0.14271 oder höher ausgeführt wird. Wir empfehlen Ihnen, das Feedback-Steuerelement standardmäßig auszublenden und es in Ihrem Initialisierungscode nur dann anzuzeigen, wenn der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Im nächsten Schritt wird die Vorgehensweise erläutert.

  Der folgende Code veranschaulicht die XAML-Definition einer [Schaltfläche](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), die wie oben beschrieben konfiguriert ist.
  ```xml
  <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
  ```
7. Verwenden Sie im Initialisierungscode der App-Seite, auf der Ihr Feedback-Steuerelement gehostet wird, die [IsSupported](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.issupported.aspx)-Eigenschaft der [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx)-Klasse, um zu ermitteln, ob der Feedback-Hub auf dem Gerät des Benutzers verfügbar ist. Wenn diese Eigenschaft **true** zurückgibt, legen Sie fest, dass das Steuerelement sichtbar wird. Der folgende Code veranschaulicht die Vorgehensweise für eine [Schaltfläche](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx).
```CSharp
if (Microsoft.Services.Store.Engagement.Feedback.IsSupported)
{
        this.feedbackButton.Visibility = Visibility.Visible;
}
```
8. Rufen Sie im Ereignishandler, der ausgeführt wird, wenn der Benutzer auf das Steuerelement klickt, die statische [LaunchFeedbackAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.launchfeedbackasync.aspx)-Methode der [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx)-Klasse auf, um die Feedback-Hub-App zu starten. Es gibt zwei Überladungen für diese Methode: eine ohne Parameter und eine weitere, die ein Wörterbuch mit Schlüssel-Wert-Paaren akzeptiert, die wiederum Metadaten enthalten, die Sie mit dem Feedback verknüpfen möchten. Im folgenden Beispiel wird veranschaulicht, wie Sie den Feedback-Hub im [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)-Ereignishandler für eine [Schaltfläche](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) starten.
```CSharp
private async void feedbackButton_Click(object sender, RoutedEventArgs e)
{
        await Microsoft.Services.Store.Engagement.Feedback.LaunchFeedbackAsync();
}
```

## Designempfehlungen für Ihre Feedback-Benutzeroberfläche

Um den Feedback-Hub zu starten, empfehlen wir, in Ihrer App ein Benutzeroberflächenelement (z. B. eine Schaltfläche) hinzuzufügen, in dem das folgende standardmäßige Feedbacksymbol mit der Schriftart „Segoe MDL2 Assets“ und dem Zeichencode E939 angezeigt wird.

![]Feedback icon](images/feedback_icon.PNG)

Außerdem wird empfohlen, mindestens eine der folgenden Platzierungsoptionen für die Verknüpfung zum Feedback-Hub in Ihrer App zu verwenden.
* **Direkt in der App-Leiste** Je nach Implementierung möchten Sie möglicherweise nur das Symbol verwenden oder Text hinzufügen (siehe unten).

  ![]Feedbacksymbol](images/feedback_appbar_placement.png)

* **In den Einstellungen Ihrer App** Dies ist eine subtilere Art, um Benutzern Zugriff auf den Feedback-Hub zu gewähren. Im folgenden Beispiel wird der Feedbacklink als einer der Links unter der App angezeigt.

  ![]Feedbacksymbol](images/feedback_settings_placement.png)

* **In einem ereignisgesteuerten Flyout** Dies ist hilfreich, wenn Sie vor dem Start des Windows-Feedback-Hubs die Meinung Ihrer Kunden zu einer bestimmten Frage erfahren möchten. Beispiel: Nachdem Ihre App eine bestimmte Funktion verwendet, könnten Sie den Kunden mit einer gezielten Frage zu seiner Zufriedenheit mit diesem Feature dazu auffordern, Ihnen seine Meinung mitzuteilen. Wenn der Kunde auf die Frage reagieren möchte, wird über Ihre App der Feedback-Hub gestartet.


## Verwandte Themen

* [Feedbackbericht](../publish/feedback-report.md)



<!--HONumber=Jun16_HO4-->


