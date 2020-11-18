---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347753"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ab507-101">在本练习中，你将使用 Azure 门户创建新的 Bot 通道注册和 Azure AD web 应用程序注册。</span><span class="sxs-lookup"><span data-stu-id="ab507-101">In this exercise, you will create a new Bot Channels registration and an Azure AD web application registration using the Azure Portal.</span></span>

## <a name="create-a-bot-channels-registration"></a><span data-ttu-id="ab507-102">创建 Bot 通道注册</span><span class="sxs-lookup"><span data-stu-id="ab507-102">Create a Bot Channels registration</span></span>

1. <span data-ttu-id="ab507-103">打开浏览器并导航到 [Azure 门户](https://portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="ab507-103">Open a browser and navigate to the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="ab507-104">使用与你的 Azure 订阅关联的帐户登录。</span><span class="sxs-lookup"><span data-stu-id="ab507-104">Login using the account associated with your Azure subscription.</span></span>

1. <span data-ttu-id="ab507-105">选择左上角的菜单，然后选择 " **创建资源**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-105">Select the upper-left menu, then select **Create a resource**.</span></span>

    ![Azure 门户菜单的屏幕截图](images/create-resource.png)

1. <span data-ttu-id="ab507-107">在 " **新建** " 页上，搜索 `Bot Channel` 并选择 " **Bot 通道注册**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-107">On the **New** page, search for `Bot Channel` and select **Bot Channels Registration**.</span></span>

1. <span data-ttu-id="ab507-108">在 " **Bot 通道注册** " 页上，选择 " **创建**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-108">On the **Bot Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="ab507-109">填写必填字段，并将 **消息终结点** 保留为空。</span><span class="sxs-lookup"><span data-stu-id="ab507-109">Fill in the required fields, and leave **Messaging endpoint** blank.</span></span> <span data-ttu-id="ab507-110">**机器人句柄** 字段必须是唯一的。</span><span class="sxs-lookup"><span data-stu-id="ab507-110">The **Bot handle** field must be unique.</span></span> <span data-ttu-id="ab507-111">请务必查看不同的定价层，并选择对你的方案有意义的内容。</span><span class="sxs-lookup"><span data-stu-id="ab507-111">Be sure to review the different pricing tiers and select what makes sense for your scenario.</span></span> <span data-ttu-id="ab507-112">如果这只是一个学习练习，您可能需要选择 "免费" 选项。</span><span class="sxs-lookup"><span data-stu-id="ab507-112">If this is just a learning exercise, you may want to select the free option.</span></span>

1. <span data-ttu-id="ab507-113">选择 " **Microsoft 应用程序 ID 和密码**"，然后选择 " **新建**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-113">Select the **Microsoft App ID and password**, then select **Create New**.</span></span>

1. <span data-ttu-id="ab507-114">**在应用注册门户中选择 "创建应用程序 ID"**。</span><span class="sxs-lookup"><span data-stu-id="ab507-114">Select **Create App ID in the App Registration Portal**.</span></span> <span data-ttu-id="ab507-115">这将为 Azure 门户中的 **应用程序注册** 边栏打开一个新的窗口或选项卡。</span><span class="sxs-lookup"><span data-stu-id="ab507-115">This will open a new window or tab to the **App registrations** blade in the Azure Portal.</span></span>

1. <span data-ttu-id="ab507-116">在 " **应用程序注册** " 边栏选项卡中，选择 " **新建注册**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-116">In the **App registrations** blade, select **New registration**.</span></span>

1. <span data-ttu-id="ab507-117">设置值，如下所示。</span><span class="sxs-lookup"><span data-stu-id="ab507-117">Set the values as follows.</span></span>

    - <span data-ttu-id="ab507-118">将“名称”设置为“`Graph Calendar Bot`”。</span><span class="sxs-lookup"><span data-stu-id="ab507-118">Set **Name** to `Graph Calendar Bot`.</span></span>
    - <span data-ttu-id="ab507-119">将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。</span><span class="sxs-lookup"><span data-stu-id="ab507-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ab507-120">保留“重定向 URI”为空。</span><span class="sxs-lookup"><span data-stu-id="ab507-120">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. <span data-ttu-id="ab507-122">选择“**注册**”。</span><span class="sxs-lookup"><span data-stu-id="ab507-122">Select **Register**.</span></span> <span data-ttu-id="ab507-123">在 " **图形日历** 自动程序" 页上，将应用程序的值复制 **(客户端) ID** 并保存它，您将在以下步骤中需要它。</span><span class="sxs-lookup"><span data-stu-id="ab507-123">On the **Graph Calendar Bot** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. <span data-ttu-id="ab507-125">选择“管理”下的“证书和密码”。</span><span class="sxs-lookup"><span data-stu-id="ab507-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="ab507-126">选择“新客户端密码”按钮。</span><span class="sxs-lookup"><span data-stu-id="ab507-126">Select the **New client secret** button.</span></span> <span data-ttu-id="ab507-127">在 " **说明** " 中输入一个值，然后选择 " **过期** " 选项之一，然后选择 " **添加**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="ab507-128">离开此页前，先复制客户端密码值。</span><span class="sxs-lookup"><span data-stu-id="ab507-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ab507-129">您将在以下步骤中需要它。</span><span class="sxs-lookup"><span data-stu-id="ab507-129">You will need it in the following steps.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ab507-130">此客户端密码不会再次显示，所以请务必现在就复制它。</span><span class="sxs-lookup"><span data-stu-id="ab507-130">This client secret is never shown again, so make sure you copy it now.</span></span> <span data-ttu-id="ab507-131">您需要在多个位置输入此值，以使其安全。</span><span class="sxs-lookup"><span data-stu-id="ab507-131">You will need to enter this value in multiple places so keep it safe.</span></span>

1. <span data-ttu-id="ab507-132">返回到浏览器中的 "Bot 通道注册" 窗口，并将应用程序 ID 粘贴到 " **Microsoft 应用 id** " 字段中。</span><span class="sxs-lookup"><span data-stu-id="ab507-132">Return to the Bot Channel Registration window in your browser, and paste the application ID into the **Microsoft App ID** field.</span></span> <span data-ttu-id="ab507-133">将客户端密码粘贴到 **密码** 字段中。</span><span class="sxs-lookup"><span data-stu-id="ab507-133">Paste your client secret into the **Password** field.</span></span> <span data-ttu-id="ab507-134">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="ab507-134">Select **OK**.</span></span>

1. <span data-ttu-id="ab507-135">在 " **Bot 通道注册** " 页上，选择 " **创建**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-135">On the **Bots Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="ab507-136">等待创建机器人频道注册。</span><span class="sxs-lookup"><span data-stu-id="ab507-136">Wait for the Bot Channels registration to be created.</span></span> <span data-ttu-id="ab507-137">创建后，返回到 Azure 门户中的主页，然后选择 " **Bot 服务**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-137">Once created, return to the Home page in the Azure Portal, then select **Bot Services**.</span></span> <span data-ttu-id="ab507-138">选择新的 Bot 频道注册以查看其属性。</span><span class="sxs-lookup"><span data-stu-id="ab507-138">Select your new Bots Channel registration to view its properties.</span></span>

## <a name="create-a-web-app-registration"></a><span data-ttu-id="ab507-139">创建 web 应用注册</span><span class="sxs-lookup"><span data-stu-id="ab507-139">Create a web app registration</span></span>

1. <span data-ttu-id="ab507-140">返回到 Azure 门户的 " **应用程序注册** " 部分。</span><span class="sxs-lookup"><span data-stu-id="ab507-140">Return to the **App registrations** section of the Azure Portal.</span></span>

1. <span data-ttu-id="ab507-141">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="ab507-141">Select **New registration**.</span></span> <span data-ttu-id="ab507-142">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="ab507-142">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ab507-143">将“名称”设置为“`Graph Calendar Bot Auth`”。</span><span class="sxs-lookup"><span data-stu-id="ab507-143">Set **Name** to `Graph Calendar Bot Auth`.</span></span>
    - <span data-ttu-id="ab507-144">将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。</span><span class="sxs-lookup"><span data-stu-id="ab507-144">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ab507-145">在“重定向 URI”下，将第一个下拉列表设置为“`Web`”，并将值设置为“`https://token.botframework.com/.auth/web/redirect`”。</span><span class="sxs-lookup"><span data-stu-id="ab507-145">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://token.botframework.com/.auth/web/redirect`.</span></span>

1. <span data-ttu-id="ab507-146">选择“**注册**”。</span><span class="sxs-lookup"><span data-stu-id="ab507-146">Select **Register**.</span></span> <span data-ttu-id="ab507-147">在 " **图形日历机器人身份验证** " 页上，将应用程序的值复制 **(客户端) ID** 并保存它，您将在以下步骤中需要它。</span><span class="sxs-lookup"><span data-stu-id="ab507-147">On the **Graph Calendar Bot Auth** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

1. <span data-ttu-id="ab507-148">选择“管理”下的“证书和密码”。</span><span class="sxs-lookup"><span data-stu-id="ab507-148">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="ab507-149">选择“新客户端密码”按钮。</span><span class="sxs-lookup"><span data-stu-id="ab507-149">Select the **New client secret** button.</span></span> <span data-ttu-id="ab507-150">在 " **说明** " 中输入一个值，然后选择 " **过期** " 选项之一，然后选择 " **添加**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-150">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="ab507-151">离开此页前，先复制客户端密码值。</span><span class="sxs-lookup"><span data-stu-id="ab507-151">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ab507-152">您将在以下步骤中需要它。</span><span class="sxs-lookup"><span data-stu-id="ab507-152">You will need it in the following steps.</span></span>

1. <span data-ttu-id="ab507-153">选择 " **API 权限**"，然后选择 " **添加权限**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-153">Select **API permissions**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="ab507-154">选择 " **Microsoft Graph**"，然后选择 " **委派权限**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-154">Select **Microsoft Graph**, then select **Delegated permissions**.</span></span>

1. <span data-ttu-id="ab507-155">选择以下权限，然后选择 " **添加权限**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-155">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="ab507-156">**openid**</span><span class="sxs-lookup"><span data-stu-id="ab507-156">**openid**</span></span>
    - <span data-ttu-id="ab507-157">**个人资料**</span><span class="sxs-lookup"><span data-stu-id="ab507-157">**profile**</span></span>
    - <span data-ttu-id="ab507-158">**Calendars.ReadWrite**</span><span class="sxs-lookup"><span data-stu-id="ab507-158">**Calendars.ReadWrite**</span></span>
    - <span data-ttu-id="ab507-159">**MailboxSettings.Read**</span><span class="sxs-lookup"><span data-stu-id="ab507-159">**MailboxSettings.Read**</span></span>

    ![配置的权限的屏幕截图](images/configured-permissions.png)

### <a name="about-permissions"></a><span data-ttu-id="ab507-161">关于权限</span><span class="sxs-lookup"><span data-stu-id="ab507-161">About permissions</span></span>

<span data-ttu-id="ab507-162">请考虑这些权限范围中的每一个允许机器人执行的操作，以及机器人将对其使用的功能。</span><span class="sxs-lookup"><span data-stu-id="ab507-162">Consider what each of those permission scopes allows the bot to do, and what the bot will use them for.</span></span>

- <span data-ttu-id="ab507-163">**openid** and **profile**：允许机器人对用户进行签名，并从 Azure AD 中获取标识令牌中的基本信息。</span><span class="sxs-lookup"><span data-stu-id="ab507-163">**openid** and **profile**: allows the bot to sign users in and get basic information from Azure AD in the identity token.</span></span>
- <span data-ttu-id="ab507-164">"自动 **读写**"：允许机器人读取用户的日历并将新事件添加到用户的日历中。</span><span class="sxs-lookup"><span data-stu-id="ab507-164">**Calendars.ReadWrite**: allows the bot to read the user's calendar and to add new events to the user's calendar.</span></span>
- <span data-ttu-id="ab507-165">**MailboxSettings**：允许机器人读取用户的邮箱设置。</span><span class="sxs-lookup"><span data-stu-id="ab507-165">**MailboxSettings.Read**: allows the bot to read the user's mailbox settings.</span></span> <span data-ttu-id="ab507-166">Bot 将使用它来获取用户选定的时区。</span><span class="sxs-lookup"><span data-stu-id="ab507-166">The bot will use this to get the user's selected time zone.</span></span>
- <span data-ttu-id="ab507-167">**User. Read**：允许机器人从 Microsoft Graph 获取用户的配置文件。</span><span class="sxs-lookup"><span data-stu-id="ab507-167">**User.Read**: allows the bot to get the user's profile from Microsoft Graph.</span></span> <span data-ttu-id="ab507-168">Bot 将使用它来获取用户的名称。</span><span class="sxs-lookup"><span data-stu-id="ab507-168">The bot will use this to get the user's name.</span></span>

## <a name="add-oauth-connection-to-the-bot"></a><span data-ttu-id="ab507-169">向 bot 添加 OAuth 连接</span><span class="sxs-lookup"><span data-stu-id="ab507-169">Add OAuth connection to the bot</span></span>

1. <span data-ttu-id="ab507-170">导航到 Azure 门户上的你的 bot 的机器人频道注册页。</span><span class="sxs-lookup"><span data-stu-id="ab507-170">Navigate to your bot's Bot Channels Registration page on the Azure Portal.</span></span> <span data-ttu-id="ab507-171">选择 " **Bot 管理**" 下的 **设置**。</span><span class="sxs-lookup"><span data-stu-id="ab507-171">Select **Settings** under **Bot Management**.</span></span>

1. <span data-ttu-id="ab507-172">在页面底部附近的 " **OAuth 连接设置** " 下，选择 " **添加设置**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-172">Under **OAuth Connection Settings** near the bottom of the page, select **Add Setting**.</span></span>

1. <span data-ttu-id="ab507-173">按如下所示填写窗体，然后选择 " **保存**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-173">Fill in the form as follows, then select **Save**.</span></span>

    - <span data-ttu-id="ab507-174">**名称**： `GraphBotAuth`</span><span class="sxs-lookup"><span data-stu-id="ab507-174">**Name**: `GraphBotAuth`</span></span>
    - <span data-ttu-id="ab507-175">**提供程序**： **Azure Active Directory v2**</span><span class="sxs-lookup"><span data-stu-id="ab507-175">**Provider**: **Azure Active Directory v2**</span></span>
    - <span data-ttu-id="ab507-176">**客户端 id**：您的 **图形日历机器人身份验证** 注册的应用程序 id。</span><span class="sxs-lookup"><span data-stu-id="ab507-176">**Client id**: The application ID of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="ab507-177">**客户端密码**：您的 **图形日历机器人身份验证** 注册的客户端密码。</span><span class="sxs-lookup"><span data-stu-id="ab507-177">**Client secret**: The client secret of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="ab507-178">**令牌交换 URL**：留空</span><span class="sxs-lookup"><span data-stu-id="ab507-178">**Token Exchange URL**: Leave blank</span></span>
    - <span data-ttu-id="ab507-179">**租户 ID**： `common`</span><span class="sxs-lookup"><span data-stu-id="ab507-179">**Tenant ID**: `common`</span></span>
    - <span data-ttu-id="ab507-180">**作用域**： `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span><span class="sxs-lookup"><span data-stu-id="ab507-180">**Scopes**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span></span>

1. <span data-ttu-id="ab507-181">选择 " **OAuth 连接设置**" 下的 " **GraphBotAuth** " 条目。</span><span class="sxs-lookup"><span data-stu-id="ab507-181">Select the **GraphBotAuth** entry under **OAuth Connection Settings**.</span></span>

1. <span data-ttu-id="ab507-182">选择 " **测试连接**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-182">Select **Test Connection**.</span></span> <span data-ttu-id="ab507-183">这将打开一个新的浏览器窗口或选项卡以启动 OAuth 流。</span><span class="sxs-lookup"><span data-stu-id="ab507-183">This opens a new browser window or tab to start the OAuth flow.</span></span>

1. <span data-ttu-id="ab507-184">如有必要，请登录。</span><span class="sxs-lookup"><span data-stu-id="ab507-184">If necessary, sign in.</span></span> <span data-ttu-id="ab507-185">查看请求的权限列表，然后选择 " **接受**"。</span><span class="sxs-lookup"><span data-stu-id="ab507-185">Review the list of requested permissions, then select **Accept**.</span></span>

1. <span data-ttu-id="ab507-186">您应该会看到一个 **与 "GraphBotAuth" 成功消息的测试连接** 。</span><span class="sxs-lookup"><span data-stu-id="ab507-186">You should see a **Test Connection to 'GraphBotAuth' Succeeded** message.</span></span>

> [!TIP]
> <span data-ttu-id="ab507-187">您可以在此页上选择 " **复制令牌** " 按钮，然后将令牌粘贴到中， [https://jwt.ms](https://jwt.ms) 以查看令牌中的声明。</span><span class="sxs-lookup"><span data-stu-id="ab507-187">You can select the **Copy Token** button on this page, and paste the token into [https://jwt.ms](https://jwt.ms) to see the claims inside the token.</span></span> <span data-ttu-id="ab507-188">这在对身份验证错误进行故障排除时很有用。</span><span class="sxs-lookup"><span data-stu-id="ab507-188">This is useful when troubleshooting authentication errors.</span></span>
