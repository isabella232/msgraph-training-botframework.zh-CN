---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347760"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，你将使用 Bot 框架的 **OAuthPrompt** 在 Bot 中实现身份验证，并获取用于调用 MICROSOFT Graph API 的访问令牌。

1. 打开 **。/appsettings.js** ，并进行以下更改。

    - 将值更改 `MicrosoftAppId` 为您的 **图形日历机器人** 应用注册的应用程序 ID。
    - 将值更改 `MicrosoftAppPassword` 为您的 **图形日历机器人** 客户端密码。
    - 将名为的值添加为 `ConnectionName` `GraphBotAuth` 。

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > 如果你使用的值不是 `GraphBotAuth` Azure 门户的 **OAuth 连接设置** 中条目的名称，请使用该条目的值 `ConnectionName` 。

## <a name="implement-dialogs"></a>实现对话

1. 在名为 " **对话**" 的项目的根目录中创建一个新目录。 在名为 **LogoutDialog.cs** 的 **/Dialogs** 目录中创建一个新文件，并添加以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    此对话框为要从其派生的自动程序中的所有其他对话框提供基类。 这使用户可以注销，无论它们位于机器人对话框中的什么位置。

1. 在名为 **MainDialog.cs** 的 **/Dialogs** 目录中创建一个新文件，并添加以下代码。

    ```csharp
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Dialogs.Choices;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;

    namespace CalendarBot.Dialogs
    {
        public class MainDialog : LogoutDialog
        {
            const string NO_PROMPT = "no-prompt";
            protected readonly ILogger _logger;

            public MainDialog(IConfiguration configuration, ILogger<MainDialog> logger)
                : base(nameof(MainDialog), configuration["ConnectionName"])
            {
                _logger = logger;

                // OAuthPrompt dialog handles the authentication and token
                // acquisition
                AddDialog(new OAuthPrompt(
                    nameof(OAuthPrompt),
                    new OAuthPromptSettings
                    {
                        ConnectionName = ConnectionName,
                        Text = "Please login",
                        Title = "Login",
                        Timeout = 300000, // User has 5 minutes to login
                    }));

                AddDialog(new ChoicePrompt(nameof(ChoicePrompt)));

                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    LoginPromptStepAsync,
                    ProcessLoginStepAsync,
                    PromptUserStepAsync,
                    CommandStepAsync,
                    ProcessStepAsync,
                    ReturnToPromptStepAsync
                }));

                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }

            private async Task<DialogTurnResult> LoginPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessLoginStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                // Get the token from the previous step. If it's there, login was successful
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;
                    if (!string.IsNullOrEmpty(tokenResponse?.Token))
                    {
                        await stepContext.Context.SendActivityAsync(
                            MessageFactory.Text("You are now logged in."), cancellationToken);
                        return await stepContext.NextAsync(null, cancellationToken);
                    }
                }

                await stepContext.Context.SendActivityAsync(
                    MessageFactory.Text("Login was not successful please try again."), cancellationToken);
                return await stepContext.EndDialogAsync();
            }

            private async Task<DialogTurnResult> PromptUserStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                var options = new PromptOptions
                {
                    Prompt = MessageFactory.Text("Please choose an option below"),
                    Choices = new List<Choice> {
                        new Choice { Value = "Show token" },
                        new Choice { Value = "Show me" },
                        new Choice { Value = "Show calendar" },
                        new Choice { Value = "Add event" },
                        new Choice { Value = "Log out" },
                    }
                };

                return await stepContext.PromptAsync(
                    nameof(ChoicePrompt),
                    options,
                    cancellationToken);
            }

            private async Task<DialogTurnResult> CommandStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Save the command the user entered so we can get it back after
                // the OAuthPrompt completes
                var foundChoice = stepContext.Result as FoundChoice;
                // Result could be a FoundChoice (if user selected a choice button)
                // or a string (if user just typed something)
                stepContext.Values["command"] = foundChoice?.Value ?? stepContext.Result;

                // There is no reason to store the token locally in the bot because we can always just call
                // the OAuth prompt to get the token or get a new token if needed. The prompt completes silently
                // if the user is already signed in.
                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;

                    // If we have the token use the user is authenticated so we may use it to make API calls.
                    if (tokenResponse?.Token != null)
                    {
                        var command = ((string)stepContext.Values["command"] ?? string.Empty).ToLowerInvariant();

                        if (command.StartsWith("show token"))
                        {
                            // Show the user's token - for testing and troubleshooting
                            // Generally production apps should not display access tokens
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text($"Your token is: {tokenResponse.Token}"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show me"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show calendar"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("add event"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I'm sorry, I didn't understand. Please try again."),
                                cancellationToken);
                        }
                    }
                }
                else
                {
                    await stepContext.Context.SendActivityAsync(
                        MessageFactory.Text("We couldn't log you in. Please try again later."),
                        cancellationToken);
                    return await stepContext.EndDialogAsync(cancellationToken: cancellationToken);
                }

                // Go to the next step
                return await stepContext.NextAsync(cancellationToken: cancellationToken);
            }

            private async Task<DialogTurnResult> ReturnToPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Restart the dialog, but skip the initial login prompt
                return await stepContext.ReplaceDialogAsync(InitialDialogId, NO_PROMPT, cancellationToken);
            }
        }
    }
    ```

    请花点时间查看此代码。

    - 在构造函数中，它将设置一个 [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) ，其中包含一组按顺序发生的步骤。
        - 在 `LoginPromptStepAsync` 其中发送 **OAuthPrompt**。 如果用户未登录，则会向用户发送 UI 提示。
        - 在 `ProcessLoginStepAsync` 该示例中检查登录是否成功，并发送一条确认消息。
        - 在中，将 `PromptUserStepAsync` 使用可用命令发送 **ChoicePrompt** 。
        - 在 `CommandStepAsync` 其中保存用户的选择，然后重新发送 **OAuthPrompt**。
        - 在 `ProcessStepAsync` 此过程中，将根据收到的命令执行操作。
        - 在 `ReturnToPromptStepAsync` 该示例中，将启动瀑布，但传递一个标志以跳过初始用户登录。

## <a name="update-calendarbot"></a>更新 CalendarBot

下一步是更新 **CalendarBot** 以使用这些新对话框。

1. 打开 **/Bots/CalendarBot.cs** ，并使用以下代码替换其全部内容。

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    以下是更改的简短摘要。

    - 将 **CalendarBot** 类更改为模板类，并接收 **对话框**。
    - 将 **CalendarBot** 类更改为扩展 **TeamsActivityHandler**，使其能够在 Microsoft 团队中登录。
    - 添加了其他方法重写以启用身份验证。

## <a name="update-startupcs"></a>更新 Startup.cs

最后一步是更新方法， `ConfigureServices` 以添加身份验证所需的服务和新对话框。

1. 打开 **./Startup.cs** 并 `services.AddTransient<IBot, Bots.CalendarBot>();` 从方法中删除该行 `ConfigureServices` 。

1. 将以下代码插入到方法的末尾 `ConfigureServices` 。

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a>测试身份验证

1. 保存所有更改并使用启动机器人 `dotnet run` 。

1. 打开 "Bot 框架" 模拟器。 选择 " **文件** " 菜单，然后选择 " **新建 Bot 配置 ...**"。

1. 按如下所示填写字段。

    - **Bot 名称：**`CalendarBot`
    - **终结点 URL：**`https://localhost:3978/api/messages`
    - **Microsoft 应用程序 id：** 您的 **图形日历机器人** 应用注册的应用程序 id
    - **Microsoft 应用密码：** 您的 **图形日历机器人** 客户端密码
    - **加密存储在你的 bot 配置中的密钥：** 了

    ![新机器人配置对话框的屏幕截图](images/new-bot-config.png)

1. 选择 " **保存并连接**"。 在仿真程序连接之后，您应该会看到 `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`

1. 键入一些文本并将其发送到机器人。 机器人将使用登录提示进行响应。

1. 选择 " **登录** " 按钮。 仿真程序将提示您确认以开头的 URL `oauthlink://https://token.botframeworkcom` 。 选择 " **确认** " 以继续。

1. 在弹出窗口中，使用 Microsoft 365 帐户登录。 查看请求的权限并接受。

1. 完成身份验证和同意后，弹出窗口将提供验证代码。 复制代码并关闭窗口。

    ![Bot 框架仿真程序的屏幕截图验证代码](images/validation-code.png)

1. 在聊天窗口中输入验证代码，以完成登录。

    ![与示例 bot 的登录会话的屏幕截图](images/bot-login.png)

1. 如果选择 " **显示令牌** " 按钮 (或键入 `show token`) ，则 bot 将显示访问令牌。  (或键入) 的 " **注销** " 按钮 `log out` 将注销。

> [!TIP]
> 在使用 bot 启动对话时，您可能会在 Bot 框架仿真器中收到以下错误消息。
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> 如果发生这种情况，请关闭仿真程序并重新启动它。
