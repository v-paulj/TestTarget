---
Description: Dieser Artikel bietet eine Übersicht über die Konzepte und Technologien in Barrierefreiheitsszenarien für Apps der universellen Windows-Plattform (UWP).
title: Übersicht über die Barrierefreiheit
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
label: Accessibility
template: detail.hbs
---

Übersicht über die Barrierefreiheit
=================================================================================

\[ Aktualisiert für UWP-Apps unter Windows 10. Artikel zu Windows 8.x finden Sie im [Archiv](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Dieser Artikel bietet eine Übersicht über die Konzepte und Technologien in Barrierefreiheitsszenarien für UWP-Apps (Universelle Windows-Plattform).

<span id="Accessibility_and_your_app"></span><span id="accessibility_and_your_app"></span><span id="ACCESSIBILITY_AND_YOUR_APP"></span>Barrierefreiheit und Ihre App
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

Benutzer können viele unterschiedliche Behinderungen oder Beeinträchtigungen haben, einschließlich Beschränkungen im Hinblick auf Mobilität, Sehvermögen, Farbwahrnehmung, Hörvermögen, Sprache, Kognition und Lese-und Schreibfähigkeit. Sie können die meisten Anforderungen jedoch erfüllen, indem Sie sich an den hier dargelegten Richtlinien orientieren. Hierzu muss Folgendes bereitgestellt werden:

-   Unterstützung für Tastaturinteraktionen und Bildschirmlesprogramme.
-   Unterstützung für Benutzeranpassungen wie beispielsweise Schriftart, Zoomeinstellung (Vergrößerung), Farbe und hoher Kontrast.
-   Alternativen oder Ergänzungen für Teile der Benutzeroberfläche.

Steuerelemente für eine XAML-Benutzeroberfläche bieten integrierte Tastaturunterstützung und Unterstützung für Hilfstechnologien wie Bildschirmleseprogramme, die Barrierefreiheitsframeworks nutzen, die UWP-Apps, HTML und andere UI-Technologien bereits unterstützen. Diese integrierte Unterstützung ermöglicht ein Basisniveau an Barrierefreiheit, das Sie mit sehr wenig Aufwand anpassen können, indem Sie nur eine Handvoll von Eigenschaften festlegen. Wenn Sie eigene benutzerdefinierte XAML-Komponenten und -Steuerelemente erstellen, können Sie auch eine ähnliche Unterstützung für diese Steuerelemente bereitstellen, indem Sie einen *Automatisierungspeer* verwenden.

Zudem können Sie mithilfe von Datenbindungs-, Stil- und Vorlagenfeatures die Unterstützung dynamischer Änderungen an Anzeigeeinstellungen und Text für alternative Benutzeroberflächen einfacher implementieren.

<span id="UI_Automation"></span><span id="ui_automation"></span><span id="UI_AUTOMATION"></span>Benutzeroberflächenautomatisierung
-------------------------------------------------------------------------------------------------------------

Die Unterstützung der Barrierefreiheit stammt hauptsächlich aus der integrierten Unterstützung für das Microsoft-Benutzeroberflächenautomatisierungs-Framework. Diese Unterstützung wird mithilfe von Basisklassen und des integrierten Verhaltens der Klassenimplementierung für Steuerelementtypen sowie einer Schnittstellendarstellung der Anbieter-API für die Benutzeroberflächenautomatisierung bereitgestellt. Jede Steuerelementklasse verwendet als Benutzeroberflächenautomatisierungskonzepte Automatisierungspeers und Automatisierungsmuster, von denen die Rolle und der Inhalt des Steuerelements an die Benutzeroberflächenautomatisierungsclients gemeldet werden. Die App wird von der Benutzeroberflächenautomatisierung als Hauptfenster behandelt. Über das Benutzeroberflächenautomatisierungs-Framework ist der gesamte für die Barrierefreiheit relevante Inhalt des App-Fensters für einen Benutzeroberflächenautomatisierungsclient verfügbar. Weitere Informationen zur Benutzeroberflächenautomatisierung finden Sie unter [Übersicht über die Benutzeroberflächenautomatisierung](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

<span id="Assistive_technology"></span><span id="assistive_technology"></span><span id="ASSISTIVE_TECHNOLOGY"></span>Hilfstechnologie
-----------------------------------------------------------------------------------------------------------------------------------------

Viele Benutzeranforderungen im Hinblick auf Barrierefreiheit werden durch vom Benutzer installierte Hilfstechnologieprodukte oder durch Tools und Einstellungen erfüllt, die vom Betriebssystem bereitgestellt werden. Dies umfasst Funktionen wie etwa Bildschirmleseprogramme, die Bildschirmlupe und Einstellungen für hohen Kontrast.

Hilfstechnologieprodukte umfassen eine breite Palette an Software und Hardware. Diese Produkte erfüllen ihre Aufgabe über die Standardtastaturschnittstelle und Barrierefreiheitsframeworks, die Informationen zu Inhalt und Struktur einer UI an Bildschirmleseprogramme und andere Hilfstechnologien melden. Beispiele für Hilfstechnologieprodukte:

-   Die Bildschirmtastatur, mit der ein Zeiger anstelle einer Tastatur verwendet werden kann.
-   Spracherkennungssoftware, mit der Sprache in geschriebenen Text konvertiert werden kann.
-   Bildschirmleseprogramme, die Text in Sprache oder andere Formate wie Braille konvertieren.
-   Die Sprachausgabe, die Teil von Windows ist. Die Sprachausgabe verfügt über einen Touchmodus, mit dem die Sprachausgabe anhand von Toucheingabe gesteuert werden kann, für Situationen, in denen keine Tastatur verfügbar ist.
-   Programme oder Einstellungen, mit denen die Anzeige oder bestimmte Bereiche der Anzeige angepasst werden, z. B. Designs mit hohem Kontrast, DPI (Dots per Inch)-Einstellungen der Anzeige oder die Bildschirmlupe.

Apps mit guter Tastatur- und Bildschirmleseprogrammunterstützung funktionieren normalerweise ohne Probleme mit unterschiedlichen Hilfstechnologieprodukten. In vielen Fällen kann eine UWP-App ohne zusätzliche Anpassung der Informationen oder Struktur mit diesen Produkten verwendet werden. Es kann jedoch empfehlenswert sein, einige Einstellungen anzupassen, um eine optimale Barrierefreiheit zu erzielen oder um weitere Unterstützung zu implementieren.

Einige Optionen, mit denen Sie grundlegende Szenarien für die Barrierefreiheit mit Hilfstechnologien testen können, sind unter [Barrierefreiheitstests](accessibility-testing.md) aufgeführt.

<span id="Screen_reader_support_and_basic_accessibility_information"></span><span id="screen_reader_support_and_basic_accessibility_information"></span><span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"></span>Unterstützung von Bildschirmleseprogrammen und grundlegende Barrierefreiheitsinformationen
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Sprachausgabeprogramme bieten Zugriff auf Text in einer App, indem dieser in einem anderen Format, beispielsweise als gesprochene Sprache oder als Braille-Ausgabe, gerendert wird. Das genaue Verhalten der Sprachausgabe hängt von der Software und der Benutzerkonfiguration der Software ab.

Einige Sprachausgabeprogramme lesen beispielsweise die gesamte App-UI, wenn der Benutzer die App startet oder die angezeigte App wechselt. Hierdurch stehen den Benutzern alle verfügbaren Informationsinhalte vor deren Navigation zur Verfügung. Einige Sprachausgabeprogramme lesen außerdem den Text, der einem einzelnen Steuerelement zugeordnet ist, wenn dieses während der TAB-Navigation den Fokus erhält. Auf diese Weise können sich die Benutzer orientieren, während sie zwischen den Eingabesteuerelementen einer Anwendung navigieren. Die Sprachausgabe von Windows ist ein Beispiel für ein Sprachausgabeprogramm, das beide Verhalten je nach Wahl des Benutzers bereitstellt.

Die wichtigsten Informationen, die ein Sprachausgabeprogramm oder eine andere Hilfstechnologie benötigt, um Benutzern die App zu beschreiben oder ihnen bei der Navigation zu helfen, sind *barrierefreie Namen* für grundlegende Teile der App. In vielen Fällen hat ein Steuerelement oder ein UI-Element bereits einen barrierefreien Namen. Dieser wird aus anderen Eigenschaftswerten berechnet, die bereitgestellt wurden. Der häufigste Fall, in dem ein bereits berechneter Name zur Verfügung steht, ist ein Element, das internen Text unterstützt und anzeigt. Bei anderen Elementen müssen Sie ggf. andere Möglichkeiten nutzen,und sich an den bewährten Methoden einer Elementstruktur orientieren, um einen barrierefreien Namen bereitzustellen. Und manchmal müssen Sie einen Namen bereitstellen, der explizit als barrierefreier Name für die App-Barrierefreiheit vorgesehen ist. Eine Auflistung dazu, wie viele dieser berechneten Werte in gängigen UI-Elementen funktionieren, und weitere Informationen zu barrierefreien Namen im Allgemeinen finden Sie unter [Grundlegende Barrierefreiheitsinformationen](basic-accessibility-information.md).

Es sind einige andere Automatisierungseigenschaften verfügbar (einschließlich der Tastatureigenschaften, die im nächsten Abschnitt beschrieben werden). Es werden aber nicht alle Automatisierungseigenschaften von allen Bildschirmleseprogrammen unterstützt. Im Allgemeinen sollten Sie alle geeigneten Automatisierungseigenschaften festlegen und testen, um möglichst breite Unterstützung für Sprachausgabeprogramme bereitzustellen.

<span id="Keyboard_support"></span><span id="keyboard_support"></span><span id="KEYBOARD_SUPPORT"></span>Tastaturunterstützung
-------------------------------------------------------------------------------------------------------------------------

Um eine gute Tastaturunterstützung bereitzustellen, müssen Sie dafür sorgen, dass jeder Teil Ihrer Anwendung mit einer Tastatur verwendet werden kann. Wenn Ihre App hauptsächlich die Standardsteuerelemente und keine benutzerdefinierten Steuerelemente verwendet, ist das meiste schon getan. Das grundlegende XAML-Steuerelementmodell bietet integrierte Tastaturunterstützung, einschließlich Unterstützung für die TAB-Navigation, Texteingaben und steuerelementspezifischer Unterstützung. Die Elemente, die als Layoutcontainer dienen (z. B. Panels), verwenden die Layoutreihenfolge, um eine Standardaktivierreihenfolge festzulegen. Diese Reihenfolge ist häufig die richtige Aktivierreihenfolge zur barrierefreien Darstellung der UI. Wenn Sie die Steuerelemente [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) und [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) zum Anzeigen von Daten verwenden, bieten diese Steuerelemente eine integrierte Unterstützung für die Navigation mittels Pfeiltasten. Oder wenn Sie ein [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)-Steuerelement verwenden, werden die LEERTASTE oder EINGABETASTE bereits zur Schaltflächenaktivierung behandelt.

Weitere Informationen zu allen Aspekten der Tastaturunterstützung, einschließlich Aktivierreihenfolge und tastenbasierter Aktivierung oder Navigation, finden Sie unter [Barrierefreiheit der Tastaturnavigation](keyboard-accessibility.md).

<span id="Media_and_captioning"></span><span id="media_and_captioning"></span><span id="MEDIA_AND_CAPTIONING"></span>Medien und Untertitelung
-----------------------------------------------------------------------------------------------------------------------------------------

Sie zeigen audiovisuelle Medien normalerweise über ein [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926)-Objekt an. Mit **MediaElement**-APIs steuern Sie die Medienwiedergabe. Stellen Sie aus Gründen der Barrierefreiheit Steuerelemente bereit, mit denen Benutzer die Medienwiedergabe nach Bedarf starten, anhalten oder beenden können. Gelegentlich umfassen Medien zusätzliche Komponenten, die für die Barrierefreiheit vorgesehen sind, z. B. Untertitelung oder alternative Audiospuren mit einer Sprachausgabe der Beschreibungen.

<span id="Accessible_text"></span><span id="accessible_text"></span><span id="ACCESSIBLE_TEXT"></span>Barrierefreier Text
---------------------------------------------------------------------------------------------------------------------

Für die Barrierefreiheit sind drei Hauptaspekte von Text relevant:

-   Tools müssen feststellen können, ob der Text als Teil der durchlaufenen TAB-Sequenz oder nur als Teil der Gesamtdarstellung eines Dokuments gelesen wird. Sie können dem Steuerelement bei dieser Unterscheidung helfen, indem Sie das richtige Element für die Anzeige des Texts auswählen oder die Eigenschaften dieser Textelemente anpassen. Jedes Element erfüllt einen bestimmten Zweck, und mit diesem ist oft eine entsprechende Benutzeroberflächenautomatisierungsrolle verbunden. Wenn Sie das falsche Element verwenden, kann der Benutzeroberflächenautomatisierung die falsche Rolle gemeldet werden, sodass die Benutzerfreundlichkeit für Hilfstechnologienutzer beeinträchtigt wird.
-   Bei vielen Benutzern ist das Sehvermögen eingeschränkt, sodass Text für sie schwierig zu lesen ist, sofern der Textkontrast zum Hintergrund nicht ausreichend ist. Wie sich dies auf die Benutzer auswirkt, ist für App-Designer, deren Sehvermögen nicht eingeschränkt ist, nicht intuitiv nachvollziehbar. So kann eine schlechte Farbauswahl im Design beispielsweise dazu führen, dass einige farbenblinde Benutzer den Text nicht lesen können. Empfehlungen für die Barrierefreiheit, die ursprünglich für Webinhalt vorgesehen waren, definieren Standards für den Kontrast, die diese Probleme auch in Apps verhindern können. Weitere Informationen finden Sie unter [Anforderungen für barrierefreien Text](accessible-text-requirements.md).
-   Viele Benutzer haben Schwierigkeiten mit dem Lesen von Text, da dieser einfach zu klein ist. Sie können dieses Problem verhindern, indem Sie von vornherein den Text auf der Benutzeroberfläche der App groß genug machen. Dies ist jedoch schwierig für Apps, die sehr viel Text anzeigen oder bei denen Text zwischen andere visuelle Elemente eingestreut wird. Stellen Sie in diesen Fällen sicher, dass die App korrekt mit den Systemfeatures interagiert, die die Anzeige skalieren können, damit der gesamte Text in App ebenfalls vergrößert werden kann. (Einige Benutzer ändern die DPI-Werte zu Zwecken der Barrierefreiheit. Die Option ist unter **Elemente auf dem Bildschirm vergrößern** in **Erleichterte Bedienung** verfügbar, die auf die **Systemsteuerung** und den Bildschirm für **Darstellung und Anpassung**/**Anzeige** verweist.)

<span id="Supporting_high-contrast_themes"></span><span id="supporting_high-contrast_themes"></span><span id="SUPPORTING_HIGH-CONTRAST_THEMES"></span>Unterstützung von Designs mit hohem Kontrast (HTML)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Steuerelemente der Benutzeroberfläche verwenden eine visuelle Darstellung, die als Teil eines XAML-Ressourcenwörterbuchs mit Designs definiert ist. Mindestens eines dieser Themen wird speziell verwendet, wenn das System auf hohen Kontrast eingestellt ist. Wenn der Benutzer zu einer Darstellung mit hohem Kontrast wechselt, wird durch die dynamische Suche nach dem geeigneten Thema in einem Ressourcenwörterbuch auch in allen Benutzeroberflächen-Steuerelementen ein geeignetes Thema mit hohem Kontrast verwendet. Stellen Sie einfach sicher, dass Sie die Designs nicht deaktiviert haben, indem Sie einen expliziten Stil angeben oder ein anderes Verfahren zum Anwenden eines Stils verwenden, das verhindert, dass die Designs für hohen Kontrast geladen und ihre Stiländerungen überschrieben werden. Weitere Informationen finden Sie unter [Designs mit hohem Kontrast](high-contrast-themes.md).

<span id="Design_for_alternative_UI"></span><span id="design_for_alternative_ui"></span><span id="DESIGN_FOR_ALTERNATIVE_UI"></span>Design für alternative UI
-------------------------------------------------------------------------------------------------------------------------------------------------------------

Beim Entwerfen von Apps sollten Sie immer berücksichtigen, wie diese von Menschen mit Beeinträchtigungen im Bereich Mobilität, Sehvermögen oder Hörvermögen verwendet werden können. Da Hilfstechnologieprodukte ausgiebig Gebrauch von Standard-UI machen, ist es besonders wichtig, angemessene Unterstützung für Tastatur und Bildschirmleseprogramme bereitzustellen, auch wenn Sie ansonsten keine weiteren Anpassungen im Hinblick auf Barrierefreiheit vornehmen.

In vielen Fällen können Sie wesentliche Informationen mithilfe mehrerer Techniken transportieren, um die Zielgruppe zu vergrößern. Sie können beispielsweise Informationen sowohl durch Symbole als auch mit Farbinformationen hervorheben, um farbenblinden Benutzern zu helfen, und Sie können visuelle Hinweise zusammen mit Soundeffekten verwenden, um Unterstützung für hörgeschädigte Benutzer bereitzustellen.

Falls notwendig, können Sie alternative, barrierefreie UI-Elemente bereitstellen, durch die unwesentliche Elemente und Animationen vollständig entfernt werden, und weitere Vereinfachungen bereitstellen, um die Benutzererfahrung zu optimieren. Das folgende Codebeispiel zeigt, wie je nach Benutzereinstellung eine [**UserControl**](https://msdn.microsoft.com/library/windows/apps/BR227647)-Instanz anstelle einer anderen angezeigt werden kann.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;StackPanel x:Name=&quot;LayoutRoot&quot; Background=&quot;White&quot;&gt;

  &lt;CheckBox x:Name=&quot;ShowAccessibleUICheckBox&quot; Click=&quot;ShowAccessibleUICheckBox_Click&quot;&gt;
    Anzeigen der barrierefreien UI
  &lt;/CheckBox&gt;

  &lt;UserControl x:Name=&quot;ContentBlock&quot;&gt;
    &lt;local:ContentPage/&gt;
  &lt;/UserControl&gt;

&lt;/StackPanel&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="VisualBasic"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">VB</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If

End Sub</code></pre></td>
</tr>
</tbody>
</table>

<span codelanguage="CSharp"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}</code></pre></td>
</tr>
</tbody>
</table>

<span id="Verification_and_publishing"></span><span id="verification_and_publishing"></span><span id="VERIFICATION_AND_PUBLISHING"></span>Überprüfung und Veröffentlichung
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

Weitere Informationen zum Ausweis der Barrierefreiheit und zum Veröffentlichen Ihrer App finden Sie unter [Barrierefreiheit im Store](accessibility-in-the-store.md).

**Hinweis**  Das Ausweisen der App als barrierefrei ist nur für den Windows Store relevant.

<span id="Assistive_technology_support_in_custom_controls"></span><span id="assistive_technology_support_in_custom_controls"></span><span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"></span>Hilfstechnologieunterstützung in benutzerdefinierten Steuerelementen
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Beim Erstellen eines benutzerdefinierten Steuerelements empfehlen wir, dass Sie auch eine oder mehrere [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)-Unterklassen implementieren oder erweitern, um Unterstützung für Barrierefreiheit bereitzustellen. In einigen Fällen bietet die Automatisierungsunterstützung für Ihre abgeleitete Klasse eine ausreichende Grundfunktionalität, wenn Sie die gleiche Peerklasse verwenden, die von der Basissteuerungsklasse verwendet wurde. Sie müssen dies jedoch testen. Dennoch wird die Implementierung eines Peers als bewährte Methode empfohlen, damit der Peer den Namen der Klasse Ihrer neuen Steuerelementklasse richtig melden kann. Das Implementieren eines benutzerdefinierten Automatisierungspeers umfasst einige Schritte. Weitere Informationen finden Sie unter [Benutzerdefinierte Automatisierungspeers](custom-automation-peers.md).

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"></span><span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"></span><span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"></span>Hilfstechnologien in Apps, die XAML/Microsoft DirectX-Interoperabilität unterstützen
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Auf in einer XAML-Benutzeroberfläche gehostete Microsoft DirectX-Inhalte (mit [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/Dn252834) oder [**SurfaceImageSource**](https://msdn.microsoft.com/library/windows/apps/Hh702041)) kann standardmäßig nicht direkt zugegriffen werden. Im [Beispiel für SwapChainPanel-Beispiel für DirectX/XAML-Interoperabilität](http://go.microsoft.com/fwlink/p/?LinkID=309155) wird veranschaulicht, wie Benutzeroberflächenautomatisierungs-Peers für den gehosteten DirectX-Inhalt erstellt werden. Diese Technik erlaubt den Zugriff auf den gehosteten Inhalt per Benutzeroberflächenautomatisierung.

<span id="related_topics"></span>Verwandte Themen
-----------------------------------------------

* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179)
* [Entwerfen für Barrierefreiheit](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [XAML-Beispiel für Barrierefreiheit](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 





<!--HONumber=Mar16_HO3-->


