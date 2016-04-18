---
title: Auswählen einer Programmiersprache
ms.assetid: 6CA46432-BF03-4B20-9187-565B3503B497
description: Auswählen einer Programmiersprache
---

# Erste Schritte: Auswählen einer Programmiersprache

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Auswählen einer Programmiersprache

Bevor wir weiter ins Detail gehen, sollten Sie die Programmiersprachen kennen, die Ihnen bei der Entwicklung von universellen Windows-Plattform-Apps (UWP) zur Verfügung stehen. Obwohl bei der exemplarischen Vorgehensweise in diesem Artikel C# verwendet wird, können Sie UWP-Apps ggf. mit mehreren Programmiersprachen entwickeln (siehe [Sprachen, Tools und Frameworks](https://msdn.microsoft.com/library/windows/apps/dn465799)).

UWP-Apps können mit C++, C#, Microsoft Visual Basic und JavaScript entwickelt werden. JavaScript verwendet HTML5-Markup für das Benutzeroberflächenlayout, und andere Programmiersprachen verwenden die Markupsprache *Extensible Application Markup Language (XAML)* für die Beschreibung der Benutzeroberfläche.

Obwohl wir uns in diesem Artikel auf C# konzentrieren, bieten die restlichen Sprachen ebenfalls klare Vorteile, die Sie kennen sollten. Wenn Sie bei der App beispielsweise in erster Linie Wert auf die Leistung legen, ist C++ möglicherweise die richtige Wahl (insbesondere bei einer aufwändigen Grafik). Die Microsoft .NET-Version von Visual Basic ist hervorragend für Visual Basic-App-Entwickler geeignet. JavaScript mit HTML5 ist eine gute Wahl für Entwickler mit Webentwicklungshintergrund. Weitere Informationen finden Sie unter einem der folgenden Themen:

-   [Erstellen Ihrer ersten Windows Store-App mit C++](https://msdn.microsoft.com/library/windows/apps/hh974580)
-   [Erstellen Ihrer ersten Windows Store-App mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Erstellen Ihrer ersten Windows Store-App mit JavaScript](https://msdn.microsoft.com/library/windows/apps/br211385)
-   [Erstellen Ihrer ersten Windows Phone Store-App mit C# oder Visual Basic](http://go.microsoft.com/fwlink/p/?LinkID=397877)
-   [WinJS unter Windows Phone 8.1](http://go.microsoft.com/fwlink/p/?LinkID=397879)

**Hinweis**  Bei Apps mit 3D-Grafiken sind die Standards OpenGL und OpenGL ES für UWP-Apps nicht verfügbar. Wenn Sie Ihren OpenGL ES-Code in Microsoft DirectX nicht neu schreiben möchten, könnte **Angle** von Interesse für Sie sein. Angle ist ein laufendes Projekt, das zum Konvertieren von OpenGL in DirectX entwickelt wurde, indem OpenGL-API-Aufrufe in DirectX-API-Aufrufe übersetzt werden. Weitere Informationen hierzu finden Sie unter den folgenden Themen:
-   [Angle](https://code.google.com/p/angleproject/)
-   [Erstellen Ihrer ersten Windows Store-App mit DirectX](https://msdn.microsoft.com/library/windows/apps/br229580)
-   [Beispiele für Windows Store-Apps mit DirectX](http://go.microsoft.com/fwlink/p/?LinkId=263603)
-   [Wo finde ich das DirectX SDK?](https://msdn.microsoft.com/library/windows/desktop/ee663275)

## Testen von C#

Als iOS-Entwickler sind Sie die Arbeit mit Objective-C und Swift gewohnt. C# ist die Microsoft-Programmiersprache, die beiden am ähnlichsten ist. Unserer Meinung nach ist C# für die meisten Entwickler und die meisten Apps die Sprache, die am einfachsten und schnellsten erlernbar ist. Daher liegt der Schwerpunkt der Informationen und exemplarischen Vorgehensweisen in diesem Artikel auf dieser Sprache. Weitere Informationen zu C# finden Sie unter den folgenden Themen:

-   [Erstellen Ihrer ersten Windows Store-App mit C# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/hh974581)
-   [Beispiele für Windows Store-Apps mit C#](http://go.microsoft.com/fwlink/p/?LinkId=263453)
-   [Visual C#](http://go.microsoft.com/fwlink/p/?LinkId=263450)

Es folgt ein Vergleich einer Klasse, die sowohl in Objective-C als auch in C# geschrieben wurde. Die Objective-C-Version wird zuerst dargestellt. Die C#-Version folgt danach.

```obj-c
// Objective-C header: SampleClass.h.

#import <Foundation/Foundation.h>

@interface SampleClass : NSObject {
    BOOL localVariable;
}

@property (nonatomic) BOOL localVariable;

-(int) addThis: (int) firstNumber andThis: (int) secondNumber;

@end
```

```obj-c
// Objective-C implementation.

#import "SampleClass.h"

@implementation SampleClass

@synthesize localVariable = _localVariable;

- (id)init {
    self = [super init];
    if (self) {
        localVariable = true;
    }
    return self;
}

-(int) addThis: (int) firstNumber andThis: (int) secondNumber {
    return firstNumber + secondNumber;
}

@end
```

```obj-c
// Objective-C usage.

SampleClass *mySampleClass = [[SampleClass alloc] init];
mySampleClass.localVariable = false;
int result = [mySampleClass addThis:1 andThis:2];
```

Nun die C#-Version. Wie Sie sehen, befinden sich Header und Implementierung, wie auch bei Swift, nicht in separaten Dateien.

```csharp
// C# header and implementation.

using System;

namespace MyApp  // Defines this code' s scope.
{
    class SampleClass
    {
        private bool localVariable;

        public SampleClass() // Constructor.
        {
            localVariable = true;
        }

        public bool myLocalVariable // Property.
        {
            get
            {
                return localVariable;
            }
            set
            {
                localVariable = value; 
            }
        }

        public int AddTwoNumbers(int numberOne, int numberTwo)
        {
            return numberOne + numberTwo;
        }        
    }
}
```

```csharp
// C# usage.

SampleClass mySampleClass = new SampleClass();
mySampleClass.myLocalVariable = false;
int result = mySampleClass.AddTwoNumbers(1, 2);
```

C# ist eine einfach erlernbare Sprache und verfügt über viele Unterstützungsklassen und Frameworks, die .NET ausmachen. Sie werden in kürzester Zeit problemlos Ihren Code ohne eine eckige Klammer schreiben können!

## Nächster Schritt

[Erste Schritte: Aufbau von Visual Studio](getting-started-getting-around-in-visual-studio.md)


<!--HONumber=Mar16_HO1-->


