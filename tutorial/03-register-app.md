---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347753"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，你将使用 Azure 门户创建新的 Bot 通道注册和 Azure AD web 应用程序注册。

## <a name="create-a-bot-channels-registration"></a>创建 Bot 通道注册

1. 打开浏览器并导航到 [Azure 门户](https://portal.azure.com)。 使用与你的 Azure 订阅关联的帐户登录。

1. 选择左上角的菜单，然后选择 " **创建资源**"。

    ![Azure 门户菜单的屏幕截图](images/create-resource.png)

1. 在 " **新建** " 页上，搜索 `Bot Channel` 并选择 " **Bot 通道注册**"。

1. 在 " **Bot 通道注册** " 页上，选择 " **创建**"。

1. 填写必填字段，并将 **消息终结点** 保留为空。 **机器人句柄** 字段必须是唯一的。 请务必查看不同的定价层，并选择对你的方案有意义的内容。 如果这只是一个学习练习，您可能需要选择 "免费" 选项。

1. 选择 " **Microsoft 应用程序 ID 和密码**"，然后选择 " **新建**"。

1. **在应用注册门户中选择 "创建应用程序 ID"**。 这将为 Azure 门户中的 **应用程序注册** 边栏打开一个新的窗口或选项卡。

1. 在 " **应用程序注册** " 边栏选项卡中，选择 " **新建注册**"。

1. 设置值，如下所示。

    - 将“名称”设置为“`Graph Calendar Bot`”。
    - 将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。
    - 保留“重定向 URI”为空。

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. 选择“**注册**”。 在 " **图形日历** 自动程序" 页上，将应用程序的值复制 **(客户端) ID** 并保存它，您将在以下步骤中需要它。

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. 选择“管理”下的“证书和密码”。 选择“新客户端密码”按钮。 在 " **说明** " 中输入一个值，然后选择 " **过期** " 选项之一，然后选择 " **添加**"。

1. 离开此页前，先复制客户端密码值。 您将在以下步骤中需要它。

    > [!IMPORTANT]
    > 此客户端密码不会再次显示，所以请务必现在就复制它。 您需要在多个位置输入此值，以使其安全。

1. 返回到浏览器中的 "Bot 通道注册" 窗口，并将应用程序 ID 粘贴到 " **Microsoft 应用 id** " 字段中。 将客户端密码粘贴到 **密码** 字段中。 选择“确定”。

1. 在 " **Bot 通道注册** " 页上，选择 " **创建**"。

1. 等待创建机器人频道注册。 创建后，返回到 Azure 门户中的主页，然后选择 " **Bot 服务**"。 选择新的 Bot 频道注册以查看其属性。

## <a name="create-a-web-app-registration"></a>创建 web 应用注册

1. 返回到 Azure 门户的 " **应用程序注册** " 部分。

1. 选择“新注册”。 在“注册应用”页上，按如下方式设置值。

    - 将“名称”设置为“`Graph Calendar Bot Auth`”。
    - 将“受支持的帐户类型”设置为“任何组织目录中的帐户和个人 Microsoft 帐户”。
    - 在“重定向 URI”下，将第一个下拉列表设置为“`Web`”，并将值设置为“`https://token.botframework.com/.auth/web/redirect`”。

1. 选择“**注册**”。 在 " **图形日历机器人身份验证** " 页上，将应用程序的值复制 **(客户端) ID** 并保存它，您将在以下步骤中需要它。

1. 选择“管理”下的“证书和密码”。 选择“新客户端密码”按钮。 在 " **说明** " 中输入一个值，然后选择 " **过期** " 选项之一，然后选择 " **添加**"。

1. 离开此页前，先复制客户端密码值。 您将在以下步骤中需要它。

1. 选择 " **API 权限**"，然后选择 " **添加权限**"。

1. 选择 " **Microsoft Graph**"，然后选择 " **委派权限**"。

1. 选择以下权限，然后选择 " **添加权限**"。

    - **openid**
    - **个人资料**
    - **Calendars.ReadWrite**
    - **MailboxSettings.Read**

    ![配置的权限的屏幕截图](images/configured-permissions.png)

### <a name="about-permissions"></a>关于权限

请考虑这些权限范围中的每一个允许机器人执行的操作，以及机器人将对其使用的功能。

- **openid** and **profile**：允许机器人对用户进行签名，并从 Azure AD 中获取标识令牌中的基本信息。
- "自动 **读写**"：允许机器人读取用户的日历并将新事件添加到用户的日历中。
- **MailboxSettings**：允许机器人读取用户的邮箱设置。 Bot 将使用它来获取用户选定的时区。
- **User. Read**：允许机器人从 Microsoft Graph 获取用户的配置文件。 Bot 将使用它来获取用户的名称。

## <a name="add-oauth-connection-to-the-bot"></a>向 bot 添加 OAuth 连接

1. 导航到 Azure 门户上的你的 bot 的机器人频道注册页。 选择 " **Bot 管理**" 下的 **设置**。

1. 在页面底部附近的 " **OAuth 连接设置** " 下，选择 " **添加设置**"。

1. 按如下所示填写窗体，然后选择 " **保存**"。

    - **名称**： `GraphBotAuth`
    - **提供程序**： **Azure Active Directory v2**
    - **客户端 id**：您的 **图形日历机器人身份验证** 注册的应用程序 id。
    - **客户端密码**：您的 **图形日历机器人身份验证** 注册的客户端密码。
    - **令牌交换 URL**：留空
    - **租户 ID**： `common`
    - **作用域**： `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`

1. 选择 " **OAuth 连接设置**" 下的 " **GraphBotAuth** " 条目。

1. 选择 " **测试连接**"。 这将打开一个新的浏览器窗口或选项卡以启动 OAuth 流。

1. 如有必要，请登录。 查看请求的权限列表，然后选择 " **接受**"。

1. 您应该会看到一个 **与 "GraphBotAuth" 成功消息的测试连接** 。

> [!TIP]
> 您可以在此页上选择 " **复制令牌** " 按钮，然后将令牌粘贴到中， [https://jwt.ms](https://jwt.ms) 以查看令牌中的声明。 这在对身份验证错误进行故障排除时很有用。
