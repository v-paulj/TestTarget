---
author: Xansky
Description: Beschreibt die Schritte, mit denen Sie die Verwendbarkeit Ihrer UWP-App (App für die universelle Windows-Plattform) sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Designs mit hohem Kontrast
label: High-contrast themes
template: detail.hbs
---

# Designs mit hohem Kontrast  



Beschreibt die Schritte, mit denen Sie die Verwendbarkeit Ihrer UWP-App (App für die universelle Windows-Plattform) sicherstellen können, wenn ein Design mit hohem Kontrast aktiviert ist.

Eine UWP-App unterstützt standardmäßig Designs mit hohem Kontrast. Wenn sich ein Benutzer für die Verwendung eines Designs mit hohem Kontrast aus den Systemeinstellungen oder Tools für die Barrierefreiheit entscheidet, werden vom Framework automatisch Farben und Stileinstellungen verwendet, mit denen für Steuerelemente und Komponenten auf der Benutzeroberfläche ein Layout und Rendering mit hohem Kontrast entsteht.

Diese standardmäßige Unterstützung basiert auf der Verwendung der Standarddesigns und -vorlagen. Diese Designs und Vorlagen verweisen auf Systemfarben als Ressourcendefinitionen. Die Ressourcenquellen werden automatisch geändert, wenn sich das System in einem Modus mit hohem Kontrast befindet. Wenn Sie jedoch benutzerdefinierte Vorlagen, Designs und Stile für die Steuerelemente verwenden, achten Sie darauf, dass die integrierte Unterstützung des hohen Kontrasts nicht deaktiviert ist. Bei Verwendung eines XAML-Designers für Microsoft Visual Studio für die Formatierung wird vom Designer neben dem primären Design immer ein separates Design mit hohem Kontrast generiert, wenn Sie eine Vorlage definieren, die sich wesentlich von der Standardvorlage unterscheidet. Die separaten Designwörterbücher sind in der [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807)-Sammlung, einer dedizierten Eigenschaft eines [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Elements, enthalten.

Weitere Informationen zu Designs und Steuerelementvorlagen finden Sie unter [Schnellstart: Steuerelementvorlagen](https://msdn.microsoft.com/library/windows/apps/xaml/Hh465374). Die XAML-Ressourcenwörterbücher und -Designs enthalten Informationen zu bestimmten Steuerelementen sowie dazu, wie die Designs konstruiert sind und wie sie auf Ressourcen verweisen, die für die möglichen Einstellungen für hohen Kontrast ähnlich, jedoch nicht identisch sind.

<span id="Detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="detecting_when_a_high-contrast_theme_is_enabled"/>
<span id="DETECTING_WHEN_A_HIGH-CONTRAST_THEME_IS_ENABLED"/>
## Ermitteln, wann ein Design mit hohem Kontrast aktiviert ist  
Eine UWP-App kann mithilfe von Membern der [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)-Klasse die aktuellen Einstellungen für Designs mit hohem Kontrast ermitteln. Mit der [**HighContrast**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrast)-Eigenschaft wird ermittelt, ob derzeit ein Design mit hohem Kontrast ausgewählt ist. Falls **true** für **HighContrast** festgelegt ist, besteht der nächste Schritt in der Überprüfung des Werts der [**HighContrastScheme**](https://msdn.microsoft.com/library/windows/apps/BR242237_highcontrastscheme)-Eigenschaft, um den Namen des verwendeten Designs mit hohem Kontrast abzurufen. „Hoher Kontrast (Weiß)“ und „Hoher Kontrast (Schwarz)“ sind typische Werte für **HighContrastScheme**, auf die Ihr Code reagieren sollte. XAML-definierte [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794)-Schlüssel dürfen keine Leerstellen enthalten. Daher lauten die Schlüssel für diese Designs in einem Ressourcenwörterbuch in der Regel „HighContrastWhite“ und „HighContrastBlack“. Darüber hinaus müssen Sie Fallback-Logik für ein Standarddesign mit hohem Kontrast aufnehmen, falls als Wert eine andere Zeichenfolge angegeben ist. Im [XAML-Beispiel für hohen Kontrast](http://go.microsoft.com/fwlink/p/?linkid=254993) wird die Logik hierfür gezeigt.

> [!NOTE] Achten Sie darauf, dass Sie den [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)-Konstruktor aus einem Bereich aufrufen, in dem die App initialisiert ist und bereits Inhalte anzeigt.

Apps können während ihrer Ausführung zur Verwendung von Ressourcenwerten mit hohem Kontrast wechseln. Dies funktioniert, solange die Ressourcen mithilfe der [{ThemeResource}-Markuperweiterung](https://msdn.microsoft.com/library/windows/apps/Mt185591) im Stil- oder Vorlagen-XAML angefordert werden. Dieses Verfahren der {ThemeResource}-Markuperweiterung wird von allen Standarddesigns (generic.xaml) verwendet. Sie können also dieses Verhalten erzielen, wenn Sie die Standarddesigns für Steuerelemente verwenden. Mit benutzerdefinierten Steuerelementen oder benutzerdefinierten Steuerelementformatierungen ist dies möglich, wenn Sie das Ressourcenverfahren der {ThemeResource}-Markuperweiterung auch in Ihren benutzerdefinierten Vorlagen und Stilen verwendet haben.

<span id="related_topics"/>
## Verwandte Themen  
* [Barrierefreiheit](accessibility.md)
* [Beispiel für UI-Kontrast und -Einstellungen](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [XAML-Beispiel für hohen Kontrast](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)


<!--HONumber=May16_HO2-->


