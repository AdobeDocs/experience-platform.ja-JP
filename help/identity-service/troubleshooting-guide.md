---
keywords: Experience Platform;home;popular topics;identity namespace;Identity namespace
solution: Experience Platform
title: Adobe Experience Platform ID サービストラブルシューティングガイド
topic: troubleshooting
description: このドキュメントでは、Adobe Experience Platform ID サービスに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。
translation-type: tm+mt
source-git-commit: 04efbf63741ef39bbf0b22795be74087f1f7c595
workflow-type: tm+mt
source-wordcount: '2248'
ht-degree: 82%

---


# ID サービストラブルシューティングガイド

This document provides answers to frequently asked questions about Adobe Experience Platform [!DNL Identity Service], as well as a troubleshooting guide for common errors. For questions and troubleshooting regarding [!DNL Platform] APIs in general, see the [Adobe Experience Platform API troubleshooting guide](../landing/troubleshooting.md).

1 人の顧客を識別するデータは、多くの場合、顧客がブランドに関与するために使用する様々なデバイスやシステムにわたって断片化されます。[!DNL Identity Service] これらの断片化されたIDをまとめて収集し、顧客の行動を完全に把握し、効果的なデジタルエクスペリエンスをリアルタイムで提供できます。 詳しくは、「[ID サービスの概要](./home.md)」を参照してください。

## FAQ

The following is a list of answers to frequently asked questions about [!DNL Identity Service].

## ID データとは

ID データは、個人を識別するために使用できる任意のデータです。組織内でのデータの使用方法のコンテキストに応じて、ID データには、ユーザー名、電子メールアドレス、CRM システムの ID が含まれる場合があります。匿名ユーザーはデバイスや cookie ID で識別できるので、ID データは Web サイトやサービスの登録ユーザーに限定されません。

## データフィールドを ID としてラベル付けする利点は何ですか。

特定のデータフィールドをレコードおよび時系列データの ID としてラベル付けすると、データの自然構造内で ID の関係をマッピングし、重複データをチャネル間で調整できます。詳しくは、「[ID サービスの概要](./home.md)」を参照してください。

## 既知 ID と匿名 ID とは

既知 ID とは、個人を識別、連絡、または場所を特定するために、単独または他の情報と共に使用できる ID 値を指します。既知 ID の例としては、電子メールアドレス、電話番号、CRM ID などがあります。

匿名 ID とは、個人を識別、連絡、または場所を特定するために、単独で、または他の情報と共に使用できない ID 値を指します（cookie ID など）。

## プライベート ID グラフとは

プライベート ID グラフは、組織にのみ表示される、結合済み ID とリンクされた ID の間の関係を示すプライベートマップです。

When more than one identity is included in any data ingested from a streaming endpoint or sent to a dataset enabled for [!DNL Identity Service], those identities are linked in the Private Identity Graph. [!DNL Identity Service] このグラフを利用して、特定のコンシューマーまたはエンティティのIDを収集し、IDのステッチとプロファイルの結合を可能にします。

## XDM スキーマ内に複数の ID フィールドを作成する方法を教えてください。

[エクスペリエンスデータモデル（XDM）](../xdm/home.md)スキーマは、複数の ID フィールドをサポートします。XDM Individual Profile または XDM ExperienceEvent クラスを実装するスキーマ内の任意の `string` 型のデータフィールドは、ID フィールドとしてラベル付けできます。ラベルが付けられると、これらのフィールドに含まれるすべてのデータがプロファイルの ID マップに追加されます。

ユーザーインターフェイスを使用して XDM フィールドに ID フィールドのラベルを付ける手順については、『スキーマエディタのチュートリアル』の「[ID](../xdm/tutorials/create-schema-ui.md)」節を参照してください。API を使用している場合は、『スキーマレジストリ API のチュートリアル』の「[ID 記述子](../xdm/tutorials/create-schema-api.md)」の節を参照してください。

## 一部のフィールドをコンテキストとしてラベル付けしないようにする必要はありますか。

「ID」フィールドは、個々のユーザーに固有の値のために予約する必要があります。例えば、顧客のロイヤリティープログラムのデータセットを考えてみましょう。「ロイヤリティーレベル」フィールド（ゴールド、シルバー、ブロンズ）は有用な ID フィールドではありませんが、一意の値であるロイヤリティー ID は有用な ID フィールドです。

郵便番号や IP アドレスなどのフィールドは、値が複数の個人に適用される場合があるので、個人の ID としてラベル付けできません。これらのタイプのフィールドは、世帯レベルのマーケティング戦略の ID としてのみラベル付けする必要があります。

## ID フィールドが期待どおりにリンクされないのはなぜですか。

ID サービス API の[`/cluster/members`エンドポイント](./api/list-cluster-identites.md)を使用して、1 つ以上の ID フィールドに関連付けられた ID を表示できます。応答が期待するリンクされた ID を返さない場合は、XDM データに適切な ID 情報を提供していることを確認します。詳しくは、「ID サービスの概要」の「[XDM データの ID サービスへの提供](./home.md)」の節を参照してください。

## ID 名前空間とは

ID 名前空間は、ID フィールドと顧客の ID との関係を示すコンテキストを提供します。例えば、「Email」名前空間の下の ID フィールドは標準的な電子メールの形式（name<span>@emailprovider.com）に準拠し、「Phone」名前空間を使用するフィールドは標準電話番号（北米では 987-555-1234 など）に準拠する必要があります。

名前空間は、異なる CRM システム間で類似した ID 値を区別します。例えば、企業の報酬プログラムに関連付けられた数値のロイヤリティー ID を含むプロファイルを考えてみます。「Loyalty」名前空間では、同じプロファイルにも表示される e コマースシステムの類似した数値 ID からこの値が分離されます。

詳しくは、「[ID 名前空間の概要](./home.md)」を参照してください。

## ID を ID 名前空間に関連付ける方法を教えてください。

ID フィールドは、作成時に既存の ID 名前空間に関連付ける必要があります。新しい名前空間は、[API を使用して作成](#how-do-i-create-a-custom-namespace-for-my-organization)してから、ID フィールドに関連付ける必要があります。

API を使用して ID 記述子を作成する際に名前空間を定義する手順については、『スキーマレジストリ開発者ガイド』の「[記述子の作成](../xdm/tutorials/create-schema-ui.md)」の節を参照してください。UI でスキーマフィールドを ID としてマークする場合は、『[スキーマエディタのチュートリアル](../xdm/tutorials/create-schema-api.md)』の手順に従います。

## Experience Platform が提供する標準 ID 名前空間は何ですか。 {#standard-namespaces}

次の標準名前空間は、Experience Platform 内のすべての組織で使用できます。

| 表示名 | ID | コード | 説明 |
| ------------ | --- | --- | ----------- |
| CORE | 0 | CORE | レガシー名：「Adobe AudienceManager」 |
| ECID | 4 | ECID | エイリアス：「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」 |
| Email | 6 | Email |  |
| 電子メール（SHA256、小文字） | 11 | Emails | 事前にハッシュ化された電子メール用標準名前空間。この名前空間で指定された値は、SHA-256 でハッシュする前に小文字に変換されます。 |
| Phone | 7 | Phone |  |
| Windows AID | 8 | WAID |  |
| AdCloud | 411 | AdCloud | エイリアス：Ad Cloud |
| Adobe Target | 9 | TNTID | ターゲット ID |
| Google 広告 ID | 20914 | GAID | GAID |
| Apple IDFA | 20915 | IDFA | 広告主の ID |

## 組織で使用できる ID 名前空間のリストはどこで見つけられますか？

[ID サービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml) を使用して、`/idnamespace/identities` エンドポイントに GET リクエストをおこなうことで、組織で使用可能なすべての ID 名前空間をリストできます。詳しくは、「ID サービス API の概要」で[使用可能な名前空間のリスト](./api/list-namespaces.md)を参照してください。

## 組織のカスタム名前空間を作成する方法を教えてください。

[ID サービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml) を使用して `/idnamespace/identities` エンドポイントに POST リクエストをおこなうことで、組織のカスタム ID 名前空間を作成できます。詳しくは、「ID サービス API の概要」の[カスタム名前空間の作成](./api/create-custom-namespace.md)に関する節を参照してください。

## 複合 ID と XID とは何ですか。

ID は、API 呼び出しで複合 ID または XID によって参照されます。**複合 ID** は、ID 値と名前空間を含む ID を表します。**XID** は、複合 ID（ID および名前空間）と同じ構成を表す単一値 ID で、ID サービスによって永続化されると、新しい ID に自動的に割り当てられます。詳しくは、「[ID サービス API の概要](./home.md)」を参照してください。

## ID サービスが個人を特定できる情報（PII）をどのように処理するのか教えてください。

ID サービスは、値を永続化する前に、PII の強力で一方向の暗号化ハッシュを作成します。「Phone」名前空間と「Email」名前空間の ID データは、SHA-256 を使用して自動的にハッシュ化され、「Email」値はハッシュ前に自動的に小文字に変換されます。

## Platform に送信する前に、すべての PII を暗号化する必要がありますか。

Platform に取得する前に、PII データを手動で暗号化する必要はありません。Platform は、`I1` データ使用ラベルを適用可能なすべてのデータフィールドに適用することで、取得時にこれらのフィールドをハッシュ ID 値に自動的に変換します。

データ使用ラベルを適用および管理する手順については、『[データ使用ラベルのチュートリアル](../data-governance/labels/user-guide.md)』を参照してください。

## PII ベースの ID をハッシュする際に考慮すべき点はありますか。

ハッシュ化された PII 値を ID サービスに送信する場合は、データセット全体で同じ暗号化方式を使用する必要があります。これにより、データセット間で同じ ID 値が同じハッシュ値を生成し、ID グラフで適切に一致およびリンクできるようになります。

<!-- Documentation does not show any methods of editing the identityMap directly, and this table never overtly recommends using identityMap anyway. This should probably be removed unless PM thinks otherwise. -->
<!-- ## When should I use the Identity map rather than labeling individual XDM schema fields?

The following table describes when the recommended approach for including identity data in your XDM would be identity map and when an identity field is the better method.

>[!NOTE]
>
>An advantage `identityMap` has is the ability to include multiple identity values for a single namespace.

Write|XDM identity field|`identityMap`
---|---|---
UI|Supported|Supported
Developer|Recommended|Supported
ETL|Recommended|Avoid - While this is supported, data should be formatted naturally when using an ETL, favoring identity fields over `identityMap`.
Internal solutions|Preferred|Common

--- -->

## トラブルシューティング

The following section provides troubleshooting suggestions for specific error codes and unexpected behavior you may encounter while working with the [!DNL Identity Service] API.

## [!DNL Identity Service] エラーメッセージ

The following is a list of error messages you may encounter when using the [!DNL Identity Service] API.

### 必要なクエリーパラメーターがありません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Missing required query parameter - namespace"
}
```

このエラーは、リクエストされたクエリーパラメータがリクエストパスに含まれていない場合に表示されます。エラーメッセージの `detail` に、見つからないパラメーターの名前が表示されます。このエラーメッセージには、次のようなバリエーションがあります。

- 必要なクエリーパラメーターがありません — nsId
- 必要なクエリーパラメーターがありません — id
- 必要なクエリーパラメーターがありません — xid または（nsid,id）
- 必要なクエリーパラメーターがありません — targetNs
- 必要なクエリーパラメーターがありません — xids または compositeXids

再試行する前に、指定したパラメーターがリクエストパスに正しく含まれていることを確認してください。

### タイムスタンプは過去 180 日以内にする必要があります

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Timestamp should be within last 180 days"
}
```

[!DNL Identity Service] 180日を超える古いデータを削除します。 このエラーメッセージは、これより古いデータにアクセスしようとすると表示されます。

### 1 回の呼び出しで使用できる XID は 1,000 個までです

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit of 1000 XIDs in a single call"
}
```

このエラーメッセージは、1 回の API 呼び出しで許可される [XID](#what-are-composite-identities-and-xids) の最大数を超える ID 情報を取得しようとすると表示されます。この問題を解決するには、リクエストの XID の数を表示された上限を下回るように減らします。


### 1 回の呼び出しで使用できる compositeXids は 1,000 個までです

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit for 1000 compositeXids in a single call"
}
```

このエラーメッセージは、1 回の API 呼び出しで許可される[複合 ID](#what-are-composite-identities-and-xids) の最大数を超える ID 情報を取得しようとすると表示されます。この問題を解決するには、リクエスト内の複合 ID の数を、表示される制限を下回るように減らします。

### 指定されたグラフの種類は無効です

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "The graph-type abc specified is invalid. Please provide a valid graph-type"
}
```

このエラーメッセージは、リクエストパスで `graph-type` クエリーパラメーターに無効な値が与えられた場合に表示されます。See the section on [identity graphs](./home.md) in the [!DNL Identity Service] overview to learn which graph-types are supported.

### サービストークンに有効なスコープがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Service token does not have valid scope. Either acp.core.identity or acp.foundation is required"
}
```

This error message displays when your IMS Organization has not been provisioned with the proper permissions for [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### ゲートウェイサービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Gateway service token is not valid"
}
```

このエラーが発生した場合、アクセストークンは無効です。Access tokens expire every 24 hours and must be regenerated to continue using [!DNL Platform] APIs. 新しいアクセストークンの生成手順については、[認証に関するチュートリアル](../tutorials/authentication.md)を参照してください。

### 認証サービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Authorization service token is not valid"
}
```

このエラーが発生した場合、アクセストークンは無効です。Access tokens expire every 24 hours and must be regenerated to continue using [!DNL Platform] APIs. 新しいアクセストークンの生成手順については、[認証に関するチュートリアル](../tutorials/authentication.md)を参照してください。

### ユーザートークンに有効な製品コンテキストがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "User token does not have valid product context"
}
```

This error message displays when your access token has not been generated from an [!DNL Experience Platform] integration.  統合のための新しいアクセストークンの生成手順については、[認証に関するチュートリアル](../tutorials/authentication.md)を参照してください。[!DNL Experience Platform]

### ID とネイティブ XID を取得する際の内部エラーと名前空間コード

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Invalid IMS Token/IMS Org | Internal error - when tried to get native XID from identity and namespace code"
}
```

When [!DNL Identity Service] persists an identity, the identity&#39;s ID and associated namespace ID are assigned a unique identifier called an XID. このメッセージは、特定の ID 値と名前空間の XID を見つけるプロセス中にエラーが発生した場合に表示されます。

### The IMS Org is not provisioned for [!DNL Identity Service] usage

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "The IMS Org. {IMS_ORG_NAME} is not provisioned for Identity Service usage"
}
```

This error message displays when your IMS Organization has not been provisioned with the proper permissions for [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### 内部サーバーエラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Server Error. There was a problem processing your request"
}
```

This error displays when an unexpected exception occurs in the execution of a [!DNL Platform] service call. ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。問題が解決しない場合は、システム管理者に問い合わせてください。

## バッチ取得エラーコード

[!DNL Identity Service] バッチインジェストを [!DNL Platform] 使用してアップロードされたレコードおよび時系列データからIDデータを取り込みます。 バッチ取得は非同期的なプロセスなので、エラーを表示するには、バッチの詳細を表示する必要があります。バッチが完了するまで、バッチの進行に合わせてエラーが蓄積されます。

The following is a list of error messages related to [!DNL Identity Service] you may encounter when using the [Data Ingestion API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml).

### 不明な XDM スキーマ

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Unknown XDM schema"
}
```

[!DNL Identity Service] IDは、それぞれまたはクラスに適合するレコードまたは時系列データに対して [!DNL Profile] のみ [!DNL ExperienceEvent] 使用します。 Attempting to ingest data for [!DNL Identity Service] that does not adhere to either class will trigger this error.

### 処理されたバッチの最初の 100 行に 0 個の有効な ID がありました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There were 0 valid identities in the first 100 rows of the processed batch"
}
```

このエラーは、バッチの最初の 100 行に ID が見つからなかった場合に表示されます。ただし、このエラーは、後続のレコードで ID が見つからなかったことを最終的に示すものではありません。

### XDM レコード 1 つにつき ID が 1 つしかないので、レコードをスキップしました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Skipped {NUMBER_OF_RECORDS} records as they had only 1 identity per XDM record"
}
```

[!DNL Identity Service] 単一のレコードに2つ以上のID値が存在する場合は、IDのみがリンクされます。 このエラーメッセージは、取得されたバッチごとに 1 回ずつ発生し、1 つの ID しか見つからずに ID グラフに変更が生じなかったレコードの数を表示します。

### 名前空間コードがこの IMS 組織に登録されていません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Namespace Code {ERRONEOUS_CODE} is not registered for this IMS Org"
}
```

このエラーは、取得したレコードに関連付けられた名前空間が存在しないか、IMS 組織からアクセスできない ID が存在する場合に表示されます。

### IMS 組織がプライベート ID グラフ用にプロビジョニングされていないため、バッチ取得をスキップします

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "Skipping batch ingestion as IMS Org is not provisioned for Private Identity Graph"
}
```

When ingesting batch data, this error message displays when your IMS Organization has not been provisioned with the proper permissions for [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### 内部エラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Error. There was a problem during the ingestion"
}
```

このエラーは、バッチ取得中に予期しない例外が発生した場合に表示されます。ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。問題が解決しない場合は、システム管理者に問い合わせてください。
