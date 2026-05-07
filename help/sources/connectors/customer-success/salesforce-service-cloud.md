---
title: Salesforce Service Cloud Source コネクタの概要
description: APIまたはユーザーインターフェイスを使用して、Salesforce Service CloudをAdobe Experience Platformに接続する方法について説明します。
exl-id: 9bebbc00-55b3-4aec-9357-4127c05844e2
source-git-commit: b9a9b00114b3c1159a14b7e39484d250fa7563ba
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 10%

---

# [!DNL Salesforce Service Cloud]

[!DNL Salesforce Service Cloud]は、サービスワークフローを自動化し、企業と顧客の間のコミュニケーションを合理化するように設計されたカスタマーサクセスプラットフォームです。 メール、電話、ソーシャルメディア、ライブチャットなど、さまざまなチャネルからのリクエストを統合されたエージェントコンソールに集約できます。 これにより、サポートチームは、顧客の履歴の全体像を把握して「ケース」を管理することができ、顧客からの問い合わせに関係なく、応答がパーソナライズされた効率的なものになります。

Adobe Experience Platform ソースの[!DNL Salesforce Service Cloud] ソースコネクタを使用して、[!DNL Salesforce Service Cloud] アカウントを接続し、データをExperience Platform サービスで使用できます。

このドキュメントでは、[!DNL Salesforce Service Cloud] アカウントを設定してExperience Platformに接続する方法について説明します。

## 前提条件 {#prerequisites}

Experience Platformに正常に接続する前に完了する必要がある前提条件の設定については、この節を参照してください。

### IP アドレスの許可リスト {#allowlist}

ソースをExperience Platformに接続する前に、リージョン固有のIP アドレスをードに追加する必要があります。 詳しくは、[Experience PlatformへのIP アドレスの許可リストに加える](../../ip-address-allow-list.md)に関するガイドを参照してください。

### 必要な資格情報の収集 {#credentials}

OAuth2 クライアント資格情報を使用して[!DNL Salesforce Service Cloud] アカウントを接続するには、次の資格情報の値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| 環境 URL | [!DNL Salesforce Service Cloud] ソースインスタンスのURL。 |
| クライアント ID | クライアント IDは、OAuth2認証の一環として、クライアント秘密鍵と並行して使用されます。 クライアント IDとクライアント秘密鍵を組み合わせることで、アプリケーションを[!DNL Salesforce Service Cloud]に対して識別し、アカウントの代理でアプリケーションを操作できるようになります。 |
| クライアントシークレット | クライアント秘密鍵は、OAuth2認証の一環として、クライアント IDと並行して使用されます。 クライアント IDとクライアント秘密鍵を組み合わせることで、アプリケーションを[!DNL Salesforce Service Cloud]に対して識別し、アカウントの代理でアプリケーションを操作できるようになります。 |
| API バージョン | 使用している[!DNL Salesforce Service Cloud] インスタンスのREST API バージョン。 API バージョンの値は、10進数でフォーマットする必要があります。 例えば、API バージョン `52`を使用している場合、値を`52.0`として入力する必要があります。 このフィールドを空白のままにすると、Experience Platformは使用可能な最新バージョンを自動的に使用します。 |

[!DNL Salesforce Service Cloud]でのOAuthの使用について詳しくは、[[!DNL Salesforce Service Cloud] OAuth認証フローに関するガイド ](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5)を参照してください。

## APIを使用して[!DNL Salesforce Service Cloud]をExperience Platformに接続

- [Flow Service APIを使用したSalesforce Service Cloud ベース接続の作成](../../tutorials/api/create/customer-success/salesforce-service-cloud.md)
- [Flow Service API を使用したデータテーブルの探索](../../tutorials/api/explore/tabular.md)
- [Flow Service API を使用したカスタマーサクセスソースのデータフローの作成](../../tutorials/api/collect/customer-success.md)

## UIを使用して[!DNL Salesforce Service Cloud]をExperience Platformに接続する

- [UIでのSalesforce Service Cloud ソース接続の作成](../../tutorials/ui/create/customer-success/salesforce-service-cloud.md)
- [UI でのカスタマーサクセスソース接続のデータフローの作成](../../tutorials/ui/dataflow/customer-success.md)
