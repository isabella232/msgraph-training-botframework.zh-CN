---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347773"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="324bc-101">在本节中，你将使用 Microsoft Graph SDK 获取已登录用户。</span><span class="sxs-lookup"><span data-stu-id="324bc-101">In this section you'll use the Microsoft Graph SDK to get the logged-in user.</span></span>

## <a name="create-a-graph-service"></a><span data-ttu-id="324bc-102">创建 Graph 服务</span><span class="sxs-lookup"><span data-stu-id="324bc-102">Create a Graph service</span></span>

<span data-ttu-id="324bc-103">首先实现一个服务，bot 可以使用该服务从 Microsoft Graph SDK 获取 **GraphServiceClient** ，然后通过依赖关系注入使该服务可供机器人。</span><span class="sxs-lookup"><span data-stu-id="324bc-103">Start by implementing a service that the bot can use to get a **GraphServiceClient** from the Microsoft Graph SDK, then making that service available to the bot via dependency injection.</span></span>

1. <span data-ttu-id="324bc-104">在名为 **Graph** 的项目的根目录中创建一个新目录。</span><span class="sxs-lookup"><span data-stu-id="324bc-104">Create a new directory in the root of the project named **Graph**.</span></span> <span data-ttu-id="324bc-105">在名为 **IGraphClientService.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="324bc-105">Create a new file in the **./Graph** directory named **IGraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. <span data-ttu-id="324bc-106">在名为 **GraphClientService.cs** 的 **/Graph** 目录中创建一个新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="324bc-106">Create a new file in the **./Graph** directory named **GraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. <span data-ttu-id="324bc-107">打开 **./Startup.cs** ，并将以下代码添加到函数的末尾 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="324bc-107">Open **./Startup.cs** and add the following code to the end of the `ConfigureServices` function.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. <span data-ttu-id="324bc-108">打开 **./Dialogs/MainDialog.cs**。</span><span class="sxs-lookup"><span data-stu-id="324bc-108">Open **./Dialogs/MainDialog.cs**.</span></span> <span data-ttu-id="324bc-109">将以下 `using` 语句添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="324bc-109">Add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. <span data-ttu-id="324bc-110">将以下属性添加到 **MainDialog** 类中。</span><span class="sxs-lookup"><span data-stu-id="324bc-110">Add the following property to the **MainDialog** class.</span></span>

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. <span data-ttu-id="324bc-111">找到 **MainDialog** 类的构造函数，并更新其签名以采用 **IGraphServiceClient** 参数。</span><span class="sxs-lookup"><span data-stu-id="324bc-111">Locate the constructor for the **MainDialog** class and update its signature to take an **IGraphServiceClient** parameter.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. <span data-ttu-id="324bc-112">将以下代码添加到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="324bc-112">Add the following code to the constructor.</span></span>

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a><span data-ttu-id="324bc-113">获取已登录用户</span><span class="sxs-lookup"><span data-stu-id="324bc-113">Get the logged on user</span></span>

<span data-ttu-id="324bc-114">在本节中，你将使用 Microsoft Graph 获取用户的姓名、电子邮件地址和照片。</span><span class="sxs-lookup"><span data-stu-id="324bc-114">In this section you'll use the Microsoft Graph to get the user's name, email address, and photo.</span></span> <span data-ttu-id="324bc-115">然后，您将创建一个自适应卡片以显示信息。</span><span class="sxs-lookup"><span data-stu-id="324bc-115">Then you'll create an Adaptive Card to show the information.</span></span>

1. <span data-ttu-id="324bc-116">在名为 **CardHelper.cs** 的项目的根目录中创建一个新文件。</span><span class="sxs-lookup"><span data-stu-id="324bc-116">Create a new file in the root of the project named **CardHelper.cs**.</span></span> <span data-ttu-id="324bc-117">将以下代码添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="324bc-117">Add the following code to the file.</span></span>

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

    <span data-ttu-id="324bc-118">此代码使用 **自适应卡片** NuGet 包构建一个自适应卡片以显示用户。</span><span class="sxs-lookup"><span data-stu-id="324bc-118">This code uses the **AdaptiveCard** NuGet package to build an Adaptive Card to display the user.</span></span>

1. <span data-ttu-id="324bc-119">将以下函数添加到 **MainDialog** 类中。</span><span class="sxs-lookup"><span data-stu-id="324bc-119">Add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    <span data-ttu-id="324bc-120">请考虑此代码执行的操作。</span><span class="sxs-lookup"><span data-stu-id="324bc-120">Consider what this code does.</span></span>

    - <span data-ttu-id="324bc-121">它使用 **graphClient** [获取已登录的用户](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0)。</span><span class="sxs-lookup"><span data-stu-id="324bc-121">It uses the **graphClient** to [get the logged-in user](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).</span></span>
        - <span data-ttu-id="324bc-122">它使用 `Select` 方法来限制返回的字段。</span><span class="sxs-lookup"><span data-stu-id="324bc-122">It uses the `Select` method to limit which fields are returned.</span></span>
    - <span data-ttu-id="324bc-123">它使用 **graphClient** [获取用户的照片](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0)，并请求最小受支持的48x48 像素大小。</span><span class="sxs-lookup"><span data-stu-id="324bc-123">It uses the **graphClient** to [get the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0), requesting the smallest supported size of 48x48 pixels.</span></span>
    - <span data-ttu-id="324bc-124">它使用 **CardHelper** 类构建一个自适应卡片，并将该卡片作为附件发送。</span><span class="sxs-lookup"><span data-stu-id="324bc-124">It uses the **CardHelper** class to construct an Adaptive Card and sends the card as an attachment.</span></span>

1. <span data-ttu-id="324bc-125">将块内的代码替换 `else if (command.StartsWith("show me"))` `ProcessStepAsync` 为以下代码。</span><span class="sxs-lookup"><span data-stu-id="324bc-125">Replace the code inside the `else if (command.StartsWith("show me"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. <span data-ttu-id="324bc-126">保存所有更改，然后重新启动机器人。</span><span class="sxs-lookup"><span data-stu-id="324bc-126">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="324bc-127">使用 Bot 框架仿真程序连接到机器人并登录。</span><span class="sxs-lookup"><span data-stu-id="324bc-127">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="324bc-128">选择 " **显示我** " 按钮以显示已登录的用户。</span><span class="sxs-lookup"><span data-stu-id="324bc-128">Select the **Show me** button to display the logged-on user.</span></span>

    ![显示用户的自适应卡片的屏幕截图](images/user-card.png)
