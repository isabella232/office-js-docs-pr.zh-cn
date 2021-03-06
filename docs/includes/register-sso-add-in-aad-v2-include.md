

1. 导航到“Azure 门户 - 应用注册”[](https://go.microsoft.com/fwlink/?linkid=2083908)页面以注册你的应用。

1. 使用 ***管理员*** 凭据登录到 Microsoft 365 租赁。 例如，MyName@contoso.onmicrosoft.com。

1. 选择“新注册”****。 在“注册应用”**** 页上，按如下方式设置值。

    * 将“名称”**** 设置为 **$ADD-IN-NAME$**。
    * 将“受支持的帐户类型”**** 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”****（例如，Skype、Xbox、Outlook.com）。
    * 保留“重定向 URI”**** 为空。
    * 选择“注册”****。

1. 在 **$ADD-IN-NAME$** 页面上，复制并保存“应用程序（客户端）ID”**** 和“目录（租户）ID”**** 的值。 你将在后面的过程中使用它们。

    > [!NOTE]
    > 当其他应用程序（如 Office 客户端应用程序 (例如 PowerPoint、Word、Excel) ）寻求对应用程序的授权访问时，此 ID 是 "受众" 值。 当它反过来寻求 Microsoft Graph 的授权访问权限时，它同时也是应用程序的“客户端 ID”。

1. 选择“管理”**** 下的“证书和密码”****。 选择“新客户端密码”**** 按钮。 输入“描述”**** 的值，然后选择“到期”**** 的适当选项，并选择“添加”****。 在继续操作前，*立即复制客户端机密码值并使用应用程序 ID 保存它*，因为在后面的过程中，将需要用到它。

1. 在“管理”**** 下选择“公开 API”****。 选择“设置”**** 链接在窗体“api://$App ID GUID$”中生成应用 ID URI。 在双正斜框和 GUID 之间插入 **$FQDN-WITHOUT-PROTOCOL$**（在末尾附加一个正斜框“/”）。 整个 ID 的格式应为 `api://$FQDN-WITHOUT-PROTOCOL$/$App ID GUID$`；例如 `api://localhost:6789/c6c1f32b-5e55-4997-881a-753cc1d563b7`。

    > [!NOTE]
    > 此时可能会收到一个不准确的错误：“应用程序 ID URI 必须是以 HTTPS、API、URN、MS-APPX 开头的有效 URI。 不得以斜杠结尾。” 如果该 ID 符合所述的条件，忽略该错误并保存更改。

    > [!NOTE]
    > 如果收到一条错误，指出域已有所有者，但你拥有该域，请按照[快速入门： 将自定义域名添加到 Azure Active Directory](/azure/active-directory/add-custom-domain) 中的步骤进行操作来注册该域，然后重复此步骤。  (如果您未使用 Microsoft 365 租赁中管理员的凭据登录，也会发生此错误。 请参阅步骤 2 。 注销并使用管理员凭据再次登录，然后重复步骤 3 中的过程。）

1. 选择“添加一个作用域”**** 按钮。 在打开的面板中，输入 `access_as_user` 作为“作用域名称”****。

1. 将“谁能同意?”**** 设置为“管理员和用户”****。

1. 填写用于配置管理员和用户同意提示的字段，其中包含适用于该范围的值， `access_as_user` 使 Office 客户端应用程序能够使用与当前用户相同的权限来使用您的外接程序的 Web api。 建议：

    - **管理员许可标题：** Office 可以充当用户。
    - **管理员许可描述：** 使 Office 能够借助与当前用户相同的权限调用加载项的 Web API。
    - **用户许可标题：** Office 可以充当你。
    - **管理员许可描述：** 使 Office 能够借助与你相同的权限调用加载项的 Web API。

1. 确保将“状态”**** 设置为“已启用”****。

1. 选择“添加作用域”****。

    > [!NOTE]
    > 显示在文本字段正下方的“作用域名称”**** 的域部分应自动与上一步骤中设置的“应用 ID URI”**** 匹配，并将 `/access_as_user` 附加到末尾；例如，`api://localhost:6789/c6c1f32b-5e55-4997-881a-753cc1d563b7/access_as_user`。

1. 在“授权客户端应用程序”**** 部分中，确定要授权给加载项 Web 应用程序的应用程序。 下面每个 ID 都需要进行预授权。
  
    * `d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)
    * `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)
    * `57fb890c-0dab-4253-a5e0-7188c88b2bb4`（Office 网页版）
    * `08e18876-6177-487e-b8b5-cf950c1e598c`（Office 网页版）
    * `bc59ab01-8403-45c6-8796-ac3ef710b3e3`（Outlook 网页版）

    对于每个 ID，执行以下步骤：

      a. 选择“添加客户端应用程序”**** 按钮，然后在打开的面板中，将“客户端 ID”**** 设置为相应的 GUID 并勾选 `api://$FQDN-WITHOUT-PROTOCOL$/$App ID GUID$/access_as_user` 框。

      b. 选择“添加应用程序”****。

1. 选择“管理”**** 下的“身份验证”****。 在“重定向 URI”**** 部分中，选择“类型”**** 下拉菜单中的 **Web**，然后将“重定向 URI”**** 值设置为 `https://$FQDN-WITHOUT-PROTOCOL$`。

1. 在窗体顶部，选择“保存”****。

1. 选择“管理”**** 下的“API 权限”****，然后选择“添加权限”****。 在打开的面板上，选择 **Microsoft Graph**，然后选择“委派权限”****。

1. 使用“选择权限”**** 搜索框来搜索加载项需要的权限。 示例如下。

    * Files.Read.All
    * offline_access
    * openid
    * profile

    > [!NOTE]
    > `User.Read` 权限可能已默认列出。 根据最佳做法，最好不要请求授予不需要的权限，因此，如果加载项实际上不需要此权限，我们建议取消选中此权限对应的框。

1. 在每个权限显示时，选择其复选框（请注意，选择每个权限后，它不会在列表中保持可见）。 选择加载项需要的权限后，选择面板底部的“添加权限”**** 按钮。
