---
author: mtoepke
title: "Hinzufügen einer Benutzeroberfläche"
description: Sie haben gesehen, wie das Beispiel das Hauptspielobjekt implementiert, und das grundlegende Renderingframework kennengelernt.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d7af6683ab2def1af62e73be008e9189190cde95

---

# Hinzufügen einer Benutzeroberfläche


\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Sie haben gesehen, wie das Beispiel das Hauptspielobjekt implementiert, und das grundlegende Renderingframework kennengelernt. Jetzt wollen wir uns ansehen, wie das Spiel dem Spieler Feedback zum Spielzustand gibt. Hier erfahren Sie, wie Sie einfache Menüoptionen und Heads-Up-Anzeigekomponenten zur 3D-Grafikpipelineausgabe hinzufügen.

## Ziel


-   Hinzufügen einfacher Benutzeroberflächengrafiken und -verhalten zu einem DirectX-Spiel für die universelle Windows-Plattform (UWP)

## Benutzeroberflächenoverlay


Obwohl in einem DirectX-Spiel viele Möglichkeiten zum Anzeigen von Text und UI-Elementen zur Auswahl stehen, werden wir uns hier auf eine Methode beschränken: [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) (mit [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) für Textelemente).

Zunächst sollten wir klären, was Direct2D nicht ist. Anders als HTML oder XAML ist Direct2D nicht speziell für Benutzeroberflächen oder Layouts ausgelegt. Es stellt keine Benutzeroberflächenkomponenten wie Felder oder Schaltflächen und keine Layoutkomponenten wie DIVs, Tabellen oder Raster bereit.

Direct2D ist ein Satz von 2D-Zeichnungs-APIs zum Zeichnen von pixelbasierten Grundtypen und Effekten. Wenn Sie am Anfang der Entwicklung mit Direct2D stehen, sollten Sie den Code einfach halten. Komplexe Layouts und Benutzeroberflächenverhalten erfordern Zeit und Planung. Wenn für Ihr Spiel eine komplexe Benutzeroberfläche wie in Simulations- und Strategiespielen erforderlich ist, sollten Sie stattdessen ggf. XAML verwenden.

(Informationen zum Entwickeln einer Benutzeroberfläche mit XAML in einem UWP-DirectX-Spiel finden Sie unter [Erweitern des Beispielspiels](tutorial-resources.md).)

In diesem Beispielspiel sind zwei Hauptoberflächenkomponenten enthalten: die Head-Up-Anzeige für den Spielstand und spielinterne Steuerelemente sowie ein Overlay zum Anzeigen von Text und Optionen zum Spielzustand (wie etwa Pauseninformationen und Levelstartoptionen).

### Verwenden von Direct2D für die Heads-Up-Anzeige

Dies ist die spielinterne Heads-Up-Anzeige für das Beispielspiel ohne die visuellen Spielelemente. Sie ist einfach und übersichtlich, sodass sich der Spieler auf die Navigation in der 3D-Welt und das Abschießen der Ziele konzentrieren kann. Eine gute Benutzeroberfläche oder Head-Up-Anzeige darf den Spieler niemals daran hindern, Ereignisse im Spiel wahrzunehmen und auf sie zu reagieren.

![Screenshot des Spieloverlays](images/sample3dgame-overlay-nogame.png)

Wie Sie sehen, besteht das Overlay aus einfachen Grundtypen: zwei sich schneidende Liniensegmente für die Fadenkreuze und zwei Rechtecke für den [Bewegungs-/Blickcontroller](tutorial--adding-controls.md). In der oberen rechten Ecke informiert DirectWrite-Text den Spieler über die aktuelle Anzahl von Treffern, die Anzahl von Schüssen, die verbleibende Zeit auf dem Level und die aktuelle Levelnummer. Der spielinterne Head-Up-Anzeigezustand des Overlays wird in der **Render**-Methode der **GameHud**-Klasse gezeichnet und wie folgt programmiert:

```cpp
void GameHud::Render(
    _In_ Simple3DGame^ game,
    _In_ ID2D1DeviceContext* d2dContext,
    _In_ Windows::Foundation::Rect windowBounds
    )
{
    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );

        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels, start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input for the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw the cross hairs.
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
    }
}
```

In diesem Code wird das für das Overlay eingerichtete Direct2D-Renderziel aktualisiert, um die Änderungen der Trefferzahl, die verbleibende Zeit und die Levelnummer zu aktualisieren. Die Rechtecke werden mit [**DrawRect**](https://msdn.microsoft.com/library/windows/desktop/dd371902)-Aufrufen und die Fadenkreuze mit zwei [**DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)-Aufrufen gezeichnet.

> **Hinweis**   Ihnen ist vermutlich aufgefallen, dass der Aufruf von **GameHud::Render** einen [**Windows::Foundation::Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)-Parameter verwendet, der die Größe des Hauptfenster-Rechtecks enthält. Dieser Aufruf veranschaulicht einen wichtigen Teil der Oberflächenprogrammierung: Das Abrufen der Fenstergröße in einer als DIPs (geräteunabhängige Pixel) bezeichneten Einheit, wobei ein DIP als 1/96 Zoll definiert ist. Beim Zeichnen skaliert Direct2D die Zeichnungseinheiten mithilfe der Dots per Inch (DPI)-Einstellung von Windows in tatsächliche Pixel. Ebenso geben Sie beim Zeichnen von Text mit DirectWrite DIPs anstelle von Punkten für die Schriftgröße an. DIPs werden als Gleitkommazahlen angegeben.

 

### Anzeigen der Spielzustandsinformationen mit einem Overlay

Neben der Heads-Up-Anzeige enthält das Beispielspiel ein Overlay mit fünf Spielzuständen, die alle aus dem Grundtyp eines großen schwarzen Rechtecks bestehen, in dem Text für den Spieler angezeigt wird. (Beachten Sie, dass die Rechtecke für den Bewegungs-/Blickcontroller nicht gezeichnet werden, weil sie in diesen Zuständen nicht aktiv sind.) Diese Overlayzustände sind:

-   Das Spielstartoverlay. Dieser Overlayzustand wird angezeigt, wenn der Spieler das Spiel startet. Er enthält den spielübergreifenden Highscore.

    ![Screenshot des Startbildschirms für „simple3dgamedx“](images/simple3dgamestart.png)

-   Der Pausenzustand.

    ![Screenshot des Pausenbildschirms für „simple3dgamedx“](images/simple3dgame-overlay-pause.png)

-   Der Levelstartzustand. Dieser Overlayzustand wird angezeigt, wenn der Spieler ein neues Level startet.

    ![Screenshot des Levelstartbildschirms für „simple3dgamedx“](images/simple3dgame-overlay-newgame.png)

-   Der Spielendezustand. Dieser Overlayzustand wird angezeigt, wenn der Spieler das Levelziel nicht erreicht.

    ![Screenshot des Spielendebildschirms für „simple3dgamedx“](images/simple3dgame-overlay-gameover.png)

-   Der Spielstatistikanzeigezustand. Dieser Zustand wird angezeigt, wenn der Spieler gewinnt. Er enthält den Endpunktestand, den der Spieler erzielt hat.

    ![Siegbildschirm für „simple3dgamedx“](images/simple3dgame-overlay-gamestats.png)

Im Folgenden beschäftigen wir uns damit, wie das Overlay für diese fünf Zustände initialisiert und gezeichnet wird.

### Initialisieren und Zeichnen des Overlays

Die fünf expliziten Zustände habe drei Dinge gemeinsam: Sie enthalten alle ein schwarzes Rechteck in der Mitte des Bildschirms als Hintergrund, der angezeigte Text ist entweder Titeltext oder Textkörper, und der Text wird oben im Rechteck in der Schriftart „Segoe UI“ gezeichnet. Die erforderlichen Ressourcen und die Methoden zum Implementieren der Zustände sind daher sehr ähnlich.

Das Beispielsspiel verfügt über vier Methoden (**GameInfoOverlay::Initialize**, **GameInfoOverlay::SetDpi**, **GameInfoOverlay::RecreateDirectXResources** und **GameInfoOverlay::RecreateDpiDependentResources**), die zum Initialisieren, Festlegen der Punkte pro Zoll, erneuten Erstellen der DirectWrite-Ressourcen (Textelemente) bzw. zum Konstruieren des Overlays für die Anzeige verwendet werden. Im Folgenden sehen Sie den Code für diese vier Methoden:

```cpp
void GameInfoOverlay::Initialize(
    _In_ ID2D1Device*         d2dDevice,
    _In_ ID2D1DeviceContext*  d2dContext,
    _In_ IDWriteFactory*      dwriteFactory,
    _In_ float                dpi)
{
    m_initialized = true;

    m_dwriteFactory = dwriteFactory;
    m_dpi = dpi;
    m_d2dDevice = d2dDevice;
    m_d2dContext = d2dContext;

    ComPtr<ID2D1Factory> factory;
    d2dDevice->GetFactory(&factory);

    DX::ThrowIfFailed(
        factory.As(&m_d2dFactory)
        );

    RecreateDirectXResources();
}
```

```cpp
void GameInfoOverlay::SetDpi(float dpi)
{
    if (m_initialized)
    {
        if (dpi != m_dpi)
        {
            m_dpi = dpi;
            RecreateDpiDependentResources();
        }
    }
}
```

```cpp
void GameInfoOverlay::RecreateDirectXResources()
{
    if (!m_initialized)
    {
        return;
    }

    // Create D2D Resources.
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_MEDIUM,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            32,         // font size
            L"en-us",   // locale
            &m_textFormatTitle
            )
        );

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            24,         // font size
            L"en-us",   // locale
            &m_textFormatBody
            )
        );

    DX::ThrowIfFailed(
        m_textFormatTitle->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_CENTER)
        );
    DX::ThrowIfFailed(
        m_textFormatTitle->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR)
        );
    DX::ThrowIfFailed(
        m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING)
        );
    DX::ThrowIfFailed(
        m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR)
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Black),
            &m_backgroundBrush
            )
        );
     DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(0xdb7100, 1.0f),
            &m_actionBrush
            )
        );

     RecreateDpiDependentResources();
}
```

```cpp
void GameInfoOverlay::RecreateDpiDependentResources()
{
    m_levelBitmap = nullptr;

    // Create a D2D bitmap to be used for Game Info Overlay when waiting to
    // start a level or to display game statistics.
    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = m_dpi;
    properties.dpiY = m_dpi;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET;
    properties.colorContext = nullptr;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmap(
            D2D1::SizeU(
                static_cast<UINT32>(GameInfoOverlayConstant::Width * m_dpi / 96.0f),
                static_cast<UINT32>(GameInfoOverlayConstant::Height * m_dpi / 96.0f)
                ),
            nullptr,
            0,
            &properties,
            &m_levelBitmap
            )
        );
    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::Black));
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}

```

Die **Initialize**-Methode ruft eine Factory von dem an sie übergebenen [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478)-Objekt ab, die sie dann zum Erstellen eines [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479)-Elements verwendet, in das das Overlayobjekt selbst zeichnen kann, und legt das **m\_dWriteFactory**-Feld auf den angegebenen [**IDWriteFactory**](https://msdn.microsoft.com/library/windows/desktop/dd368183)-Verweis fest. Außerdem legt sie den DPI-Wert für den Kontext fest. Anschließend ruft sie **RecreateDeviceResources** auf, um das Overlay zusammenzusetzen und zu zeichnen.

**RecreateDeviceResources** verwendet das DirectWrite-Factoryobjekt, um Formatierer (Pinsel) für die Titel- und Textkörperzeichenfolgen zu erstellen, die im Overlay angezeigt werden. Sie erstellt einen weißen Pinsel zum Zeichnen des Texts, einen schwarzen Pinsel zum Zeichnen des Hintergrunds und einen orangefarbenen Pinsel zum Zeichnen von Aktionsmeldungen. Anschließend ruft sie **RecreateDpiDependentResources** auf, um eine Bitmap zum Zeichnen des Texts für [**ID2D1DeviceContext::CreateBitmap**](https://msdn.microsoft.com/library/windows/desktop/hh404480) vorzubereiten. Zum Schluss legt **RecreateDpiDependentResources** das Renderziel für den Direct2D-Gerätekontext auf die Bitmap fest und löscht sie, wodurch dann jedes Pixel in der Bitmap auf die Farbe Schwarz festgelegt wird.

Jetzt ist nur noch der Text erforderlich, der im Overlay angezeigt wird.

### Darstellen des Spielzustands im Overlay

Jeder der fünf Overlayzustände im Beispielspiel verfügt über eine entsprechende Methode des **GameInfoOverlay**-Objekts. Diese Methoden zeichnen eine Variante des Overlays, um dem Spieler explizite Informationen zum Spiel selbst mitzuteilen. Diese Kommunikation wird natürlich durch zwei Zeichenfolgen dargestellt: eine Titelzeichenfolge und eine Textkörperzeichenfolge. Da im Beispiel die Ressourcen und das Layout für diese Informationen bereits in der **RecreateDeviceResources**-Methode konfiguriert wurden, müssen nur die spezifischen Zeichenfolgen für den jeweiligen Overlayzustand bereitgestellt werden.

Im Beispiel wurden in der Definition der **GameInfoOverlay**-Klasse drei rechteckige Bereiche deklariert, die, wie hier zu sehen, bestimmten Bereichen des Overlays entsprechen:

```cpp
static const D2D1_RECT_F titleRectangle = D2D1::RectF(50.0f, 50.0f, GameInfoOverlayConstant::Width - 50.0f, 100.0f);
static const D2D1_RECT_F bodyRectangle = D2D1::RectF(50.0f, 110.0f, GameInfoOverlayConstant::Width - 50.0f, GameInfoOverlayConstant::Height - 50.0f);
static const D2D1_RECT_F actionRectangle = D2D1::RectF(50.0f, GameInfoOverlayConstant::Height - 45.0f, GameInfoOverlayConstant::Width - 50.0f, GameInfoOverlayConstant::Height - 5.0f);
```

Diese Bereiche dienen alle einem bestimmten Zweck:

-   **titleRectangle**: Hier wird der Titeltext gezeichnet.
-   **bodyRectangle**: Hier wird der Textkörper gezeichnet.
-   **actionRectangle**: Hier wird der Text gezeichnet, mit dem der Spieler zum Ausführen einer bestimmten Aktion aufgefordert wird. (Dieser Bereich befindet sich unten links in der Overlaybitmap.)

Unter Berücksichtigung dieser Bereiche wollen wir nun eine der zustandsspezifischen Methoden (**GameInfoOverlay::SetGameStats**) näher untersuchen und sehen, wie das Overlay gezeichnet wird.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;
    Platform::String^ string;

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    string = "High Score";

    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatTitle.Get(),
        titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    string = ref new Platform::String(wsbuffer, length);
    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatBody.Get(),
        bodyRectangle,
        m_textBrush.Get()
        );
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

Das **GameInfoOverlay**-Objekt wurde mit dem Direct2D-Gerätekontext mithilfe von **Initialize** und **RecreateDirectXResources** initialisiert und konfiguriert. Diese Methode füllt die Titel- und Textkörperrechtecke mithilfe des Hintergrundpinsels mit der Farbe Schwarz. Sie zeichnet mit dem weißen Textpinsel den Text für die Zeichenfolge „High Score“ im Titelrechteck und eine Zeichenfolge mit den aktualisierten Spielzuständen im Textkörperrechteck.

Das Aktionsrechteck wird durch einen anschließenden **GameInfoOverlay::SetAction**-Aufruf von einer Methode des **DirectXApp**-Objekts aktualisiert, das die für **SetAction** erforderlichen Informationen zum Spielzustand bereitstellt, um die passende Meldung für den Spieler anzuzeigen (z. B. „Tap to continue“).

Das Overlay für einen Zustand wird wie folgt in der **SetGameInfoOverlay**-Methode für **DirectXApp** aufgerufen:

```cpp
void DirectXApp::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}
```

Jetzt kann das Beispielspiel basierend auf dem Spielzustand Textinformationen für den Spieler anzeigen.

### Nächste Schritte

Im nächsten Thema zum [Hinzufügen von Steuerungen](tutorial--adding-controls.md) untersuchen wir, wie der Spieler mit dem Beispielspiel interagiert und wie sich der Spielzustand durch Eingaben ändert.

### Vollständiger Beispielcode für diesen Abschnitt

GameHud.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"
#include "DirectXSample.h"

ref class Simple3DGame;

ref class GameHud
{
internal:
    GameHud(
        _In_ Platform::String^ titleHeader,
        _In_ Platform::String^ titleBody
        );

    void CreateDeviceIndependentResources(
        _In_ IDWriteFactory* dwriteFactory,
        _In_ IWICImagingFactory* wicFactory
        );

    void CreateDeviceResources(_In_ ID2D1DeviceContext* d2dContext);
    void UpdateForWindowSizeChange(_In_ Windows::Foundation::Rect windowBounds);
    void Render(
        _In_ Simple3DGame^ game,
        _In_ ID2D1DeviceContext* d2dContext,
        _In_ Windows::Foundation::Rect windowBounds
        );

private:
    Microsoft::WRL::ComPtr<IDWriteFactory>              m_dwriteFactory;
    Microsoft::WRL::ComPtr<IWICImagingFactory>          m_wicFactory;

    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>        m_textBrush;
    Microsoft::WRL::ComPtr<IDWriteTextFormat>           m_textFormatBody;
    Microsoft::WRL::ComPtr<IDWriteTextFormat>           m_textFormatBodySymbol;

    Microsoft::WRL::ComPtr<IDWriteTextFormat>           m_textFormatTitleHeader;
    Microsoft::WRL::ComPtr<IDWriteTextFormat>           m_textFormatTitleBody;
    Microsoft::WRL::ComPtr<ID2D1Bitmap>                 m_logoBitmap;
    Microsoft::WRL::ComPtr<IDWriteTextLayout>           m_titleHeaderLayout;
    Microsoft::WRL::ComPtr<IDWriteTextLayout>           m_titleBodyLayout;

    bool                                                m_showTitle;
    Platform::String^                                   m_titleHeader;
    Platform::String^                                   m_titleBody;

    float                                               m_titleBodyVerticalOffset;
    D2D1_SIZE_F                                         m_logoSize;
    D2D1_SIZE_F                                         m_maxTitleSize;
};
```

GameHud.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "GameHud.h"
#include "GameConstants.h"

using namespace Microsoft::WRL;
using namespace Windows::UI::Core;
using namespace Windows::ApplicationModel;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::ViewManagement;
using namespace Windows::Graphics::Display;
using namespace D2D1;

//----------------------------------------------------------------------

GameHud::GameHud(
    _In_ Platform::String^ titleHeader,
    _In_ Platform::String^ titleBody
    )
{
    m_titleHeader = titleHeader;
    m_titleBody = titleBody;

    m_showTitle = true;
    m_titleBodyVerticalOffset = GameConstants::Margin;
    m_logoSize = D2D1::SizeF(0.0f, 0.0f);
}

//----------------------------------------------------------------------

void GameHud::CreateDeviceIndependentResources(
    _In_ IDWriteFactory* dwriteFactory,
    _In_ IWICImagingFactory* wicFactory
    )
{
    m_dwriteFactory = dwriteFactory;
    m_wicFactory = wicFactory;

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBody
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Symbol",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudBodyPointSize,
            L"en-us",
            &m_textFormatBodySymbol
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleHeaderPointSize,
            L"en-us",
            &m_textFormatTitleHeader
            )
        );
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI Light",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            GameConstants::HudTitleBodyPointSize,
            L"en-us",
            &m_textFormatTitleBody
            )
        );

    DX::ThrowIfFailed(m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatBodySymbol->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleHeader->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING));
    DX::ThrowIfFailed(m_textFormatTitleBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR));
}

//----------------------------------------------------------------------

void GameHud::CreateDeviceResources(_In_ ID2D1DeviceContext* d2dContext)
{
    auto location = Package::Current->InstalledLocation;
    Platform::String^ path = Platform::String::Concat(location->Path, "\\");
    path = Platform::String::Concat(path, "windows-sdk.png");

    ComPtr<IWICBitmapDecoder> wicBitmapDecoder;
    DX::ThrowIfFailed(
        m_wicFactory->CreateDecoderFromFilename(
            path->Data(),
            nullptr,
            GENERIC_READ,
            WICDecodeMetadataCacheOnDemand,
            &wicBitmapDecoder
            )
        );

    ComPtr<IWICBitmapFrameDecode> wicBitmapFrame;
    DX::ThrowIfFailed(
        wicBitmapDecoder->GetFrame(0, &wicBitmapFrame)
        );

    ComPtr<IWICFormatConverter> wicFormatConverter;
    DX::ThrowIfFailed(
        m_wicFactory->CreateFormatConverter(&wicFormatConverter)
        );

    DX::ThrowIfFailed(
        wicFormatConverter->Initialize(
            wicBitmapFrame.Get(),
            GUID_WICPixelFormat32bppPBGRA,
            WICBitmapDitherTypeNone,
            nullptr,
            0.0,
            WICBitmapPaletteTypeCustom  // The BGRA format has no palette, so this value is ignored.
            )
        );

    double dpiX = 96.0f;
    double dpiY = 96.0f;
    DX::ThrowIfFailed(
        wicFormatConverter->GetResolution(&dpiX, &dpiY)
        );

    // Create D2D Resources.
    DX::ThrowIfFailed(
        d2dContext->CreateBitmapFromWicBitmap(
            wicFormatConverter.Get(),
            BitmapProperties(
                PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
                static_cast<float>(dpiX),
                static_cast<float>(dpiY)
                ),
            &m_logoBitmap
            )
        );

    m_logoSize = m_logoBitmap->GetSize();

    DX::ThrowIfFailed(
        d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
}

//----------------------------------------------------------------------

void GameHud::UpdateForWindowSizeChange(_In_ Windows::Foundation::Rect windowBounds)
{
    m_maxTitleSize.width = windowBounds.Width - GameConstants::HudSafeWidth;
    m_maxTitleSize.height = windowBounds.Height;

    float headerWidth = m_maxTitleSize.width - (m_logoSize.width + 2 * GameConstants::Margin);

    if (headerWidth > 0)
    {
        // Only resize the text layout for the Title area when there is enough space.
        m_showTitle = true;

        DX::ThrowIfFailed(
            m_dwriteFactory->CreateTextLayout(
                m_titleHeader->Data(),
                m_titleHeader->Length(),
                m_textFormatTitleHeader.Get(),
                headerWidth,
                m_maxTitleSize.height,
                &m_titleHeaderLayout
                )
            );

        DWRITE_TEXT_METRICS metrics = {0};
        DX::ThrowIfFailed(
            m_titleHeaderLayout->GetMetrics(&metrics)
            );

        // Compute the vertical size of the laid out header and logo.  This could change
        // based on the window size and the layout of the text.  In some cases, the text
        // may wrap.
        m_titleBodyVerticalOffset = max(m_logoSize.height + GameConstants::Margin * 2, metrics.height + 2 * GameConstants::Margin);

        DX::ThrowIfFailed(
            m_dwriteFactory->CreateTextLayout(
                m_titleBody->Data(),
                m_titleBody->Length(),
                m_textFormatTitleBody.Get(),
                m_maxTitleSize.width,
                m_maxTitleSize.height - m_titleBodyVerticalOffset,
                &m_titleBodyLayout
                )
            );
    }
    else
    {
        // Not enough horizontal space for the titles, so just turn it off.
        m_showTitle = false;
    }
}

//----------------------------------------------------------------------

void GameHud::Render(
    _In_ Simple3DGame^ game,
    _In_ ID2D1DeviceContext* d2dContext,
    _In_ Windows::Foundation::Rect windowBounds
    )
{
    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );

        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input for the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw the cross hairs.
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
    }
}
```

GameInfoOverlay.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

namespace GameInfoOverlayConstant
{
    static const float Width    = 750.0f;
    static const float Height   = 380.0f;
};

enum class GameInfoOverlayCommand
{
    None,
    TapToContinue,
    PleaseWait,
    PlayAgain,
};

ref class GameInfoOverlay
{
internal:
    GameInfoOverlay();

    void Initialize(
        _In_ ID2D1Device*         d2dDevice,
        _In_ ID2D1DeviceContext*  d2dContext,
        _In_ IDWriteFactory*      dwriteFactory,
        _In_ float                dpi
        );

    void RecreateDirectXResources();
    void SetDpi(float dpi);

    void SetGameLoading(uint32 dots);
    void SetGameStats(int maxLevel, int hitCount, int shotCount);
    void SetGameOver(bool win, int maxLevel, int hitCount, int shotCount, int highScore);
    void SetLevelStart(int level, Platform::String^ objective, float timeLimit, float bonusTime);
    void SetPause();
    void SetAction(GameInfoOverlayCommand action);
    void HideGameInfoOverlay() { m_visible = false; };
    void ShowGameInfoOverlay() { m_visible = true; };
    bool Visible() { return m_visible; };
    ID2D1Bitmap1* Bitmap() { return m_levelBitmap.Get(); }

private:
    void RecreateDpiDependentResources();

    bool                                            m_initialized;
    float                                           m_dpi;
    bool                                            m_visible;

    Microsoft::WRL::ComPtr<ID2D1Factory1>           m_d2dFactory;
    Microsoft::WRL::ComPtr<ID2D1Device>             m_d2dDevice;
    Microsoft::WRL::ComPtr<ID2D1DeviceContext>      m_d2dContext;
    Microsoft::WRL::ComPtr<IDWriteFactory>          m_dwriteFactory;

    Microsoft::WRL::ComPtr<ID2D1Bitmap1>            m_levelBitmap;
    Microsoft::WRL::ComPtr<IDWriteTextFormat>       m_textFormatTitle;
    Microsoft::WRL::ComPtr<IDWriteTextFormat>       m_textFormatBody;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_textBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_backgroundBrush;
    Microsoft::WRL::ComPtr<ID2D1SolidColorBrush>    m_actionBrush;
};
```

GameInfoOverlay.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "GameInfoOverlay.h"
#include "DirectXSample.h"

using namespace Windows::UI::Core;
using namespace Windows::Foundation;
using namespace Microsoft::WRL;
using namespace Windows::UI::ViewManagement;
using namespace Windows::Graphics::Display;
using namespace D2D1;

static const D2D1_RECT_F titleRectangle = D2D1::RectF(50.0f, 50.0f, GameInfoOverlayConstant::Width - 50.0f, 100.0f);
static const D2D1_RECT_F bodyRectangle = D2D1::RectF(50.0f, 110.0f, GameInfoOverlayConstant::Width - 50.0f, GameInfoOverlayConstant::Height - 50.0f);
static const D2D1_RECT_F actionRectangle = D2D1::RectF(50.0f, GameInfoOverlayConstant::Height - 45.0f, GameInfoOverlayConstant::Width - 50.0f, GameInfoOverlayConstant::Height - 5.0f);
static const int bufferLength = 1000;
static char16 wsbuffer[bufferLength];

GameInfoOverlay::GameInfoOverlay():
    m_initialized(false),
    m_visible(false)
{
}
//----------------------------------------------------------------------
void GameInfoOverlay::Initialize(
    _In_ ID2D1Device*         d2dDevice,
    _In_ ID2D1DeviceContext*  d2dContext,
    _In_ IDWriteFactory*      dwriteFactory,
    _In_ float                dpi)
{
    m_initialized = true;

    m_dwriteFactory = dwriteFactory;
    m_dpi = dpi;
    m_d2dDevice = d2dDevice;
    m_d2dContext = d2dContext;

    ComPtr<ID2D1Factory> factory;
    d2dDevice->GetFactory(&factory);

    DX::ThrowIfFailed(
        factory.As(&m_d2dFactory)
        );

    RecreateDirectXResources();
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetDpi(float dpi)
{
    if (m_initialized)
    {
        if (dpi != m_dpi)
        {
            m_dpi = dpi;
            RecreateDpiDependentResources();
        }
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::RecreateDirectXResources()
{
    if (!m_initialized)
    {
        return;
    }

    // Create D2D resources.
    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_MEDIUM,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            32,         // font size
            L"en-us",   // locale
            &m_textFormatTitle
            )
        );

    DX::ThrowIfFailed(
        m_dwriteFactory->CreateTextFormat(
            L"Segoe UI",
            nullptr,
            DWRITE_FONT_WEIGHT_LIGHT,
            DWRITE_FONT_STYLE_NORMAL,
            DWRITE_FONT_STRETCH_NORMAL,
            24,         // font size
            L"en-us",   // locale
            &m_textFormatBody
            )
        );

    DX::ThrowIfFailed(
        m_textFormatTitle->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_CENTER)
        );
    DX::ThrowIfFailed(
        m_textFormatTitle->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR)
        );
    DX::ThrowIfFailed(
        m_textFormatBody->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_LEADING)
        );
    DX::ThrowIfFailed(
        m_textFormatBody->SetParagraphAlignment(DWRITE_PARAGRAPH_ALIGNMENT_NEAR)
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::White),
            &m_textBrush
            )
        );
    DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(D2D1::ColorF::Black),
            &m_backgroundBrush
            )
        );
     DX::ThrowIfFailed(
        m_d2dContext->CreateSolidColorBrush(
            D2D1::ColorF(0xdb7100, 1.0f),
            &m_actionBrush
            )
        );

     RecreateDpiDependentResources();
}
//----------------------------------------------------------------------
void GameInfoOverlay::RecreateDpiDependentResources()
{
    m_levelBitmap = nullptr;

    // Create a D2D bitmap to be used for Game Info Overlay when waiting to
    // start a level or when displaying game statistics.
    D2D1_BITMAP_PROPERTIES1 properties;
    properties.pixelFormat.format = DXGI_FORMAT_B8G8R8A8_UNORM;
    properties.pixelFormat.alphaMode = D2D1_ALPHA_MODE_PREMULTIPLIED;
    properties.dpiX = m_dpi;
    properties.dpiY = m_dpi;
    properties.bitmapOptions = D2D1_BITMAP_OPTIONS_TARGET;
    properties.colorContext = nullptr;
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmap(
            D2D1::SizeU(
                static_cast<UINT32>(GameInfoOverlayConstant::Width * m_dpi / 96.0f),
                static_cast<UINT32>(GameInfoOverlayConstant::Height * m_dpi / 96.0f)
                ),
            nullptr,
            0,
            &properties,
            &m_levelBitmap
            )
        );
    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->Clear(D2D1::ColorF(D2D1::ColorF::Black));
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetGameLoading(uint32 dots)
{
    int length;
    Platform::String^ string = "Loading Resources";

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&actionRectangle, m_backgroundBrush.Get());

    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatTitle.Get(),
        titleRectangle,
        m_textBrush.Get()
        );

    dots = dots % 10;
    for (length = 0; length < 25; length++)
    {
        wsbuffer[length] = L' ';
    }
    for (uint32 i = 0; i < dots; i++)
    {
        wsbuffer[length++] = 0x25CF;   // This is a Dot character in the font.
        wsbuffer[length++] = L' ';
        wsbuffer[length++] = L' ';
        wsbuffer[length++] = L' ';
    }

    m_d2dContext->DrawText(
        wsbuffer,
        length,
        m_textFormatBody.Get(),
        bodyRectangle,
        m_actionBrush.Get()
        );

    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;
    Platform::String^ string;

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    string = "High Score";

    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatTitle.Get(),
        titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    string = ref new Platform::String(wsbuffer, length);
    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatBody.Get(),
        bodyRectangle,
        m_textBrush.Get()
        );
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetGameOver(bool win, int maxLevel, int hitCount, int shotCount, int highScore)
{
    int length;
    Platform::String^ string;


    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    if (win)
    {
        string = "You WON!";
    }
    else
    {
        string = "Game Over";
    }
    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatTitle.Get(),
        titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d\n\nHigh Score %d\n",
        maxLevel,
        hitCount,
        shotCount,
        highScore
        );
    m_d2dContext->DrawText(
        wsbuffer,
        length,
        m_textFormatBody.Get(),
        bodyRectangle,
        m_textBrush.Get()
        );
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetLevelStart(int level, Platform::String^ objective, float timeLimit, float bonusTime)
{
    int length;
    Platform::String^ string;

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    length = swprintf_s(wsbuffer, bufferLength, L"Level %d", level);
    m_d2dContext->DrawText(
        wsbuffer,
        length,
        m_textFormatTitle.Get(),
        titleRectangle,
        m_textBrush.Get()
        );

    if (bonusTime > 0.0f)
    {
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Objective: %s\nTime  Limit: %6.1f sec\nBonus Time: %6.1f sec\n",
            objective->Data(),
            timeLimit,
            bonusTime
            );
    }
    else
    {
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Objective: %s\nTime  Limit: %6.1f sec\n",
            objective->Data(),
            timeLimit
            );
    }
    string = ref new Platform::String(wsbuffer, length);
    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatBody.Get(),
        bodyRectangle,
        m_textBrush.Get()
        );
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetPause()
{
    Platform::String^ string;

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&titleRectangle, m_backgroundBrush.Get());
    m_d2dContext->FillRectangle(&bodyRectangle, m_backgroundBrush.Get());
    string = "Game Paused";

    m_d2dContext->DrawText(
        string->Data(),
        string->Length(),
        m_textFormatTitle.Get(),
        bodyRectangle,
        m_textBrush.Get()
        );
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
//----------------------------------------------------------------------
void GameInfoOverlay::SetAction(GameInfoOverlayCommand action)
{
    Platform::String^ string;

    m_d2dContext->SetTarget(m_levelBitmap.Get());
    m_d2dContext->BeginDraw();
    m_d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    m_d2dContext->FillRectangle(&actionRectangle, m_backgroundBrush.Get());

    switch (action)
    {
    case GameInfoOverlayCommand::PlayAgain:
        string = "Tap to play again ...";
        break;
    case GameInfoOverlayCommand::PleaseWait:
        string = "Level loading, please wait ...";
        break;
    case GameInfoOverlayCommand::TapToContinue:
        string = "Tap to continue ...";
        break;
    default:
        string = "";
        break;
    }
    if (action != GameInfoOverlayCommand::None)
    {
        m_d2dContext->DrawText(
            string->Data(),
            string->Length(),
            m_textFormatBody.Get(),
            actionRectangle,
            m_actionBrush.Get()
            );
    }
    HRESULT hr = m_d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

## Verwandte Themen


[Erstellen eines einfachen UWP-Spiels mit DirectX](tutorial--create-your-first-metro-style-directx-game.md)

 

 







<!--HONumber=Jun16_HO4-->


