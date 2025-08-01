---
title: Salesforce Marketing Cloud Sourceの概要
description: API またはユーザーインターフェイスを使用してSalesforce Marketing CloudをAdobe Experience Platformに接続する方法について説明します。
exl-id: 2177d68c-0cef-4031-a0e7-8bf22ee2e70b
last-substantial-update: 2025-05-17T00:00:00Z
source-git-commit: 0c0a58df4beae499008e52c118b40bed86ff0596
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 8%

---

# [!DNL Salesforce Marketing Cloud]

>[!WARNING]
>
>[!DNL Salesforce Marketing Cloud] ソースは 2026 年 1 月に非推奨（廃止予定）になります。 新しいソースは、代替手段として今年後半にリリースされる予定です。 新しいソースがリリースされたら、2026 年 1 月末までに、新しいアカウント接続とデータフローを作成して、新しいソースに移行する計画を立てる必要があります。

[!DNL Salesforce Marketing Cloud] を使用すると、メール、モバイル、ソーシャルメディア、広告をまたいで顧客エンゲージメントを 1 つのプラットフォームから管理および自動化できます。 Email Studio、Customer Builder、Audience Builder などのツールを使用すると、オーディエンスに合わせてパーソナライズされたキャンペーンやジャーニージャーニーを作成できます。

[!DNL Salesforce Marketing Cloud] ソースを使用してアカウントを接続し、データをAdobe Experience Platformに取り込むことができます。

## 前提条件

[!DNL Salesforce Marketing Cloud] ソースをExperience Platformに接続する前に、次の **権限範囲** が [!DNL Salesforce Marketing Cloud] クライアント ID とクライアントシークレットの組み合わせにプロビジョニングされていることを確認する必要があります。

* `campaign_read`
* `list_and_subscribers_read`

[!DNL Salesforce Marketing Cloud] API の `v2/userinfo` リソースを呼び出すことで、スコープをリクエストできます。 範囲をリクエストして比較する方法については、[[!DNL Salesforce Marketing Cloud] API 統合権限の範囲 ](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/data-access-permissions.html>) ドキュメントを参照してください。

関連する権限と動作のリストなど、範囲について詳しくは、この [[!DNL Salesforce Marketing Cloud] REST API ドキュメント ](<https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/rest-permissions-and-scopes.html>) を参照してください。

>[!IMPORTANT]
>
>カスタムオブジェクトの取り込みは、現在、[!DNL Salesforce Marketing Cloud] ソース統合ではサポートされていません。

### IP アドレスの許可リスト

ソースをExperience Platformに接続する前に、地域固有の IP アドレスを許可リストに追加する必要があります。 詳しくは、[Experience Platformへの接続に対する IP アドレスの許可リストに加える](../../ip-address-allow-list.md) に関するガイドを参照してください。

>[!WARNING]
>
>必要な IP アドレスを許可リストに追加しない場合、[!DNL Salesforce Marketing Cloud] アカウントはExperience Platformに接続されません。

### Azure 上のExperience Platformに対する認証 {#azure}

[!DNL Azure] 上のExperience Platformに接続するには、次の資格情報の値を指定 [!DNL Salesforce Marketing Cloud] る必要があります。

| 資格情報 | 説明 |
| --- | --- |
| ホスト | アプリケーションのホストサーバー。 多くの場合、これはサブドメインです。 **メモ：**`host` 値を入力する場合は、`{subdomain}.rest.marketingcloudapis.com` を指定する必要があります。 例えば、ホスト URL が `https://acme-ab12c3d4e5fg6hijk7lmnop8qrst.auth.marketingcloudapis.com/` の場合、ホスト値として `acme-ab12c3d4e5fg6hijk7lmnop8qrst.rest.marketingcloudapis.com/` を入力する必要があります。 |
| クライアント ID | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント ID。 |
| クライアントシークレット | [!DNL Salesforce Marketing Cloud] アプリケーションに関連付けられたクライアント秘密鍵。 |
| 接続仕様 ID | **接続仕様** は、データソースのコネクタプロパティを提供します。 認証仕様や、**ベース** 接続と **ソース** 接続の両方を作成するための要件などの詳細が含まれます。 [!DNL Salesforce Marketing Cloud] の場合、接続仕様 ID は `ea1c2a08-b722-11eb-8529-0242ac130003` です。 **メモ：** この資格情報は、API 経由で接続する場合にのみ必要です。 |

### Amazon Web Services上のExperience Platformに対する認証（AWS） {#aws}

>[!AVAILABILITY]
>
>この節の内容は、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](../../../landing/multi-cloud.md) を参照してください。

AWS上のExperience Platformに接続するには、次の資格情報の値 [!DNL Salesforce Marketing Cloud] 指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| サブドメイン | API エンドポイントの構築に使用される、[!DNL Salesforce Marketing Cloud] インスタンスの URL の一意の部分。 |
| クライアント ID | アプリケーションの公開識別子。インストール済みのパッケージを [!DNL Salesforce Marketing Cloud] で作成する際に生成されます |
| クライアントシークレット | クライアント ID に関連付けられた機密キーで、インストールされたパッケージでも生成されます。 |
| 接続仕様 ID | **接続仕様** は、データソースのコネクタプロパティを提供します。 認証仕様や、**ベース** 接続と **ソース** 接続の両方を作成するための要件などの詳細が含まれます。 [!DNL Salesforce Marketing Cloud] の場合、接続仕様 ID は `ea1c2a08-b722-11eb-8529-0242ac130003` です。 **メモ：** この資格情報は、API 経由で接続する場合にのみ必要です。 |

詳しくは、[[!DNL Salesforce]  サーバー間統合用のアクセストークンに関するドキュメント ](https://developer.salesforce.com/docs/marketing/marketing-cloud/guide/access-token-s2s.html) を参照してください。

## API を使用した [!DNL Salesforce Marketing Cloud] のExperience Platformへの接続

以下のドキュメントでは、API を使用して [!DNL Salesforce Marketing Cloud] をExperience Platformに接続する方法について説明します。

* [Flow Service API [!DNL Salesforce Marketing Cloud]  使用したExperience Platformへの接続](../../tutorials/api/create/marketing-automation/salesforce-marketing-cloud.md)
* [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
* [Flow Service API を使用して、マーケティングの自動処理ソースのデータフローを作成する](../../tutorials/api/collect/marketing-automation.md)

## UI を使用した [!DNL Salesforce Marketing Cloud] のExperience Platformへの接続

以下のドキュメントでは、ユーザーインターフェイスを使用して [!DNL Salesforce Marketing Cloud] をExperience Platformに接続する方法について説明します。

* [UI [!DNL Salesforce Marketing Cloud] Experience Platformへの接続](../../tutorials/ui/create/marketing-automation/salesforce-marketing-cloud.md)
* [UI でのマーケティングの自動処理ソース接続のデータフローの作成](../../tutorials/ui/dataflow/marketing-automation.md)
