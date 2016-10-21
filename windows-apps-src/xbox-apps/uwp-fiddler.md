---
author: WilliamsJason
title: "Verwendung von Fiddler mit Xbox One bei der Entwicklung für UWP"
description: "Beschreibt die Verwendung des Freeware-Tools Fiddler, um den Netzwerkverkehr für einen Xbox One Dev Kit für UWP anzuzeigen."
translationtype: Human Translation
ms.sourcegitcommit: 11c6cffab7934937b6d89c30e4d03ae752f6b3b7
ms.openlocfilehash: 241fa495c7277fe2bf4feafeb4062842f97e59b1

---

# Verwendung von Fiddler mit Xbox One bei der Entwicklung für UWP

Fiddler ist ein Webdebuggingproxy, der den gesamten HTTP- und HTTPS-Datenverkehr zwischen Ihrem Xbox One Dev Kit und dem Internet protokolliert. Sie werden ihn, um sich anzumelden und den Datenverkehr zu und von den Xbox-Diensten und Diensten vertrauender Seiten zu prüfen und Webdienstaufrufe zu verstehen und zu debuggen. 

Im normalen Betrieb besteht das Risiko, dass die Kommunikation einer Konsole, die über einen Proxy kommuniziert, durch diesen verändert wird, sodass Spieler möglicherweise mogeln können. Daher werden Konsolen so programmiert, dass sie keine Kommunikation über einen Proxy zulassen. Wenn Sie Fiddler mit Ihrem Xbox One Dev Kit verwenden, müssen Sie einige spezielle Konfigurationsschritte für den Dev Kit ausführen, damit dieser den Fiddler-Proxy verwenden kann. 

Fiddler ist Freeware und kann von der [Fiddler-Website](http://www.fiddler2.com/fiddler2/) heruntergeladen werden. 

Fiddler kann Auswirkungen auf den Netzwerkstatus haben, der von der Konsole gemeldet wird. Wenn eine Upstreamverbindung auf dem Computer deaktiviert wird, auf dem Fiddler ausgeführt wird, erkennt die Konsole diese Trennung möglicherweise erst nach Ablauf der Authentifizierung der Konsole. Wenn Sie Fiddler verwenden, müssen Sie die Verbindung zwischen der Konsole und dem Computer trennen, auf dem Fiddler ausgeführt wird, anstelle Fiddler zu verwenden, um eine Trennung zu simulieren

### So installieren und aktivieren Sie Fiddler auf Ihrem Entwicklungscomputer
Führen Sie diese Schritte zum Installieren und Aktivieren von Fiddler aus, um den Datenverkehr von Ihrem Dev Kit zu überwachen:

1. Installieren Sie Fiddler auf Ihrem Entwicklungscomputer, indem Sie die Anweisungen auf der [Fiddler-Website](http://www.fiddler2.com/fiddler2/) befolgen. 
2. Starten Sie Fiddler, und wählen Sie im Menü **Extras** **Fiddler-Optionen** aus. 
3. Wählen Sie die Registerkarte **Verbindungen** aus, und stellen Sie sicher, dass **Remoteverbindungen für Remotecomputer zulassen** ausgewählt ist. 
4. Klicken Sie auf **OK**, um die Änderung der Einstellungen zu akzeptieren. Ihnen wird nun ein Dialogfeld angezeigt, in dem Ihnen mitgeteilt wird, dass Fiddler neu gestartet werden muss, damit die Änderung wirksam wird, und Sie Ihre möglicherweise Ihre Firewall manuell konfigurieren müssen. Klicken Sie auf in diesem Dialogfeld auf **OK**, *starten Sie Fiddler jedoch noch nicht neu*.
5. Konfigurieren Sie die erforderliche Firewallregel, um Remotecomputern das Herstellen von Verbindungen zu gestatten. Starten Sie das Windows-Firewall-Systemsteuerungsapplet. Klicken Sie auf **Erweiterte Einstellungen** und anschließend auf **Eingehende Regel**. Suchen Sie die Regel mit dem Namen „FiddlerProxy“, und führen Sie einen Bildlauf nach rechts durch. Überprüfen Sie dabei, ob jede Einstellung in der folgenden Tabelle für diese Regel angezeigt wird.
  
  | Einstellung           | Bevorzugter Wert                |
  | ----              | ----                           |
  | Name              | FiddlerProxy                   |
  | Gruppe             | *Kein Wert* |
  | Profil           | Alle                            |
  | Aktiviert           | Ja                            |
  | Aktion            | Zulassen                          |
  | Überschreiben          | Nein                             |
  | Programm           | *Pfad zu fiddler.exe*          |
  | LocalAddress      | Beliebig                            |
  | RemoteAddress     | Beliebig                            |
  | Protokoll          | TCP                            |
  | LocalPort         | Beliebig                            |
  | RemotePort        | Beliebig                            |
  | AllowedUsers      | Beliebig                            |
  | AllowedComputers  | Beliebig                            |


6. Konfigurieren Sie Fiddler zum Aufzeichnen und Entschlüsseln von HTTPS-Datenverkehr wie folgt:
  1. Um eine optimale Leistung zu erhalten, legen Sie Fiddler auf die Verwendung des Streamingmodus fest, indem Sie auf der Tastenleiste auf die Taste **Stream** klicken.
  2. Wählen Sie im Menü **Extras** von Fiddler **Fiddler-Optionen** aus, und klicken Sie dann auf **HTTPS**.
  3. Aktivieren Sie das Kontrollkästchen **HTTPS-Datenverkehr entschlüsseln**. Wenn Sie in einem Dialogfeld gefragt werden, ob Windows so konfiguriert werden soll, dass dem CA-Zertifikat vertraut wird, klicken Sie auf **Nein**.
  4. Klicken Sie auf **Stammzertifikat zu Desktop exportieren**.
7. Beenden Sie Fiddler, und starten Sie das Tool neu.

### So konfigurieren Sie ein Dev Kit für die Verwendung von Fiddler als dessen Proxy für das Internet

1. Navigieren Sie in der Benutzeroberfläche des Xbox-Geräteportals zum **Netzwerk**-Tool.
2. Suchen Sie nach dem Fiddler-Stammzertifikat, das Sie zum Desktop exportiert haben. 
3. Geben Sie die IP-Adresse oder den Hostnamen des Entwicklungscomputers ein, auf dem Fiddler ausgeführt wird.
4. Geben Sie die Portnummer ein, an der Fiddler zuhört (standardmäßig verwendet Fiddler den Port 8888). 
5. Klicken Sie auf **Aktivieren**. Damit wird Ihr Dev Kit neu gestartet.

### So beenden Sie die Verwendung von Fiddler
Um die Verwendung von Fiddler als Proxy für das Internet zu beenden (und zu verhindern, dass Fiddler den gesamten Netzwerkdatenverkehr des Dev Kit protokolliert), führen Sie folgende Schritte aus:

1. Navigieren Sie in der Benutzeroberfläche des Xbox-Geräteportals zum **Netzwerk**-Tool.
2. Klicken Sie auf **Deaktivieren**.

> [!NOTE]
> Jeder PC, auf dem Fiddler installiert ist, verwendet ein anderes Fiddler-Stammzertifikat. Wenn Sie mehrere PCs besitzen, die verwendet werden könnten, um Ihrem Dev Kit einen Fiddler-Proxy bereitzustellen, müssen Sie das neue Stammzertifikat auswählen, wenn Sie zwischen diesen wechseln. Wenn Sie nur einen PC verwenden, müssen Sie das Stammzertifikat nur bei der ersten Aktivierung von Fiddler auswählen. Sie müssen dennoch die IP-Adresse und den Port angeben.

## Weitere Informationen
- [Fiddler-Einstellungen – API-Referenz](wdp-fiddler-api.md)
- [Häufig gestellte Fragen](frequently-asked-questions.md)
- [UWP auf XboxOne](index.md)






<!--HONumber=Aug16_HO3-->


