---
keywords: Experience Platform;ホーム;人気のトピック;ID 名前空間
solution: Experience Platform
title: ID サービストラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform ID サービスに関するよくある質問への回答のほか、一般的なエラーのトラブルシューティングガイドを示します。
exl-id: dac31bc3-7003-46d6-9d41-9f6fd3645c2c
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '2176'
ht-degree: 95%

---

# ID サービストラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Identity Service] に関するよくある質問への回答のほか、一般的なエラーのトラブルシューティングガイドを示します。[!DNL Platform] API 全般に関する質問とトラブルシューティングについては、[Adobe Experience Platform API トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

単一の顧客を識別するデータは、多くの場合、その顧客がブランドに関与するために使用する様々なデバイスやシステムにわたって断片化されています。[!DNL Identity Service] では、断片化されたこれらの ID をひとまとめにして、顧客の行動の完全な把握を促進するので、インパクトの強いデジタルエクスペリエンスをリアルタイムで提供できるようになります。詳しくは、「[ID サービスの概要](./home.md)」を参照してください。

## FAQ

[!DNL Identity Service] に関するよくある質問への回答のリストを以下に示します。

## ID データとは

ID データは、個人を識別するために使用できる任意のデータです。組織内でのデータの使用方法のコンテキストに応じて、ID データには、ユーザー名、電子メールアドレス、CRM システムの ID が含まれる場合があります。匿名ユーザーはデバイスや cookie ID で識別できるので、ID データは Web サイトやサービスの登録ユーザーに限定されません。

## データフィールドを ID としてラベル付けする利点は何ですか。

特定のデータフィールドをレコードおよび時系列データの ID としてラベル付けすると、データの自然構造内で ID の関係をマッピングし、重複データをチャネル間で調整できます。詳しくは、「[ID サービスの概要](./home.md)」を参照してください。

## 既知 ID と匿名 ID とは

既知の ID とは、個人の識別、個人への連絡または個人の所在の特定を行うために単独でまたは他の情報と共に使用できる ID 値を指します。既知の ID の例としては、メールアドレス、電話番号、CRM ID などがあります。

匿名 ID とは、個人の識別、個人への連絡または個人の所在の特定を行うために単独でまたは他の情報と共に使用することができない ID 値を指します（Cookie ID など）。

## プライベート ID グラフとは

プライベート ID グラフは、組織にのみ表示される、結合済み ID とリンクされた ID の間の関係を示すプライベートマップです。

ストリーミングエンドポイントから取り込まれたデータや [!DNL Identity Service] で使用できるようになっているデータセットに送信されたデータに複数の ID が含まれている場合、それらの ID はプライベート ID グラフにリンクされます。[!DNL Identity Service] では、このグラフを活用して特定の消費者またはエンティティの ID を収集するので、ID スティッチングとプロファイル結合が可能になります。

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

標準 ID 名前空間は、すべての組織で使用できる名前空間です。使用可能な標準名前空間の完全なリストについては、[ID 名前空間の概要](./namespaces.md)を参照してください。

## 組織で使用できる ID 名前空間のリストはどこで見つけられますか？

[ID サービス API](https://www.adobe.io/experience-platform-apis/references/identity-service) を使用して、`/idnamespace/identities` エンドポイントに GET リクエストをおこなうことで、組織で使用可能なすべての ID 名前空間をリストできます。詳しくは、「ID サービス API の概要」で[使用可能な名前空間のリスト](./api/list-namespaces.md)を参照してください。

## 組織のカスタム名前空間を作成する方法を教えてください。

[ID サービス API](https://www.adobe.io/experience-platform-apis/references/identity-service) を使用して `/idnamespace/identities` エンドポイントに POST リクエストをおこなうことで、組織のカスタム ID 名前空間を作成できます。詳しくは、「ID サービス API の概要」の[カスタム名前空間の作成](./api/create-custom-namespace.md)に関する節を参照してください。

## 複合 ID と XID とは何ですか。

ID は、API 呼び出しで複合 ID または XID によって参照されます。複合 ID は、ID 値と名前空間を含んだ ID を表します。XID は、複合 ID（ID および名前空間）と同じコンストラクトを表す単一値 ID で、ID サービスによって永続化されると、新しい ID に自動的に割り当てられます。詳しくは、「[ID サービス API の概要](./home.md)」を参照してください。

## ID サービスが個人を特定できる情報（PII）をどのように処理するのか教えてください。

ID サービスには、電話番号とメールのハッシュ化された ID 値の取り込みをサポートする標準名前空間があります。ただし、値のハッシュ化はユーザーが行う必要があります。Platform に取り込まれるデータのハッシュ化について詳しくは、[[!DNL Data Prep] マッピング関数ガイド](../data-prep/functions.md#hashing)を参照してください。

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

## ID グラフページや API にアクセスできないのはなぜですか？

ID グラフデータを表示するには、Platform 管理者から `view-identity-graph` 権限をプロビジョニングしてもらう必要があります。この権限がないと、ID グラフビューアページと、Platform API の呼び出し時に、権限が拒否されたというメッセージが表示されます。権限について詳しくは、[アクセス制御の概要](../access-control/home.md)を参照してください。

## トラブルシューティング

次の節では、[!DNL Identity Service] API の操作中に発生する可能性のある特定のエラーコードと予期しない動作に関するトラブルシューティングの推奨事項を示します。

## [!DNL Identity Service] のエラーメッセージ

[!DNL Identity Service] API の使用時に表示される可能性のあるエラーメッセージのリストを以下に示します。

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

[!DNL Identity Service] では、180 日より古いデータを消去します。このエラーメッセージは、これより古いデータにアクセスしようとすると表示されます。

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

このエラーメッセージは、リクエストパスで `graph-type` クエリーパラメーターに無効な値が与えられた場合に表示されます。サポートされているグラフタイプについては、[!DNL Identity Service] の概要の [ID グラフ](./home.md)の節を参照してください。

### サービストークンに有効なスコープがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Service token does not have valid scope. Either acp.core.identity or acp.foundation is required"
}
```

このエラーメッセージは、組織が [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### ゲートウェイサービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Gateway service token is not valid"
}
```

このエラーが発生した場合、アクセストークンは無効です。アクセストークンは 24 時間ごとに期限切れになるので、引き続き [!DNL Platform] API を使用するには再生成する必要があります。新しいアクセストークンの生成手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

### 認証サービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Authorization service token is not valid"
}
```

このエラーが発生した場合、アクセストークンは無効です。アクセストークンは 24 時間ごとに期限切れになるので、引き続き [!DNL Platform] API を使用するには再生成する必要があります。新しいアクセストークンの生成手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

### ユーザートークンに有効な製品コンテキストがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "User token does not have valid product context"
}
```

このエラーメッセージが表示されるのは、アクセストークンが [!DNL Experience Platform] 統合から生成されていない場合です。[!DNL Experience Platform] 統合用の新しいアクセストークンの生成手順については、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を参照してください。

### ID と名前空間コードからネイティブ XID を取得する際の内部エラー

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Invalid IMS Token/IMS Org | Internal error - when tried to get native XID from identity and namespace code"
}
```

[!DNL Identity Service] が識別情報を永続化する場合、その識別情報の ID および関連する名前空間 ID には、XID と呼ばれる一意の ID が割り当てられます。このメッセージが表示されるのは、特定の ID 値および名前空間に対応する XID を見つける過程でエラーが発生した場合です。

### IMS 組織は、[!DNL Identity Service] を使用できるようにプロビジョニングされていません

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "The IMS Org. {IMS_ORG_NAME} is not provisioned for Identity Service usage"
}
```

このエラーメッセージは、組織が [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### 内部サーバーエラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Server Error. There was a problem processing your request"
}
```

このエラーが表示されるのは、[!DNL Platform] サービス呼び出しの実行で予期しない例外が発生した場合です。ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。問題が解決しない場合は、システム管理者に問い合わせてください。

## バッチ取得エラーコード

[!DNL Identity Service] では、バッチ取り込みを使用して [!DNL Platform] にアップロードされたレコードデータや時系列データから ID データを取り込みます。バッチ取得は非同期的なプロセスなので、エラーを表示するには、バッチの詳細を表示する必要があります。バッチが完了するまで、バッチの進行に合わせてエラーが蓄積されます。

次に、 [!DNL Identity Service] を使用する際に [バッチ取得 API](https://developer.adobe.com/experience-platform-apis/references/batch-ingestion/).

### 不明な XDM スキーマ

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Unknown XDM schema"
}
```

[!DNL Identity Service] は、それぞれ [!DNL Profile] クラスまたは [!DNL ExperienceEvent] クラスに準拠するレコードまたは時系列データの ID のみを使用します。どちらのクラスにも準拠していない [!DNL Identity Service] のデータを取得しようとすると、このエラーが発生します。

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

[!DNL Identity Service] は、1 つのレコードに複数の ID 値が存在する場合にのみ ID をリンクします。このエラーメッセージは、取得されたバッチごとに 1 回ずつ発生し、1 つの ID しか見つからずに ID グラフに変更が生じなかったレコードの数を表示します。

### 名前空間コードがこの IMS 組織に登録されていません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Namespace Code {ERRONEOUS_CODE} is not registered for this IMS Org"
}
```

このエラーは、取り込んだレコードに関連付けられた名前空間が存在しないか、組織からアクセスできない ID が存在する場合に表示されます。

### IMS 組織がプライベート ID グラフ用にプロビジョニングされていないため、バッチ取得をスキップします

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "Skipping batch ingestion as IMS Org is not provisioned for Private Identity Graph"
}
```

バッチデータを取り込むと、このエラーメッセージは、組織が [!DNL Identity Service]. この問題を解決するには、システム管理者に問い合わせてください。

### 内部エラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Error. There was a problem during the ingestion"
}
```

このエラーは、バッチ取得中に予期しない例外が発生した場合に表示されます。ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。問題が解決しない場合は、システム管理者に問い合わせてください。
