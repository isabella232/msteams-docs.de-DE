---
title: Registerkarten auf mobilen Geräten
description: Beschreibt die Richtlinien für das Entwerfen von Registerkarten, die auf mobilen Geräten funktionieren.
keywords: Teams-Entwurfsrichtlinien – Referenzrahmen-Mobile Registerkarten für persönliche apps
ms.openlocfilehash: a1939465b04a1fe4b803efaf83402852ca536059
ms.sourcegitcommit: b51a4982842948336cfabedb63bdf8f72703585e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "48279776"
---
# <a name="tabs-on-mobile"></a>Registerkarten auf mobilen Geräten

> [!NOTE]
> Wenn die Registerkarte Kanal/Gruppe auf mobilen Teams-Clients angezeigt werden soll, `setSettings()` muss die Konfiguration über einen Wert für die `websiteUrl` Eigenschaft verfügen (siehe unten).

Benutzerdefinierte Registerkarten können Teil eines Kanals, Gruppenchats oder einer persönlichen APP sein (apps, die statische Registerkarten und/oder einen 1:1-bot enthalten).

Persönliche apps sind auf mobilen Clients in der APP-Schublade verfügbar. Die APP kann nur von einem Desktop-oder WebClient installiert werden, und es kann bis zu 24 Stunden dauern, bis Sie auf mobilen Clients angezeigt wird.

Kanal Registerkarten sind auch auf mobilen Geräten verfügbar. Das Standardverhalten besteht derzeit darin, dass Sie Ihre `websiteUrl` Registerkarte in einem Browserfenster starten können. Sie können jedoch auf einem mobilen Client geladen werden, indem Sie auf das `...` Überlaufmenü neben der Registerkarte und dann auf **Öffnen**klicken, mit dem Sie `contentUrl` die Registerkarte in den mobilen Microsoft Teams-Client laden.

## <a name="accessing-personal-tabs"></a>Zugreifen auf persönliche Registerkarten

In der folgenden Abbildung wird gezeigt, wie Sie auf eine persönliche Registerkarte in Mobile zugreifen.

:::image type="content" source="../../assets/images/tabs/mobile-app-drawer.png" alt-text="Abbildung mit den Teams Mobile App Schublade." border="false":::

## <a name="accessing-channel-tabs"></a>Zugreifen auf Kanal Registerkarten

In der folgenden Abbildung wird gezeigt, wie Sie auf eine Kanal Registerkarte in Mobile zugreifen.

:::image type="content" source="../../assets/images/tabs/mobile-tab.png" alt-text="Abbildung mit einer mobilen Teams-Registerkarte." border="false":::

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Mit unserer mobilen Plattform können apps eine immersive Erfahrung mit dem App-Inhalt sein, der den gesamten Bildschirm außer der Navigation in den Haupt Teams aufnimmt. Befolgen Sie diese Richtlinien, um eine immersive Umgebung zu erstellen, die in Microsoft Teams passt.

### <a name="responsive-design"></a>Dynamisches Designs

Da die Registerkarte auf Geräten mit einer großen Bandbreite von Bildschirmgrößen geöffnet werden kann, muss Sie den Grundsätzen für das [reagieren auf Designs](https://www.w3schools.com/html/html_responsive.asp) entsprechen. Auf alle Schlüssel Konstrukte sollte auf mobilen Geräten zugegriffen werden können, und die Ansichten sollten nicht verzerrt werden. Stellen Sie sicher, dass beim Laden der Registerkarte auf einem mobilen Gerät alle Schaltflächen und Links leicht über die Finger basierte Navigation zugänglich sind.

### <a name="layouts"></a>Layouts

Die Auswahl des richtigen Layouts für Ihre Registerkarte ist wichtig. Sie sollten die Art von Informationen berücksichtigen, die Sie präsentieren, und ein Layout auswählen, das es für einen einfachen Verbrauch organisiert. Einige potenzielle Optionen werden unten erläutert.

#### <a name="single-canvas"></a>Einzelne Leinwand

Dies ist ein großer Bereich, in dem Arbeit erledigt wird. Die wiki-app "Teams" folgt diesem Muster. Wenn Sie über eine APP verfügen, die Inhalte nicht in kleinere Komponenten unterteilt, wäre dies gut geeignet.

:::image type="content" source="../../assets/images/tabs/mobile-tab-single-canvas.png" alt-text="Abbildung mit einer mobilen Teams-Registerkarte mit einer einzelnen Leinwand." border="false":::

#### <a name="list"></a>Liste

Listen eignen sich hervorragend zum Sortieren und Filtern großer Datenmengen und bieten eine große Rolle, um die wichtigsten Dinge am besten zu halten. Es ist hilfreich, sortierbare Spalten zu verwenden. Im Menü mit den Auslassungspunkten können jedem Listenelementaktionen hinzugefügt werden.

:::image type="content" source="../../assets/images/tabs/mobile-tab-list.png" alt-text="Abbildung mit einer mobilen Teams-Listen Registerkarte." border="false":::

#### <a name="grid"></a>Raster

Raster sind nützlich, um Elemente anzuzeigen, die sehr visuell sind. Es hilft, ein Filter-oder Suchsteuerelement am oberen Rand einzuschließen.

:::image type="content" source="../../assets/images/tabs/mobile-tab-grid.png" alt-text="Abbildung zeigt eine Mobile Teams-Registerkarte mit einem Rasterlayout." border="false":::

### <a name="tabs-with-bots-on-mobile"></a>Tabs mit Bots auf mobilen Geräten

Das folgende Beispiel ist eine persönliche APP, die über Registerkarten und einen bot verfügt.

:::image type="content" source="../../assets/images/tabs/mobile-tab-with-bot.png" alt-text="Abbildung, die zeigt, wie Mobile Teams-App mit Registerkarten und bot." border="false":::

## <a name="ui-components"></a>Benutzeroberflächenkomponenten

### <a name="color-palettes"></a>Farbpaletten

Die Verwendung unserer genehmigten neutralen Palette für Hintergründe, Benachrichtigungen, Text und Schaltflächen hilft Ihrer APP, sich in Microsoft Teams zu Hause zu fühlen. Da Teams Mobile zwei Farbdesigns (hell und dunkel) aufweist, sollten Sie sicherstellen, dass Ihre APP in beiden Farben gut aussieht.

#### <a name="light-color"></a>Helle Farbe

![helle Farbpalette](../../assets/images/light-color.png)

#### <a name="dark-color"></a>Dunkle Farbe

![dunkle Farbpalette](../../assets/images/dark-color.png)

### <a name="buttons-and-controls"></a>Schaltflächen und Steuerelemente

Die Art und Weise, wie Schaltflächen formatiert werden, hilft bei der Kommunikation, welche Art von Aktion ausgelöst wird. Wir halten eine Vielzahl von Schaltflächen, die formatiert sind, um unterschiedliche Schwerpunkte anzuzeigen. Schaltflächen können Text, ein Symbol oder eine Kombination aus Text und Symbol aufweisen. Um verschiedene Ebenen in einer Hierarchie zu kommunizieren, haben wir die primären und sekundären Schaltflächen innerhalb der einzelnen Kategorien entworfen.

#### <a name="buttons"></a>Schaltflächen

Primäre und sekundäre Schaltflächen.

![Schaltflächenbild](../../assets/images/buttons.png)

#### <a name="selection-controls"></a>Auswahlsteuerelemente

Optionsfelder, Kontrollkästchen und Umschaltflächen.

![Auswahlsteuerelemente](../../assets/images/selection-controls.png)

#### <a name="chiclets-and-pills"></a>Chiclets und Pillen

![Chiclets und Pillen](../../assets/images/chiclets-and-pills.png)

### <a name="typography"></a>Typografie

Typografie sollte eindeutig und zielgerichtet sein. Betonen Sie wichtige Informationen, und vermeiden Sie die Verwendung mehrerer Schriftarten und Größen zur Verringerung von Verwirrung. Wir empfehlen die Verwendung von satzfall und vermeiden die Verwendung aller Caps für Lokalisierung und Lesbarkeit.

![Mobile Typograph](../../assets/images/mobile-typography.png)

### <a name="fields-and-flyouts"></a>Felder und Flyouts

Felder sind Bereiche, in denen Benutzer Text eingeben können. Flyouts sind leichter als Dialogfelder und werden aus dem oberen Bereich angezeigt.

#### <a name="list-controls"></a>Steuerelemente auflisten

![Mobile Listensteuerelemente](../../assets/images/mobile-list-controls.png)

#### <a name="field-controls"></a>Feldsteuerelemente

![Mobile Feldsteuerelemente](../../assets/images/mobile-field-controls.png)

## <a name="developer-considerations"></a>Entwickler Überlegungen

Wenn Sie eine APP erstellen, die eine Registerkarte enthält, müssen Sie prüfen (und testen), wie Ihre Registerkarte sowohl auf den Android-als auch IOS-Microsoft Teams-Clients funktioniert. In den folgenden Abschnitten werden einige der wichtigsten Szenarien beschrieben, die Sie in diesem Punkt behandeln müssen.

### <a name="testing-on-mobile-clients"></a>Testen auf mobilen Clients

Sie müssen überprüfen, ob Ihre Registerkarte auf mobilen Geräten in verschiedenen Größen und Qualitäten ordnungsgemäß funktioniert. Für Android-Geräte können Sie die [devtools](~/tabs/how-to/developer-tools.md) verwenden, um Ihre Registerkarte während der Ausführung zu debuggen. Es wird empfohlen, sowohl auf High-als auch auf niedrig leistungsfähigen Geräten sowie auf einem Tablet zu testen.

### <a name="authentication"></a>Authentifizierung

Damit die Authentifizierung auf mobilen Clients funktioniert, müssen Sie Microsoft Teams JavaScript SDK auf mindestens Version 1.4.1 aktualisieren.

### <a name="low-bandwidth-and-intermittent-connections"></a>Niedrige Bandbreite und zeitweilige Verbindungen

Mobile Clients müssen regelmäßig mit niedriger Bandbreite und zeitweiligen Verbindungen arbeiten. Ihre APP sollte alle Timeouts entsprechend behandeln, indem Sie dem Benutzer eine Kontext Meldung bereitstellt. Sie sollten auch Benutzer Fortschrittsindikatoren angeben, um Ihren Benutzern Feedback für alle langwierigen Prozesse zur Verfügung zu stellen.
