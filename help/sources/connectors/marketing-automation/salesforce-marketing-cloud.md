---
solution: Experience Platform
title: Salesforce Marketing Cloud Sourceの概要
description: API またはユーザーインターフェイスを使用してSalesforce Marketing CloudをAdobe Experience Platformに接続する方法について説明します。
exl-id: 2177d68c-0cef-4031-a0e7-8bf22ee2e70b
last-substantial-update: 2025-04-29T00:00:00Z
source-git-commit: ce96dbc64845fddb40ebee709828c56d51a6c6ba
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 14%

---

# [!DNL Salesforce Marketing Cloud]

>[!WARNING]
>
>[!DNL Salesforce Marketing Cloud] ソースは 2026 年 1 月に非推奨（廃止予定）になります。 新しいソースは、代替手段として今年後半にリリースされる予定です。 新しいソースがリリースされたら、2026 年 1 月末までに、新しいアカウント接続とデータフローを作成して、新しいソースに移行する計画を立てる必要があります。

Adobe Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。

Experience Platformでは、サードパーティのマーケティング自動化システムからのデータ取り込みがサポートされています。 マーケティング自動化プロバイダーのサポートには、[!DNL Salesforce Marketing Cloud] が含まれます。

## 前提条件

[!DNL Salesforce Marketing Cloud] ソースをExperience Platformに接続する前に、次の **権限範囲** が [!DNL Salesforce Marketing Cloud] クライアント ID とクライアントシークレットの組み合わせにプロビジョニングされていることを確認する必要があります。

* `campaign_read`
* `list_and_subscribers_read`

[!DNL Salesforce Marketing Cloud] API の `v2/userinfo` リソースを呼び出すことで、スコープをリクエストできます。 範囲をリクエストして比較する方法については、[[!DNL Salesforce Marketing Cloud] API 統合権限の範囲 ](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/data-access-permissions.html>) ドキュメントを参照してください。

関連する権限と動作のリストなど、範囲について詳しくは、この [[!DNL Salesforce Marketing Cloud] REST API ドキュメント ](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rest-permissions-and-scopes.html>) を参照してください。

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、[!DNL Salesforce Marketing Cloud] ソース統合ではサポートされていません。

## IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

>[!WARNING]
>
>必要な IP アドレスを許可リストに追加しない場合、[!DNL Salesforce Marketing Cloud] アカウントはExperience Platformに接続されません。

## API を使用した [!DNL Salesforce Marketing Cloud] のExperience Platformへの接続

以下のドキュメントでは、API を使用して [!DNL Salesforce Marketing Cloud] をExperience Platformに接続する方法について説明します。

* [Flow Service API を使用したSalesforce Marketing Cloud ベース接続の作成](../../tutorials/api/create/marketing-automation/salesforce-marketing-cloud.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用して、マーケティングの自動処理ソースのデータフローを作成する](../../tutorials/api/collect/marketing-automation.md)

## UI を使用した [!DNL Salesforce Marketing Cloud] のExperience Platformへの接続

以下のドキュメントでは、ユーザーインターフェイスを使用して [!DNL Salesforce Marketing Cloud] をExperience Platformに接続する方法について説明します。

* [UI でのSalesforce Marketing Cloud ソースコネクタの作成](../../tutorials/ui/create/marketing-automation/salesforce-marketing-cloud.md)
* [UI でのマーケティングの自動処理ソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md)
