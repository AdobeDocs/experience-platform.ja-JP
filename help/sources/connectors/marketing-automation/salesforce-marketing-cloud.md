---
keywords: Experience Platform；ホーム；人気のトピック；Salesforce Marketing Cloud;SalesforceMarketing Cloud；マーケティング自動化
solution: Experience Platform
title: SalesforceMarketing Cloudソースの概要
topic-legacy: overview
description: API またはユーザーインターフェイスを使用して SalesforceMarketing CloudをAdobe Experience Platformに接続する方法を説明します。
exl-id: 2177d68c-0cef-4031-a0e7-8bf22ee2e70b
source-git-commit: 6876ad592e4924dc884568899662dfa9c8eecc62
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 13%

---

# (ベータ) [!DNL Salesforce Marketing Cloud]

>[!NOTE]
>
>この [!DNL Salesforce Marketing Cloud] ソースはベータ版です。 詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platform を使用すると、データを外部ソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、サードパーティのマーケティング自動化システムからデータを取り込む機能を備えています。 マーケティング自動化プロバイダーのサポートは次のとおりです。 [!DNL Salesforce Marketing Cloud].

## 前提条件

接続する前に [!DNL Salesforce Marketing Cloud] Platform にソースする場合は、次の点を確認する必要があります。 **権限範囲** が [!DNL Salesforce Marketing Cloud] クライアント ID とクライアント秘密鍵の組み合わせ：

* `campaign_read`
* `list_and_subscribers_read`

スコープをリクエストするには、 `v2/userinfo` リソース [!DNL Salesforce Marketing Cloud] API 詳しくは、 [[!DNL Salesforce Marketing Cloud] API 統合権限スコープドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/data-access-permissions.html) スコープのリクエスト方法と比較方法に関するガイダンスを参照してください。

スコープの関連する権限と動作の一覧を含むスコープの詳細については、次を参照してください [[!DNL Salesforce Marketing Cloud] REST API ドキュメント](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rest-permissions-and-scopes.html).

## IP アドレスの許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、 [IP アドレスの許可リスト](../../ip-address-allow-list.md) ページを参照してください。

## 接続 [!DNL Salesforce Marketing Cloud] API を使用して Platform に接続

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Salesforce Marketing Cloud] API を使用して Platform に接続するには：

* [フローサービス API を使用して SalesforceMarketing Cloudベース接続を作成する](../../tutorials/api/create/marketing-automation/salesforce-marketing-cloud.md)
* [フローサービス API を使用したマーケティング自動化ソースのデータ構造とコンテンツの調査](../../tutorials/api/explore/marketing-automation.md)
* [フローサービス API を使用して、マーケティング自動化ソースのデータフローを作成する](../../tutorials/api/collect/marketing-automation.md)

## 接続 [!DNL Salesforce Marketing Cloud] UI を使用して Platform に接続

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Salesforce Marketing Cloud] ユーザーインターフェイスを使用して Platform に接続するには：

* [UI での SalesforceMarketing Cloudソース接続の作成](../../tutorials/ui/create/marketing-automation/salesforce-marketing-cloud.md)
* [UI でのマーケティング自動化ソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md)
