## 什么是ldap服务器？

LDAP服务器简单来说它是一种得到某些数据的快捷方式，同时LDAP服务器也是一个协议，它经常被用作集体的地址本使用，甚至可以做到更加庞大。它是一种特殊的数据库，与一般的数据库相比有很大的差距，LDAP服务器的读性与一般服务器相比更加优秀。同时LDAP服务器在查询上总了很多的优化，所以利用它可以快速查询出想要得到的结果，当然它也有缺陷，比如在更新方面，它会更新的很慢。



LDAP服务器的目录有哪些优势和特点，第一个特点就是LDAP服务器目录可以帮助大多数的用户解决网络服务的账户问题。第二个特点就是LDAP服务器目录它可以很好地保证了数据的完整性，因为你在LDAP服务器目录中规定了统一的数据库，从而可以实现资源的统一性。LDAP服务器目录的最后一个优势就是它的设计可以适用多种行业的服务组织。



使用LDAP服务器的格式，在LDAP服务器中会采用一种命名格式，这种常见的命名格式一般有两种，一种为RFC822命名法，它的标准格式是object_name@domain_name，这种命名方式非常像邮件的形式。另一种命名格式是LDAP URL和X.500，这种命名法也叫做属性化命名法，它可以包括服务对象的属性和活动目录所在的服务器。

## ldap有什么用？

**统一身份认证**
**单点登录**
单点登录：（Single Sign On），简称为 SSO，是目前比较流行的企业业务整合的解决方案之一。SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。
单点登录网络生活中随处可见，比如登录了QQ客户端，然后你可以打开腾讯微博，QQ空间，QQ邮箱，校友录等等一系列的应用，这时候我们不需要在一个个再输入用户名和密码了，作为受信任的站点，就可以直接登录了。这些我们都已经习以为常了，其实这就是单点登录的例子，离我们一点都不遥远。但作为一个开发者，不只是要使用其功能，更要明白其原理并开发出支持单点登录的应用出来。
**需求**
**从个人的角度**
入职后每个人会接触多个系统，项目管理系统，需求管理系统，代码仓库，持续集成，文件服务器，有八个系统就有八个密码，每个系统用一个密码安全隐患较大，每个系统都用不同的密码也不容易记住。
**从管理角度**
公司需要一个统一认证系统，自助分配权限系统，定期审计，集中分配与追踪所有权限，便于记录。没有的后果，操作与安全问题无法追溯，工作效率低。会占用大量人力去添加账户。建议建立统一认证系统与自助分配权限的系统。人员复杂与传统公司不同我们人员不固定流动性较大更需要整体把握与权限，而且需要定期审计。多项目情况下人员变动更为复杂。解决方案通过openldap或者ad进行权限划分配合一定的二次开发，为什么不建议分组分散到各个系统，添加人员与迁移成本非常高。
**单点登录 (SSO)**
为用户提供一组用户名和密码来登录需要访问的所有应用，让他们的生活更加轻松。无缝集成 Nginx,Ftp,Gitlab,Jenkins,Jira、Confluence 和 Bitbucket 等所有产品，为用户提供单点登录 (SSO) 体验。
**集中多个目录**
如果公司属于集团或者已有统一认证，windowsad或ldap需要集成多个已有目录，将任意目录组合映射到单个应用（非常适用于管理不在主目录中的用户），然后在同一位置管理身份验证权限。开始使用适用于 AD、LDAP、Microsoft Azure AD、Novell eDirectory 等的连接器。您甚至可以创建自己的自定义连接器。
**Ldap协议**
市面上只要你能够想像得到的所有工具软件，全部都支持 LDAP协议。比如说你公司要安装一个项目管理工具，那么这个工具几乎必然支持 LDAP协议，你公司要安装一个 bug管理工具，这工具必然也支持 LDAP协议，你公司要安装一套软件版本管理工具，这工具也必然支持 LDAP协议。 LDAP协议的好处就是你公司的所有员工在所有这些工具里共享同一套用户名和密码，来人的时候新增一个用户就能自动访问所有系统，走人的时候一键删除就取消了他对所有系统的访问权限，这就是 LDAP。
**LdapServer方案**
公司搭建LDAP Server的几个方案。
**crowd3**
**什么是Crowd?**
以下是来自官网的介绍：
能够管理来自多个目录（Active Directory、LDAP、OpenLDAP 或 Microsoft Azure AD）的用户，并在一个位置控制应用身份验证权限。
**为什么考虑crowd3？**
Atlassian产品中最知名的就是confluence和jira。confluence和jira易用性和专业性。收费较高，公司使用不建议使用破解版本。有webui易用性较强。
**Freeipa**
FreeIPA是一款集成的安全信息管理解决方案。FreeIPA包含Linux (Fedora),389 Directory Server MIT Kerberos, NTP, DNS, Dogtag (Certificate System)等等身份，认证和策略功能。搭建可以用Redhat,Fedora,Centos搭建。有webui但是专业性较强，配置复杂。
**Openldap**
openLDAP，这个比较著名，yum可以直接安装。openldap开发用c和c++实现的。配置较为复杂为命令式的配置。可以采用docker搭建。无webui使用复杂。可以采用docker搭建，apt搭建，yum搭建，编译搭建。
**Appacheds**
Appacheds提供客户端与server端。Appacheds为java实现。
**WindowsAD**
WindowsAD是一个常见方案，Windows Server也是一个常见方案。Windows Ad可以快速通过windows Server搭建统一认证还可以同时管理笔记本。
**Ldap客户端方案**
**PhpLDAPAdmin**
是web版本的ldap客户端用web管理较为方便。
**Apache Directory Studio**
java版本客户端。
**LDAP Admin**
LdapAdmin是一个常用的客户端界面较为简单。
**选型对比后的结果**
crowd3收费，openldap过于简陋，freeipa过于复杂。WindowsAD管理windows电脑，Appacheds管理openldap相关认证。

