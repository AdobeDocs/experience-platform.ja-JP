---
keywords: Experience Platform；ホーム；人気の高いトピック；トラブルシューティング；アクセス制御
solution: Experience Platform
title: アクセス制御トラブルシューティングガイド
topic-legacy: troubleshooting guide
description: このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。
exl-id: c299c0c4-dbee-4e6d-8af4-2446444bed69
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 68%

---

# アクセス制御トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform のアクセス制御に関するよくある質問に対する回答を示します。他の[!DNL Platform]サービスに関するご質問やトラブルシューティングについては、[Experience Platformトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

[!DNL Experience Platform] は、[Adobe Admin Console](http://adminconsole.adobe.com) の製品プロファイルを活用して、役割ベースのアクセス制御を提供し、ユーザーを権限およびサンドボックスにリンクします。  詳しくは、「[アクセス制御の概要](home.md)」を参照してください。

## 現在のアクセス権限はどこで確認できますか。

IMS 組織のシステム管理者、製品管理者、製品プロファイル管理者は、割り当てられた製品プロファイルと提供される権限を Adobe Admin Console で表示できます。 を使用して製品プロファイルの権限を表示する方法については、『[アクセス制御ユーザガイド](./ui/overview.md)』を参照してください。[!DNL Admin Console]

管理者以外の場合でも、アクセス制御 API の `/acl/effective-policies` エンドポイントにリクエストを送信することで、現在のアクセス権限を表示することができます。詳しくは、『[アクセス制御開発者ガイド](./api/effective-policies.md)』の「有効ポリシーの表示」の節を参照してください。

## [!DNL Platform] UIの一部の機能は使用できません。 これらの機能へのアクセスは、権限によってどのように制御されますか。

特定の[!DNL Platform]機能に対するアクセス権限を持っていない場合、その機能は[!DNL Experience Platform] UIで非表示またはグレー表示になります。 例えば、「[!UICONTROL プロファイル]」タブを表示するには、「[!UICONTROL 表示プロファイル]」または「[!UICONTROL プロファイルを管理]」権限が必要です。 [!DNL Experience Platform]機能に対する追加の権限が必要な場合は、管理者に問い合わせてください。

## 権限はどのようにグループ化されていますか。使用したい権限を含むグループはどれですか。

権限は、[!DNL Platform]機能（[!DNL Data Management]や[!DNL Profile Management]など）によってグループ化され、分類されます。 使用可能な権限とその属するグループの完全なリストについては、「アクセス制御の概要」の「[権限](home.md#permissions)」の節を参照してください。

役割ベースのアクセス制御の提供について詳しくは、「[アクセス制御の概要](home.md)」を参照してください。
