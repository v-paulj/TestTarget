---
description: listet Unterstützung auf Sprachebene in XAML für die Windows-Runtime für bestimmte Datentypen in der CLR und in anderen Programmiersprachen wie C++ auf.
title: Systeminterne XAML-Datentypen
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
---

# Systeminterne XAML-Datentypen

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

XAML für die Windows-Runtime stellt auf Sprachebene Unterstützung für mehrere Datentypen bereit, die häufig verwendete Grundtypen in der Common Language Runtime (CLR) und in anderen Programmiersprachen wie C++ sind.

Am häufigsten finden Sie die Verwendung systeminterner XAML-Datentypen, wenn Ressourcen in einem XAML-Ressourcenwörterbuch definiert werden. Sie können in diesem Wörterbuch Konstanten definieren, z. B. Zahlen, die Sie für mehrere Werte verwenden. Sie können auch eine Storyboardanimation verwenden, um eine Animation mithilfe eines Zeichenfolgen- oder booleschen Werts zu erreichen. Sie benötigen dann ein XAML-Objektelement, das die Zeichenfolge oder den booleschen Wert darstellt, um den Keyframe der [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320)-Definition aufzufüllen. Die Windows-Runtime-XAML-Standardvorlagen verwenden beide Techniken.

XAML für die Windows-Runtime bietet auf Sprachebene Unterstützung für diese Typen:

| XAML-Grundtyp | Beschreibung |
| **x:Boolean**  | Entspricht für CLR-Unterstützung [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx). XAML analysiert Werte für **x:Boolean** ohne Berücksichtigung der Groß-/Kleinschreibung. Beachten Sie, dass "x:Bool" keine akzeptierte Alternative ist. |
| **x:String**   | Entspricht für CLR-Unterstützung [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx). Die Codierung für die Zeichenfolge wird standardmäßig auf die umgebende XML-Codierung festgelegt. |
| **x:Double**   | Entspricht für CLR-Unterstützung [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Zusätzlich zu den numerischen Werten lässt die Textsyntax für **x:Double** das Token "NaN" zu. So kann "Auto" für das Layoutverhalten als Ressourcenwert gespeichert werden. Die Token werden unter Berücksichtigung der Groß-/Kleinschreibung behandelt. Sie können die wissenschaftliche Notation verwenden, z. B. "1+E06" für `1,000,000`. |
| **x:Int32**    | Entspricht für CLR-Unterstützung [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx). **x:Int32** wird als signiert behandelt. Sie können das Minuszeichen ("-") für eine negative Ganzzahl verwenden. In XAML wird das Fehlen eines Zeichens in der Textsyntax als positiver signierter Wert impliziert. |

Diese XAML-Sprachgrundtypen sind im Allgemeinen die einzigen Fälle, in denen Sie ein Objektelement definieren, das das Präfix **x:** in XAML verwendet. Alle anderen XAML-Sprachfeatures werden in der Regel in Attributform oder als Markuperweiterung verwendet.

**Hinweis**  Üblicherweise werden die Sprachgrundtypen für XAML und alle anderen XAML-Sprachelemente mit dem "x:"-Präfix angezeigt. So werden XAML-Sprachelemente in der Regel im echten Markup verwendet. Diese Konvention wird in der Dokumentation für XAML und auch in der XAML-Spezifikation befolgt.

## Andere XAML-Grundtypen

In der XAML 2009-Spezifikation werden weitere XAML-Grundtypen auf Sprachebene aufgeführt, etwa **x:Uri** und **x:Single**. Falls diese nicht in der Tabelle in diesem Artikel aufgeführt sind, werden andere XAML-Grundtypen auf Sprachebene, die in einem anderen XAML-Vokabular oder in der XAML-Spezifikation von 2009 definiert sind, derzeit von XAML für die Windows-Runtime nicht unterstützt.

**Hinweis**  Datums- und Uhrzeitangaben (Eigenschaften, die [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) oder [**DateTimeOffset**](T:System.DateTimeOffset), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996) oder [**System.TimeSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/system.timespan.aspx) verwenden) können nicht mit einem XAML-Grundtyp festgelegt werden. Diese Eigenschaften können in XAML grundsätzlich nicht festgelegt werden, da im XAML-Parser der Windows-Runtime kein Standardverhalten für die Konvertierung der Ausgangszeichenfolge für Datums- und Uhrzeitangaben vorhanden ist. Für Initialisierungswerte von Datums- und Uhrzeiteigenschaften müssen Sie CodeBehind verwenden, der ausgeführt wird, wenn eine Seite oder ein Element geladen wird.

## Verwandte Themen

* [Übersicht über XAML](xaml-overview.md)
* [Anleitung zur XAML-Syntax](xaml-syntax-guide.md)
* [Storyboardanimationen](https://msdn.microsoft.com/library/windows/apps/mt187354)
 



<!--HONumber=Mar16_HO1-->


