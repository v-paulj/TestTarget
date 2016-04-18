---
Description: Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen.
title: Adaptive und interaktive Popupbenachrichtigungen
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive und interaktive Popupbenachrichtigungen
template: detail.hbs
---

# Adaptive und interaktive Popupbenachrichtigungen


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Mit adaptiven und interaktiven Popupbenachrichtigungen können Sie flexible Popupbenachrichtigungen mit mehr Inhalt, optionalen Inlinebildern und optionaler Benutzerinteraktion erstellen.

Beim adaptiven und interaktiven Popupbenachrichtigungsmodell haben diese Updates Vorrang vor dem Legacy-Popupvorlagenkatalog:

-   Die Option zum Einschließen von Schaltflächen und Eingaben in den Benachrichtigungen.
-   Drei verschiedene Aktivierungstypen für die wichtigste Popupbenachrichtigung und für jede Aktion.
-   Die Option zum Erstellen von Benachrichtigungen für bestimmte Szenarios wie Alarme, Erinnerungen und eingehende Anrufe.

**Hinweis**  Die Legacyvorlagen von Windows 8.1 und Windows Phone 8.1 finden Sie im [Legacy-Popupvorlagenkatalog](https://msdn.microsoft.com/library/windows/apps/hh761494).

 

## <span id="toast_structure"></span><span id="TOAST_STRUCTURE"></span>Struktur der Popupbenachrichtigung


Popupbenachrichtigungen werden mit XML erstellt und enthalten in der Regel diese wichtigen Elemente:

-   &lt;visual&gt; umfasst den Inhalt, den Benutzer visuell wahrnehmen können, z. B. Text und Bilder.
-   &lt;actions&gt; enthält Schaltflächen/Eingaben, die der Entwickler innerhalb der Benachrichtigung hinzufügen möchte.
-   &lt;audio&gt; legt den Ton beim Anzeigen der Benachrichtigung fest.

Hier sehen Sie ein Codebeispiel:

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Sample</text>
      <text>This is a simple toast notification example</text>
      <image placement="AppLogoOverride" src="oneAlarm.png" />
    </binding>
  </visual>
  <actions>
    <action content="check" arguments="check" imageUri="check.png" />
    <action content="cancel" arguments="cancel" />
  </actions>
  <audio src="ms-winsoundevent:Notification.Reminder"/>
</toast>
```

Und eine visuelle Darstellung der Struktur:

![Struktur der Popupbenachrichtigung](images/adaptivetoasts-structure.jpg)

### <span id="Visual"></span><span id="visual"></span><span id="VISUAL"></span>Visuelle Elemente

Innerhalb des visuellen Elements benötigen Sie exakt ein Bindungselement, das den visuellen Inhalt des Popups aufweist.

Kachelbenachrichtigungen in Apps für die Universelle Windows-Plattform (UWP) unterstützen mehrere Vorlagen, die auf unterschiedlichen Kachelgrößen basieren. Popupbenachrichtigungen haben jedoch nur einen Vorlagennamen: **ToastGeneric**. Mit nur einer Vorlage profitieren Sie mehrfach:

-   Sie können den Popupinhalt ändern, indem Sie z. B. eine weitere Textzeile bzw. ein Inlinebild hinzufügen oder die Anzeige der Miniaturansicht so ändern, dass statt des App-Symbols etwas anderes angezeigt wird. Dabei müssen Sie weder die gesamte Vorlage ändern noch eine ungültige Nutzlast aufgrund einer Nichtübereinstimmung zwischen dem Vorlagennamen und dem Inhalt riskieren.
-   Sie können denselben Code verwenden, um die gleiche Nutzlast für die **Popupbenachrichtigung** an verschiedene Arten von Microsoft Windows-Geräten wie Smartphones, Tablets, PCs und Xbox zu schaffen. Diese Geräte akzeptieren die Benachrichtigung und zeigt sie dem Benutzer gemäß ihren UI-Richtlinien mit den entsprechenden visuellen Angeboten und Interaktionsmodellen an.

Alle Attribute, die im Abschnitt „Visuelle Elemente“ und dessen untergeordneten Elemente unterstützt werden, finden Sie im Abschnitt mit den Schemas. Weitere Beispiele finden Sie unten im Abschnitt mit den XML-Beispielen.

### <span id="Actions"></span><span id="actions"></span><span id="ACTIONS"></span>Aktionen

In UWP-Apps können Sie Ihren Popupbenachrichtigungen Schaltflächen und andere Eingaben hinzufügen, um Benutzern mehr Aktionen außerhalb der App zu ermöglichen. Diese Aktionen werden unter dem Element &lt;actions&gt; angegeben. Davon können Sie zwei Arten angeben:

-   &lt;action&gt; wird als Schaltfläche auf Desktops und mobilen Geräten angezeigt. Sie können bis zu fünf benutzerdefinierte Aktionen oder Systemaktionen innerhalb einer Popupbenachrichtigung angeben.
-   &lt;input&gt; ermöglicht Benutzereingaben, z. B. schnelles Beantworten einer Nachricht oder Auswählen einer Option aus einem Dropdownmenü.

Sowohl &lt;action&gt; als auch &lt;input&gt; sind bei Windows-Geräten adaptiv. Bei mobilen Geräten oder Desktop-Geräten stellt &lt;action&gt; beispielsweise eine Schaltfläche dar, auf die Benutzer tippen/klicken können. &lt;input&gt; für Text ist ein Feld, in dem Benutzer über eine physische Tastatur oder eine Bildschirmtastatur Text eingeben können. Diese Elemente passen sich auch an zukünftige Interaktionsszenarios an, z. B. eine per Sprachansage angekündigte Aktion oder eine Texteingabe per Diktat.

Wenn vom Benutzer eine Aktion ausgeführt wird, können Sie eine der folgenden Aktionen ausführen, indem Sie das Attribut [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) im &lt;action&gt;-Element angeben:

-   Aktivieren der App im Vordergrund mit einem aktionsspezifischen Argument, das zum Navigieren zu einer bestimmten Seite bzw. einem bestimmten Kontext verwendet werden kann
-   Aktivieren der Hintergrundaufgabe der App ohne Auswirkung auf die Benutzer.
-   Aktivieren einer anderen App per Protokollstart.
-   Angeben einer auszuführenden Systemaktion. Die aktuell verfügbaren Systemaktionen sind die Funktionen zum erneuten Erinnern und zum Schließen geplanter Alarme/Erinnerungen. Auf diese wird in einem späteren Abschnitt eingegangen.

Alle Attribute, die im Abschnitt „Visuelle Elemente“ und dessen untergeordneten Elemente unterstützt werden, finden Sie im Abschnitt mit den Schemas. Weitere Beispiele finden Sie unten im Abschnitt mit den XML-Beispielen.

### <span id="Audio"></span><span id="audio"></span><span id="AUDIO"></span>Audio

Benutzerdefinierte Töne werden derzeit nicht von UWP-Apps für die Desktop-Plattform unterstützt. Stattdessen können Sie aus der Liste „ms-winsoundevents“ für Ihre App auf dem Desktop wählen. UWP-Apps auf mobilen Plattformen unterstützen sowohl „ms-winsoundevents“ als auch benutzerdefinierte Töne in den folgenden Formaten:

-   ms-appx:///
-   ms-appdata:///

Auf der [Seite mit den Audioschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie Informationen zu Tönen für Popupbenachrichtigungen, darunter die vollständige Liste „ms-winsoundevents“.

## <span id="Alarms__reminders__and_incoming_calls"></span><span id="alarms__reminders__and_incoming_calls"></span><span id="ALARMS__REMINDERS__AND_INCOMING_CALLS"></span>Alarme, Erinnerungen und eingehende Anrufe


Sie können Popupbenachrichtigungen für Alarme, Erinnerungen und eingehende Anrufe verwenden. Das Design dieser speziellen Popups stimmt mit dem von Standardpopups überein. Für einige benutzerdefinierte, szenariobasierte Benutzeroberflächen und Muster sind jedoch spezielle Popups möglich:

-   Eine Erinnerungs-Popupbenachrichtigung bleibt auf dem Bildschirm, bis der Benutzer sie schließt oder eine Aktion ausführt. Unter Windows Mobile können Erinnerungs-Popupbenachrichtigungen auch vorab vergrößert angezeigt werden.
-   Alarmbenachrichtigungen teilen nicht nur die oben genannten Verhaltensweisen mit Erinnerungsbenachrichtigungen, sondern können zudem automatisch Audioschleifen abspielen.
-   Benachrichtigungen über eingehende Anrufe werden auf Windows Mobile-Geräten im Vollbildmodus angezeigt. Dazu wird das Szenario-Attribut innerhalb des Stammelements einer Popupbenachrichtigung angegeben – &lt;toast&gt;:
    &lt;toast scenario=" { default | alarm | reminder | incomingCall } " &gt;

## <span id="xml_examples"></span><span id="XML_EXAMPLES"></span>XML-Beispiele


**Hinweis**  Die Screenshots für diese Beispiele zu Popupbenachrichtigungen stammen aus einer Desktop-App. Auf mobilen Geräten wird die Popupbenachrichtigung möglicherweise reduziert angezeigt. Über einen Ziehpunkt am unteren Rand des Popups kann sie vergrößert werden.

 

**Benachrichtigung mit umfassenden Visualisierungen**

Dieses Beispiel zeigt, wie Sie mehrere Textzeilen, ein optionales kleines Bild zum Außerkraftsetzen des Anwendungslogos und eine optionale Miniaturansicht für ein Inlinebild erstellen.

```XML
<toast launch="app-defined-string">
  <visual>
<binding template="ToastGeneric">
    <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image placement="appLogoOverride" src="A.png" />
    <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
</toast>
```

![Benachrichtigung mit umfassenden Visualisierungen](images/adaptivetoasts-xmlsample01.png)

 

**Benachrichtigung mit Aktionen, Beispiel 1**

Dieses Beispiel zeigt Folgendes:

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="see more details" arguments="details" imageUri="check.png"/>
    <action activationType="background" content="remind me later" arguments="later" imageUri="cancel.png"/>
  </actions>
</toast>
```

![Benachrichtigung mit Aktionen, Beispiel 1](images/adaptivetoasts-xmlsample02.png)

 

**Benachrichtigung mit Aktionen, Beispiel 2**

Dieses Beispiel zeigt Folgendes:

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Cortana</text>
      <text>We noticed that you are near Wasaki.</text>
      <text>Thomas left a 5 star rating after his last visit, do you want to try?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="reviews" arguments="reviews" />
    <action activationType="protocol" content="show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

![Benachrichtigung mit Aktionen, Beispiel 2](images/adaptivetoasts-xmlsample03.png)

 

**Benachrichtigung mit Texteingabe und Aktionen, Beispiel 1**

Dieses Beispiel zeigt Folgendes:

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" />
    <action activationType="foreground" content="video call" arguments="video" />
  </actions>
</toast>
```

![Benachrichtigung mit Texteingabe und Aktionen](images/adaptivetoasts-xmlsample04.png)

 

**Benachrichtigung mit Texteingabe und Aktionen, Beispiel 2**

Dieses Beispiel zeigt Folgendes:

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" imageUri="send.png" hint-inputId="message"/>
  </actions>
</toast>
```

![Benachrichtigung mit Texteingabe und Aktionen](images/adaptivetoasts-xmlsample05.png)

 

**Benachrichtigung mit Auswahleingabe und Aktionen**

Dieses Beispiel zeigt Folgendes:

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
  <selection id="1" content="Breakfast" />
  <selection id="2" content="Lunch" />
  <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="background" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

![Benachrichtigung mit Auswahleingabe und Aktionen](images/adaptivetoasts-xmlsample06.png)

 

**Erinnerungsbenachrichtigung**

Dieses Beispiel zeigt Folgendes:

```XML
<toast scenario="reminder" launch="developer-pre-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Adam&#39;s Hiking Camp</text>
      <text>You have an upcoming event for this Friday!</text>
      <text>RSVP before it"s too late.</text>
      <image placement="appLogoOverride" src="A.png" />
      <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="background" content="RSVP" arguments="rsvp" />
    <action activationType="background" content="Reminder me later" arguments="later" />
  </actions>
</toast>
```

![Erinnerungsbenachrichtigung](images/adaptivetoasts-xmlsample07.png)

 

## <span id="Activation_samples"></span><span id="activation_samples"></span><span id="ACTIVATION_SAMPLES"></span>Aktivierungsbeispiele


Wie oben erwähnt, können über den Text und die Aktionen des Popups Apps auf unterschiedliche Weise aktiviert werden. Das folgende Beispiel zeigt, wie Sie über den Popuptext und/oder Popupaktionen verschiedene Arten von Aktivierungen vornehmen.

**Vordergrund**

In diesem Szenario verwendet eine App eine Vordergrundaktivierung, um auf eine Aktion innerhalb einer relevanten Popupbenachrichtigung zu reagieren. Sie startet die App und navigiert zum richtigen Inhalt.

Die Aktivierung von Popupbenachrichtigungen wird zum Aufrufen von „OnLaunched()“ verwendet. In Windows 10 verfügt das Popup über eine eigene Aktivierungsart und ruft „OnActivated()“ auf.

```
async protected override void OnActivated(IActivatedEventArgs args)
{
        //Initialize your app if it&#39;s not yet initialized;
    //Find out if this is activated from a toast;
    If (args.Kind == ActivationKind.ToastNotification)
    {
                //Get the pre-defined arguments and user inputs from the eventargs;
        var toastArgs = args as ToastNotificationActivatedEventArgs;
        var arguments = toastArgs.Arguments;
        var input = toastArgs.UserInput["1"]; 
}
     
    //...
}
```

**Hintergrund**

In diesem Szenario verwendet eine App eine Hintergrundaufgabe, um eine Aktion innerhalb einer interaktiven Popupbenachrichtigung auszuführen. Mit dem folgenden Code wird gezeigt, wie Sie diese Hintergrundaufgabe für Popupaktivierungen innerhalb Ihres App-Manifests deklarieren und wie Sie Argumente aus der Aktion und aus Benutzereingaben abrufen, wenn auf die Schaltflächen geklickt wird.

```
<!-- Manifest Declaration -->
<!-- A new task type toastNotification is added -->
<Extension Category = "windows.backgroundTasks" 
EntryPoint = "Tasks.BackgroundTaskClass" >
  <BackgroundTasks>
    <Task Type="systemEvent" />
  </BackgroundTasks>
</Extension>
```

```
namespace ToastNotificationTask
{
    public sealed class ToastNotificationBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        //Inside here developer can retrieve and consume the pre-defined 
        //arguments and user inputs;
        var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
        var arguments = details.Arguments;
        var input = details.Input.Lookup("1");

            // ...
        }        
    }
}
```

## <span id="Schemas___visual__and__audio_"></span><span id="schemas___visual__and__audio_"></span><span id="SCHEMAS___VISUAL__AND__AUDIO_"></span>Schemas: &lt;visual&gt; und &lt;audio&gt;


In den folgenden Schemas bedeutet ein Suffix „?“, dass ein Attribut optional ist.

```
<toast launch? duration? activationType? scenario? >
    <visual version? lang? baseUri? addImageQuery? >
        <binding template? lang? baseUri? addImageQuery? >
            <text lang? >content</text>
            <text />
            <image src placement? alt? addImageQuery? hint-crop? />
        </binding>
    </visual>
    <audio src? loop? silent? />
    <actions>
    </actions>
</toast>
```

**Attribute in &lt;toast&gt;**

launch?

-   launch? = string
-   Dies ist ein optionales Attribut.
-   Eine Zeichenfolge wird an die Anwendung übergeben, wenn sie durch das Popup aktiviert wird.
-   Abhängig vom Wert für „activationType“ kann dieser Wert von der App im Vordergrund, innerhalb der Hintergrundaufgabe oder von einer anderen App empfangen werden, die per Protokollstart über die ursprüngliche App gestartet wird.
-   Das Format und der Inhalt dieser Zeichenfolge werden von der App für eigene Zwecke definiert.
-   Wenn der Benutzer zum Starten der zugeordneten App auf das Popup tippt oder klickt, stellt die Startzeichenfolge der App den Kontext bereit, um dem Benutzer eine Ansicht passend zum Popupinhalt zu ermöglichen, anstatt sie in der Standardgröße starten.
-   Ist die Aktivierung erfolgt, weil der Benutzer auf eine Aktion statt auf den Popuptext geklickt hat, ruft der Entwickler die vordefinierten „Argumente“ in diesem &lt;action&gt;-Tag zurück, anstatt das vordefinierte &lt;toast&gt; -Tag zu „starten“.

duration?

-   duration? = "short|long"
-   Dies ist ein optionales Attribut. Der Standardwert lautet „kurz“.
-   Es dient nur für bestimmte Szenarien und appCompat. Im Alarmszenario wird es nicht mehr benötigt.
-   Die Verwendung dieser Eigenschaft wird nicht empfohlen.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Dies ist ein optionales Attribut.
-   Der Standardwert lautet „foreground“

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Dies ist ein optionales Attribut, der Standardwert lautet „default“.
-   Sie benötigen es nur, wenn es sich bei Ihrem Szenario um einen Alarm, eine Erinnerung oder einen eingehenden Anruf handelt.
-   Verwenden Sie es nicht, um die Benachrichtigung dauerhaft auf dem Bildschirm anzuzeigen.

**Attribute in &lt;visual&gt;**

version?

-   version? = nonNegativeInteger
-   Dieses Attribut ist nicht erforderlich, da die Versionskontrolle bei &lt;visual&gt; veraltet ist. In Kürze gibt es ein neues Versionskontrolle-Modell, das Sie bei Bedarf aus einer höheren Hierarchie angeben können.

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

baseUri?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;binding&gt;**

template?

-   \[Important\] template? = "ToastGeneric"
-   Wenn Sie eines der neuen Features für adaptive und interaktive Benachrichtigungen verwenden, stellen Sie sicher, dass Sie mit der Vorlage „ToastGeneric“ statt mit der Legacyvorlage beginnen.
-   Möglicherweise können Sie die Legacyvorlagen mit den neuen Aktionen verwenden, aber da dies nicht der gewünschte Anwendungsfall ist, können wir keinen Erfolg garantieren.

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

baseUri?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;text&gt;**

lang?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230847) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

**Attribute in &lt;image&gt;**

src

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem erforderlichen Attribut.

placement?

-   placement? = "inline" | "appLogoOverride"
-   Dieses Attribut ist optional.
-   Es bestimmt, wo dieses Bild angezeigt wird.
-   „inline“ bedeutet innerhalb des Popuptextes, unter dem Text; „appLogoOverride“ steht für das Ersetzen des Anwendungssymbols (das in der oberen linken Ecke des Popups angezeigt wird).
-   Für jeden Platzierungswert ist maximal ein Bild möglich.

alt?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

addImageQuery?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230844) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

hint-crop?

-   hint-crop? = "none" | "circle"
-   Dieses Attribut ist optional.
-   „none“ ist der Standardwert, sodass kein Zuschneiden möglich ist.
-   „circle“ schneidet das Bild in Kreisform. Verwenden Sie diese Option für Profilbilder eines Kontakts, Bilder einer Person usw.

**Attribute in &lt;audio&gt;**

src?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

loop?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

silent?

-   In [diesem Artikel zu Elementschemas](https://msdn.microsoft.com/library/windows/apps/br230842) finden Sie ausführliche Informationen zu diesem optionalen Attribut.

## <span id="Schemas___action_"></span><span id="schemas___action_"></span><span id="SCHEMAS___ACTION_"></span>Schemas: &lt;action&gt;


In den folgenden Schemas bedeutet ein Suffix „?“, dass ein Attribut optional ist.

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions>
        <input id type title? placeHolderContent? defaultInput? >
            <selection id content />
        </input>
        <action content arguments activationType? imageUri? hint-inputId />
    </actions>
</toast>
```

**Attribute in &lt;input&gt;**

id

-   id = string
-   Dieses Attribut ist erforderlich.
-   Das id-Attribut ist erforderlich und wird von Entwicklern verwendet, um Eingaben des Benutzers abzurufen, sobald die App (im Vordergrund oder im Hintergrund) aktiviert ist.

type

-   type = "text | selection"
-   Dieses Attribut ist erforderlich.
-   Damit wird eine Texteingabe oder Eingabe aus einer Liste von vordefinierten Optionen angegeben.
-   Auf Mobilgeräten und Desktops wird damit angegeben, ob eine Eingabe per Textfeld oder per Listenfeld erfolgen soll.

title?

-   title? = string
-   Das Title-Attribut ist optional. Entwickler können damit einen Titel für die Eingabe festlegen, damit Shells gerendert werden können.
-   Auf Mobilgeräten und Desktops wird dieser Titel über der Eingabe angezeigt.

placeHolderContent?

-   placeHolderContent? = string
-   Das Attribut „placeHolderContent“ ist optional und der ausgegraute Hinweistext für den Texteingabetyp. Dieses Attribut wird ignoriert, wenn der Eingabetyp nicht „text“ lautet.

defaultInput?

-   defaultInput? = string
-   Das Attribut „defaultInput“ ist optional und wird verwendet, um einen Standardeingabewert bereitzustellen.
-   Beim Eingabetyp „text“ wird dieser als Zeichenfolgeneingabe behandelt.
-   Lautet der Eingabetyp „selection“, wird angenommen, dass dieser die ID einer der verfügbaren Auswahl in diesen Eingabeelementen ist.

**Attribute in &lt;selection&gt;**

id

-   Dieses Attribut ist erforderlich. Hiermit wird die Auswahl des Benutzers ermittelt. Die ID wird an Ihre App zurückgegeben.

content

-   Dieses Attribut ist erforderlich. Es enthält die Zeichenfolge, die für dieses Auswahlelement angezeigt werden soll.

**Attribute in &lt;action&gt;**

content

-   content = string
-   Das Inhaltsattribut ist erforderlich. Es enthält die Textzeichenfolge, die auf der Schaltfläche angezeigt wird.

arguments

-   arguments = string
-   Die Argument-Attribut ist erforderlich. Es beschreibt die von der App definierten Daten, die die App später abrufen kann, nachdem sie vom Benutzer durch diese Aktion aktiviert wird.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Das Attribut „activationType“ ist optional und der Standardwert lautet „foreground“.
-   Es beschreibt die Art der Aktivierung, die diese Aktion auslöst: im Vordergrund, im Hintergrund, Starten einer anderen App über Protokollstart oder Aufrufen einer Systemaktion.

imageUri?

-   imageUri? = string
-   ImageUri ist optional und wird verwendet, um ein Bildsymbol für diese Aktion bereitzustellen, welches auf der Schaltfläche im Textkontext angezeigt wird.

hint-inputId

-   hint-inputId = string
-   Das hint-inpudId-Attribut ist erforderlich. Es wird speziell für das schnelle Beantworten einer Nachricht verwendet.
-   Der Wert muss der ID entsprechen, die dem Eingabeelement zugeordnet werden soll.
-   Auf Mobilgeräten und Desktops wird die Schaltfläche rechts neben dem Eingabefeld platziert.

## <span id="Attributes_for_system-handled_actions"></span><span id="attributes_for_system-handled_actions"></span><span id="ATTRIBUTES_FOR_SYSTEM-HANDLED_ACTIONS"></span>Attribute für systemgesteuerte Aktionen


Das System kann Aktionen für die Funktionen zum erneuen Erinnern und zum Schließen von Benachrichtigungen auslösen, wenn Sie nicht wünschen, dass Ihre App das erneute Erinnern/Neuplanen von Benachrichtigungen im Hintergrund ausführt. Systemgesteuerte Aktionen können kombiniert (oder einzeln festgelegt) werden. Wir raten jedoch davon ab, eine erneute Erinnerung ohne Möglichkeit zum Schließen zu implementieren.

Kombinationsfeld für Systembefehle: SnoozeAndDismiss

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions hint-systemCommands? = "SnoozeAndDismiss" />
</toast>
```

Einzelne systemgesteuerte Aktionen

```
<toast>
    <visual>
    </visual>
    <audio />
<actions>
<input id="snoozeTime" type="selection" defaultInput="10">
  <selection id="5" content="5 minutes" />
  <selection id="10" content="10 minutes" />
  <selection id="20" content="20 minutes" />
  <selection id="30" content="30 minutes" />
  <selection id="60" content="1 hour" />
</input>
<action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
<action activationType="system" arguments="dismiss" content=""/>
</actions>
</toast>
```

Gehen Sie wie folgt vor, um individuelle Aktionen zum erneuten Erinnern und Schließen zu erstellen:

-   Legen Sie Folgendes fest: activationType = "system"
-   Legen Sie Argumente fest = "snooze" | "dismiss"
-   Legen Sie Inhalt fest:
    -   Wenn Sie wünschen, dass lokalisierte Zeichenfolgen für „snooze“ und „dismiss“ in den Aktionen angezeigt werden, legen Sie den Inhalt als leere Zeichenfolge fest: &lt;action content = "/&gt;
    -   Wenn Sie eine benutzerdefinierte Zeichenfolge wünschen, geben Sie seinen Wert an: &lt;action content="Remind me later" /&gt;
-   Legen Sie Eingaben fest:
    -   Wenn Sie nicht möchten, dass der Benutzer ein Intervall für das erneute Erinnern auswählen kann, sondern das erneute Erinnern an die Benachrichtigung nur einmal in einem vom System definierten (in allen Betriebssystemen einheitlichen) Zeitintervall erfolgt, legen Sie keinen Wert für &lt;input&gt; fest.
    -   Wenn Sie mögliche Intervalle für das erneute Erinnern bereitstellen möchten:
        -   Gegen Sie „hint-inputId„ in der Aktion für das erneute Erinnerung an.
        -   Stimmen Sie die ID der Eingabe auf den Wert für „hint-inputId“ der Aktion für das erneute Erinnern ab: &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Legen Sie für die Auswahl-ID eine positive ganze Zahl (nonNegativeInteger) fest, die dem Intervall für das erneute Erinnern in Minuten entspricht: &lt;selection id="240" /&gt; bedeutet, dass die erneute Erinnerung in vier Stunden erfolgt.
        -   Stellen Sie sicher, dass der Wert für „defaultInput“ in &lt;input&gt; einer der IDs der untergeordneten &lt;selection&gt;-Elemente entspricht.
        -   Sie können bis zu (aber nicht mehr als) 5 &lt;selection&gt;-Werte bereitstellen

 

 






<!--HONumber=Mar16_HO1-->


