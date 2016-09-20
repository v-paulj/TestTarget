---
title: "Umwandlung zwischen Zeichenfolgen und binären Daten"
description: "Dieser Beispielcode zeigt, wie Sie in einer App für die universelle Windows-Plattform (UWP) eine Konvertierung zwischen Zeichenfolgen und binären Daten durchführen."
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: awkoren
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 667338ed3808f79b75230bc0c3ea00cc7e274d5a

---

# Umwandlung zwischen Zeichenfolgen und binären Daten


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Beispielcode zeigt, wie Sie in einer UWP-App (Universelle Windows-Plattform) zwischen Zeichenfolgen und binären Daten konvertieren können.

```cs
public void ConvertData()
{
    // Create a string to convert.
    String strIn = "Input String";

    // Convert the string to UTF16BE binary data.
    IBuffer buffUTF16BE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16BE);

    // Convert the string to UTF16LE binary data.
    IBuffer buffUTF16LE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16LE);

    // Convert the string to UTF8 binary data.
    IBuffer buffUTF8 = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf8);
}
```


<!--HONumber=Jun16_HO4-->


