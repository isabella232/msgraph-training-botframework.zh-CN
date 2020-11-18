---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347775"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，你将使用 Microsoft Graph SDK 获取当前周的用户日历上后续3个即将发生的事件。

## <a name="get-a-calendar-view"></a>获取日历视图

日历视图是用户日历上属于两个日期/时间值的事件列表。 使用日历视图的优势在于，它包括定期会议的任何事件。

1. 打开 **./CardHelper.cs** ，并将以下函数添加到 **CardHelper** 类。

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    此代码将生成一个自适应卡片以呈现日历事件。

1. 打开 **/Dialogs/MainDialog.cs** ，并将以下函数添加到 **MainDialog** 类中。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    请考虑此代码执行的操作。

    - 它将获取用户的 **MailboxSettings** ，以确定用户的首选时区和日期/时间格式。
    - 它分别将 **startDateTime** 和 **endDateTime** 值设置为现在和结束的一周。 这将定义日历视图使用的时间窗口。
    - 它会调用 `graphClient.Me.CalendarView` 并提供以下详细信息。
        - 它将 `Prefer: outlook.timezone` 标头设置为用户的首选时区，从而导致事件的开始和结束时间成为用户的时区。
        - 它使用 `Top(3)` 方法将结果限制为仅前3个事件。
        - 它用于 `Select` 将返回的字段限制为仅由 bot 使用的字段。
        - 它用于 `OrderBy` 按开始时间对事件进行排序。
    - 它将每个事件的自适应卡片添加到回复邮件。

1. 将块内的代码替换 `else if (command.StartsWith("show calendar"))` `ProcessStepAsync` 为以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. 保存所有更改，然后重新启动机器人。

1. 使用 Bot 框架仿真程序连接到机器人并登录。 选择 " **显示日历** " 按钮以显示 "日历" 视图。

    ![显示接下来的三个事件的自适应卡片的屏幕截图](images/calendar-view.png)
