---
title: Codieren und Decodieren von Daten
description: "Dieser Beispielcode zeigt, wie Sie base64- und Hexadezimaldaten in einer App für die universelle Windows-Plattform (UWP) codieren und decodieren."
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: b07a040cafd2248f12fee571552632080e117692

---

# Codieren und Decodieren von Daten


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dieser Beispielcode zeigt, wie Sie base64- und Hexadezimaldaten in einer App für die universelle Windows-Plattform (UWP) codieren und decodieren.

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```



<!--HONumber=Aug16_HO3-->


