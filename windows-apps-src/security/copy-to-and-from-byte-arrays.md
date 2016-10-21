---
title: Kopieren in und aus Bytearrays
description: "Dieser Beispielcode zeigt, wie Sie in einer App für die universelle Windows-Plattform (UWP) in und aus Bytearrays kopieren."
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: f700d3d9c09424ef8fffd73a08e12563acbbc497

---

# Kopieren in und aus Bytearrays


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Dieser Beispielcode zeigt, wie Sie in einer UWP (Universelle Windows-Plattform)-App in und aus Bytearrays kopieren können.

```cs
public void ByteArrayCopy()
{
    // Initialize a byte array.
    byte[] bytes = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Create a buffer from the byte array.
    IBuffer buffer = CryptographicBuffer.CreateFromByteArray(bytes);

    // Encode the buffer into a hexadecimal string (for display);
    string hex = CryptographicBuffer.EncodeToHexString(buffer);

    // Copy the buffer back into a new byte array.
    byte[] newByteArray;
    CryptographicBuffer.CopyToByteArray(buffer, out newByteArray);
}
```


<!--HONumber=Aug16_HO3-->


