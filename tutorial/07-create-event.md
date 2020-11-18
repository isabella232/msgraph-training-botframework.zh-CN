---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347750"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将使用 Microsoft Graph SDK 向用户的日历中添加事件。

## <a name="implement-a-dialog"></a>实现对话

首先创建一个新的自定义对话框，提示用户输入将事件添加到其日历中所需的值。 此对话框将使用 **WaterfallDialog** 执行以下步骤。

- 提示输入主题
- 询问用户是否要邀请人员
- 如果用户在上一步中说 "是"，则提示与会者 () 
- 提示输入开始日期和时间
- 提示输入结束日期和时间
- 显示所有收集的值，并要求用户确认
- 如果用户确认，则从 **OAuthPrompt** 获取访问令牌
- 创建事件

1. 在名为 **NewEventDialog.cs** 的 **/Dialogs** 目录中创建一个新文件，并添加以下代码。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;
    using CalendarBot.Graph;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    using TimexTypes = Microsoft.Recognizers.Text.DataTypes.TimexExpression.Constants.TimexTypes;

    namespace CalendarBot.Dialogs
    {
        public class NewEventDialog : LogoutDialog
        {
            protected readonly ILogger _logger;
            private readonly IGraphClientService _graphClientService;

            public NewEventDialog(
                IConfiguration configuration,
                IGraphClientService graphClientService)
                : base(nameof(NewEventDialog), configuration["ConnectionName"])
            {

            }

            // Generate a DateTime from the list of
            // DateTimeResolutions provided by the DateTimePrompt
            private static DateTime GetDateTimeFromResolutions(IList<DateTimeResolution> resolutions)
            {
                var timex = new TimexProperty(resolutions[0].Timex);

                // Handle the "now" case
                if (timex.Now ?? false)
                {
                    return DateTime.Now;
                }

                // Otherwise generate a DateTime
                return TimexHelpers.DateFromTimex(timex);
            }
        }
    }
    ```

    这是一个新对话框的命令行管理程序，该对话框将提示用户输入将事件添加到其日历中所需的值。

1. 将以下函数添加到 **NewEventDialog** 类，以提示用户输入主题。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以存储用户在上一步中提供的主题，并询问是否要添加与会者。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以检查用户对上一步骤的响应，并在需要时提示用户输入与会者列表。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类以存储上一步骤中的与会者列表 (如果存在) 并提示用户输入开始日期和时间。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类以存储上一步骤的起始值，并提示用户输入结束日期和时间。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类以存储上一步骤的结束值，并要求用户确认所有输入。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以检查用户在上一步中的响应。 如果用户确认输入，则使用 **OAuthPrompt** 类获取访问令牌。 否则，请结束对话框。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以使用 MICROSOFT Graph SDK 创建新事件。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    请考虑此代码执行的操作。

    - 它将获取用户的 **MailboxSettings** ，以确定用户的首选时区。
    - 它使用用户提供的值创建 **Event** 对象。 请注意， **开始** 和 **结束** 属性设置为用户的时区。
    - 它会在用户的日历上创建事件。

## <a name="add-validation"></a>添加验证

现在，将验证添加到用户的输入，以避免在使用 Microsoft Graph 创建事件时出现错误。 我们希望确保：

- 如果用户提供了与会者列表，则它应是以分号分隔的有效电子邮件地址列表。
- 开始日期/时间应为有效的日期和时间。
- 结束日期/时间应为有效的日期和时间，并且应晚于 start。

1. 将以下函数添加到 **NewEventDialog** 类以验证用户输入的与会者。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以验证用户输入的开始日期和时间。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. 将以下函数添加到 **NewEventDialog** 类，以验证用户输入的结束日期和时间。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a>向 WaterfallDialog 添加步骤

现在，您已拥有对话框的所有 "步骤"，最后一步是将其添加到构造函数中的 **WaterfallDialog** ，然后将 **NewEventDialog** 添加到 **MainDialog** 中。

1. 找到 **NewEventDialog** 构造函数，并将以下代码添加到该构造函数中。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    这将添加所有使用的对话框，并将您实现的所有函数添加为 **WaterfallDialog** 中的步骤。

1. 打开 **/Dialogs/MainDialog.cs** ，并将以下行添加到构造函数中。

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. 将块内的代码替换 `else if (command.StartsWith("add event"))` `ProcessStepAsync` 为以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. 保存所有更改，然后重新启动机器人。

1. 使用 Bot 框架仿真程序连接到机器人并登录。 选择 " **添加事件** " 按钮。

1. 响应创建新事件的提示。 请注意，当系统提示输入起始和结束值时，您可以使用 "今天 3PM" 或 "now" 之类的短语。

    ![Bot 框架仿真程序中的 "添加事件" 对话框的屏幕截图](images/add-event.png)
