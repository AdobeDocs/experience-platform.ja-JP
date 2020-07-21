---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アクセス制御トラブルシューティングガイド
topic: troubleshooting guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---


# アクセス制御トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformでのアクセス制御に関するよくある質問に対する回答を示します。 他の [!DNL Platform] サービスに関する質問とトラブルシューティングについては、 [Experience Platformトラブルシューティングガイドを参照してください](../landing/troubleshooting.md)。

[!DNL Experience Platform] ユーザーを権限およびサンドボックスにリンクする、 [AdobeAdmin Consoleの製品プロファイルを活用して、ロールベースの](http://adminconsole.adobe.com) アクセス制御 ****（ユーザーを権限およびサンドボックスにリンク）を提供します。  See the [access control overview](home.md) for more information.

## 現在のアクセス権限はどこにありますか。

IMS組織のシステム管理者、製品管理者または製品プロファイル管理者は、割り当てられた製品プロファイルとAdobeAdmin Console内で提供される権限を表示できます。 製品プロファイルの権限を表示に移動する方法については、 [アクセス制御ユーザーガイド](./ui/overview.md)[!DNL Admin Console] を参照してください。

管理者でない場合でも、アクセス制御APIのエンドポイントにリクエストを送信することで、現在のアクセス権限を表示でき `/acl/effective-policies` ます。 詳しくは、 [アクセス制御開発者ガイドの「表示効果の高いポリシー」の節を参照してください](./api/effective-policies.md) 。

## UIの一部の機能は使用できません [!DNL Platform] 。 権限によって制御されるこれらの機能へのアクセス方法

特定の [!DNL Platform] 機能に対するアクセス権限を持っていない場合、その機能は [!DNL Experience Platform] UIで非表示または灰色表示になります。 例えば、「[!UICONTROL プロファイル]」タブを表示するには、「[!UICONTROL 表示プロファイル]」または「プロファイル[!UICONTROL の管理]」権限が必要です。 機能に対する追加の権限が必要な場合は、管理者に問い合わせて [!DNL Experience Platform] ください。

## 権限のグループ化方法、および使用する権限が含まれるグループ

権限は、適用する [!DNL Platform] 機能( [!DNL Data Management] および [!DNL Profile Management]など)によってグループ化され、分類されます。 使用可能な権限とその権限が属するグループの完全なリストについては、アクセス制御の概要の [権限の節](home.md#permissions) を参照してください。

ロールベースのアクセス制御の提供について詳しくは、 [アクセス制御の概要](home.md) （英語）を参照してください。