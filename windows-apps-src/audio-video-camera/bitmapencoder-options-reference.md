---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit BitmapEncoder verwendet werden können."
title: Referenz zu BitmapEncoder-Optionen
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 510cb363b258d20688ea212856af4b7ac0311e61

---

# Referenz zu BitmapEncoder-Optionen

\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Artikel sind die Codierungsoptionen aufgeführt, die mit [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) verwendet werden können. Eine Codierungsoption wird durch ihren Namen (eine Zeichenfolge) und einen Wert mit einem bestimmten Datentyp definiert ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Informationen zum Arbeiten mit Bildern finden Sie unter [Bildverarbeitung](imaging.md).

| Name                    | PropertyType | Verwendungshinweise                                                                                        | Gültige Formate |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Gültige Werte von 0 bis 1.0 Größere Werte bedeuten höhere Qualität.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Gültige Werte von 0 bis 1.0 Höhere Werte bedeuten ein effizienteres und langsameres Komprimierungsverfahren. | TIFF          |
| Lossless                | boolean      | Wenn dieser Wert auf „true“ festgelegt ist, wird die Option „ImageQuality“ ignoriert.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Gibt an, ob der Interlacemodus für das Bild verwendet wird                                                                    | PNG           |
| FilterOption            | uint8        | Verwenden Sie die [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389)-Aufzählung.                                | PNG           |
| TiffCompressionMethod   | uint8        | Verwenden Sie die [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399)-Aufzählung.                    | TIFF          |
| Luminance               | uint32Array  | Ein Array mit 64Elementen, das die Quantifizierungskonstanten für die Leuchtdichte enthält.                               | JPEG          |
| Chrominance             | uint32Array  | Ein Array mit 64Elementen, das die Quantifizierungskonstanten für die Chrominanz enthält.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Verwenden Sie die [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386)-Aufzählung.                    | JPEG          |
| SuppressApp0            | boolean      | Gibt an, ob die Erstellung eines App0-Metadatenblocks unterdrückt wird.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Gibt an, ob als Version5 des BMP-Formats, die Alpha-Werte unterstützt, codiert werden soll.                                         | BMP           |

 

## Verwandte Themen

* [Bildverarbeitung](imaging.md)
 

 







<!--HONumber=Jun16_HO4-->


