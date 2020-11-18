---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347773"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，你将使用 Microsoft Graph SDK 获取已登录用户。

## <a name="create-a-graph-service"></a>创建 Graph 服务

首先实现一个服务，bot 可以使用该服务从 Microsoft Graph SDK 获取 **GraphServiceClient** ，然后通过依赖关系注入使该服务可供机器人。

1. 在名为 **Graph** 的项目的根目录中创建一个新目录。 在名为 **IGraphClientService.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. 在名为 **GraphClientService.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. 打开 **./Startup.cs** ，并将以下代码添加到函数的末尾 `ConfigureServices` 。

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. 打开 **./Dialogs/MainDialog.cs**。 将以下 `using` 语句添加到文件顶部。

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. 将以下属性添加到 **MainDialog** 类中。

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. 找到 **MainDialog** 类的构造函数，并更新其签名以采用 **IGraphServiceClient** 参数。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. 将以下代码添加到构造函数中。

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a>获取已登录用户

在本节中，你将使用 Microsoft Graph 获取用户的姓名、电子邮件地址和照片。 然后，您将创建一个自适应卡片以显示信息。

1. 在名为 **CardHelper.cs** 的项目的根目录中创建一个新文件。 将以下代码添加到文件中。

    ```csharp
    using AdaptiveCards;
    using Microsoft.Graph;
    using System;
    using System.IO;

    namespace CalendarBot
    {
        public class CardHelper
        {
            public static AdaptiveCard GetUserCard(User user, Stream photo)
            {
              // Create an Adaptive Card to display the user
                // See https://adaptivecards.io/designer/ for possibilities
                var userCard = new AdaptiveCard("1.2");

                var columns = new AdaptiveColumnSet();
                userCard.Body.Add(columns);

                var userPhotoColumn = new AdaptiveColumn { Width = AdaptiveColumnWidth.Auto };
                columns.Columns.Add(userPhotoColumn);

                userPhotoColumn.Items.Add(new AdaptiveImage {
                    Style = AdaptiveImageStyle.Person,
                    Size = AdaptiveImageSize.Small,
                    Url = GetDataUriFromPhoto(photo)
                });

                var userInfoColumn = new AdaptiveColumn {Width = AdaptiveColumnWidth.Stretch };
                columns.Columns.Add(userInfoColumn);

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Weight = AdaptiveTextWeight.Bolder,
                    Wrap = true,
                    Text = user.DisplayName
                });

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Spacing = AdaptiveSpacing.None,
                    IsSubtle = true,
                    Wrap = true,
                    Text = user.Mail ?? user.UserPrincipalName
                });

                return userCard;
            }

            private static Uri GetDataUriFromPhoto(Stream photo)
            {
                // Copy to a MemoryStream to get access to bytes
                var photoStream = new MemoryStream();
                photo.CopyTo(photoStream);

                var photoBytes = photoStream.ToArray();

                return new Uri($"data:image/png;base64,{Convert.ToBase64String(photoBytes)}");
            }
        }
    }
    ```

    此代码使用 **自适应卡片** NuGet 包构建一个自适应卡片以显示用户。

1. 将以下函数添加到 **MainDialog** 类中。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    请考虑此代码执行的操作。

    - 它使用 **graphClient** [获取已登录的用户](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0)。
        - 它使用 `Select` 方法来限制返回的字段。
    - 它使用 **graphClient** [获取用户的照片](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0)，并请求最小受支持的48x48 像素大小。
    - 它使用 **CardHelper** 类构建一个自适应卡片，并将该卡片作为附件发送。

1. 将块内的代码替换 `else if (command.StartsWith("show me"))` `ProcessStepAsync` 为以下代码。

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. 保存所有更改，然后重新启动机器人。

1. 使用 Bot 框架仿真程序连接到机器人并登录。 选择 " **显示我** " 按钮以显示已登录的用户。

    ![显示用户的自适应卡片的屏幕截图](images/user-card.png)
