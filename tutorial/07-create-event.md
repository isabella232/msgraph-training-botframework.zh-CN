---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347750"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="42457-101">在本节中，您将使用 Microsoft Graph SDK 向用户的日历中添加事件。</span><span class="sxs-lookup"><span data-stu-id="42457-101">In this section you'll use the Microsoft Graph SDK to add an event to the user's calendar.</span></span>

## <a name="implement-a-dialog"></a><span data-ttu-id="42457-102">实现对话</span><span class="sxs-lookup"><span data-stu-id="42457-102">Implement a dialog</span></span>

<span data-ttu-id="42457-103">首先创建一个新的自定义对话框，提示用户输入将事件添加到其日历中所需的值。</span><span class="sxs-lookup"><span data-stu-id="42457-103">Start by creating a new custom dialog to prompt the user for the values needed to add an event to their calendar.</span></span> <span data-ttu-id="42457-104">此对话框将使用 **WaterfallDialog** 执行以下步骤。</span><span class="sxs-lookup"><span data-stu-id="42457-104">This dialog will use a **WaterfallDialog** to do the following steps.</span></span>

- <span data-ttu-id="42457-105">提示输入主题</span><span class="sxs-lookup"><span data-stu-id="42457-105">Prompt for a subject</span></span>
- <span data-ttu-id="42457-106">询问用户是否要邀请人员</span><span class="sxs-lookup"><span data-stu-id="42457-106">Ask if the user wants to invite people</span></span>
- <span data-ttu-id="42457-107">如果用户在上一步中说 "是"，则提示与会者 () </span><span class="sxs-lookup"><span data-stu-id="42457-107">Prompt for attendees (if user said yes to previous step)</span></span>
- <span data-ttu-id="42457-108">提示输入开始日期和时间</span><span class="sxs-lookup"><span data-stu-id="42457-108">Prompt for a start date and time</span></span>
- <span data-ttu-id="42457-109">提示输入结束日期和时间</span><span class="sxs-lookup"><span data-stu-id="42457-109">Prompt for an end date and time</span></span>
- <span data-ttu-id="42457-110">显示所有收集的值，并要求用户确认</span><span class="sxs-lookup"><span data-stu-id="42457-110">Display all of the collected values and ask user to confirm</span></span>
- <span data-ttu-id="42457-111">如果用户确认，则从 **OAuthPrompt** 获取访问令牌</span><span class="sxs-lookup"><span data-stu-id="42457-111">If user confirms, get access token from **OAuthPrompt**</span></span>
- <span data-ttu-id="42457-112">创建事件</span><span class="sxs-lookup"><span data-stu-id="42457-112">Create the event</span></span>

1. <span data-ttu-id="42457-113">在名为 **NewEventDialog.cs** 的 **/Dialogs** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="42457-113">Create a new file in the **./Dialogs** directory named **NewEventDialog.cs** and add the following code.</span></span>

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

    <span data-ttu-id="42457-114">这是一个新对话框的命令行管理程序，该对话框将提示用户输入将事件添加到其日历中所需的值。</span><span class="sxs-lookup"><span data-stu-id="42457-114">This is the shell of a new dialog that will prompt the user for the values needed to add an event to their calendar.</span></span>

1. <span data-ttu-id="42457-115">将以下函数添加到 **NewEventDialog** 类，以提示用户输入主题。</span><span class="sxs-lookup"><span data-stu-id="42457-115">Add the following function to the **NewEventDialog** class to prompt the user for a subject.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. <span data-ttu-id="42457-116">将以下函数添加到 **NewEventDialog** 类，以存储用户在上一步中提供的主题，并询问是否要添加与会者。</span><span class="sxs-lookup"><span data-stu-id="42457-116">Add the following function to the **NewEventDialog** class to store the subject the user gave in the previous step, and to ask if they want to add attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. <span data-ttu-id="42457-117">将以下函数添加到 **NewEventDialog** 类，以检查用户对上一步骤的响应，并在需要时提示用户输入与会者列表。</span><span class="sxs-lookup"><span data-stu-id="42457-117">Add the following function to the **NewEventDialog** class to check the user's response from the previous step and prompt the user for a list of attendees if needed.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. <span data-ttu-id="42457-118">将以下函数添加到 **NewEventDialog** 类以存储上一步骤中的与会者列表 (如果存在) 并提示用户输入开始日期和时间。</span><span class="sxs-lookup"><span data-stu-id="42457-118">Add the following function to the **NewEventDialog** class to store the list of attendees from the previous step (if present) and prompt the user for a start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. <span data-ttu-id="42457-119">将以下函数添加到 **NewEventDialog** 类以存储上一步骤的起始值，并提示用户输入结束日期和时间。</span><span class="sxs-lookup"><span data-stu-id="42457-119">Add the following function to the **NewEventDialog** class to store the start value from the previous step and prompt the user for an end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. <span data-ttu-id="42457-120">将以下函数添加到 **NewEventDialog** 类以存储上一步骤的结束值，并要求用户确认所有输入。</span><span class="sxs-lookup"><span data-stu-id="42457-120">Add the following function to the **NewEventDialog** class to store the end value from the previous step and ask the user to confirm all of the inputs.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. <span data-ttu-id="42457-121">将以下函数添加到 **NewEventDialog** 类，以检查用户在上一步中的响应。</span><span class="sxs-lookup"><span data-stu-id="42457-121">Add the following function to the **NewEventDialog** class to check the user's response from the previous step.</span></span> <span data-ttu-id="42457-122">如果用户确认输入，则使用 **OAuthPrompt** 类获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="42457-122">If the user confirms the inputs, use the **OAuthPrompt** class to get an access token.</span></span> <span data-ttu-id="42457-123">否则，请结束对话框。</span><span class="sxs-lookup"><span data-stu-id="42457-123">Otherwise, end the dialog.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. <span data-ttu-id="42457-124">将以下函数添加到 **NewEventDialog** 类，以使用 MICROSOFT Graph SDK 创建新事件。</span><span class="sxs-lookup"><span data-stu-id="42457-124">Add the following function to the **NewEventDialog** class to use the Microsoft Graph SDK to create the new event.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    <span data-ttu-id="42457-125">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="42457-125">Consider what this code does.</span></span>

    - <span data-ttu-id="42457-126">它将获取用户的 **MailboxSettings** ，以确定用户的首选时区。</span><span class="sxs-lookup"><span data-stu-id="42457-126">It gets the user's **MailboxSettings** to determine the user's preferred time zone.</span></span>
    - <span data-ttu-id="42457-127">它使用用户提供的值创建 **Event** 对象。</span><span class="sxs-lookup"><span data-stu-id="42457-127">It creates an **Event** object using the values provided by the user.</span></span> <span data-ttu-id="42457-128">请注意， **开始** 和 **结束** 属性设置为用户的时区。</span><span class="sxs-lookup"><span data-stu-id="42457-128">Note that the **Start** and **End** properties are set with the user's time zone.</span></span>
    - <span data-ttu-id="42457-129">它会在用户的日历上创建事件。</span><span class="sxs-lookup"><span data-stu-id="42457-129">It creates the event on the user's calendar.</span></span>

## <a name="add-validation"></a><span data-ttu-id="42457-130">添加验证</span><span class="sxs-lookup"><span data-stu-id="42457-130">Add validation</span></span>

<span data-ttu-id="42457-131">现在，将验证添加到用户的输入，以避免在使用 Microsoft Graph 创建事件时出现错误。</span><span class="sxs-lookup"><span data-stu-id="42457-131">Now add validation to the user's input to avoid errors when creating the event with Microsoft Graph.</span></span> <span data-ttu-id="42457-132">我们希望确保：</span><span class="sxs-lookup"><span data-stu-id="42457-132">We want to make sure that:</span></span>

- <span data-ttu-id="42457-133">如果用户提供了与会者列表，则它应是以分号分隔的有效电子邮件地址列表。</span><span class="sxs-lookup"><span data-stu-id="42457-133">If the user gives an attendee list, it should be a semicolon-delimited list of valid email addresses.</span></span>
- <span data-ttu-id="42457-134">开始日期/时间应为有效的日期和时间。</span><span class="sxs-lookup"><span data-stu-id="42457-134">The start date/time should be a valid date and time.</span></span>
- <span data-ttu-id="42457-135">结束日期/时间应为有效的日期和时间，并且应晚于 start。</span><span class="sxs-lookup"><span data-stu-id="42457-135">The end date/time should be a valid date and time, and should be later than the start.</span></span>

1. <span data-ttu-id="42457-136">将以下函数添加到 **NewEventDialog** 类以验证用户输入的与会者。</span><span class="sxs-lookup"><span data-stu-id="42457-136">Add the following function to the **NewEventDialog** class to validate the user's entry for attendees.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. <span data-ttu-id="42457-137">将以下函数添加到 **NewEventDialog** 类，以验证用户输入的开始日期和时间。</span><span class="sxs-lookup"><span data-stu-id="42457-137">Add the following functions to the **NewEventDialog** class to validate the user's entry for start date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. <span data-ttu-id="42457-138">将以下函数添加到 **NewEventDialog** 类，以验证用户输入的结束日期和时间。</span><span class="sxs-lookup"><span data-stu-id="42457-138">Add the following function to the **NewEventDialog** class to validate the user's entry for end date and time.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a><span data-ttu-id="42457-139">向 WaterfallDialog 添加步骤</span><span class="sxs-lookup"><span data-stu-id="42457-139">Add steps to WaterfallDialog</span></span>

<span data-ttu-id="42457-140">现在，您已拥有对话框的所有 "步骤"，最后一步是将其添加到构造函数中的 **WaterfallDialog** ，然后将 **NewEventDialog** 添加到 **MainDialog** 中。</span><span class="sxs-lookup"><span data-stu-id="42457-140">Now that you have all of the "steps" for the dialog, the last step is to add them to a **WaterfallDialog** in the constructor, then add the **NewEventDialog** to the **MainDialog**.</span></span>

1. <span data-ttu-id="42457-141">找到 **NewEventDialog** 构造函数，并将以下代码添加到该构造函数中。</span><span class="sxs-lookup"><span data-stu-id="42457-141">Locate the **NewEventDialog** constructor and add the following code to it.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    <span data-ttu-id="42457-142">这将添加所有使用的对话框，并将您实现的所有函数添加为 **WaterfallDialog** 中的步骤。</span><span class="sxs-lookup"><span data-stu-id="42457-142">This adds all of the dialogs that are used, and adds all of the functions you implemented as steps in the **WaterfallDialog**.</span></span>

1. <span data-ttu-id="42457-143">打开 **/Dialogs/MainDialog.cs** ，并将以下行添加到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="42457-143">Open **./Dialogs/MainDialog.cs** and add the following line to the constructor.</span></span>

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. <span data-ttu-id="42457-144">将块内的代码替换 `else if (command.StartsWith("add event"))` `ProcessStepAsync` 为以下代码。</span><span class="sxs-lookup"><span data-stu-id="42457-144">Replace the code inside the `else if (command.StartsWith("add event"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. <span data-ttu-id="42457-145">保存所有更改，然后重新启动机器人。</span><span class="sxs-lookup"><span data-stu-id="42457-145">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="42457-146">使用 Bot 框架仿真程序连接到机器人并登录。</span><span class="sxs-lookup"><span data-stu-id="42457-146">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="42457-147">选择 " **添加事件** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="42457-147">Select the **Add event** button.</span></span>

1. <span data-ttu-id="42457-148">响应创建新事件的提示。</span><span class="sxs-lookup"><span data-stu-id="42457-148">Respond to the prompts to create a new event.</span></span> <span data-ttu-id="42457-149">请注意，当系统提示输入起始和结束值时，您可以使用 "今天 3PM" 或 "now" 之类的短语。</span><span class="sxs-lookup"><span data-stu-id="42457-149">Note that when prompted for start and end values, you can use phrases like "today at 3PM", or "now".</span></span>

    ![Bot 框架仿真程序中的 "添加事件" 对话框的屏幕截图](images/add-event.png)
