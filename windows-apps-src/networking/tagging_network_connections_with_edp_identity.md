---
Description: 'In diesem Thema wird veranschaulicht, wie Sie einen geschützten Threadkontext erstellen, bevor Sie in einem Unternehmensdatenschutz-Szenario (Enterprise Data Protection, EDP) Netzwerkverbindungen erstellen.'
MS-HAID: 'dev\_networking.tagging\_network\_connections\_with\_edp\_identity'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: Markieren von Netzwerkverbindungen mit EDP-Identität
---

# Markieren von Netzwerkverbindungen mit EDP-Identität

__Hinweis__ Die Richtlinie für den Unternehmensdatenschutz (Enterprise Data Protection, EDP) kann nicht auf Windows 10, Version 1511 (Build 10586) oder früher angewendet werden.

In diesem Thema wird veranschaulicht, wie Sie einen geschützten Threadkontext erstellen, bevor Sie in einem Unternehmensdatenschutz-Szenario (Enterprise Data Protection, EDP) Netzwerkverbindungen erstellen. Umfassende Informationen zu den Zusammenhängen zwischen EDP und Dateien, Datenströmen, Zwischenablage, Netzwerk, Hintergrundaufgaben und dem Schutz von Daten im Sperrzustand finden Sie unter [Unternehmensdatenschutz (EDP)](../enterprise/edp-hub.md).

**Hinweis**  [Unternehmensdatenschutz (EDP) – Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) veranschaulicht viele EDP-Szenarien.



## Voraussetzungen


-   **Vorbereitung für EDP**

    Weitere Informationen finden Sie unter [Einrichten des Computers für EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Ausrichtung auf unternehmensoptimierte Apps**

    Eine App, die von sich aus dafür sorgt, dass Unternehmensdaten unter der Kontrolle des verwaltenden Unternehmens bleiben, wird als unternehmensoptimierte App bezeichnet. Eine optimierte App ist leistungsfähiger, intelligenter, flexibler und vertrauenswürdiger als eine nicht optimierte App. Durch Deklaration der eingeschränkten **enterpriseDataPolicy**-Funktion teilt die App dem System mit, dass sie optimiert ist. Zur Optimierung gehört jedoch mehr als nur das Festlegen einer Funktion. Weitere Informationen finden Sie unter [Unternehmensoptimierte Apps](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C\# oder Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Zugreifen auf Unternehmensressourcen über das Netzwerk


In diesem Szenario synchronisiert eine optimierte Mail-App eine Reihe von Postfächern, bei denen es sich um eine Mischung aus geschäftlichen und privaten Postfächern handelt. Die App übergibt die Identität des Benutzers an einen Aufruf von [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025), um einen geschützten Threadkontext zu erstellen. Hierdurch werden alle Netzwerkverbindungen markiert, die mit dieser Identität anschließend auf demselben Thread hergestellt werden, und der durch die Unternehmensrichtlinie gesteuerte Zugriff auf Unternehmensnetzwerkressourcen wird ermöglicht.

Der Begriff „Unternehmen“ bezieht sich hierbei auf das Unternehmen, zu dem die Benutzeridentität gehört. [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) gibt unabhängig von der Richtliniendurchsetzung ein [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029)-Objekt zurück. Wenn die App auf die Behandlung gemischter Ressourcen vorbereitet ist, kann sie sich dafür entscheiden, **CreateCurrentThreadNetworkContext** für alle Identitäten aufzurufen. Nach dem Abrufen von Netzwerkressourcen ruft die App **Dispose** für das **ThreadNetworkContext**-Objekt auf, um alle Identitätsmarkierungen aus dem aktuellen Thread zu löschen. Es hängt von Ihrer Programmiersprache ab, welches Muster Sie zum Verwerfen des Kontextobjekts verwenden.

Falls die Identität nicht bekannt ist, kann die App die per Unternehmensrichtlinie verwaltete Identität über die Netzwerkadresse der Ressource mit der [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027)-API abfragen.

**Hinweis**  Wie im Codebeispiel zu sehen ist, besteht das richtige Verwendungsmuster für  [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) darin, den Geltungszeitraum auf ein Minimum zu beschränken. Sie sollten den Kontext des Unternehmensnetzwerks festlegen, Netzwerkverbindungen erstellen, den Kontext zurücksetzen, Verbindungen verwenden und dann schließen. Im folgenden Codebeispiel werden die Details veranschaulicht. Beachten Sie, dass Sie keine Dateien für einen Thread erstellen können, während der Unternehmensnetzwerkkontext für diesen Thread festgelegt ist. Dadurch würde die Datei automatisch verschlüsselt werden, und zwar unabhängig davon, ob die Datei personenbezogen sein soll. Dies ist einer der Gründe, warum das Zurücksetzen des Kontexts zum frühestmöglichen Zeitpunkt empfohlen wird.



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**Hinweis**  Dieser Artikel ist für Windows 10-Entwickler bestimmt, die Apps für die universelle Windows-Plattform (UWP) schreiben. Wenn Sie für Windows 8.x oder Windows Phone 8.x entwickeln, hilft Ihnen die [archivierte Dokumentation](http://go.microsoft.com/fwlink/p/?linkid=619132) weiter.



## Verwandte Themen


[Unternehmensdatenschutz (EDP) – Beispiel](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData-Namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=Mar16_HO5-->


