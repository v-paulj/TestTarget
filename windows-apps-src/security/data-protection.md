---
title: Datenschutz
description: "In diesem Artikel wird erläutert, wie Sie mithilfe der DataProtectionProvider-Klasse im Windows.Security.Cryptography.DataProtection-Namespace digitale Daten in einer UWP-App verschlüsseln und entschlüsseln können."
ms.assetid: 9EE3CC45-5C44-4196-BD8B-1D64EFC5C509
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: 2a3be202f6754c452185aecbae3b259e09bd3fd3

---

# Datenschutz


\[ Aktualisiert für UWP-Apps unter Windows10. Artikel zu Windows8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

In diesem Artikel wird erläutert, wie Sie mithilfe der [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559)-Klasse im [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585)-Namespace digitale Daten in einer UWP-App verschlüsseln und entschlüsseln können.

Sie können die Datenschutz-APIs auf verschiedene Weise verwenden:

-   Zum Schützen von Daten durch einen Active Directory-Sicherheitsprinzipal (AD), wie z. B. eine AD-Gruppe. Jedes Mitglied der Gruppe kann die Daten entschlüsseln.
-   Zum Schützen von Daten durch einen öffentlichen Schlüssel, der in einem X.509-Zertifikat enthalten ist. Der Besitzer eines privaten Schlüssels kann die Daten entschlüsseln.
-   Zum Schützen von Daten mithilfe eines symmetrischen Schlüssels. Damit können zum Beispiel die Daten eines Nicht-AD-Prinzipals wie Live ID geschützt werden.
-   Zum Schützen von Daten anhand von Anmeldeinformationen (Kennwort), die zum Anmelden bei einer Website erforderlich sind.

Um Daten zu schützen, müssen Sie beim Erstellen eines [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559)-Objekts einen Schutzdeskriptor angeben, bevor Sie [**ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/br241563) oder [**ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/br241564) aufrufen. Im Folgenden sehen Sie Beispiele für mögliche Schutzdeskriptoren.

## Schützen statischer Daten


Das folgende Beispiel zeigt, wie Sie mithilfe der Methoden [**ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/br241563) und [**UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/br241565) statische Daten für die SID des aktuellen Benutzers asynchron schützen können.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectAsync
{
    sealed partial class StaticDataProtectionApp : Application
    {
        public StaticDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect data asynchronously.
            this.Protect();
        }

        public async void Protect()
        {
            // Initialize function arguments.
            String strMsg = "This is a message to be protected.";
            String strDescriptor = "LOCAL=user";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;

            // Protect a message to the local user.
            IBuffer buffProtected = await this.SampleProtectAsync(
                strMsg,
                strDescriptor,
                encoding);

            // Decrypt the previously protected message.
            String strDecrypted = await this.SampleUnprotectData(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleProtectAsync(
            String strMsg,
            String strDescriptor,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(strDescriptor);

            // Encode the plaintext input message to a buffer.
            encoding = BinaryStringEncoding.Utf8;
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Encrypt the message.
            IBuffer buffProtected = await Provider.ProtectAsync(buffMsg);

            // Execution of the SampleProtectAsync function resumes here
            // after the awaited task (Provider.ProtectAsync) completes.
            return buffProtected;
        }

        public async Task<String> SampleUnprotectData(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Decrypt the protected message specified on input.
            IBuffer buffUnprotected = await Provider.UnprotectAsync(buffProtected);

            // Execution of the SampleUnprotectData method resumes here
            // after the awaited task (Provider.UnprotectAsync) completes
            // Convert the unprotected message from an IBuffer object to a string.
            String strClearText = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotected);

            // Return the plaintext string.
            return strClearText;
        }
    }
}
```

## Schützen von Datenstromdaten


Das folgende Beispiel zeigt, wie Sie mithilfe der Methoden [**ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/br241564) und [**UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/br241566) Datenstromdaten für die SID des aktuellen Benutzers asynchron schützen können.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectStreamAsync
{

    sealed partial class StreamDataProtectionApp : Application
    {
        public StreamDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect a stream synchronously
            this.ProtectData();
         }

        public async void ProtectData()
        {
            // Initialize function arguments.
            String strDescriptor = "LOCAL=user";
            String strLoremIpsum = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse elementum "
                + "ullamcorper eros, vitae gravida nunc consequat sollicitudin. Vivamus lacinia, "
                + "diam a molestie porttitor, sapien neque volutpat est, non suscipit leo dolor "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "porttitor massa, at convallis dolor dictum suscipit. Nullam vitae lectus in "
                + "lorem scelerisque convallis sed scelerisque orci. Praesent sed ligula vel erat "
                + "eleifend tempus. Nullam dignissim aliquet mauris a aliquet. Nulla augue justo, "
                + "posuere a consectetur ut, suscipit et sem. Proin eu libero ut felis tincidunt "
                + "interdum. Curabitur vulputate eros nec sapien elementum ut dapibus eros "
                + "dapibus. Suspendisse quis dui dolor, non imperdiet leo. In consequat, odio nec "
                + "aliquam tincidunt, magna enim ultrices massa, ac pharetra est urna at arcu. "
                + "Nunc suscipit, velit non interdum suscipit, lectus lectus auctor tortor, quis "
                + "ultrices orci felis in dolor. Etiam congue pretium libero eu vestibulum. "
                + "Mauris bibendum erat eleifend nibh consequat eu pharetra metus convallis. "
                + "Morbi sem eros, venenatis vel vestibulum consequat, hendrerit rhoncus purus.";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf16BE;

            // Encrypt the data as a stream.
            IBuffer buffProtected = await this.SampleDataProtectionStream(
                strDescriptor,
                strLoremIpsum,
                encoding);

            // Decrypt a data stream.
            String strUnprotected = await this.SampleDataUnprotectStream(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleDataProtectionStream(
            String descriptor,
            String strMsg,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(descriptor);

            // Convert the input string to a buffer.
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a random access stream to contain the plaintext message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream protectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (plaintext) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffMsg);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with encrypted data.
            IOutputStream dest = protectedData.GetOutputStreamAt(0);
            await Provider.ProtectStreamAsync(source, dest);
            await dest.FlushAsync();

            //Verify that the protected data does not match the original
            DataReader reader1 = new DataReader(inputData.GetInputStreamAt(0));
            DataReader reader2 = new DataReader(protectedData.GetInputStreamAt(0));
            await reader1.LoadAsync((uint)inputData.Size);
            await reader2.LoadAsync((uint)protectedData.Size);
            IBuffer buffOriginalData = reader1.ReadBuffer((uint)inputData.Size);
            IBuffer buffProtectedData = reader2.ReadBuffer((uint)protectedData.Size);

            if (CryptographicBuffer.Compare(buffOriginalData, buffProtectedData))
            {
                throw new Exception("ProtectStreamAsync returned unprotected data");
            }

            // Return the encrypted data.
            return buffProtectedData;
        }

        public async Task<String> SampleDataUnprotectStream(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the decrypted data.
            InMemoryRandomAccessStream unprotectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (encrypted) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffProtected);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input (encrypted) data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with decrypted data.
            IOutputStream dest = unprotectedData.GetOutputStreamAt(0);
            await Provider.UnprotectStreamAsync(source, dest);
            await dest.FlushAsync();

            // Write the decrypted data to an IBuffer object.
            DataReader reader2 = new DataReader(unprotectedData.GetInputStreamAt(0));
            await reader2.LoadAsync((uint)unprotectedData.Size);
            IBuffer buffUnprotectedData = reader2.ReadBuffer((uint)unprotectedData.Size);

            // Convert the IBuffer object to a string using the same encoding that was
            // used previously to conver the plaintext string (before encryption) to an
            // IBuffer object.
            String strUnprotected = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotectedData);

            // Return the decrypted data.
            return strUnprotected;
        }
    }
}
```


<!--HONumber=Aug16_HO3-->


