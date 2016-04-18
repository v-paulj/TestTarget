---
title: Hinzufügen von Sound
description: In diesem Schritt untersuchen wir, wie das Beispielshooterspiel mit den XAudio2-APIs ein Objekt für die Soundwiedergabe erstellt.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
---

# Hinzufügen von Sound


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

In diesem Schritt untersuchen wir, wie das Beispielshooterspiel mit den [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)-APIs ein Objekt für die Soundwiedergabe erstellt.

## Ziel


-   So fügen Sie mit [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) eine Soundausgabe hinzu.

Im Beispielspiel sind die Audio-Objekte und -Verhalten in drei Dateien definiert:

-   **Audio.h/.cpp**. Diese Codedatei definiert das **Audio**-Objekt, das die XAudio2-Ressourcen für die Soundwiedergabe enthält. Außerdem definiert sie die Methode zum Anhalten und Fortsetzen der Audiowiedergabe, wenn das Spiel angehalten oder deaktiviert wurde.
-   **MediaReader.h/.cpp**. Dieser Code definiert die Methoden zum Lesen von WAV-Audiodateien aus einem lokalen Speicher.
-   **SoundEffect.h/.cpp**. Dieser Code definiert ein Objekt für die Soundwiedergabe im Spiel.

## Definieren des Audiomoduls


Wenn das Beispielspiel gestartet wird, erstellt es ein **Audio**-Objekt, das die Audioressourcen für das Spiel zuordnet. Der Code zum Deklarieren dieses Objekts sieht wie folgt aus:

```cpp
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Die Methoden **Audio::MusicEngine** und **Audio::SoundEffectEngine** geben Verweise auf [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Objekte zurück, die die Mastering Voice für jeden Audiotyp festlegen. Eine Mastering Voice ist das für die Wiedergabe verwendete Audiogerät. Sounddatenpuffer können nicht direkt an Mastering Voices übermittelt werden, an andere Voice-Typen übermittelte Daten müssen aber an eine Mastering Voice umgeleitet werden, damit sie gehört werden können.

## Initialisieren der Audioressourcen


Im Beispiel werden die [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Objekte für die Musik- und Soundeffektmodule mit [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212)-Aufrufen initialisiert. Nachdem die Module instanziiert wurden, wird wie hier gezeigt mit [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048)-Aufrufen eine Mastering Voice für jedes Modul erstellt:

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

Wenn eine Musik- oder Soundeffekt-Audiodatei geladen wird, ruft diese Methode [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607) für die Mastering Voice auf, die eine Instanz der Source Voice für die Wiedergabe erstellt. Diesen Code werden wir uns ansehen, nachdem wir uns damit befasst haben, wie das Spiel Audiodateien lädt.

## Lesen einer Audiodatei


Im Beispielspiel ist der Code zum Lesen von Dateien im Audioformat in **MediaReader.cpp** definiert. Die spezifische Methode zum Lesen einer codierten WAV-Audiodatei (**MediaReader::LoadMedia**) sieht wie folgt aus:

```cpp
Platform::Array<byte>^  MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
        );

    // Get the complete WAVEFORMAT from the Media Type.
    Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
        );

    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    // Get the total length of the stream in bytes.
    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &propVariant)
        );
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes;

    double durationInSeconds = (duration / static_cast<double>(10000 * 1000));
    maxStreamLengthInBytes = static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

    // Make the length a multiple of 4 bytes.
    maxStreamLengthInBytes = (maxStreamLengthInBytes + 3) / 4 * 4;

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        DX::ThrowIfFailed(
            reader->ReadSample(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, nullptr, &flags, nullptr, &sample)
            );

        if (sample != nullptr)
        {
            DX::ThrowIfFailed(
                sample->ConvertToContiguousBuffer(&mediaBuffer)
                );

            BYTE *audioData = nullptr;
            DWORD sampleBufferLength = 0;
            DX::ThrowIfFailed(
                mediaBuffer->Lock(&audioData, nullptr, &sampleBufferLength)
                );

            for (DWORD i = 0; i < sampleBufferLength; i++)
            {
                fileData[positionInData++] = audioData[i];
            }
        }
        if (flags & MF_SOURCE_READERF_ENDOFSTREAM)
        {
            done = true;
        }
    }

    // Fix up the array size on match the actual length.
    Platform::Array<byte>^ realfileData = ref new Platform::Array<byte>((positionInData + 3) / 4 * 4);
    memcpy(realfileData->Data, fileData->Data, positionInData);
    return realfileData;
}
```

Diese Methode verwendet die [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)-APIs, um die WAV-Audiodatei als Pulse Code Modulation (PCM)-Puffer einzulesen.

1.  Sie erstellt einen Medienquellenleser ([**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655)), indem sie [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110) aufruft.
2.  Sie erstellt einen Medientyp ([**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850)) für die Decodierung der Audiodatei, indem sie [**MFCreateMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms693861) aufruft. Diese Methode legt fest, dass es sich bei der decodierten Ausgabe um PCM-Audio handelt – ein von XAudio2 unterstützter Audiotyp.
3.  Sie legt den decodierten Ausgabemedientyp für den Leser fest, indem sie [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432) aufruft.
4.  Sie erstellt einen [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799)-Puffer und kopiert die Ergebnisse eines [**IMFMediaType::MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177)-Aufrufs für das [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850)-Objekt. Dadurch wird der Puffer formatiert, der die Audiodatei nach dem Laden enthält.
5.  Sie ruft die Dauer (in Sekunden) des Audiodatenstroms ab, indem sie [**IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662) aufruft, und konvertiert die Dauer dann in Bytes.
6.  Sie liest die Audiodatei als Datenstrom, indem sie [**IMFSourceReader::ReadSample**](https://msdn.microsoft.com/library/windows/desktop/dd374665) aufruft.
7.  Sie kopiert den Inhalt des Audiosample-Puffers in ein von der Methode zurückgegebenes Array.

Das Wichtigste bei **SoundEffect::Initialize** ist die Erstellung des Source Voice-Objekts (**m\_sourceVoice**) auf der Grundlage der Mastering Voice. Die Source Voice wird für die eigentliche Wiedergabe des von **MediaReader::LoadMedia** abgerufenen Sounddatenpuffers verwendet.

Im Beispielspiel wird die Methode beim Initialisieren des **SoundEffect**-Objekts wie folgt aufgerufen:

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

Wie Sie sehen, werden an diese Methode die Ergebnisse der Aufrufe von **Audio::SoundEffectEngine** (oder **Audio::MusicEngine**), **MediaReader::GetOutputWaveFormatEx** und der von einem **MediaReader::LoadMedia**-Aufruf zurückgegebene Puffer übergeben.

```cpp
MediaReader^ mediaReader = ref new MediaReader;
auto targetHitSound = mediaReader->LoadMedia("hit.wav");
```

```cpp
myTarget->HitSound(ref new SoundEffect());
myTarget->HitSound()->Initialize(
                m_audioController->SoundEffectEngine(),
                mediaReader->GetOutputWaveFormatEx(),
                targetHitSound);
```

**SoundEffect::Initialize** wird von der **Simple3DGame:Initialize**-Methode aufgerufen, die das Hauptspielobjekt initialisiert.

Nachdem der Speicher des Beispielspiels nun eine Audiodatei enthält, können wir uns ansehen, wie die Datei während des Spiels wiedergegeben wird.

## Wiedergeben einer Audiodatei


```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

Für die Soundwiedergabe verwendet diese Methode das Source Voice-Objekt **m\_sourceVoice**, um die Wiedergabe des Sounddatenpuffers **m\_soundData** zu starten. Sie erstellt einen [**XAUDIO2\_BUFFER**](https://msdn.microsoft.com/library/windows/desktop/ee419228), für den sie einen Verweis auf den Sounddatenpuffer bereitstellt, und übermittelt ihn dann mit einem Aufruf von [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473). Wenn die Sounddaten in die Warteschlange eingereiht sind, startet **SoundEffect::PlaySound** die Wiedergabe durch einen Aufruf von [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471).

Wenn die Munition jetzt ein Ziel trifft, wird durch einen Aufruf von **SoundEffect::PlaySound** ein Geräusch wiedergegeben.

## Nächste Schritte


Das war eine Blitzeinführung in die Entwicklung von DirectX-Spielen für die universelle Windows-Plattform (UWP). Sie sollten nun wissen, was Sie tun müssen, um selbst ein großartiges Spiel für Windows 8 zu entwickeln. Denken Sie daran, dass Ihr Spiel auf vielen unterschiedlichen Windows 8-Geräten und -Plattformen gespielt werden kann. Entwerfen Sie daher alle Komponenten – Grafik, Steuerelemente, Benutzeroberfläche und Sound – für so viele Konfigurationen wie möglich.

Weitere Informationen zum Ändern des Beispielspiels in diesen Dokumenten finden Sie unter [Erweitern des Beispielspiels](tutorial-resources.md).

## Vollständiger Beispielcode für diesen Abschnitt


Audio.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    bool                                m_audioAvailable;
    Microsoft::WRL::ComPtr<IXAudio2>    m_musicEngine;
    Microsoft::WRL::ComPtr<IXAudio2>    m_soundEffectEngine;
    IXAudio2MasteringVoice*             m_musicMasteringVoice;
    IXAudio2MasteringVoice*             m_soundEffectMasteringVoice;
};
```

Audio.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "Audio.h"
#include "DirectXSample.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

Audio::Audio():
    m_audioAvailable(false)
{
}

void Audio::Initialize()
{
}

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

#if defined(_DEBUG)
    XAUDIO2_DEBUG_CONFIGURATION debugConfiguration = {0};
    debugConfiguration.BreakMask = XAUDIO2_LOG_ERRORS;
    debugConfiguration.TraceMask = XAUDIO2_LOG_ERRORS;
    m_musicEngine->SetDebugConfiguration(&debugConfiguration);
#endif


    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

#if defined(_DEBUG)
    m_soundEffectEngine->SetDebugConfiguration(&debugConfiguration);
#endif

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}

IXAudio2* Audio::MusicEngine()
{
    return m_musicEngine.Get();
}

IXAudio2* Audio::SoundEffectEngine()
{
    return m_soundEffectEngine.Get();
}

void Audio::SuspendAudio()
{
    if (m_audioAvailable)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
}

void Audio::ResumeAudio()
{
    if (m_audioAvailable)
    {
        DX::ThrowIfFailed(m_musicEngine->StartEngine());
        DX::ThrowIfFailed(m_soundEffectEngine->StartEngine());
    }
}
```

SoundEffect.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

ref class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected:
    bool                    m_audioAvailable;
    IXAudio2SourceVoice*    m_sourceVoice;
    Platform::Array<byte>^  m_soundData;
};
```

SoundEffect.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "SoundEffect.h"
#include "DirectXSample.h"

SoundEffect::SoundEffect():
    m_audioAvailable(false)
{
}
//----------------------------------------------------------------------
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available, so return.
        m_audioAvailable = false;
        return;
    }

    // Create and reuse a single source voice for the single sound effect in this sample.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
//----------------------------------------------------------------------
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

 

 






<!--HONumber=Mar16_HO1-->


