---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347775"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e658f-101">在本节中，你将使用 Microsoft Graph SDK 获取当前周的用户日历上后续3个即将发生的事件。</span><span class="sxs-lookup"><span data-stu-id="e658f-101">In this section you'll use the Microsoft Graph SDK to get the next 3 upcoming events on the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="e658f-102">获取日历视图</span><span class="sxs-lookup"><span data-stu-id="e658f-102">Get a calendar view</span></span>

<span data-ttu-id="e658f-103">日历视图是用户日历上属于两个日期/时间值的事件列表。</span><span class="sxs-lookup"><span data-stu-id="e658f-103">A calendar view is a list of events on a user's calendar that fall between two date/time values.</span></span> <span data-ttu-id="e658f-104">使用日历视图的优势在于，它包括定期会议的任何事件。</span><span class="sxs-lookup"><span data-stu-id="e658f-104">The advantage of using a calendar view is that it includes any occurrences of recurring meetings.</span></span>

1. <span data-ttu-id="e658f-105">打开 **./CardHelper.cs** ，并将以下函数添加到 **CardHelper** 类。</span><span class="sxs-lookup"><span data-stu-id="e658f-105">Open **./CardHelper.cs** and add the following function to the **CardHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    <span data-ttu-id="e658f-106">此代码将生成一个自适应卡片以呈现日历事件。</span><span class="sxs-lookup"><span data-stu-id="e658f-106">This code builds an Adaptive Card to render a calendar event.</span></span>

1. <span data-ttu-id="e658f-107">打开 **/Dialogs/MainDialog.cs** ，并将以下函数添加到 **MainDialog** 类中。</span><span class="sxs-lookup"><span data-stu-id="e658f-107">Open **./Dialogs/MainDialog.cs** and add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    <span data-ttu-id="e658f-108">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="e658f-108">Consider what this code does.</span></span>

    - <span data-ttu-id="e658f-109">它将获取用户的 **MailboxSettings** ，以确定用户的首选时区和日期/时间格式。</span><span class="sxs-lookup"><span data-stu-id="e658f-109">It gets the user's **MailboxSettings** to determine the user's preferred time zone and date/time formats.</span></span>
    - <span data-ttu-id="e658f-110">它分别将 **startDateTime** 和 **endDateTime** 值设置为现在和结束的一周。</span><span class="sxs-lookup"><span data-stu-id="e658f-110">It sets the **startDateTime** and **endDateTime** values to now and the end of the week, respectively.</span></span> <span data-ttu-id="e658f-111">这将定义日历视图使用的时间窗口。</span><span class="sxs-lookup"><span data-stu-id="e658f-111">This defines the window of time that the calendar view uses.</span></span>
    - <span data-ttu-id="e658f-112">它会调用 `graphClient.Me.CalendarView` 并提供以下详细信息。</span><span class="sxs-lookup"><span data-stu-id="e658f-112">It calls `graphClient.Me.CalendarView` with the following details.</span></span>
        - <span data-ttu-id="e658f-113">它将 `Prefer: outlook.timezone` 标头设置为用户的首选时区，从而导致事件的开始和结束时间成为用户的时区。</span><span class="sxs-lookup"><span data-stu-id="e658f-113">It sets the `Prefer: outlook.timezone` header to the user's preferred time zone, causing the start and end times for the events to be in the user's timezone.</span></span>
        - <span data-ttu-id="e658f-114">它使用 `Top(3)` 方法将结果限制为仅前3个事件。</span><span class="sxs-lookup"><span data-stu-id="e658f-114">It uses the `Top(3)` method to limit the results to only the first 3 events.</span></span>
        - <span data-ttu-id="e658f-115">它用于 `Select` 将返回的字段限制为仅由 bot 使用的字段。</span><span class="sxs-lookup"><span data-stu-id="e658f-115">It uses `Select` to limit the fields returned to just those fields used by the bot.</span></span>
        - <span data-ttu-id="e658f-116">它用于 `OrderBy` 按开始时间对事件进行排序。</span><span class="sxs-lookup"><span data-stu-id="e658f-116">It uses `OrderBy` to sort the events by start time.</span></span>
    - <span data-ttu-id="e658f-117">它将每个事件的自适应卡片添加到回复邮件。</span><span class="sxs-lookup"><span data-stu-id="e658f-117">It adds an Adaptive Card for each event to the reply message.</span></span>

1. <span data-ttu-id="e658f-118">将块内的代码替换 `else if (command.StartsWith("show calendar"))` `ProcessStepAsync` 为以下代码。</span><span class="sxs-lookup"><span data-stu-id="e658f-118">Replace the code inside the `else if (command.StartsWith("show calendar"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. <span data-ttu-id="e658f-119">保存所有更改，然后重新启动机器人。</span><span class="sxs-lookup"><span data-stu-id="e658f-119">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="e658f-120">使用 Bot 框架仿真程序连接到机器人并登录。</span><span class="sxs-lookup"><span data-stu-id="e658f-120">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="e658f-121">选择 " **显示日历** " 按钮以显示 "日历" 视图。</span><span class="sxs-lookup"><span data-stu-id="e658f-121">Select the **Show calendar** button to display the calendar view.</span></span>

    ![显示接下来的三个事件的自适应卡片的屏幕截图](images/calendar-view.png)
