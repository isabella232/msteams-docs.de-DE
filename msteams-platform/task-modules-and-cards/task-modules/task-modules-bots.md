---
title: Verwenden von Aufgaben Modulen in Microsoft Teams-Bots
description: Vorgehensweise verwenden von Aufgaben Modulen mit Microsoft Teams-Bots, einschließlich bot-Framework-Karten, Adaptive Karten und Deep Links.
keywords: Aufgaben Module-Bots für Teams
ms.openlocfilehash: 32fb6a4aa0a8bf2297a4f60331dc5c6c6aceb4e2
ms.sourcegitcommit: 214eccbadb7f3a67236b79a041ef487b7bf6dfbd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2020
ms.locfileid: "44801328"
---
# <a name="using-task-modules-from-microsoft-teams-bots"></a>Verwenden von Aufgaben Modulen von Microsoft Teams-Bots

Aufgaben Module können von Microsoft Teams-Bots mithilfe von Schaltflächen auf adaptiven Karten und bot-Framework-Karten (Hero, Thumbnail und Office 365-Konnektor) aufgerufen werden. Aufgaben Module sind häufig eine bessere Benutzeroberfläche als mehrere Unterhaltungs Schritte, bei denen Sie als Entwickler den Status von bot überwachen und dem Benutzer die Unterbrechung/Absage der Sequenz ermöglichen müssen.

Es gibt zwei Möglichkeiten zum Aufrufen von Aufgaben Modulen:

* **Eine neue Art von Invoke `task/fetch` -Nachricht.** Wenn Sie die Karten `invoke` [Aktion](~/task-modules-and-cards/cards/cards-actions.md#invoke) für bot-Framework-Karten verwenden oder die Karten `Action.Submit` [Aktion](~/task-modules-and-cards/cards/cards-actions.md#adaptive-cards-actions) für Adaptive Karten mit `task/fetch` , wird das Aufgabenmodul (entweder eine URL oder eine Adaptive Karte) dynamisch von Ihrem bot abgerufen.
* **Deep Link-URLs.** Mithilfe der [Deep Link-Syntax für Aufgaben Module](~/task-modules-and-cards/what-are-task-modules.md#task-module-deep-link-syntax)können Sie die Karten `openUrl` [Aktion](~/task-modules-and-cards/cards/cards-actions.md#openurl) für bot-frameworkkarten oder die `Action.OpenUrl` [Karten Aktion](~/task-modules-and-cards/cards/cards-actions.md#adaptive-cards-actions) für Adaptive Karten verwenden. Bei Deep Link-URLs ist die URL des Aufgabenmoduls oder der Adaptive Kartentext offensichtlich vorab bekannt, sodass ein Server-Roundtrip relativ zu vermieden wird `task/fetch` .

>[!IMPORTANT]
>Um die sichere Kommunikation sicherzustellen `url` , `fallbackUrl` müssen Sie das HTTPS-Verschlüsselungsprotokoll implementieren.

## <a name="invoking-a-task-module-via-taskfetch"></a>Aufrufen eines Aufgabenmoduls über Task/FETCH

Wenn das `value` Objekt der `invoke` Karte `Action.Submit` auf ordnungsgemäße Weise initialisiert wird (im folgenden näher erläutert), wird beim Drücken der Schaltfläche eine `invoke` Nachricht an den bot gesendet. In der HTTP-Antwort auf die `invoke` Nachricht gibt es ein [taskinfo-Objekt](~/task-modules-and-cards/what-are-task-modules.md#the-taskinfo-object) , das in ein Wrapperobjekt eingebettet ist, das die Teams zum Anzeigen des Aufgabenmoduls verwenden.

![Aufgabe/Abrufanforderung/Antwort](~/assets/images/task-module/task-module-invoke-request-response.png)

Lassen Sie uns die einzelnen Schritte etwas ausführlicher betrachten:

1. In diesem Beispiel wird eine Hero-Karte für bot-Frameworks mit einer "Buy" `invoke` - [Karten Aktion](~/task-modules-and-cards/cards/cards-actions.md#invoke)gezeigt. Der Wert der `type` Eigenschaft ist `task/fetch` -der Rest des `value` Objekts kann sein, was Sie möchten.
2. Der bot erhält die `invoke` http-Post-Nachricht.
3. Der bot erstellt ein Response-Objekt und gibt es im Text der Post-Antwort mit einem HTTP 200-Antwortcode zurück. Das Schema für Antworten wird [im folgenden in der Diskussion über Aufgabe/Submit](#the-flexibility-of-tasksubmit)beschrieben, aber wichtig ist jetzt, dass der Text der HTTP-Antwort ein [taskinfo-Objekt](~/task-modules-and-cards/what-are-task-modules.md#the-taskinfo-object) enthält, das in ein Wrapperobjekt eingebettet ist, beispielsweise:

    ```json
    {
      "task": {
        "type": "continue",
        "value": {
          "title": "Task module title",
          "height": 500,
          "width": "medium",
          "url": "https://contoso.com/msteams/taskmodules/newcustomer",
          "fallbackUrl": "https://contoso.com/msteams/taskmodules/newcustomer"
        }
      }
    }
    ```

    Das `task/fetch` Ereignis und seine Antwort auf Bots ähneln konzeptuell der `microsoftTeams.tasks.startTask()` Funktion im Client-SDK.
4. Microsoft Teams zeigt den Aufgabenmodul an.

## <a name="submitting-the-result-of-a-task-module"></a>Übermitteln des Ergebnisses eines Aufgabenmoduls

Wenn der Benutzer mit dem Aufgabenmodul fertig ist, ist das Senden des Ergebnisses zurück an den bot ähnlich wie [bei Registerkarten](~/task-modules-and-cards/task-modules/task-modules-tabs.md#example-submitting-the-result-of-a-task-module), es gibt jedoch einige Unterschiede, die daher auch hier beschrieben werden.

* **HTML/JavaScript ( `TaskInfo.url` )**. Nachdem Sie überprüft haben, was der Benutzer eingegeben hat, rufen Sie die `microsoftTeams.tasks.submitTask()` SDK-Funktion auf (wird im folgenden als `submitTask()` Zweck der Lesbarkeit bezeichnet). Sie können `submitTask()` ohne Parameter aufrufen, wenn Sie lediglich möchten, dass Teams den Aufgabenmodul schließen, aber in den meisten Fällen möchten Sie ein Objekt oder eine Zeichenfolge an Ihren übergeben `submitHandler` . Übergeben Sie es einfach als ersten Parameter, `result` . Teams werden aufgerufen `submitHandler` : `err` wird `null` und ist `result` das Objekt/die Zeichenfolge, die Sie an übergeben haben `submitTask()` . Wenn Sie `submitTask()` einen `result` Parameter aufrufen, **müssen** Sie ein `appId` oder ein Array von `appId` Zeichenfolgen übergeben: Dadurch können Teams überprüfen, ob die APP, die das Ergebnis sendet, dieselbe ist, die das Aufgabenmodul aufgerufen hat. Ihr bot erhält eine `task/submit` Nachricht `result` wie [unten](#payload-of-taskfetch-and-tasksubmit-messages)beschrieben.
* **Adaptive Karte ( `TaskInfo.card` )**. Der Adaptive Kartentext (wie vom Benutzer ausgefüllt) wird über eine Nachricht an den bot gesendet, `task/submit` Wenn der Benutzer eine beliebige `Action.Submit` Schaltfläche drückt.

## <a name="the-flexibility-of-tasksubmit"></a>Flexibilität von Task/Submit

Im vorherigen Abschnitt haben Sie gelernt, dass der bot immer eine Nachricht erhält, wenn der Benutzer mit einem Aufgabenmodul endet, das von einem bot aufgerufen wird `task/submit invoke` . Als Entwickler haben Sie mehrere Möglichkeiten, wenn Sie auf die Nachricht *reagieren* `task/submit` :

| HTTP-Text Antwort                      | Szenario                                |
| --------------------------------------- | --------------------------------------- |
| None (Nachricht ignorieren `task/submit` ) | Die einfachste Antwort ist überhaupt keine Antwort. Ihr bot muss nicht Antworten, wenn der Benutzer mit dem Aufgabenmodul fertig ist. |
| <pre>{<br/>  "task": {<br/>    "type": "message",<br/>    "value": "Message text"<br/>  }<br/>}</pre> | In Microsoft Teams wird der Wert `value` in einem Popupmeldungsfeld angezeigt. |
| <pre>{<br/>  "task": {<br/>    "type": "continue",<br/>    "value": &lt;TaskInfo object&gt;<br/>  }<br/>}</pre> | Ermöglicht es Ihnen, Sequenzen von adaptiven Karten in einer Assistenten-/mehrstufigen Umgebung zu "verketten". _Beachten Sie, dass das Verketten von adaptiven Karten in eine Sequenz ein erweitertes Szenario ist und hier nicht dokumentiert ist. Die Node.js-Beispiel-App unterstützt Sie jedoch und wie Sie funktioniert, ist in der [Readme.MD-Datei](https://github.com/OfficeDev/microsoft-teams-sample-task-module-nodejs#implementation-notes)dokumentiert._ |

## <a name="payload-of-taskfetch-and-tasksubmit-messages"></a>Nutzlast von Task/FETCH und Task/Submit-Nachrichten

Dieser Abschnitt definiert das Schema dessen, was Ihr bot erhält, wenn er ein `task/fetch` oder `task/submit` bot Framework `Activity` -Objekt empfängt. Die wichtige obere Ebene wird unten angezeigt:

| Eigenschaft | Beschreibung                          |
| -------- | ------------------------------------ |
| `type`   | Wird immer`invoke`              |
| `name`   | Entweder `task/fetch` oder`task/submit` |
| `value`  | Die vom Entwickler definierte Nutzlast. Normalerweise spiegelt die Struktur des `value` Objekts das, was von Teams gesendet wurde. In diesem Fall ist es jedoch anders, da wir Dynamic FETCH ( `task/fetch` ) sowohl von bot-Framework () als `value` auch von Adaptive Card-Aktionen () unterstützen möchten `Action.Submit` `data` , und wir benötigen eine Möglichkeit, Teams `context` mit dem bot zu kommunizieren, zusätzlich zu dem, was in enthalten war `value` / `data` .<br/><br/>Dies erfolgt durch Kombinieren der beiden Elemente in einem übergeordneten Objekt:<br/><br/><pre>{<br/>  "context": {<br/>    "theme": "default" &vert; "dark" &vert; "contrast",<br/>  },<br/>  "data": [value field from Bot Framework card] &vert; [data field from Adaptive Card] <br/>}</pre>  |

## <a name="example-receiving-and-responding-to-taskfetch-and-tasksubmit-invoke-messages---nodejs"></a>Beispiel: empfangen und reagieren auf Tasks/FETCH-und Task/Submit-Invoke-Nachrichten – Node.js

> [!NOTE]
> Der folgende Beispielcode wurde zwischen der technischen Vorschau und der endgültigen Version dieses Features geändert: das Schema der `task/fetch` Anforderung wurde geändert, um zu verfolgen, was [im vorherigen Abschnitt dokumentiert](#payload-of-taskfetch-and-tasksubmit-messages)wurde. Das heißt, die Dokumentation war richtig, aber die Implementierung war nicht. In den `// for Technical Preview [...]` Kommentaren unten finden Sie Informationen zu den geänderten Funktionen.

```typescript
// Handle requests and responses for a "Custom Form" and an "Adaptive card" task module.
// Assumes request is coming from an Adaptive card Action.Submit button that has a "taskModule" property indicating what to invoke
private async onInvoke(event: builder.IEvent, cb: (err: Error, body: any, status?: number) => void): Promise<void> {
    let invokeType = (event as any).name;
    let invokeValue = (event as any).value;
    if (invokeType === undefined) {
        invokeType = null;
    }
    switch (invokeType) {
        case "task/fetch": {
            if (invokeValue !== undefined && invokeValue.data.taskModule === "customform") { // for Technical Preview, was invokeValue.taskModule
                // Return the specified task module response to the bot
                let fetchTemplate: any = {
                    "task": {
                        "type": "continue",
                        "value": {
                            "title": "Custom Form",
                            "height": 510,
                            "width": 430,
                            "fallbackUrl": "https://contoso.com/teamsapp/customform",
                            "url": "https://contoso.com/teamsapp/customform",
                        }
                    }
                };
                cb(null, fetchTemplate, 200);
            };
            if (invokeValue !== undefined && invokeValue.data.taskModule === "adaptivecard") { // for Technical Preview, was invokeValue.taskModule
                let adaptiveCard = {
                    "type": "AdaptiveCard",
                    "body": [
                        {
                            "type": "TextBlock",
                            "text": "Here is a ninja cat:"
                        },
                        {
                            "type": "Image",
                            "url": "http://adaptivecards.io/content/cats/1.png",
                            "size": "Medium"
                        }
                    ],
                    "version": "1.0"
                };
                // Return the specified task module response to the bot
                let fetchTemplate: any = {
                    "task": {
                        "type": "continue",
                        "value": {
                            "title": "Ninja Cat",
                            "height": "small",
                            "width": "small",
                            "card": {
                                contentType: "application/vnd.microsoft.card.adaptive",
                                content: adaptiveCard,
                            }
                        }
                    }
                };
                cb(null, fetchTemplate, 200);
            };
            break;
        }
        case "task/submit": {
            if (invokeValue.data !== undefined) {
                // It's a valid task module response
                let submitResponse: any = {
                    "task": {
                        "type": "message",
                        "value": "Task complete!",
                    }
                };
                cb(null, fetchTemplates.submitMessageResponse, 200)
            }
        }
    }
}
```

*Siehe auch* [Microsoft Teams-Beispielcode für Aufgaben Module –](https://github.com/OfficeDev/microsoft-teams-sample-task-module-nodejs/blob/master/src/TeamsBot.ts) Beispiele für nodejs und [bot-Framework](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md).

## <a name="example-receiving-and-responding-to-taskfetch-and-tasksubmit-invoke-messages---c"></a>Beispiel: empfangen und reagieren auf Tasks/FETCH-und Task/Submit-Invoke-Nachrichten-C #

In C#-Bots `invoke` werden Nachrichten von einem `HttpResponseMessage()` Controller verarbeitet, der eine `Activity` Nachricht verarbeitet. Die `task/fetch` `task/submit` Anforderungen und Antworten sind JSON. In C# ist es nicht so bequem, sich mit RAW-JSON wie in Node.js zu beschäftigen, daher benötigen Sie Wrapperklassen, um die Serialisierung von und zu JSON zu verarbeiten. Es gibt noch keine direkte Unterstützung dafür im Microsoft Teams [C# SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams) , aber Sie sehen ein Beispiel dafür, wie diese einfachen Wrapperklassen in der c#-Beispiel- [App](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Models/TaskModel.cs)aussehen würden.

Im folgenden finden Sie Beispielcode in C# zur Behandlung `task/fetch` und `task/submit` Nachrichten mit diesen Wrapperklassen ( `TaskInfo` , `TaskEnvelope` ), die aus dem [Beispiel](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Controllers/MessagesController.cs)entnommen wurden:

```csharp
private HttpResponseMessage HandleInvokeMessages(Activity activity)
{
    var activityValue = activity.Value.ToString();
    if (activity.Name == "task/fetch")
    {
        var action = Newtonsoft.Json.JsonConvert.DeserializeObject<Models.BotFrameworkCardValue<string>>(activityValue);

        Models.TaskInfo taskInfo = GetTaskInfo(action.Data);
        Models.TaskEnvelope taskEnvelope = new Models.TaskEnvelope
        {
            Task = new Models.Task()
            {
                Type = Models.TaskType.Continue,
                TaskInfo = taskInfo
            }
        };
        return Request.CreateResponse(HttpStatusCode.OK, taskEnvelope);
    }
    else if (activity.Name == "task/submit")
    {
        ConnectorClient connector = new ConnectorClient(new Uri(activity.ServiceUrl));
        Activity reply = activity.CreateReply("Received = " + activity.Value.ToString());
        connector.Conversations.ReplyToActivity(reply);
    }
    return new HttpResponseMessage(HttpStatusCode.Accepted);
}

// Helper function for building the TaskInfo object based on the incoming request
private static Models.TaskInfo GetTaskInfo(string actionInfo)
{
    Models.TaskInfo taskInfo = new Models.TaskInfo();
    switch (actionInfo)
    {
        case TaskModuleIds.YouTube:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.YouTube;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.YouTube);
            break;
        case TaskModuleIds.PowerApp:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.PowerApp;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.PowerApp);
            break;
        case TaskModuleIds.CustomForm:
            taskInfo.Url = taskInfo.FallbackUrl = ApplicationSettings.BaseUrl + "/" + TaskModuleIds.CustomForm;
            SetTaskInfo(taskInfo, TaskModuleUIConstants.CustomForm);
            break;
        case TaskModuleIds.AdaptiveCard:
            taskInfo.Card = AdaptiveCardHelper.GetAdaptiveCard();
            SetTaskInfo(taskInfo, TaskModuleUIConstants.AdaptiveCard);
            break;
        default:
            break;
    }
    return taskInfo;
}
```

Im obigen Beispiel nicht dargestellt ist die- `SetTaskInfo()` Funktion, die die `height` , `width` und die `title` Eigenschaften des `TaskInfo` Objekts für jeden Fall festlegt. Hier ist der [Quellcode für SetTaskInfo ()](https://github.com/OfficeDev/microsoft-teams-sample-task-module-csharp/blob/master/Microsoft.Teams.Samples.TaskModule.Web/Controllers/MessagesController.cs).

### <a name="bot-framework-card-actions-vs-adaptive-card-actionsubmit-actions"></a>Bot-Framework-Karten Aktionen vs. Adaptive Karten Aktion. Submit-Aktionen

Das Schema für Aktionen von bot-Framework-Karten unterscheidet sich geringfügig von adaptiven Karten `Action.Submit` Aktionen. Dadurch unterscheidet sich auch die Möglichkeit zum Aufrufen von Aufgaben Modulen geringfügig: das `data` Objekt in `Action.Submit` enthält ein `msteams` Objekt, sodass es nicht mit anderen Eigenschaften in der Karte in Konflikt kommt. Die folgende Tabelle zeigt ein Beispiel:

| Bot-Framework-Karten Aktion                              | Adaptive Karten Aktion. Submit-Aktion                     |
| ------------------------------------------------------ | ------------------------------------------------------ |
| <pre>{<br/>  "type": "invoke",<br/>  "title": "Buy",<br/>  "value": {<br/>    "type": "task/fetch",<br/>    &lt;...&gt;<br/>  }<br/>}</pre> | <pre>{<br/>  "type": "Action.Submit",<br/>  "id": "btnBuy",<br/>  "title": "Buy",<br/>  "data": {<br/>    &lt;...&gt;,<br/>    "msteams": {<br/>      "type": "task/fetch"<br/>    }<br/>  }<br/>}</pre>  |
