---
title: 使用自定义策略配置会话行为 - Azure Active Directory B2C | Microsoft Docs
description: 在 Azure Active Directory B2C 中使用自定义策略配置会话行为。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/27/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 2ef379a699d1c67a67381dbd5a2531a5f18a119c
ms.sourcegitcommit: dd2bc914f6fc2309f122b1c7109e258ceaa7c868
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87297714"
---
# <a name="configure-session-behavior-using-custom-policies-in-azure-active-directory-b2c"></a>在 Azure Active Directory B2C 中使用自定义策略配置会话行为

使用 Azure Active Directory B2C (Azure AD B2C) 中的[单一登录 (SSO) 会话](session-overview.md)管理，管理员可在用户已通过身份验证之后控制与用户的交互。 例如，管理员可以控制是否显示标识提供者选择，或是否需要再次输入帐户详细信息。 本文介绍如何配置 Azure AD B2C SSO 的设置。

## <a name="session-behavior-properties"></a>会话行为属性

可使用以下属性来管理 Web 应用程序会话：

- **Web 应用会话生存期（分钟）** - 身份验证成功后，存储在用户浏览器上的 Azure AD B2C 会话 Cookie 的生存期。
    - 默认值 = 86400 秒（1440 分钟）。
    - 最小值（含）= 900 秒（15 分钟）。
    - 最大值（含）= 86400 秒（1440 分钟）。
- **Web 应用会话超时** - [会话到期类型](session-overview.md#session-expiry-type)：“滚动”或“绝对” 。 
- **单一登录配置** - Azure AD B2C 租户中跨多个应用和用户流的单一登录 (SSO) 行为的[会话范围](session-overview.md#session-scope)。 

## <a name="configure-the-properties"></a>配置属性

若要更改会话行为和 SSO 配置，需要在 [RelyingParty](relyingparty.md) 元素内添加 **UserJourneyBehaviors** 元素。  **UserJourneyBehaviors** 元素必须紧跟在 **DefaultUserJourney** 之后。 UserJourneyBehavors 元素应当如以下示例所示：

```xml
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

## <a name="single-sign-out"></a>单一登录

### <a name="configure-the-applications"></a>配置应用程序

将用户重定向到 Azure AD B2C 注销终结点（适用于 OAuth2 和 SAML 协议）时，Azure AD B2C 将从浏览器中清除该用户的会话。  若要允许[单一注销](session-overview.md#single-sign-out)，请在 Azure 门户中设置应用程序的 `LogoutUrl`：

1. 导航到 [Azure 门户](https://portal.azure.cn)。
1. 通过单击页面右上角的帐户选择 Azure AD B2C 目录。
1. 在左侧菜单中，依次选择“Azure AD B2C”、“应用注册”、你的应用程序。
1. 选择“设置”，接着选择“属性”，然后找到“注销 URL”文本框。 

### <a name="configure-the-token-issuer"></a>配置令牌颁发者 

若要支持单一注销，JWT 和 SAML 的令牌颁发者技术配置文件必须指定以下内容：

- 协议名称，例如 `<Protocol Name="OpenIdConnect" />`
- 对会话技术配置文件的引用，例如 `UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />`。

下面的示例演示了单一注销的 JWT 和 SAML 令牌颁发者：

```xml
<ClaimsProvider>
  <DisplayName>Local Account SignIn</DisplayName>
  <TechnicalProfiles>
    <!-- JWT Token Issuer -->
    <TechnicalProfile Id="JwtIssuer">
      <DisplayName>JWT token Issuer</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputTokenFormat>JWT</OutputTokenFormat>
      ...    
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-OAuth-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for OIDC based tokens -->
    <TechnicalProfile Id="SM-OAuth-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.OAuthSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>

    <!--SAML token issuer-->
    <TechnicalProfile Id="Saml2AssertionIssuer">
      <DisplayName>SAML token issuer</DisplayName>
      <Protocol Name="SAML2" />
      <OutputTokenFormat>SAML2</OutputTokenFormat>
      ...
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Saml-issuer" />
    </TechnicalProfile>

    <!-- Session management technical profile for SAML based tokens -->
    <TechnicalProfile Id="SM-Saml-issuer">
      <DisplayName>Session Management Provider</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure AD B2C 会话](session-overview.md)。

