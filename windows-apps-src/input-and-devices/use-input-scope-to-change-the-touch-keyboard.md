---
author: Karl-Bridge-Microsoft
Description: "Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird."
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur"
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 02f06ee498b136f811b4b3b8080a9cb043693504

---

# Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird.

**Wichtige APIs**

-   [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632)
-   [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)



Die Bildschirmtastatur kann für die Texteingabe verwendet werden, wenn Ihre App auf einem Gerät mit Touchscreen ausgeführt wird. Die Bildschirmtastatur wird aufgerufen, wenn der Benutzer auf ein bearbeitbares Eingabefeld tippt (etwa auf ein [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)- oder [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548)-Element). Benutzer können Daten in Ihrer App schneller und komfortabler eingeben, wenn Sie den *Eingabeumfang* des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Der Eingabeumfang bietet dem System einen Hinweis auf die Art von Text, die vermutlich über das Steuerelement eingegeben wird. Auf diese Weise kann das System ein spezielles Bildschirmtastaturlayout für den Eingabetyp bereitstellen.

Wird ein Textfeld beispielsweise nur verwendet, um eine vierstellige PIN einzugeben, legen Sie die Eigenschaft [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) auf **Number** fest. Dadurch wird das System angewiesen, die Zehnertastatur anzuzeigen, was dem Benutzer die Eingabe der PIN erleichtert.

> **Wichtig**&nbsp;&nbsp;
- Diese Informationen gelten nur für das SIP. Sie gelten nicht für Hardwaretastaturen oder die Bildschirmtastatur, die in den Windows-Optionen für erleichterte Bedienung verfügbar ist.
- Durch diesen Eingabeumfang wird keine Eingabeüberprüfung durchgeführt, und der Benutzer kann Eingaben über eine Hardwaretastatur oder ein anderes Eingabegerät vornehmen. Die Benutzereingabe muss je nach Bedarf trotzdem in Ihrem Code überprüft werden.

## Ändern des Eingabeumfangs eines Textsteuerelements

Die in einer App verfügbaren Eingabeumfangoptionen sind Member der [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)-Enumeration. Sie können die **InputScope**-Eigenschaft eines [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)- oder [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548)-Elements auf einen dieser Werte festlegen.

> **Wichtig**
            &nbsp;&nbsp;Die [**InputScope**](https://msdn.microsoft.com/library/windows/apps/dn996570)-Eigenschaft für [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) unterstützt nur die Werte **Password** und **NumericPin** values. Andere Werte werden ignoriert.

In diesem Verfahren ändern Sie den Eingabeumfang von mehreren Textfeldern, um ihn auf die erwarteten Daten für jedes Textfeld abzustimmen.

**Ändern des Eingabeumfangs in XAML**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten.
2.  Fügen Sie dem Tag das [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632)-Attribut hinzu, und geben Sie den [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)-Wert für die erwartete Eingabe an.

    Im Anschluss sehen Sie einige Textfelder, wie sie etwa in einem allgemeinen Kundenkontaktformular enthalten sein können. Durch Festlegen von [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) wird für jedes Textfeld eine Bildschirmtastatur mit geeigneten Layout für die jeweiligen Daten angezeigt.

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**Ändern des Eingabeumfangs im Code**

1.  Suchen Sie in der XAML-Datei für Ihre Seite das Tag für das Textsteuerelement, das Sie ändern möchten. Ist es nicht festgelegt, legen Sie das [x:Name-Attribut](https://msdn.microsoft.com/library/windows/apps/mt204788) fest, um im Code auf das Steuerelement verweisen zu können.

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  Instanziieren Sie ein neues [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025)-Objekt.

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  Instanziieren Sie ein neues [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027)-Objekt.
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  Legen Sie die [**NameValue**](https://msdn.microsoft.com/library/windows/apps/hh702032)-Eigenschaft des [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027)-Objekts auf den Wert der [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)-Enumeration fest.

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  Fügen Sie das [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027)-Objekt zur [**Names**](https://msdn.microsoft.com/library/windows/apps/hh702034)-Sammlung des [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025)-Objekts hinzu.

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  Legen Sie das [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025)-Objekt als Wert für die [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632)-Eigenschaft des Textsteuerelements fest.

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

Hier sehen Sie den vollständigen Code:

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

Die gleichen Schritte können zu folgendem Code zusammengefasst werden:

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## Textvorhersage, Rechtschreibprüfung und Autokorrektur

Die Steuerelemente [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) und [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) verfügen über mehrere Eigenschaften, die das Verhalten des SIP beeinflussen. Sie müssen wissen, wie sich diese Eigenschaften auf die touchbasierte Texteingabe auswirken, um Ihren Benutzer die bestmögliche Benutzererfahrung bieten zu können.

-   [
              **IsSpellCheckEnabled**
            ](https://msdn.microsoft.com/library/windows/apps/br209688): Wenn die Rechtschreibprüfung für ein Textsteuerelement aktiviert ist, interagiert das Steuerelement mit der Rechtschreibprüfung des Systems, um nicht erkannte Wörter zu markieren. Durch Tippen auf ein Wort können Sie eine Liste mit Korrekturvorschlägen anzeigen. Die Rechtschreibprüfung ist standardmäßig aktiviert.

    Bei Verwendung des Eingabeumfangs **Default** ermöglicht diese Eigenschaft auch die automatische Großschreibung des ersten Worts in einem Satz sowie eine Autokorrektur während der Eingabe. Diese Features für die automatische Korrektur sind in anderen Eingabeumfängen möglicherweise deaktiviert. Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

-   [
              **IsTextPredictionEnabled**
            ](https://msdn.microsoft.com/library/windows/apps/br209690): Wenn die Textvorhersage für ein Textsteuerelement aktiviert ist, zeigt das System eine Liste mit Wörtern an, die Sie vermutlich eingeben möchten. Sie können aus der Liste auswählen, sodass Sie nicht das gesamte Wort eingeben müssen. Die Textvorhersage ist standardmäßig aktiviert.

    Die Textvorhersage ist möglicherweise deaktiviert, wenn der Eingabeumfang nicht auf **Default** festgelegt ist (auch wenn die [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690)-Eigenschaft auf **true** festgelegt ist). Weitere Informationen finden Sie in den Tabellen weiter unten in diesem Thema.

    **Hinweis:**
            &nbsp;&nbsp; Auf Mobilgeräten werden Textvorhersage und Rechtschreibkorrektur im SIP-Bereich über der Bildschirmtastatur angezeigt. Wenn [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) auf **false** festgelegt ist, wird dieser Teil des SIP ausgeblendet und die Autokorrektur deaktiviert (auch [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688) auf **true** festgelegt ist).

-   [
              **PreventKeyboardDisplayOnProgrammaticFocus**
            ](https://msdn.microsoft.com/library/windows/apps/dn299273): Ist diese Eigenschaft auf **true** festgelegt, verhindert sie die Anzeige des SIP, wenn der Fokus programmgesteuert auf ein Textsteuerelement festgelegt wird. Stattdessen wird die Tastatur nur angezeigt, wenn der Benutzer mit dem Steuerelement interagiert.

## Tippen Sie auf den Bildschirmtastaturindex für Windows und Windows Phone

Diese Tabellen zeigen die Layouts des Soft Input Panel (SIP) auf Desktops und Mobilgeräten für allgemeine Eingabeumfangswerte. Die Auswirkungen des Eingabeumfangs auf die durch die Eigenschaften **IsSpellCheckEnabled** und **IsTextPredictionEnabled** aktivierten Features werden für jeden Eingabeumfang aufgeführt. Dies ist keine vollständige Liste der verfügbaren Eingabeumfangoptionen.

> **Hinweis:**
            &nbsp;&nbsp; Bei der kleineren Variante des SIP auf Mobilgeräten ist es für mobile Apps besonders wichtig, den richtigen Eingabeumfang festzulegen. Wie nachfolgend gezeigt, bietet Windows Phone eine größere Auswahl an speziellen Tastaturlayouts. Ein Textfeld, dessen Eingabeumfang nicht in einer Windows Store-App festgelegt werden muss, profitiert möglicherweise davon, wenn es in einer Windows Phone Store-App festgelegt wird.

> **Tipp:**
            &nbsp;&nbsp; Bei den meisten Bildschirmtastaturen ist der Wechsel zwischen einem Layout mit Buchstaben sowie einem Layout mit Zahlen und Sonderzeichen möglich. In Windows schalten Sie über die **&123**-Taste um. Drücken Sie auf einem Windows Phone die Taste **&123**, um zum Layout mit Ziffern und Sonderzeichen zu wechseln, oder die Taste **abcd**, um zum alphabetischen Layout zu gelangen.

### Standard

`<TextBox InputScope="Default"/>`

Die Standardtastatur.

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/kbdpcdefault.png) | ![Standardmäßige Windows Phone-Bildschirmtastatur](images/input-scopes/kbdwpdefault.png) |

Verfügbarkeit von Features:

-   Rechtschreibprüfung: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
-   Autokorrektur: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
-   Automatische Großschreibung: Aktiviert, wenn **IsSpellCheckEnabled** = **true**. Deaktiviert, wenn **IsSpellCheckEnabled** = **false**.
-   Textvorhersage: Aktiviert, wenn **IsTextPredictionEnabled** = **true**. Deaktiviert, wenn **IsTextPredictionEnabled** = **false**.

### CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

Das standardmäßige Tastaturlayout für Ziffern und Sonderzeichen.

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für Währungen](images/input-scopes/kbdpccurrencyamountandsymbol.png)<br>Enthält außerdem die Tasten „Seite nach links/rechts“, um weitere Sonderzeichen anzuzeigen.| ![Windows Phone-Bildschirmtastatur für Währungen](images/input-scopes/kbdwpcurrencyamountandsymbol.png) |
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul>Identisch mit **Number** und **TelephoneNumber**. | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden</li>| 

### Url

`<TextBox InputScope="Url"/>`

Enthält die Tasten **.com** und ![Los-Taste](images/input-scopes/kbdgokey.png) (Los). Halten Sie die Taste **.com** gedrückt, um weitere Optionen anzuzeigen (**.org**, **.net** und regionsspezifische Suffixe).

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für URLs](images/input-scopes/kbdpcurl.png)<br>Enthält außerdem die Tasten **:**, **-** und **/**.| ![Windows Phone-Bildschirmtastatur für URLs](images/input-scopes/kbdwpurl.png)<br>Halten Sie die Punkttaste gedrückt, um weitere Optionen ( - + &quot; / &amp; : , ) anzuzeigen. |
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden</li></ul> |

### EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

Enthält die Tasten **@** und **.com**. Halten Sie die Taste **.com** gedrückt, um weitere Optionen anzuzeigen (**.org**, **.net** und regionsspezifische Suffixe).

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für E-Mail-Adressen](images/input-scopes/kbdpcemailsmtpaddress.png)<br>Enthält außerdem die Tasten **_** und **-**.| ![Windows Phone-Bildschirmtastatur für E-Mail-Adressen](images/input-scopes/kbdwpemailsmtpaddress.png)<br>Halten Sie die PUNKTTASTE gedrückt, um weitere Optionen ( - _ , ; ) anzuzeigen. |
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden</li></ul> |

### Number

`<TextBox InputScope="Number"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für Ziffern](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![Windows Phone-Bildschirmtastatur für Ziffern](images/input-scopes/kbdwpnumber.png)<br>Tastatur enthält Ziffern und einen Dezimalpunkt. Halten Sie die Dezimalpunkttaste gedrückt, um weitere Optionen ( , - ) anzuzeigen. |
|Identisch mit **CurrencyAmountAndSymbol** und **TelephoneNumber**. | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Immer deaktiviert</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> |

### TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für Telefonnummern](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![Windows Phone-Bildschirmtastatur für Telefonnummern](images/input-scopes/kbdwptelephonenumber.png)<br>Die Tastatur entspricht der Wähltastatur eines Telefons. Halten Sie die PUNKTTASTE gedrückt, um weitere Optionen ( , ( ) X ) anzuzeigen. ). Halten Sie die 0-TASTE gedrückt, um „+“ einzugeben. |
|Identisch mit **CurrencyAmountAndSymbol** und **TelephoneNumber**. | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Immer deaktiviert</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> |

### Suchen

`<TextBox InputScope="Search"/>`

Enthält die anstelle der EINGABETASTE die Suchtaste********.

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für die Suche](images/input-scopes/kbdpcsearch.png)| ![Windows Phone-Bildschirmtastatur für die Suche](images/input-scopes/kbdwpsearch.png)|
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden</li></ul> |

### SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/kbdpcdefault.png)<br>Gleiches Layout wie **Default**.| ![Standardmäßige Windows Phone-Bildschirmtastatur](images/input-scopes/kbdwpdefault.png)|
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Identisch mit **Default**. |

### Formula

`<TextBox InputScope="Formula"/>`

Enthält die Taste **=**.

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Windows-Bildschirmtastatur für Formeln](images/input-scopes/kbdpcformula.png)<br>Enthält außerdem die Tasten **%**, **$** und **+**.| ![Windows Phone-Bildschirmtastatur für Formeln](images/input-scopes/kbdwpformula.png)<br>Halten Sie die PUNKTTASTE gedrückt, um weitere Optionen anzuzeigen: ( - ! ? , ). Halten Sie die **=**-Taste gedrückt, um weitere Optionen ( ( ) : &lt;&gt;) anzuzeigen. |
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden</li></ul> |

### Chat

`<TextBox InputScope="Chat"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/kbdpcdefault.png)<br>Gleiches Layout wie **Default**.| ![Standardmäßige Windows Phone-Bildschirmtastatur](images/input-scopes/kbdwpdefault.png)<br>Gleiches Layout wie **Default**.|
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer deaktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Automatische Großschreibung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Textvorhersage: Standardmäßig aktiviert, kann deaktiviert werden</li></ul> |

### NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![Standardmäßige Windows-Bildschirmtastatur](images/input-scopes/kbdpcdefault.png)<br>Gleiches Layout wie **Default**.| ![Windows Phone-Bildschirmtastatur für Namen oder Telefonnummern](images/input-scopes/kbdwpnameorphonenumber.png)<br>Enthält die Tasten **;** und **@**. Die **&amp;123**-Taste wird durch die **123**-Taste ersetzt, die die Wähltastatur des Telefons öffnet (siehe **TelephoneNumber**).|
|Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig aktiviert, kann deaktiviert werden</li><li>Autokorrektur: Immer deaktiviert</li><li>Automatische Großschreibung: Immer aktiviert</li><li>Textvorhersage: Immer deaktiviert</li></ul> | Verfügbarkeit von Features:<ul><li>Rechtschreibprüfung: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Autokorrektur: Standardmäßig deaktiviert, kann aktiviert werden</li><li>Automatische Großschreibung: Standardmäßig deaktiviert, kann aktiviert werden. Erster Buchstabe eines Worts wird groß geschrieben.</li><li>Textvorhersage: Standardmäßig deaktiviert, kann aktiviert werden</li></ul> |



<!--HONumber=Jun16_HO4-->


