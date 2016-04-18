---
title: Umwandlung zwischen Zeichenfolgen und binären Daten
description: Dieser Beispielcode zeigt, wie Sie in einer App für die universelle Windows-Plattform (UWP) eine Konvertierung zwischen Zeichenfolgen und binären Daten durchführen.
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: awkoren
---

# Umwandlung zwischen Zeichenfolgen und binären Daten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

<!--HONumber=Mar16_HO5-->


