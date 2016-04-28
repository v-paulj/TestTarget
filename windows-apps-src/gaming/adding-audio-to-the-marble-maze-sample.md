---
title: Hinzufügen von Audiodaten zum Marble Maze-Beispiel
description: Hier werden Methoden beschrieben, die Sie bei der Arbeit mit Audio berücksichtigen sollten, und wie diese Methoden in Marble Maze angewendet werden.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
---

# Hinzufügen von Audiodaten zum Marble Maze-Beispiel


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


In diesem Dokument werden die wichtigsten Methoden beschrieben, die Sie berücksichtigen sollten, wenn Sie mit Audio arbeiten. Außerdem erfahren Sie, wie diese Methoden in Marble Maze angewendet werden. Marble Maze verwendet Microsoft Media Foundation, um Audioressourcen aus einer Datei zu laden, und XAudio2, um Audio zu mischen und wiederzugeben und Effekte auf Audio anzuwenden.

Marble Maze gibt Musik im Hintergrund wieder und verwendet außerdem Spielsounds, die auf Spielereignisse hinweisen, beispielsweise wenn die Murmel an eine Wand prallt. Wichtig ist bei der Implementierung, dass Marble Maze einen Hall- oder Echoeffekt verwendet, um den Klang einer aufprallenden Murmel zu simulieren. Die Implementierung des Halleffekts bewirkt, dass Sie Echos in kleineren Räumen schneller und lauter hören. In größeren Räumen dagegen sind die Echos leiser und nicht so schnell zu hören.

> **Hinweis**   Den Beispielcode für dieses Dokument finden Sie im [DirectX-Beispielspiel Marble Maze](http://go.microsoft.com/fwlink/?LinkId=624011).

Hier sind einige der wichtigsten in diesem Dokument erörterten Punkte für das Arbeiten mit Audio in Ihrem Spiel:

-   Ziehen Sie die Verwendung von Media Foundation zum Decodieren von Audioressourcen und XAudio2 zum Wiedergeben von Audio in Betracht. Wenn Sie aber bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, können Sie diesen Mechanismus verwenden.
-   Ein Audiodiagramm enthält eine Quellstimme für jeden aktiven Sound, null oder mehr Submixstimmen und eine Masterstimme. Quellstimmen können in Submixstimmen und/oder die Masterstimme eingespeist werden. Submixstimmen können in andere Submixstimmen oder die Masterstimme eingespeist werden.
-   Wenn die Dateien für die Hintergrundmusik groß sind, können Sie die Musik in kleinere Puffer streamen, damit weniger Arbeitsspeicher verbraucht wird.
-   Halten Sie gegebenenfalls die Audiowiedergabe an, wenn die App nicht mehr den Fokus hat, nicht mehr sichtbar ist oder angehalten wird. Setzen Sie die Wiedergabe fort, wenn Ihre App wieder den Fokus erhält, wieder sichtbar wird oder fortgesetzt wird.
-   Legen Sie Audiokategorien fest, die den Rollen der einzelnen Sounds entsprechen. Zum Beispiel verwenden Sie in der Regel **AudioCategory\_GameMedia** für Hintergrundaudio im Spiel und **AudioCategory\_GameEffects** für Soundeffekte.
-   Behandeln Sie Geräteänderungen, einschließlich Kopfhörern, in dem Sie alle Audioressourcen und -schnittstellen freigeben und erneut erstellen.
-   Komprimieren Sie gegebenenfalls Audiodateien, wenn Sie den Speicherplatz und die Streamingkosten minimieren müssen. Anderenfalls können Sie die Audiodateien unkomprimiert lassen, damit sie schneller geladen werden.

## Einführung in XAudio2 und Microsoft Media Foundation


XAudio2 ist eine Low-Level-Audiobibliothek für Windows, die speziell Audio in Spielen unterstützt. Sie enthält eine digitale Signalverarbeitung (Digital Signal Processing, DSP) und ein Audiodiagrammmodul für Spiele. Als Erweiterung der Vorgänger DirectSound und XAudio unterstützt XAudio2 Computertrends wie zum Beispiel SIMD-Gleitkommaarchitekturen und HD-Audio. Außerdem werden die komplexeren Soundverarbeitungsanforderungen aktueller Spiele unterstützt.

Im [Dokument zu den wichtigsten Konzepten von XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415764) werden die wichtigsten Konzepte für die Verwendung von XAudio2 erläutert. Die Konzepte im Kurzüberblick:

-   Die [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Schnittstelle bildet den Kern des XAudio2-Moduls. Marble Maze verwendet diese Schnittstelle, um Stimmen zu erstellen und Benachrichtigungen zu empfangen, wenn sich das Ausgabegerät ändert oder Fehler auftreten.
-   Eine Stimme verarbeitet Audiodaten, passt sie an und gibt sie wieder.
-   Eine Quellstimme ist eine Sammlung von Audiokanälen (Mono, 5.1 usw.) und stellt einen Audiodatenstream dar. In XAudio2 ist eine Quellstimme der Ausgangspunkt der Audioverarbeitung. Normalerweise werden die Sounddaten aus einer externen Quelle wie beispielsweise einer Datei oder einem Netzwerk geladen und an eine Quellstimme gesendet. In Marble Maze werden Sounddaten mithilfe von [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) aus Dateien geladen. Media Foundation wird weiter unten in diesem Dokument vorgestellt.
-   Eine Submixstimme verarbeitet Audiodaten. Bei dieser Verarbeitung kann zum Beispiel der Audiostream geändert werden oder mehrere Streams werden zu einem Stream kombiniert. Marble Maze verwendet Submixe, um den Halleffekt zu erstellen.
-   Eine Masterstimme kombiniert Daten aus Quell- und Submixstimmen und sendet Daten an die Audiohardware.
-   Ein Audiodiagramm enthält eine Quellstimme für jeden aktiven Sound, null oder mehrere Submixstimmen und nur eine Masterstimme .
-   Der Clientcode wird über einen Rückruf informiert, dass in einer Stimme oder in einem Modulobjekt ein Ereignis aufgetreten ist. Mithilfe von Rückrufen können Sie den Arbeitsspeicher wiederverwenden, wenn XAudio2 mit einem Puffer fertig ist, reagieren, wenn sich das Audiogerät ändert (z. B. wenn Sie Kopfhörer anschließen oder trennen) usw. Weiter unten in diesem Dokument erfahren Sie im Abschnitt [Behandeln von Kopfhörern und Geräteänderungen](#phones), wie Marble Maze diesen Mechanismus für die Behandlung von Geräteänderungen verwendet.

Marble Maze verwendet zwei Audiomodule (mit anderen Worten: zwei [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Objekte) zum Verarbeiten von Audio. Ein Modul verarbeitet die Hintergrundmusik und das andere verarbeitet Spielsounds.

Außerdem muss Marble Maze für jedes Modul eine Masterstimme erstellen. Denken Sie daran, dass ein Mastermodul Audiostreams in einem Stream kombiniert und diesen Stream an die Audiohardware sendet. Der Stream für die Hintergrundmusik, eine Quellstimme, gibt Daten an eine Masterstimme und an zwei Submixstimmen aus. Die Submixstimmen führen den Halleffekt aus.

Media Foundation ist eine Multimediabibliothek, die viele Audio- und Videoformate unterstützt. XAudio2 und Media Foundation ergänzen sich gegenseitig. Marble Maze verwendet Media Foundation zum Laden von Audioressourcen aus einer Datei und XAudio2 zum Wiedergeben von Audio. Sie müssen Media Foundation nicht verwenden, um Audioressourcen zu laden. Wenn Sie bereits einen Mechanismus zum Laden von Audioressourcen haben, der in einer UWP-App funktioniert, verwenden Sie diesen Mechanismus.

Weitere Informationen zu XAudio2 finden Sie im [Programmierhandbuch](https://msdn.microsoft.com/library/windows/desktop/ee415737). Weitere Informationen zu Media Foundation finden Sie unter [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

## Initialisieren von Audioressourcen


Marble Maze verwendet eine WMA-Datei (Windows Media Audio) für die Hintergrundmusik und WAV-Dateien für Spielsounds. Diese Formate werden von Media Foundation unterstützt. Obwohl das WAV-Dateiformat nativ von XAudio2 unterstützt wird, muss das Dateiformat von einem Spiel manuell analysiert werden, um die entsprechenden XAudio2-Datenstrukturen auszufüllen. Marble Maze verwendet Media Foundation, um die Arbeit mit WAV-Dateien zu erleichtern. Die vollständige Liste der von Media Foundation unterstützten Medienformate finden Sie unter [Unterstützte Medienformate in Media Foundation](https://msdn.microsoft.com/library/windows/desktop/dd757927). Marble Maze verwendet keine getrennten Entwurfszeit- und Laufzeit-Audioformate und keine Unterstützung für XAudio2-ADPCM-Komprimierung. Weitere Informationen zur ADPCM-Komprimierung in XAudio2 finden Sie in der Übersicht über [ADPCM](https://msdn.microsoft.com/library/windows/desktop/ee415711).

Die **Audio::CreateResources**-Methode, die von **MarbleMaze::CreateDeviceIndependentResources** aufgerufen wird, lädt die Audiostreams aus der Datei, initialisiert die XAudio2-Modulobjekte und erstellt die Quell-, Submix- und Masterstimmen.

###  Erstellen der XAudio2-Module

Bedenken Sie, dass Marble Maze für jedes verwendete Audiomodul ein [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Objekt erstellt. Rufen Sie die [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212)-Funktion auf, um ein Audiomodul zu erstellen. Das folgende Beispiel zeigt, wie Marble Maze das Audiomodul für die Verarbeitung der Hintergrundmusik erstellt.

```cpp
DX::ThrowIfFailed(
    XAudio2Create(&m_musicEngine)
    );
```

Einen ähnlichen Schritt führt Marble Maze aus, um das Audiomodul zu erstellen, das die Spielsounds wiedergibt.

Zwischen der Arbeit mit der [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Schnittstelle in einer UWP-App und der Arbeit mit einer Desktop-App gibt es zwei Unterschiede. Erstens ist es nicht erforderlich, **CoInitializeEx** vor dem Aufrufen von [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212) aufzurufen. Außerdem unterstützt **IXAudio2** die Geräteaufzählung nicht mehr. Weitere Informationen zum Aufzählen von Audiogeräten finden Sie unter [Enumerieren von Geräten](https://msdn.microsoft.com/library/windows/apps/hh464977).

###  Erstellen der Masterstimmen

Das folgende Beispiel zeigt, wie die **Audio::CreateResources**-Methode die Masterstimme für die Hintergrundmusik erstellt. Beim Aufruf von [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048) werden zwei Eingabekanäle angegeben. Damit wird die Logik für den Halleffekt vereinfacht. Mit der **XAUDIO2\_DEFAULT\_SAMPLERATE**-Spezifikation wird das Audiomodul angewiesen, die Abtastrate zu verwenden, die in der Systemsteuerungsoption für Sound angegeben ist. In diesem Beispiel ist **M\_musicMasteringVoice** ein [**IXAudio2MasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415912)-Objekt.

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice, 
        2, 
        48000, 
        0, 
        nullptr, 
        nullptr, 
        AudioCategory_GameMedia
        )
);
```

Die **Audio::CreateResources**-Methode führt einen ähnlichen Schritt aus, um die Masterstimme für die Spielsounds zu erstellen. Allerdings wird dabei der Standardwert **AudioCategory\_GameEffects** für den *StreamCategory*-Parameter angegeben. Marble Maze gibt **AudioCategory\_GameMedia** für Hintergrundmusik an, damit Benutzer bei der Verwendung des Spiels Musik aus einer anderen Anwendung hören können. Bei der Wiedergabe aus einer Musik-App schaltet Windows alle mit der Option **AudioCategory\_GameMedia** erstellten Stimmen stumm. Der Benutzer hört dennoch die Sounds im Spiel, da diese mit der Option **AudioCategory\_GameEffects** erstellt werden. Weitere Informationen zu Audiokategorien finden Sie unter „[**AUDIO\_STREAM\_CATEGORY**](https://msdn.microsoft.com/library/windows/desktop/hh404178)-Enumeration“.

###  Erstellen des Halleffekts

Sie können für jede Stimme mit XAudio2 Effektsequenzen erstellen, die Audio verarbeiten. Eine solche Sequenz wird als Effektkette bezeichnet. Verwenden Sie Effektketten, wenn Sie mindestens einen Effekt auf eine Stimme anwenden möchten. Effektketten können destruktiv sein, d. h., jeder Effekt in der Kette kann den Audiopuffer überschreiben. Diese Eigenschaft ist wichtig, da XAudio2 nicht garantiert, dass Ausgabepuffer lautlos initialisiert werden. Effektobjekte werden in XAudio2 durch plattformübergreifende Audioverarbeitungsobjekte (Audio Processing Object, XAPO) dargestellt. Weitere Informationen zu XAPO finden Sie in den [Übersichten über XAPO](o:microsoft.directx_sdk.xapo.audio_overview_xapo).

Führen Sie beim Erstellen einer Effektkette die folgenden Schritte aus:

1.  Erstellen Sie das Effektobjekt.
2.  Füllen Sie eine [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236)-Struktur mit Effektdaten auf.
3.  Füllen Sie eine [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235)-Struktur mit Effektdaten auf.
4.  Wenden Sie die Effektkette auf eine Stimme an.
5.  Füllen Sie eine Effektparameterstruktur auf und wenden Sie sie auf den Effekt an.
6.  Deaktivieren oder aktivieren Sie den Effekt nach Bedarf.

Die **Audio**-Klasse definiert die **CreateReverb**-Methode, um die Effektkette zu erstellen, die den Halleffekt implementiert. Diese Methode ruft die [**XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213)-Funktion auf, um ein [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915)-Objekt zu erstellen, das als Submixstimme für den Halleffekt fungiert.

```cpp
DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

Die [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236)-Struktur enthält Informationen zu einem XAPO, das in einer Effektkette verwendet werden soll, zum Beispiel die Zielanzahl der Ausgabekanäle. Die **Audio::CreateReverb**-Methode erstellt ein **XAUDIO2\_EFFECT\_DESCRIPTOR**-Objekt, das auf den deaktivierten Zustand festgelegt wird, verwendet zwei Ausgabekanäle und verweist auf das [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915)-Objekt für den Halleffekt. Das **XAUDIO2\_EFFECT\_DESCRIPTOR**-Objekt beginnt mit dem deaktivierten Zustand, da das Spiel Parameter festlegen muss, bevor der Effekt mit dem Ändern der Spielsounds beginnt. Marble Maze verwendet zwei Ausgabekanäle, um die Logik für den Halleffekt zu vereinfachen.

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Wenn Ihre Effektkette mehrere Effekte enthält, benötigen Sie für jeden Effekt ein Objekt. Die [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235)-Struktur enthält das Array der am Effekt beteiligten [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236)-Objekte. Das folgende Beispiel zeigt, wie die **Audio::CreateReverb**-Methode den einen Effekt für die Implementierung des Halleffekts angibt.

```cpp
soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

Die **Audio::CreateReverb**-Methode ruft die [**IXAudio2::CreateSubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418608)-Methode auf, um die Submixstimme für den Effekt zu erstellen. Sie gibt das [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235)-Objekt für den *pEffectChain*-Parameter an, um die Effektkette der Stimme zuzuordnen. Außerdem gibt Marble Maze zwei Ausgabekanäle und eine Abtastrate von 48 Kilohertz an. Diese Abtastrate wurde ausgewählt, da sie ein ausgewogenes Verhältnis von Audioqualität und Umfang der erforderlichen CPU-Verarbeitung darstellt. Eine höhere Abtastrate würde mehr CPU-Verarbeitung erfordern, ohne eine merkliche Qualitätssteigerung zu bewirken.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> **Tipp**   Wenn Sie eine vorhandene Effektkette an eine vorhandene Submixstimme anfügen oder die aktuelle Effektkette ersetzen möchten, verwenden Sie die [**IXAudio2Voice::SetEffectChain**](https://msdn.microsoft.com/library/windows/desktop/ee418594)-Methode.

 

Die [**Audio::XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213)-Methode ruft [**IXAudio2Voice::SetEffectParameters**](https://msdn.microsoft.com/library/windows/desktop/ee418595) auf, um zusätzliche dem Effekt zugeordnete Parameter festzulegen. Diese Methode akzeptiert eine für den Effekt spezifische Parameterstruktur. Ein [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224)-Objekt, das die Effektparameter für den Halleffekt enthält, wird in der **Audio::Initialize**-Methode initialisiert, da alle Halleffekte die gleichen Parameter verwenden. Das folgende Beispiel zeigt, wie die **Audio::Initialize**-Methode die Hallparameter für Nahfeld-Halleffekte initialisiert.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

Dieses Beispiel verwendet die Standardwerte für die meisten Hallparameter, legt aber **DisableLateField** auf TRUE fest, um den Nahfeld-Halleffekt anzugeben, **EarlyDiffusion** auf 4, um flache Oberflächen in der Nähe zu simulieren, und **LateDiffusion** auf 15, um sehr diffuse entfernte Oberflächen zu simulieren. Flache Oberflächen in der Nähe verursachen Echos, die Sie schneller und lauter hören. Diffuse entfernte Oberflächen verursachen leisere und nicht so schnell hörbare Echos. Sie können mit den Halleffektwerten experimentieren, um in Ihrem Spiel den gewünschten Effekt zu erzielen, oder die **ReverbConvertI3DL2ToNative**-Funktion verwenden, um I3DL2-Parameter (Interactive 3D Audio Rendering Guidelines Level 2.0) nach Branchenstandard zu verwenden.

Das folgende Beispiel zeigt, wie **Audio::CreateReverb** die Hallparameter festlegt. Der parameters-Parameter ist ein [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224)-Objekt.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

Die **Audio::CreateReverb**-Methode aktiviert zum Schluss den Effekt, wenn das **enableEffect**-Kennzeichen festgelegt ist. Außerdem legt sie die Lautstärke- und Ausgabematrix fest. Mit diesem Teil wird die volle Lautstärke (1.0) festgelegt und dann die Lautstärkematrix so angegeben, dass die linke und die rechte Eingabe sowie der linke und der rechte Ausgabelautsprecher stumm sind. Dies geschieht, da später anderer Code zwischen den beiden Halleffekten überblendet (und dabei den Übergang von der Nähe zu einer Wand zu einem großen Raum simuliert) oder bei Bedarf beide Halleffekte stummschaltet. Wenn die Stummschaltung des Hallpfads später aufgehoben wird, legt das Spiel die Matrix {1.0f, 0.0f, 0.0f, 1.0f} fest, um die linke Hallausgabe an die linke Eingabe der Masterstimme und die rechte Hallausgabe an die rechte Eingabe der Masterstimme weiterzuleiten.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze ruft die **CreateReverb**-Methode viermal auf: zweimal für die Hintergrundmusik und zweimal für die Spielsounds. Das folgende Beispiel zeigt, wie Marble Maze die **CreateReverb**-Methode für die Hintergrundmusik aufruft.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Eine Liste der möglichen Quellen für Effekte, die Sie mit XAudio2 verwenden können, finden Sie unter [XAudio2-Audioeffekte](https://msdn.microsoft.com/library/windows/desktop/ee415756).

### Laden von Audiodaten aus einer Datei

Marble Maze definiert die **MediaStreamer**-Klasse, die Media Foundation zum Laden von Audioressourcen aus einer Datei verwendet. Marble Maze verwendet zum Laden jeder Audiodatei ein **MediaStreamer**-Objekt.

Marble Maze ruft die **MediaStreamer::Initialize**-Methode auf, um die einzelnen Audiostreams zu initialisieren. So ruft die **Audio::CreateResources**-Methode **MediaStreamer::Initialize** auf, um den Audiostream für die Hintergrundmusik zu initialisieren:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

Die **MediaStreamer::Initialize**-Methode ruft zunächst die [**MFStartup**](https://msdn.microsoft.com/library/windows/desktop/ms702238)-Funktion auf, um Media Foundation zu initialisieren.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

**MediaStreamer::Initialize** ruft dann [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110) auf, um ein [**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655)-Objekt zu erstellen. Ein **IMFSourceReader**-Objekt liest Mediendaten aus der Datei, die mit „url“ angegeben wird.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

Dann erstellt die **MediaStreamer::Initialize**-Methode ein [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850)-Objekt, um das Format des Audiostreams zu beschreiben. Ein Audioformat hat zwei Typen: einen Haupttyp und einen Untertyp. Der Haupttyp definiert das allgemeine Format der Medien, zum Beispiel Video, Audio, Skript usw. Der Untertyp definiert das Format, zum Beispiel PCM, ADPCM oder WMA. Die **MediaStreamer::Initialize**-Methode verwendet die [**IMFMediaType::SetGUID**](https://msdn.microsoft.com/library/windows/desktop/bb970530)-Methode, um den Haupttyp als Audio (**MFMediaType\_Audio**) und den Untertyp als nicht komprimiertes PCM-Audio (**MFAudioFormat\_PCM**) anzugeben. Die [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432)-Methode ordnet den Medientyp dem StreamReader zu.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

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
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

Die **MediaStreamer::Initialize**-Methode ruft dann das vollständige Ausgabemedienformat von Media Foundation ab und ruft die [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177)-Funktion auf, um den Media Foundation-Audiomedientyp in eine [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799)-Struktur zu konvertieren. Die **WAVEFORMATEX**-Struktur definiert das Format von Waveform-Audiodaten. Marble Maze erstellt mithilfe dieser Struktur die Quellstimmen und wendet den Tiefpassfilter auf den Sound für das Rollen der Murmel an.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> **Wichtig**   Die [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177)-Funktion verwendet **CoTaskMemAlloc** zum Zuordnen des [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799)-Objekts. Rufen Sie daher unbedingt **CoTaskMemFree** auf, wenn Sie dieses Objekt nicht mehr verwenden.

 

Die **MediaStreamer::Initialize**-Methode wird beendet, indem die Länge des Streams „m\_*maxStreamLengthInBytes*“ in Byte berechnet wird. Dazu ruft sie die [**IMFSourceReader::IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662)-Methode auf, um die Dauer des Audiostreams in Einheiten von jeweils 100 Nanosekunden abzurufen, konvertiert die Dauer in Abschnitte und multipliziert dann diese Zahl mit der durchschnittlichen Datenübertragungsrate in Bytes pro Sekunde. Diesen Wert verwendet Marble Maze später, um den Puffer zuzuordnen, in dem sich die einzelnen Spielsounds befinden.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );
LONGLONG duration = var.uhVal.QuadPart;
// The duration is in 100ns units; convert the value to seconds. 
double durationInSeconds = (duration / static_cast<double>(10000000)); 
m_maxStreamLengthInBytes = 
    static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

// Round up the buffer size to the nearest four bytes.
m_maxStreamLengthInBytes = (m_maxStreamLengthInBytes + 3) / 4 * 4;
```

### Erstellen der Quellstimmen

Marble Maze erstellt XAudio2-Quellstimmen für die Wiedergabe der einzelnen Spielsounds und der Musik in Quellstimmen. Die **Audio**-Klasse definiert ein [**IXAudio2SourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415914)-Objekt für die Hintergrundmusik und ein Array mit **SoundEffectData**-Objekten für die Spielsounds. Die **SoundEffectData**-Struktur enthält das **IXAudio2SourceVoice**-Objekt für einen Effekt und definiert außerdem andere Daten im Zusammenhang mit Effekten, beispielsweise den Audiopuffer. „Audio.h“ definiert die **SoundEvent**-Enumeration. Mithilfe dieser Enumeration identifiziert Marble Maze die einzelnen Sounds im Spiel. Außerdem verwendet die "Audio"-Klasse diese Enumeration zum Indizieren des Arrays mit **SoundEffectData**-Objekten.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

Die folgende Tabelle zeigt die Beziehung zwischen den einzelnen Werten, die Datei, in der sich die zugeordneten Sounddaten befinden, und eine kurze Beschreibung der einzelnen Sounds. Die Audiodateien befinden sich im Ordner „\Media\Audio“.

| SoundEvent-Wert  | Dateiname      | Beschreibung                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Wird wiedergegeben, wenn die Murmel rollt.                              |
| FallingEvent      | MarbleFall.wav | Wird wiedergegeben, wenn die Murmel aus dem Labyrinth fällt.               |
| CollisionEvent    | MarbleHit.wav  | Wird wiedergegeben, wenn die Murmel mit dem Labyrinth kollidiert.           |
| CheckpointEvent   | Checkpoint.wav | Wird wiedergegeben, wenn die Murmel über einen Prüfpunkt rollt.         |
| MenuChangeEvent   | MenuChange.wav | Wird wiedergegeben, wenn der Benutzer des Spiels das aktuelle Menüelement ändert. |
| MenuSelectedEvent | MenuSelect.wav | Wird wiedergegeben, wenn der Benutzer des Spiels ein Menüelement auswählt.           |

 

Das folgende Beispiel zeigt, wie die **Audio::CreateResources**-Methode die Quellstimme für die Hintergrundmusik erstellt. Die [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607)-Methode erstellt und konfiguriert eine Quellstimme. Sie akzeptiert eine [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799)-Struktur, die das Format der an die Stimme gesendeten Audiopuffer definiert. Wie bereits erwähnt verwendet Marble Maze das PCM-Format. Die [**XAUDIO2\_SEND\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419244)-Struktur definiert die Zielstimme einer anderen Stimme und gibt an, ob ein Filter verwendet werden soll. Marble Maze ruft die **Audio::SetSoundEffectFilter**-Funktion auf, um mit den Filtern den Sound der rollenden Murmel zu ändern. Die [**XAUDIO2\_VOICE\_SENDS**](https://msdn.microsoft.com/library/windows/desktop/ee419246)-Struktur definiert die Stimmen, die Daten von einer einzelnen Ausgabestimme empfangen sollen. Marble Maze sendet Daten von der Quellstimme an die Masterstimme (für den direkten oder unveränderten Teil eines wiedergegebenen Sounds) und an die zwei Submixstimmen, die den mit einem Effekt versehenen oder hallenden Teil eines wiedergegebenen Sounds implementieren.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_soundEffectMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_soundEffectReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_soundEffectReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;

// The rolling sound can have pitch shifting and a low-pass filter. 
if (sound == RollingEvent)
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            XAUDIO2_VOICE_USEFILTER,
            2.0f,
            &m_voiceContext,
            &sends)
        );
}
else
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            0,
            1.0f,
            &m_voiceContext,
            &sends)
        );
}
```

## Wiedergeben von Hintergrundmusik


Eine Quellstimme wird im angehaltenen Zustand erstellt. Marble Maze startet die Hintergrundmusik in der Spielschleife. Beim ersten Aufruf von **MarbleMaze::Update** wird **Audio::Start** aufgerufen, um die Hintergrundmusik zu starten.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

Die **Audio::Start**-Methode ruft [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) auf, um die Verarbeitung der Quellstimme für die Hintergrundmusik zu starten.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

Die Quellstimme gibt diese Audiodaten an die nächste Phase des Audiodiagramms weiter. Im Fall von Marble Maze enthält die nächste Phase zwei Submixstimmen, die die beiden Halleffekte auf die Audiodaten anwenden. Eine Submixstimme wendet einen nah klingenden, spät hörbaren Halleffekt an, die zweite wendet einen entfernt klingenden, spät hörbaren Halleffekt an. In welchem Umfang die einzelnen Submixstimmen zur endgültigen Mischung beitragen, hängt von der Größe und Form des Raums ab. Der nah klingende Halleffekt trägt mehr bei, wenn sich die Murmel in der Nähe einer Wand oder in einem kleinen Raum befindet, und der spät hörbare Halleffekt trägt mehr bei, wenn sich die Murmel in einem großen Raum befindet. Durch diese Vorgehensweise entsteht ein realistischerer Echoeffekt, wenn sich die Murmel durch das Labyrinth bewegt. Weitere Informationen zur Implementierung dieses Effekts in Marble Maze finden Sie im Marble Maze-Quellcode unter **Audio::SetRoomSize** und **Physics::CalculateCurrentRoomSize**.

> **Hinweis**  In einem Spiel mit relativ gleichen Raumgrößen können Sie ein einfacheres Hallmodell verwenden. Zum Beispiel können Sie eine Halleinstellung für alle Räume verwenden oder für jeden Raum eine vordefinierte Halleinstellung erstellen.

 

Die **Audio::CreateResources**-Methode verwendet Media Foundation zum Laden der Hintergrundmusik. An dieser Stelle hat die Quellstimme aber noch keine Audiodaten, mit denen sie arbeiten kann. Darüber hinaus muss die Quellstimme regelmäßig mit Daten aktualisiert werden, damit die Wiedergabe der Hintergrundmusik, die in einer Schleife wiedergegeben wird, fortgesetzt wird. Damit die Quellstimme immer mit Daten gefüllt ist, aktualisiert die Spielschleife die Audiopuffer in jedem Frame. Die **MarbleMaze::Render**-Methode ruft **Audio::Render** auf, um den Audiopuffer der Hintergrundmusik zu verarbeiten. Von **Audio::Render** wird ein Array mit drei Audiopuffern definiert: **m\_audioBuffers**. Jeder Puffer enthält 64 KB (65536 Byte) an Daten. Die Schleife liest Daten aus dem Media Foundation-Objekt und schreibt diese Daten in die Quellstimme, bis die Warteschlange drei Puffer enthält.

> **Achtung**  Obwohl Marble Maze einen 64 KB großen Puffer für Musikdaten verwendet, müssen Sie möglicherweise einen größeren oder kleineren Puffer verwenden. Die Größe hängt von den Anforderungen Ihres Spiels ab.

 

```cpp
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer], 
                STREAMING_BUFFER_SIZE, 
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch(...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

Die Schleife ist auch zuständig, wenn das Media Foundation-Objekt das Ende des Streams erreicht. In diesem Fall ruft sie die [**MediaStreamer::OnClockRestart**](https://msdn.microsoft.com/library/windows/desktop/ms697215)-Methode auf, um die Position der Audioquelle zurückzusetzen.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Um Audioschleifen für einen einzelnen Puffer (oder für einen vollständigen in den Arbeitsspeicher geladenen Sound) zu implementieren, können Sie das **LoopCount**-Feld auf **XAUDIO2\_LOOP\_INFINITE** festlegen, wenn Sie den Sound initialisieren. Marble Maze verwendet diese Technik, um den Rollsound der Murmel wiederzugeben.

```cpp
if(sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

Für die Hintergrundmusik verwaltet Marble Maze aber die Puffer direkt, sodass es die Menge des verwendeten Arbeitsspeichers besser steuern kann. Wenn Ihre Musikdateien zu groß sind, können Sie die Musikdaten in kleinere Puffer streamen. Dadurch können Sie die Größe des Arbeitsspeichers besser auf die Verarbeitung und das Streamen von Audiodaten im Spiel abstimmen.

> **Tipp**  Wenn Ihr Spiel eine niedrige oder veränderliche Framerate hat, kann die Verarbeitung von Audio im Hauptthread zu unerwarteten Pausen oder zu Knacken im Audio führen. Das kommt daher, dass dem Audiomodul nicht genug gepufferte Audiodaten zur Verfügung stehen. Wenn dieses Problem bei Ihrem Spiel zu erwarten ist, denken Sie darüber nach, Audio in einem getrennten Thread zu verarbeiten, in dem kein Rendering ausgeführt wird. Dieser Ansatz ist besonders hilfreich auf Computern mit mehreren Prozessoren, da Ihr Spiel die Prozessoren verwenden kann, die sich im Leerlauf befinden.

 

##  Reagieren auf Spielereignisse


Die **MarbleMaze**-Klasse stellt Methoden wie **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch** und **SetSoundEffectFilter** bereit, damit vom Spiel gesteuert werden kann, wann Sounds wiedergegeben und beendet werden. Außerdem werden damit Soundeigenschaften wie Lautstärke und Tonhöhe gesteuert. Falls die Murmel beispielsweise aus dem Labyrinth herausfällt, ruft die **MarbleMaze::Update**-Methode die **Audio::PlaySoundEffect**-Methode auf, um den **FallingEvent**-Sound wiederzugeben.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

Die **Audio::PlaySoundEffect**-Methode ruft die [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471)-Methode auf, um die Wiedergabe des Sounds zu starten. Wenn die **IXAudio2SourceVoice::Start**-Methode bereits aufgerufen wurde, wird sie nicht erneut gestartet. Anschließend führt **Audio::PlaySoundEffect** eine benutzerdefinierte Logik für bestimmte Sounds aus.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError) {
        // If there's an error, then we'll recreate the engine on the next  
        // render pass. 
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();
    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up, 
    // and allow up to two collisions to be queued up. 
    if(sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};
        soundEffect->m_soundEffectSourceVoice->GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);
        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click right away. 
        // Note that stopping and then flushing could cause a glitch due to the 
        // waveform not being at a zero-crossing, but due to the nature of the sound  
        // (fast and 'clicky'), we don't mind. 
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Für andere Sounds als den Rollsound ruft die **Audio::PlaySoundEffect**-Methode [**IXAudio2SourceVoice::GetState**](https://msdn.microsoft.com/library/windows/desktop/hh405047) auf, um die Anzahl der Puffer zu ermitteln, die die Quellstimme wiedergibt. Sie ruft [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473) auf, um die Audiodaten für den Sound der Eingabewarteschlange der Stimme hinzuzufügen, wenn keine Puffer aktiv sind. Mit der **Audio::PlaySoundEffect**-Methode kann der Kollisionssound auch zweimal hintereinander wiedergegeben werden. Dies ist beispielsweise der Fall, wenn die Murmel mit einer Ecke des Labyrinths kollidiert.

Wie bereits beschrieben verwendet die Audio-Klasse das **XAUDIO2\_LOOP\_INFINITE**-Kennzeichen, wenn sie den Sound für das Rollereignis initialisiert. Der Sound wird in einer Schleife wiedergegeben, wenn **Audio::PlaySoundEffect** erstmals für dieses Ereignis aufgerufen wird. Um die Wiedergabelogik für den Rollsound zu vereinfachen, schaltet Marble Maze den Sound stumm, anstatt ihn anzuhalten. Wenn sich die Geschwindigkeit der Murmel ändert, werden Tonhöhe und Lautstärke des Sounds geändert, damit er realistischer klingt. Unten wird gezeigt, wie die **MarbleMaze::Update**-Methode die Tonhöhe und Lautstärke der Murmel aktualisiert, wenn sich die Geschwindigkeit ändert, und wie sie beim Anhalten der Murmel den Sound stummschaltet, indem sie die Lautstärke auf Null festlegt.

```cpp
// Play the roll sound only if the marble is actually rolling. 
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly  
    // ramp up the low-pass filter the faster we go. 
    // We also reduce the Q-value of the filter, starting with a  
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent, 
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## Reagieren auf Anhalte- und Fortsetzungsereignisse


Im Dokument zur Marble Maze-Anwendungsstruktur wird beschrieben, wie Marble Maze Anhalten und Fortsetzen unterstützt. Wenn das Spiel angehalten wird, wird auch die Audiowiedergabe angehalten. Wenn das Spiel fortgesetzt wird, wird die Audiowiedergabe an der Stelle fortgesetzt, an der sie unterbrochen wurde. Damit orientieren wir uns an der bewährten Methode, keine Ressourcen zu verwenden, die nicht benötigt werden.

Wenn das Spiel angehalten wird, wird die **Audio::SuspendAudio**-Methode aufgerufen. Diese Methode ruft die [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628)-Methode auf, um die gesamte Audiowiedergabe anzuhalten. Obwohl **IXAudio2::StopEngine** sofort die gesamte Audioausgabe anhält, bleiben das Audiodiagramm und die Effektparameter erhalten (z. B. der Halleffekt, der beim Aufprallen der Murmel angewendet wird).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
    m_isAudioStarted = false;
}
```

Wenn das Spiel fortgesetzt wird, wird die **Audio::ResumeAudio**-Methode aufgerufen. Diese Methode verwendet die [**IXAudio2::StartEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418626)-Methode, um die Audiowiedergabe neu zu starten. Da beim Aufruf von [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628) das Audiodiagramm und die Effektparameter erhalten bleiben, wird die Audioausgabe an der Stelle fortgesetzt, an der sie unterbrochen wurde.

```cpp
// Restarts the audio streams. A call to this method must match a previous call  
// to SuspendAudio. This method causes audio to continue where it left off. 
// If there is a problem with the restart, the m_engineExperiencedCriticalError  
// flag is set. The next call to Render will recreate all the resources and  
// reset the audio pipeline. 
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## Behandeln von Kopfhörern und Geräteänderungen


Marble Maze behandelt Fehler des XAudio2-Moduls wie zum Beispiel eine Änderung des Audiogeräts mithilfe von Modulrückrufen. Zu einer Geräteänderung kommt es meist, wenn der Benutzer des Spiels die Kopfhörer anschließt oder trennt. Sie sollten den Modulrückruf implementieren, der Geräteänderungen behandelt. Ansonsten wird die Soundwiedergabe im Spiel bis zum Neustart angehalten, wenn der Benutzer Kopfhörer anschließt oder entfernt.

„Audio.h“ definiert die **AudioEngineCallbacks**-Klasse. Diese Klasse implementiert die [**IXAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910)-Schnittstelle.

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private: 
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

Über die [**IXAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910)-Schnittstelle kann Ihr Code benachrichtigt werden, wenn Audioverarbeitungsereignisse auftreten und im Modul ein schwerwiegender Fehler auftritt. Um sich für Rückrufe zu registrieren, ruft Marble Maze nach der Erstellung des [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)-Objekts für das Musikmodul die [**IXAudio2::RegisterForCallbacks**](https://msdn.microsoft.com/library/windows/desktop/ee418620)-Methode auf.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze muss nicht benachrichtigt werden, wenn die Audioverarbeitung gestartet oder beendet wird. Daher implementiert es die Methoden [**IXAudio2EngineCallback::OnProcessingPassStart**](https://msdn.microsoft.com/library/windows/desktop/ee418463) und [**IXAudio2EngineCallback::OnProcessingPassEnd**](https://msdn.microsoft.com/library/windows/desktop/ee418462) so, dass sie keine Aktion ausführen. Für die [**IXAudio2EngineCallback::OnCriticalError**](https://msdn.microsoft.com/library/windows/desktop/ee418461)-Methode ruft Marble Maze die **SetEngineExperiencedCriticalError**-Methode auf, die das **m\_engineExperiencedCriticalError**-Kennzeichen festlegt.

```cpp
// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Wenn ein schwerwiegender Fehler auftritt, wird die Audioverarbeitung beendet, und alle weiteren Aufrufe an XAudio2 schlagen fehl. Um diese Situation zu beheben, müssen Sie die XAudio2-Instanz freigeben und eine neue erstellen. Die **Audio::Render**-Methode, die in jedem Frame über die Spielschleife aufgerufen wird, überprüft zuerst das **m\_engineExperiencedCriticalError**-Kennzeichen. Wenn das Kennzeichen festgelegt ist, löscht die Methode das Kennzeichen, gibt die aktuelle XAudio2-Instanz frei, initialisiert Ressourcen und startet dann die Hintergrundmusik.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Marble Maze verwendet außerdem das **m\_engineExperiencedCriticalError**-Kennzeichen, damit XAudio2 nicht aufgerufen wird, wenn kein Audiogerät verfügbar ist. Wenn dieses Kennzeichen festgelegt ist, verarbeitet die **MarbleMaze::Update**-Methode zum Beispiel kein Audio für Roll- oder Kollisionsereignisse. Die App versucht, das Audiomodul in jedem Frame zu reparieren, wenn dies notwendig ist. Das **m\_engineExperiencedCriticalError**-Kennzeichen ist aber möglicherweise festgelegt, wenn der Computer nicht über ein Audiogerät verfügt oder wenn die Kopfhörer getrennt werden und kein anderes Audiogerät verfügbar ist.

> **Achtung**   Sie sollten grundsätzlich im Hauptteil eines Modulrückrufs keine Blockierungsvorgänge ausführen. Diese können zu Leistungsproblemen führen. Marble Maze legt im **OnCriticalError**-Rückruf ein Kennzeichen fest und behandelt später den Fehler in der normalen Audioverarbeitungsphase. Weitere Informationen zu XAudio2-Rückrufen finden Sie unter [XAudio2-Rückrufe](https://msdn.microsoft.com/library/windows/desktop/ee415745).

 

## Verwandte Themen


* [Hinzufügen von Eingaben und Interaktivität zum Marble Maze-Beispiel](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Entwickeln von Marble Maze, einem UWP-Spiel in C++ und DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 






<!--HONumber=Mar16_HO1-->


