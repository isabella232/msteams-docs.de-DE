---
title: Einführung von Karten
description: Beschreibt Karten und deren Verwendung in Bots, Connectors und Messaging-Erweiterungen
keywords: Connectors Bots Cards Messaging
ms.openlocfilehash: 6d850f83183f12fa0c228a7a89b23e58f523e15b
ms.sourcegitcommit: 9fbc701a9a039ecdc360aefbe86df52b9c3593f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2020
ms.locfileid: "46651656"
---
# <a name="cards"></a>Karten

Eine *Karte* ist ein Benutzeroberflächencontainer für kurze oder verwandte Informationen. Karten können mehrere Eigenschaften und Anlagen aufweisen. Karten können Schaltflächen enthalten, die [Karten Aktionen](~/task-modules-and-cards/cards/cards-actions.md)auslösen können.

## <a name="adaptive-cards"></a>Adaptive Karten

[Adaptive Cards](~/task-modules-and-cards/cards/cards-reference.md#adaptive-card) sind eine neue produktübergreifende Spezifikation für Karten in Microsoft-Produkten, einschließlich Bots, Cortana, Outlook und Windows. Sie sind der empfohlene Kartentyp für die Entwicklung neuer Teams. Allgemeine Informationen aus dem Adaptive Cards-Team finden Sie unter [Adaptive Cards Overview](/adaptive-cards). Sie können Adaptive Karten überall dort verwenden, wo Sie vorhandene heldenkarten, Office365-Karten und Miniatur Ansichtskarten verwenden können.

Zusätzlich zu adaptiven Karten unterstützt Microsoft Teams zwei andere Kartentypen:

* Verbindungskarten, die als Teil von Office 365-Konnektoren verwendet werden.
* Einfache Karten aus dem bot-Framework, wie die Thumbnail-und Hero-Karten.

Diese Kartentypen werden in der Referenz für die [Teams-Karte](~/task-modules-and-cards/cards/cards-reference.md)ausführlicher beschrieben.

Microsoft Teams verwendet Karten an drei verschiedenen Orten:

* Connectors
* Bots
* Messaging-Erweiterungen

## <a name="adaptive-cards-and-incoming-webhooks"></a>Adaptive Karten und eingehende webhooks

> [!NOTE]
> Adaptive Karten werden in eingehenden webhooks als Teil des [öffentlichen Developer Preview-Programms](../resources/dev-preview/developer-preview-intro.md)unterstützt. Öffentliche Vorschauen stehen für den frühzeitigen Zugriff und Feedback zur Verfügung. Obwohl die Version stabil ist und umfangreiche Tests unterzogen wurde, ist Sie nicht für die Verwendung in der Produktion vorgesehen.
>
> ✔ In der Entwicklervorschau werden alle systemeigenen adaptiven Karten Schemaelemente, außer `Action.Submit` , vollständig unterstützt.
>
> ✔ Die unterstützten Aktionen sind [**Action. OpenURL**](https://adaptivecards.io/explorer/Action.OpenUrl.html), [**Action. ShowCard**](https://adaptivecards.io/explorer/Action.ShowCard.html)und [**Action. ToggleVisibility**](https://adaptivecards.io/explorer/Action.ToggleVisibility.html).

## <a name="cards-in-connectors"></a>Karten in Konnektoren

Karten wurden zuerst als Teil von Outlook und Office 365 definiert und werden als Teil von Office 365 Connectors verwendet. Wie viele Office 365-Anwendungen unterstützt Microsoft Teams Connectors. Weitere Informationen zu Connectors finden Sie unter [Office 365 Connectors for Microsoft Teams](~/webhooks-and-connectors/what-are-webhooks-and-connectors.md), und suchen Sie nach der Spezifikation für Karten in Connectors in umsetzbarer [Nachrichten Karten Referenz](/outlook/actionable-messages/card-reference).

## <a name="cards-in-bots"></a>Karten in Bots

Das Microsoft bot Framework erweiterte die Karten Spezifikation durch Hinzufügen einer Reihe von vordefinierten Karten, die Bots als Teil von bot-Nachrichten verwenden konnten. Teams unterstützen Bots mithilfe des bot-Frameworks, unterstützen jedoch eine etwas andere Gruppe dieser Karten. Allgemeine Informationen zu Karten im bot-Framework finden Sie unter [Add Rich Card Attachments to messages](/bot-framework/nodejs/bot-builder-nodejs-send-rich-cards). Diese Karten werden als *einfache Karten* in Microsoft Teams bezeichnet.

Bots in Microsoft Teams können beliebige Kartentypen verwenden: Simple, Connector oder Adaptive. Karten, die von Bots in Microsoft Teams unterstützt werden, werden in der Microsoft [Teams-Karten Referenz](~/task-modules-and-cards/cards/cards-reference.md)ausführlich beschrieben.  

## <a name="cards-in-messaging-extensions"></a>Karten in Messaging-Erweiterungen

[Messaging-Erweiterungen](~/messaging-extensions/what-are-messaging-extensions.md) können auch eine Karte zurückgeben. Messaging Erweiterungen können beliebige Kartentypen verwenden: Simple, Connector oder Adaptive. Diese Karten finden Sie in der [Referenz](~/task-modules-and-cards/cards/cards-reference.md)zu den Teams-Karten.

## <a name="card-reference"></a>Karten Referenz

Alle von Microsoft Teams verwendeten Karten werden in der [Referenz](~/task-modules-and-cards/cards/cards-reference.md)für die Teams-Karte aufgeführt. Diese Referenz beschreibt auch die Unterschiede zwischen den bot-Framework-Karten und-Karten in Microsoft Teams.
