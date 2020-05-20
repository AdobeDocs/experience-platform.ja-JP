---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アクセス制御トラブルシューティングガイド
topic: troubleshooting guide
translation-type: tm+mt
source-git-commit: c4da32630d3a6476956347b76d611553ef1853fa
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---


# アクセス制御トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformのアクセス制御に関するよくある質問に対する回答を示します。 他のプラットフォームサービスに関する質問とトラブルシューティングについては、 [Experience Platformトラブルシューティングガイドを参照してください](../landing/troubleshooting.md)。

Experience Platformは、 [Adobe管理コンソールの製品プロファイルを活用して](http://adminconsole.adobe.com) 、ロールベースの ****&#x200B;アクセス制御を行い、ユーザーを権限およびサンドボックスにリンクします。  See the [access control overview](home.md) for more information.

## 現在のアクセス権限はどこにありますか。

IMS組織のシステム管理者、製品管理者または製品プロファイル管理者は、割り当てられた製品プロファイルとAdobe Admin Console内で提供する権限を表示できます。 製品プロファイルの権限を表示するための管理コンソールへの移動方法については、 [アクセス制御ガイドを参照してください](./ui/overview.md) 。

管理者でない場合でも、アクセス制御APIのエンドポイントにリクエストを送信することで、現在のアクセス権限を表示でき `/acl/effective-policies` ます。 詳しくは、 [アクセス制御開発者ガイドの「表示効果の高いポリシー」の節を参照してください](./api/effective-policies.md) 。

## プラットフォームUIの一部の機能は使用できません。 権限によって制御されるこれらの機能へのアクセス方法

特定のプラットフォーム機能に対するアクセス権限を持っていない場合、その機能は、エクスペリエンスプラットフォームUIで非表示または灰色表示になります。 例えば、「プロファイル」タブを表示するには、「表示プロファイル」または「プロファイルの管理」権限が必要です。 エクスペリエンスプラットフォーム機能に対する追加の権限が必要な場合は、管理者に問い合わせてください。

## 権限のグループ化方法、および使用する権限が含まれるグループ

権限は、適用するプラットフォーム機能(データ管理およびプロファイル管理など)によってグループ化され、分類されます。 使用可能な権限とその権限が属するグループの完全なリストについては、アクセス制御の概要の [権限の節](home.md#permissions) を参照してください。

ロールベースのアクセス制御の提供について詳しくは、 [アクセス制御の概要](home.md) （英語）を参照してください。