

## DEX

Dex是一种身份服务，它使用OpenID Connect(简称OIDC)来驱动其他应用程序的身份验证。

Dex通过“connectors.”充当其他身份提供商的门户。这使得dex可以将身份验证延迟(找不到很好的词来形容，只能硬翻了)到LDAP服务器、SAML提供程序或已建立的身份提供程序（如GitHub，Google和Active Directory）。客户端编写一次身份验证逻辑与dex通信，然后dex处理给定后端的协议。

https://www.dazhuanlan.com/2019/12/27/5e057a21760f1/

## OAuth2

OAuth（开放授权）是一个开放标准，允许用户授权第三方移动应用访问他们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方移动应用或分享他们数据的所有内容。

我相信大家都使用过类似“使用QQ登录”诸如此类的按钮来登录一些第三方的应用或者网站。在这些情况下，第三方应用程序(网站)选择让外部提供商（在这种情况下为QQ）证明您的身份，而不是让您使用应用程序本身设置用户名和密码。

服务器端应用程序的一般流程是：

- 新用户访问应用程序。

- 用户点击网站上的登录按钮(诸如“使用QQ登录”)，该应用程序将用户重定向到QQ。

- 用户登录QQ，然后QQ会提示该应用程序会获取的相应权限。

- 如果用户单击“授权并登录”，则QQ会连同获取的Access Token使用代码将用户重定向回该应用程序。

- 应用程序通过Access Token获取用户的OpenID；调用OpenAPI，来请求访问或修改用户授权的资源。

在这些情况下，dex充当QQ（在OpenID Connect中称为“provider”），而客户端应用程序重定向到它以获得最终用户的身份。

> 关于OAuth: https://oauth.net/2/

## ID Tokens

ID Tokens
是OpenID Connect和dex主要功能引入的OAuth2扩展。ID Tokens
是由dex签名的JSON Web令牌（JWT），作为OAuth2响应的一部分返回，用于证明最终用户的身份。

OpenID Connect的OAuth2主要扩展名是令牌响应中返回的额外令牌，称为ID Tokens。此令牌是由OpenID Connect服务器签名的JSON Web令牌，具有用户ID，名称，电子邮件等众所周知的字段。

## Connectors

当用户通过dex登录时，用户的身份通常存储在另一个用户管理系统中：LDAP目录，GitHub组织等。Dex充当客户端应用程序和上游身份提供者之间的中间人。客户端只需要了解OpenID Connect来查询dex，而dex实现了一组用于查询其他用户管理系统的协议。

## OpenID Connect Tokens

OpenID Connect 1.0是OAuth 2.0协议之上的简单身份层。它允许客户端根据授权服务器执行的身份验证来验证最终用户的身份，以及以可互操作和类似REST的方式获取有关最终用户的基本配置文件信息。

OpenID Connect允许所有类型的客户端（包括基于Web，移动和JavaScript客户端）请求和接收有关经过身份验证的会话和最终用户的信息。规范套件是可扩展的，允许参与者在对它们有意义时使用可选功能，例如身份数据加密，OpenID提供程序的发现和会话管理。

协议的OAuth2的主要扩展是返回的附加字段，其中访问令牌称为ID Token。此令牌是JSON Web令牌（JWT），具有由服务器签名的众所周知的字段，例如用户的电子邮件。

为了识别用户，验证者使用OAuth2 令牌响应中的id_token（而不是access_token） 作为承载令牌。

来自OpenID Connect提供商的令牌响应包括一个称为ID令牌的签名JWT。ID令牌包含名称，电子邮件，唯一标识符，在dex的情况下，包含一组可用于标识用户的组。像dex这样的OpenID Connect提供程序发布公钥; Kubernetes API服务器了解如何使用它们来验证ID令牌。

> 关于OpenID Connect: https://openid.net/connect/

身份验证流程如下所示：

- OAuth2客户端通过dex登录用户。

- 在与Kubernetes API通信时，该客户端使用返回的ID令牌作为承载令牌。

- Kubernetes使用dex的公钥来验证ID令牌。

- 指定为用户名（以及可选的组信息）的声明将与该请求相关联。

> 用户名和组信息可以与Kubernetes 授权插件（例如基于角色的访问控制（RBAC））结合使用以实施策略。

dex有自己的用户概念，但它允许它们以不同的方式进行身份验证，称为connectors。目前，dex提供两种类型的连接器：local连接器和OIDC连接器。使用local连接器进行身份验证时，用户使用电子邮件和密码登录，并使用dex本身提供的可自定义UI。使用OIDC连接器，用户可以通过登录到另一个OIDC身份提供商（如Google或Salesforce）进行身份验证。

## 从dex请求ID令牌

直接使用dex对用户进行身份验证的应用程序使用OAuth2代码流来请求令牌响应。采取的确切步骤是：

- 用户访问客户端应用。

- 客户端应用程序通过OAuth2请求将用户重定向到dex。

- Dex确定用户的身份。

- Dex使用代码将用户重定向到客户端。

- 客户端使用dex为id_token交换代码。