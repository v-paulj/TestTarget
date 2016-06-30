# Signieren der konvertierten Desktop-App

In diesem Artikel wird erläutert, wie Sie eine Desktop-App signieren, die Sie auf die universelle Windows-Plattform (UWP) konvertiert haben. Sie müssen Ihr .appx-Paket mit einem Zertifikat signieren, bevor Sie es bereitstellen können.

## Signieren Ihres .appx-Pakets

Erstellen Sie zunächst ein Zertifikat mit MakeCert.exe. Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, wählen Sie „None“ aus. 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

Kopieren Sie als Nächstes unter Verwendung von „pvk2pfx.exe“ die Informationen für den öffentlichen und privaten Schlüssel in das Zertifikat. 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Verwenden Sie zum Schluss „SignTool.exe“, um Ihr .appx-Paket mit dem Zertifikat zu signieren.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

Weitere Informationen finden Sie unter [Signieren eines App-Pakets mit SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx). 

Alle drei oben aufgeführten Tools sind in der Microsoft Windows 10 SDK enthalten. Um diese direkt aufrufen, rufen Sie das Skript ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` über eine Befehlszeile auf.

## Häufige Fehler

### Beschädigte oder falsch formatierte Authenticode-Signaturen

Dieser Abschnitt enthält ausführliche Informationen zum Identifizieren von Problemen mit übertragbaren ausführbaren Dateien in Ihrem AppX-Paket, die möglicherweise beschädigte oder falsch formatierte Authenticode-Signaturen enthalten. Ungültige Authenticode-Signaturen auf Ihren übertragbaren ausführbaren Dateien, die ein beliebiges binäres Format (z. B. .exe, .dll, chm usw.) aufweisen können, verhindern, dass Ihr Paket ordnungsgemäß signiert wird und aus einem AppX-Paket bereitgestellt werden kann. 

Die Position der Authenticode-Signatur einer übertragbaren ausführbaren Datei wird durch den Zertifikat-Tabelleneintrag in den optionalen Kopfzeilen-Datenverzeichnissen und der zugehörigen Attributzertifikatstabelle angegeben. Während der Überprüfung der Signatur dienen die Angaben in diesen Strukturen dazu, die Signatur in einer übertragbaren ausführbaren Datei zu suchen. Wenn diese Werte beschädigt werden, kann es so aussehen, als ob eine Datei ungültig signiert wurde. 

Damit die Authenticode-Signatur korrekt ist, muss für die Authenticode-Signatur Folgendes gelten:

- Der Anfang des **WIN_CERTIFICATE**-Eintrags in der übertragbaren ausführbaren Datei kann nicht über das Ende der ausführbaren Datei hinausgehen.
- Der **WIN_CERTIFCATE**-Eintrag sollte sich am Ende des Bilds befinden
- Die Größe des **WIN_CERTIFICATE**-Eintrags muss positiv sein
- Der **WIN_CERTIFICATE**-Eintrag muss bei ausführbaren 32-Bit-Dateien nach der **IMAGE_NT_HEADERS32**-Struktur und bei ausführbaren 64-Bit-Dateien nach der IMAGE_NT_HEADERS64-Struktur starten

Weitere Informationen finden Sie in der [Spezifikation zu übertragbaren ausführbaren Authenticode-Dateien](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) und der [Spezifikation zum Format übertragbarer ausführbarer Dateien](https://msdn.microsoft.com/en-us/windows/hardware/gg463119.aspx). 

Beachten Sie, dass SignTool.exe eine Liste der beschädigten oder falsch formatierten Binärdateien ausgeben kann, wenn Sie versuchen, ein AppX-Paket zu signieren. Aktivieren Sie dazu ausführliche Protokollierung , indem Sie die Umgebungsvariable APPXSIP_LOG auf 1 (z. B. ```set APPXSIP_LOG=1```) festlegen und SignTool.exe erneut ausführen.

Um diese falsch formatierten Binärdateien zu korrigieren, stellen Sie sicher, dass sie den oben angegebenen Anforderungen entsprechen.

## Siehe auch

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)
- [SignTool.exe (Signaturtool)](https://msdn.microsoft.com/library/8s9b9yaz(v=vs.110).aspx)
- [Signieren eines App-Pakets mithilfe von SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)

<!--HONumber=Jun16_HO4-->


