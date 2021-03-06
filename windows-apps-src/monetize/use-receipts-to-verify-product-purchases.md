---
author: mcleanbyron
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: "Jede WindowsStore-Transaktion, die zu einem erfolgreichen Produktkauf führt, kann optional einen Transaktionsbeleg zurückgeben."
title: "Überprüfen von Produktkäufen anhand von Belegen"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 01b75d25c385d8dd856af79581fb4a346064c400

---

# Überprüfen von Produktkäufen anhand von Belegen




>**Hinweis**&nbsp;&nbsp;In den Beispielen in diesem Artikel werden Member des [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) Namespace verwendet. Wenn Ihre App auf Windows10, Version 1607 oder höher, ausgerichtet ist, wird empfohlen, Member des [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) -Namespace und nicht den Windows.ApplicationModel.Store-Namespace zur Verwaltung von In-App-Einkäufen zu verwenden. Weitere Informationen finden Sie unter [In-App-Einkäufe und Testversionen](in-app-purchases-and-trials.md).

**Wichtige APIs**

-   [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
-   [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)

Jede WindowsStore-Transaktion, die zu einem erfolgreichen Produktkauf führt, kann optional einen Transaktionsbeleg zurückgeben. Dieser Beleg enthalten Informationen zum gelisteten Produkt und zu den Kosten für den Kunden.

Der Zugriff auf diese Informationen unterstützt Szenarien, in denen Ihre App überprüfen muss, ob ein Benutzer Ihre App erworben oder In-App-Produktkäufe im WindowsStore getätigt hat. Das kann zum Beispiel bei einem Spiel der Fall sein, in dem Inhalte heruntergeladen wurden. Wenn der Benutzer, der die Spielinhalte gekauft hat, das Spiel auf einem anderen Gerät spielen möchte, müssen Sie überprüfen, ob der Benutzer die Inhalte bereits besitzt. Gehen Sie dazu wie folgt vor:

## Anfordern eines Belegs


Der **Windows.ApplicationModel.Store**-Namespace unterstützt zwei Methoden zum Anfordern eines Belegs: mithilfe der [**CurrentApp.RequestProductPurchaseAsync | requestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381)- oder [**CurrentApp.RequestAppPurchaseAsync | requestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)-Methode und des *includeReceipt*-Parameters oder durch Aufrufen der [**CurrentApp.GetAppReceiptAsync | getAppReceiptAsync**](https://msdn.microsoft.com/library/windows/apps/hh967811)-Methode. Ein App-Beleg sieht ungefähr so aus:

```XML
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Ein Produktbeleg sieht so aus:

```XML
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Sie können beide Belegbeispiele verwenden, um den Überprüfungscode zu testen.

## Überprüfen eines Belegs


Wenn Sie einen Beleg erhalten haben, muss dieser von Ihrem Back-End-System (einem Webdienst oder einer vergleichbaren Einrichtung) überprüft werden. Hier sehen Sie ein .NET Framework-Beispiel dieses Prüfprozesses.

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Security.Cryptography;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IO;
using System.Security.Cryptography.Xml;
using System.Net;

namespace ReceiptVerificationSample
{
        public sealed class RSAPKCS1SHA256SignatureDescription : SignatureDescription
        {
            public RSAPKCS1SHA256SignatureDescription()
            {
                base.KeyAlgorithm = typeof(RSACryptoServiceProvider).FullName;
                base.DigestAlgorithm = typeof(SHA256Managed).FullName;
                base.FormatterAlgorithm = typeof(RSAPKCS1SignatureFormatter).FullName;
                base.DeformatterAlgorithm = typeof(RSAPKCS1SignatureDeformatter).FullName;
            }

            public override AsymmetricSignatureDeformatter CreateDeformatter(AsymmetricAlgorithm key)
            {
                if (key == null)
                {
                    throw new ArgumentNullException("key");
                }

                RSAPKCS1SignatureDeformatter deformatter = new RSAPKCS1SignatureDeformatter(key);
                deformatter.SetHashAlgorithm("SHA256");
                return deformatter;
            }

            public override AsymmetricSignatureFormatter CreateFormatter(AsymmetricAlgorithm key)
            {
                if (key == null)
                {
                    throw new ArgumentNullException("key");
                }

                RSAPKCS1SignatureFormatter formatter = new RSAPKCS1SignatureFormatter(key);
                formatter.SetHashAlgorithm("SHA256");
                return formatter;
            }

        }

        class Program
        {

            // Utility function to read the bytes from an HTTP response
            private static int ReadResponseBytes(byte[] responseBuffer, Stream resStream)
            {
                int count = 0;

                int numBytesRead = 0;
                int numBytesToRead = responseBuffer.Length;

                do
                {
                    count = resStream.Read(responseBuffer, numBytesRead, numBytesToRead);

                    numBytesRead += count;
                    numBytesToRead -= count;

                } while (count > 0);

                return numBytesRead;
            }

            public static X509Certificate2 RetrieveCertificate(string certificateId)
            {
                const int MaxCertificateSize = 10000;

                // We are attempting to retrieve the following url. The getAppReceiptAsync website at
                // http://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getappreceiptasync.aspx
                // lists the following format for the certificate url.
                String certificateUrl = String.Format("https://go.microsoft.com/fwlink/?LinkId=246509&cid={0}", certificateId);

                // Make an HTTP GET request for the certificate
                HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(certificateUrl);
                request.Method = "GET";

                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Retrieve the certificate out of the response stream
                byte[] responseBuffer = new byte[MaxCertificateSize];
                Stream resStream = response.GetResponseStream();
                int bytesRead = ReadResponseBytes(responseBuffer, resStream);

                if (bytesRead < 1)
                {
                    //TODO: Handle error here
                }

                return new X509Certificate2(responseBuffer);
            }

            static bool ValidateXml(XmlDocument receipt, X509Certificate2 certificate)
            {
                // Create the signed XML object.
                SignedXml sxml = new SignedXml(receipt);

                // Get the XML Signature node and load it into the signed XML object.
                XmlNode dsig = receipt.GetElementsByTagName("Signature", SignedXml.XmlDsigNamespaceUrl)[0];
                if (dsig == null)
                {
                    // If signature is not found return false
                    System.Console.WriteLine("Signature not found.");
                    return false;
                }

                sxml.LoadXml((XmlElement)dsig);

                // Check the signature
                bool isValid = sxml.CheckSignature(certificate, true);

                return isValid;
            }

            static void Main(string[] args)
            {
                // .NET does not support SHA256-RSA2048 signature verification by default, so register this algorithm for verification
                CryptoConfig.AddAlgorithm(typeof(RSAPKCS1SHA256SignatureDescription), "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256");

                // Load the receipt that needs to be verified as an XML document
                XmlDocument xmlDoc = new XmlDocument();
                xmlDoc.Load("..\\..\\receipt.xml");

                // The certificateId attribute is present in the document root, retrieve it
                XmlNode node = xmlDoc.DocumentElement;
                string certificateId = node.Attributes["CertificateId"].Value;

                // Retrieve the certificate from the official site.
                // NOTE: For sake of performance, you would want to cache this certificate locally.
                //       Otherwise, every single call will incur the delay of certificate retrieval.
                X509Certificate2 verificationCertificate = RetrieveCertificate(certificateId);

                try
                {
                    // Validate the receipt with the certificate retrieved earlier
                    bool isValid = ValidateXml(xmlDoc, verificationCertificate);
                    System.Console.WriteLine("Certificate valid: " + isValid);
                }
                catch (Exception ex)
                {
                    System.Console.WriteLine(ex.ToString());
                }
            }
        }
}
```

 

 



<!--HONumber=Aug16_HO5-->


