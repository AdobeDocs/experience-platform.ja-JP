---
solution: Experience Platform
title: SalesforceMarketing Cloudソースの概要
description: API またはユーザーインターフェイスを使用して SalesforceMarketing CloudをAdobe Experience Platformに接続する方法を説明します。
exl-id: 2177d68c-0cef-4031-a0e7-8bf22ee2e70b
last-substantial-update: 2023-05-25T00:00:00Z
source-git-commit: bc37d41d0f7b0ff0cf4d52242f41467f2891d613
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 42%

---

# [!DNL Salesforce Marketing Cloud]

Adobe Experience Platform を使用すると、外部ソースからデータを取り込みながら、Platform サービスを使用して受信データの構造化、ラベル付けおよび拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformは、サードパーティのマーケティングオートメーションシステムからデータを取り込む機能を提供しています。 マーケティング自動化プロバイダーのサポートは次のとおりです。 [!DNL Salesforce Marketing Cloud].

## 前提条件

接続する前に [!DNL Salesforce Marketing Cloud] Platform にソースする場合は、次の点を確認する必要があります。 **権限範囲** が [!DNL Salesforce Marketing Cloud] クライアント ID とクライアント秘密鍵の組み合わせ：

* `campaign_read`
* `list_and_subscribers_read`

スコープをリクエストするには、 `v2/userinfo` リソース [!DNL Salesforce Marketing Cloud] API 詳しくは、 [[!DNL Salesforce Marketing Cloud] API 統合権限スコープドキュメント](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/data-access-permissions.html>) スコープのリクエスト方法と比較方法に関するガイダンスを参照してください。

スコープの関連する権限と動作の一覧を含むスコープの詳細については、次を参照してください [[!DNL Salesforce Marketing Cloud] REST API ドキュメント](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rest-permissions-and-scopes.html>).

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、 [!DNL Salesforce Marketing Cloud] ソースの統合。

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## API を使用して [!DNL Salesforce Marketing Cloud] と Platform を接続する

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Salesforce Marketing Cloud] API を使用して Platform に接続するには：

* [フローサービス API を使用して SalesforceMarketing Cloudベース接続を作成する](../../tutorials/api/create/marketing-automation/salesforce-marketing-cloud.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用して、マーケティングの自動処理ソースのデータフローを作成する](../../tutorials/api/collect/marketing-automation.md)

## UI を使用した [!DNL Salesforce Marketing Cloud] の Platform への接続

以下のドキュメントでは、接続方法に関する情報を提供します [!DNL Salesforce Marketing Cloud] ユーザーインターフェイスを使用して Platform に接続するには：

* [UI での SalesforceMarketing Cloudソース接続の作成](../../tutorials/ui/create/marketing-automation/salesforce-marketing-cloud.md)
* [UI でのマーケティングの自動処理ソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md)
