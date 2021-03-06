---
title: "Erstellen zufälliger Zahlen"
description: "Dieser Beispielcode zeigt, wie Sie zufällige Zahlen oder Puffer für die Verwendung bei der Kryptografie in einer UWP (Universelle Windows-Plattform)-App erstellen."
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 71ac4a6bffe5004a044ee6feba21eb44762ab781

---

# Erstellen zufälliger Zahlen


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Beispielcode zeigt, wie Sie zufällige Zahlen oder Puffer für die Verwendung bei der Kryptografie in einer UWP (Universelle Windows-Plattform)-App erstellen.

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```


<!--HONumber=Aug16_HO3-->


