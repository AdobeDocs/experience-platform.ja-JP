---
keywords: Experience Platform;home;popular topics;troubleshooting;access control
solution: Experience Platform
title: アクセス制御トラブルシューティングガイド
topic: troubleshooting guide
description: このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。
translation-type: tm+mt
source-git-commit: 28b733a16b067f951a885c299d59e079f0074df8
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 71%

---


# アクセス制御トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。For questions and troubleshooting related to other [!DNL Platform] services, please refer to the [Experience Platform troubleshooting guide](../landing/troubleshooting.md).

[!DNL Experience Platform] は、[Adobe Admin Console](http://adminconsole.adobe.com) の製品プロファイルを活用して、役割ベースのアクセス制御を提供し、ユーザーを権限およびサンドボックスにリンクします。  詳しくは、「[アクセス制御の概要](home.md)」を参照してください。

## 現在のアクセス権限はどこで確認できますか。

IMS 組織のシステム管理者、製品管理者、製品プロファイル管理者は、割り当てられた製品プロファイルと提供される権限を Adobe Admin Console で表示できます。 を使用して製品プロファイルの権限を表示する方法については、『[アクセス制御ユーザガイド](./ui/overview.md)』を参照してください。[!DNL Admin Console]

管理者以外の場合でも、アクセス制御 API の `/acl/effective-policies` エンドポイントにリクエストを送信することで、現在のアクセス権限を表示することができます。詳しくは、『[アクセス制御開発者ガイド](./api/effective-policies.md)』の「有効ポリシーの表示」の節を参照してください。

## Some features in the [!DNL Platform] UI are not available. これらの機能へのアクセスは、権限によってどのように制御されますか。

If you do not have access permissions for a particular [!DNL Platform] feature, that feature will be hidden or greyed-out in the [!DNL Experience Platform] UI. For example, in order to view the &quot;[!UICONTROL Profiles]&quot; tab, you must have either the &quot;[!UICONTROL View Profiles]&quot; or &quot;[!UICONTROL Manage Profiles]&quot; permissions. Contact your administrator if you require additional permissions for [!DNL Experience Platform] capabilities.

## 権限はどのようにグループ化されていますか。使用したい権限を含むグループはどれですか。

Permissions are grouped and categorized by the [!DNL Platform] capabilities they apply to (such as [!DNL Data Management] and [!DNL Profile Management]). 使用可能な権限とその属するグループの完全なリストについては、「アクセス制御の概要」の「[権限](home.md#permissions)」の節を参照してください。

役割ベースのアクセス制御の提供について詳しくは、「[アクセス制御の概要](home.md)」を参照してください。