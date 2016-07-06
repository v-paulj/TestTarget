---
author: Jwmsft
Description: "Während der Texteingabe und -bearbeitung wird der Benutzer durch die Rechtschreibprüfung darauf aufmerksam gemacht, dass ein Wort falsch geschrieben ist, indem das Wort mit einer Wellenlinie unterstrichen wird und dem Benutzer eine Korrekturoption angeboten wird."
title: "Rechtschreibprüfung und Textvorhersage"
ms.assetid: B867C956-5AB2-4207-A8DE-179CE7871180
label: Spell checking and text prediction
template: detail.hbs
redirect_url: text-controls
ms.sourcegitcommit: 08f982f5c54d596a7624fe63528f91375e7761ca
ms.openlocfilehash: 8954b14a84241686a7c784c3d250f197bed7c8b6

---

# Richtlinien für die Rechtschreibprüfung

Während der Texteingabe und -bearbeitung wird der Benutzer durch die Rechtschreibprüfung darauf aufmerksam gemacht, dass ein Wort falsch geschrieben ist, indem das Wort mit einer roten Wellenlinie unterstrichen wird. Außerdem wird dem Benutzer eine Korrekturmöglichkeit angeboten.

**Wichtige APIs**

-   [**IsSpellCheckEnabled-Eigenschaft (XAML)**](https://msdn.microsoft.com/library/windows/apps/br209688)


## <span id="checklist_section"></span><span id="CHECKLIST_SECTION"></span>Empfehlungen


-   Verwenden Sie die Rechtschreibprüfung, um Benutzer bei der Eingabe von Wörtern oder Sätzen in die Texteingabesteuerelemente zu unterstützen. Die Rechtschreibprüfung funktioniert bei Touch-, Maus- und Tastatureingaben.
-   Verwenden Sie die Rechtschreibprüfung nicht, wenn ein Wort wahrscheinlich nicht im Wörterbuch enthalten ist oder wenn Benutzer die Rechtschreibprüfung nicht als nützlich empfinden. Aktivieren Sie sie zum Beispiel nicht für Eingabefelder für Kennwörter, Telefonnummern oder Namen. Für diese Steuerelemente ist die Rechtschreibprüfung standardmäßig deaktiviert.
-   Deaktivieren Sie die Rechtschreibprüfung nicht nur deswegen, weil das aktuelle Rechtschreibprüfungsmodul die Sprache Ihrer App nicht unterstützt. Wenn die Rechtschreibprüfung eine Sprache nicht unterstützt, passiert nichts, sodass die Option bedenkenlos aktiviert bleiben kann. Außerdem verfügen einige Benutzer eventuell über einen Eingabemethoden-Editor (Input Method Editor, IME), um eine andere Sprache in Ihre App einzugeben, die unterstützt wird. Deaktivieren Sie, wenn Sie beispielsweise eine japanische Sprachen-App erstellen, nicht die Rechtschreibprüfung, selbst wenn das Rechtschreibprüfungsmodul diese Sprache möglicherweise aktuell nicht erkennt. Der Benutzer könnte zu einem englischsprachigen IME wechseln und englischsprachige Eingaben in der App vornehmen. Wenn die Rechtschreibprüfung aktiviert ist, werden die englischsprachigen Eingaben geprüft.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Weitere Hinweise zur Verwendung


Windows Store-Apps stellen eine integrierte Rechtschreibprüfung für mehr- und einzeilige Texteingabefelder und Elemente bereit, deren **contentEditable**-Eigenschaft auf **true** festgelegt ist. Hier ist ein Beispiel für die integrierte Rechtschreibprüfung:

![Die integrierte Rechtschreibprüfung](images/spellchecking.png)

Weitere Informationen finden Sie unter der [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209683).

Verwenden Sie die Rechtschreibprüfung für Texteingabesteuerelemente aus zwei Gründen:

-   **Zur automatischen Korrektur von Rechtschreibfehlern**

    Das Rechtschreibprüfungsmodul korrigiert falsch geschriebene Wörter automatisch, wenn die Korrektur zweifelsfrei feststeht. Beispielsweise ändert das Modul "dsa" automatisch in "das".

-   **Zur Anzeige von alternativen Schreibungen**

    Wenn dem Rechtschreibprüfungsmodul keine zweifelsfreien Korrekturen zur Verfügung stehen, wird unter dem falsch geschriebenen Wort eine rote Linie eingefügt, und es werden Alternativen in einem Kontextmenü angezeigt, wenn Sie auf das Wort tippen oder mit der rechten Maustaste darauf klicken.

Für JavaScript-Steuerelemente ist die Rechtschreibprüfung für mehrzeilige Texteingabesteuerelemente standardmäßig aktiviert und für einzeilige Steuerelemente deaktiviert. Sie können sie für einzeilige Steuerelemente manuell aktivieren, indem Sie die **spellcheck**-Eigenschaft des Steuerelements auf **true** festlegen. Sie können die Rechtschreibprüfung für ein Steuerelement deaktivieren, indem Sie die **spellcheck**-Eigenschaft auf **false** festlegen.

Für XAML-TextBox-Steuerelemente ist die Rechtschreibprüfung standardmäßig deaktiviert. Sie können sie durch Festlegen der **IsSpellCheckEnabled**-Eigenschaft auf **true** aktivieren.



## <span id="related_topics"></span>Verwandte Artikel

* [Text und Textsteuerelemente](text-controls.md)
* [Richtlinien für die Texteingabe](https://msdn.microsoft.com/library/windows/apps/hh750315)
* [Richtlinien für Text und Typografie](https://msdn.microsoft.com/library/windows/apps/hh700394)
            
          
            **Für Entwickler (XAML)**
* [**TextBox.IsSpellCheckEnabled-Eigenschaft**](https://msdn.microsoft.com/library/windows/apps/br209688)
* [**TextBox-Klasse**](https://msdn.microsoft.com/library/windows/apps/br209683)

 







<!--HONumber=Jun16_HO5-->


