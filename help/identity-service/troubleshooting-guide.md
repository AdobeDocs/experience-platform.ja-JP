---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Identity Serviceトラブルシューティングガイド
topic: troubleshooting
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53

---


# IDサービストラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform Identity Serviceに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 プラットフォームAPIに関する一般的な質問とトラブルシューティングについては、 [Adobe Experience Platform APIのトラブルシューティングガイドを参照してくださ](../landing/troubleshooting.md)い。

1人の顧客を識別するデータは、多くの場合、顧客がブランドに関与するために使用する様々なデバイスやシステムにわたって断片化されます。 **IDサービスは** 、これらの断片化されたIDをまとめて収集し、顧客の行動を完全に理解し、効果的なデジタルエクスペリエンスをリアルタイムで提供します。 詳しくは、「IDサービスの概要」を [参照してください](./home.md)。

## FAQ

次に、IDサービスに関するよくあるリストを示します。

## IDデータとは

IDデータは、個人を識別するために使用できる任意のデータです。 組織内でのデータの使用方法のコンテキストに応じて、IDデータには、CRMシステムのユーザー名、電子メールアドレス、IDが含まれる場合があります。 匿名ユーザーはデバイスやcookie IDで識別できるので、IDデータはWebサイトやサービスの登録ユーザーに限定されません。

## データフィールドをIDとしてラベル付けする利点は何ですか。

特定のデータフィールドをレコードおよび時系列データのIDとしてラベル付けすると、データの自然構造内でIDの関係をマッピングし、重複データをチャネル間で調整できます。 See the [Identity Service overview](./home.md) for more information.

## 既知のIDと匿名IDとは

既知 **のIDとは** 、個人を識別、連絡、または特定するために、個人または他の情報と共に使用できるID値を指します。 既知のIDの例としては、電子メールアドレス、電話番号、CRM IDなどがあります。

匿名 **IDとは** 、個人（cookie IDなど）を識別、連絡、または特定するために単独で、または他の情報と共に使用できないID値を指します。

## プライベートIDグラフとは

プライベートIDグラフは、組織にのみ表示される、ステッチ済みIDとリンクされたIDの間の関係のプライベートマップです。

ストリーミングエンドポイントから取り込んだデータや、IDサービスが有効なデータセットに送信されたデータに複数のIDが含まれる場合、それらのIDはプライベートIDグラフにリンクされます。 IDサービスは、このグラフを利用して特定のコンシューマーまたはエンティティのIDを収集し、IDのステッチとプロファイルの結合を可能にします。

## XDMスキーマ内に複数のIDフィールドを作成する方法

[エクスペリエンスデータモデル(XDM)スキーマは](../xdm/home.md) 、複数のIDフィールドをサポートします。 XDM IndividualプロファイルまたはXDM ExperienceEventクラスを `string` 実装するスキーマ内の任意のタイプのデータフィールドは、IDフィールドとしてラベル付けできます。 ラベルが付けられると、これらのフィールドに含まれるすべてのデータがプロファイルのIDマップに追加されます。

ユーザーインターフェイスを使用してXDMフィールドにIDフィールドのラベルを付ける手順については、スキーマエディタのチュートリアルの [Identity](../xdm/tutorials/create-schema-ui.md) (ID)セクションを参照してください。 APIを使用している場合は、「 [Identity Registry API Tutorial](../xdm/tutorials/create-schema-api.md) 」の「Identity Descriptor」の節を参照してください。

## 一部のフィールドをコンテキストとしてラベル付けしないようにする必要があるか。

「ID」フィールドは、個々のユーザーに固有の値用に予約する必要があります。 例えば、顧客忠誠度データセットを考えてみましょう。プログラム 「忠誠度レベル」フィールド（金、銀、青銅）は有用なIDフィールドではありませんが、一意の値である忠誠度IDは有用なIDフィールドです。

郵便番号やIPアドレスなどのフィールドは、個人のIDとしてラベル付けしないでください。これらの値は複数の個人に適用される場合があります。 これらのタイプのフィールドは、家庭レベルのマーケティング戦略のIDとしてのみラベル付けする必要があります。

## IDフィールドが期待どおりにリンクされないのはなぜですか。

IDサービス [`/cluster/members` APIのエンドポイント](./api/list-cluster-identites.md) を使用して、1つ以上のIDフィールドに関連付けられたIDを表示できます。 応答が期待するリンクされたIDを返さない場合は、XDMデータに適切なID情報を提供していることを確認します。 詳しくは、IDサービスの概 [要のXDMデータをIDサービスに提供する](./home.md) 「」の節を参照してください。

## ID名前空間とは

ID名前空間は、IDフィールドと顧客のIDとの関係を示すコンテキストを提供します。 例えば、「Email」名前空間の下のIDフィールドは標準E メールフォーマット(name<span>@emailprovider.com)に準拠し、「Phone」名前空間を使用するフィールドは標準電話番号（北米では987-555-1234など）に準拠する必要があります。

名前空間は、異なるCRMシステム間で類似したID値を区別します。 例えば、プロファイルの報酬会社に関連付けられた数値の忠誠度IDを含むプログラムを考えます。 「忠誠度」の名前空間では、同じプロファイルにも表示されるeコマースシステムの類似した数値IDからこの値が分離されます。

See the [identity namespace overview](./home.md) for more information.

## IDをID名前空間に関連付ける方法

「ID」フィールドは、作成時に既存のID名前空間に関連付ける必要があります。 新しい名前空間は、APIを使 [用して作成してから](#how-do-i-create-a-custom-namespace-for-my-organization) 、IDフィールドに関連付ける必要があります。

APIを使用してID記述子を作成する際に名前空間を定義する手順については、スキーマレジストリ開発者ガイドの記述子の作成に関する [節](../xdm/tutorials/create-schema-ui.md) を参照してください。 UIでスキーマフィールドをIDとしてマークする場合は、 [スキーマエディタの手順に従います](../xdm/tutorials/create-schema-api.md)。

## Experience Platformが提供する標準ID名前空間は何ですか。

次の標準名前空間は、Experience Platform内のすべての組織で使用できるように用意されています。

| 表示名 | ID | コード | 説明 |
| ------------ | --- | --- | ----------- |
| コア | 0 | コア | レガシー名：「Adobe AudienceManager」 |
| ECID | 4 | ECID | alias:&quot;Adobe Marketing Cloud ID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience Platform ID&quot; |
| 電子メール | 6 | 電子メール |  |
| 電子メール（SHA256、小文字） | 11 | 電子メール | 標準名前空間（事前にハッシュ化された電子メール用）。 この名前空間で指定された値は、SHA-256でハッシュする前に小文字に変換されます。 |
| 電話番号 | 7 | 電話番号 |  |
| Windows AID | 8 | WAID |  |
| AdCloud | 411 | AdCloud | alias:広告クラウド |
| Adobe Target | 9 | TNTID | ターゲットID |
| Google広告ID | 20914 | ガイド | ガイド |
| Apple IDFA | 20915 | IDFA | 広告主のID |

## 組織で使用できるID名前空間のリストはどこで見つけられますか？

IDサービ [スAPIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)、エンドポイントにGET要求を行うことで、組織で使用可能なすべてのID名前空間をリストで `/idnamespace/identities` きます。 詳しくは、IDサービスAPI [の概要で使用可能な名前空間](./api/list-namespaces.md) のリストを参照してください。

## 組織のカスタム名前空間の作成方法

IDサービ [スAPIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)、エンドポイントにPOSTリクエストを行うことで、組織のカスタムID名前空間を作成で `/idnamespace/identities` きます。 詳しくは、IDサービスAPI [の概要でのカスタム名前空間](./api/create-custom-namespace.md) の作成に関する節を参照してください。

## 複合IDとXIDとは

IDは、API呼び出しで複合IDまたはXIDによって参照されます。 複合 **IDは** 、ID値と名前空間を含むIDを表します。 **XID** は、複合ID(IDと名前空間)と同じ構成体を表す単一値の識別子で、IDサービスで保持される場合は新しいIDに自動的に割り当てられます。 詳しくは、 [IDサービスAPIの概要](./home.md) （英語のみ）を参照してください。

## IDサービスは個人を特定できる情報(PII)をどのように処理しますか。

IDサービスは、値を永続化する前に、PIIの強力で一方向の暗号化ハッシュを作成します。 「電話」名前空間と「電子メール」データのIDデータは、SHA-256を使用して自動的にハッシュ化され、「電子メール」値はハッシュ前に自動的に小文字に変換されます。

## プラットフォームに送信する前に、すべてのPIIを暗号化する必要がありますか。

PIIデータをプラットフォームに取り込む前に、PIIデータを手動で暗号化する必要はありません。 Platformは、データ使用ラ `I1` ベルを適用可能なすべてのデータフィールドに適用することで、取り込み時にこれらのフィールドをハッシュID値に自動的に変換します。

データ使用量ラベルを適用および管理する手順については、「データ使用量ラベルのチュートリ [アル」を参照してくださ](../data-governance/labels/user-guide.md)い。

## PIIベースのIDをハッシュする際に考慮すべき点は何か。

ハッシュ化されたPII値をIDサービスに送信する場合は、データセット全体で同じ暗号化方式を使用する必要があります。 これにより、データセット間で同じID値が同じハッシュ値を生成し、IDグラフで適切に一致およびリンクできるようになります。

<!-- Documentation does not show any methods of editing the identityMap directly, and this table never overtly recommends using identityMap anyway. This should probably be removed unless PM thinks otherwise. -->
<!-- ## When should I use the Identity map rather than labeling individual XDM schema fields?

The following table describes when the recommended approach for including identity data in your XDM would be identity map and when an identity field is the better method.

>[!NOTE] An advantage `identityMap` has is the ability to include multiple identity values for a single namespace.

Write|XDM identity field|`identityMap`
---|---|---
UI|Supported|Supported
Developer|Recommended|Supported
ETL|Recommended|Avoid - While this is supported, data should be formatted naturally when using an ETL, favoring identity fields over `identityMap`.
Internal solutions|Preferred|Common

--- -->

## トラブルシューティング

次の節では、特定のエラーコードと、IDサービスAPIの操作中に発生する可能性のある予期しない動作に関するトラブルシューティングの推奨事項を示します。

## IDサービスのエラーメッセージ

次に、Identity Service APIの使用時に発生する可能性のあるエラーリストを示します。

### 必要なクエリパラメータがありません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Missing required query parameter - namespace"
}
```

このエラーは、要求されたクエリパラメータが要求パスに含まれていない場合に表示されます。 エラーメ `detail` ッセージのに、見つからないパラメータの名前が表示されます。 このエラーメッセージには、次のようなものがあります。

- 必要なクエリパラメータがありません — nsId
- 必要なクエリパラメーターがありません — id
- 必要なクエリパラメーターが見つかりません — xidまたは(nsid,id)
- 必要なクエリパラメーターがありません — targetNs
- 必要なクエリパラメーターが見つかりません — xidsまたはcompositeXids

再試行する前に、指定したパラメーターがリクエストパスに正しく含まれていることを確認してください。

### タイムスタンプは過去180日以内にする必要があります

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Timestamp should be within last 180 days"
}
```

IDサービスは180日を超えるデータを削除します。 このエラーメッセージは、これより古いデータにアクセスしようとすると表示されます。

### 1回の呼び出しで1,000個のXIDが制限されます

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit of 1000 XIDs in a single call"
}
```

このエラーメッセージは、1回のAPI呼び出しで許可されるXIDの最大数を超えるID情報を取得しよ [うとす](#what-are-composite-identities-and-xids) ると表示されます。 この問題を解決するには、リクエストのXIDの数を表示される制限を下回るように減らします。


### 1回の呼び出しで1,000個のcompositeXidに制限があります

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit for 1000 compositeXids in a single call"
}
```

このエラーメッセージは、1回のAPI呼び出しで許可される複合IDの最大数を超えるID [情報を取得しよ](#what-are-composite-identities-and-xids) うとすると表示されます。 この問題を解決するには、リクエスト内の複合IDの数を、表示される制限を下回るまで減らします。

### 指定されたグラフの種類は無効です

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "The graph-type abc specified is invalid. Please provide a valid graph-type"
}
```

このエラーメッセージは、 `graph-type` クエリパラメーターに無効な値が要求パスで与えられた場合に表示されます。 サポートされているグ [ラフのタイプについては](./home.md) 、「IDサービスの概要」の「IDグラフ」の節を参照してください。

### サービストークンに有効なスコープがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Service token does not have valid scope. Either acp.core.identity or acp.foundation is required"
}
```

このエラーメッセージは、IMS組織がIDサービスに対する適切な権限を持つプロビジョニングされていない場合に表示されます。 この問題を解決するには、システム管理者に問い合わせてください。

### ゲートウェイサービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Gateway service token is not valid"
}
```

このエラーの場合、アクセストークンは無効です。 アクセストークンは24時間ごとに期限が切れ、引き続きプラットフォームAPIを使用するには再生成する必要があります。 新しい認証の生 [成手順については](../tutorials/authentication.md) 、認証のチュートリアルを参照してください。アクセストークン

### 認証サービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Authorization service token is not valid"
}
```

このエラーの場合、アクセストークンは無効です。 アクセストークンは24時間ごとに期限が切れ、引き続きプラットフォームAPIを使用するには再生成する必要があります。 新しい認証の生 [成手順については](../tutorials/authentication.md) 、認証のチュートリアルを参照してください。アクセストークン

### ユーザートークンに有効な製品コンテキストがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "User token does not have valid product context"
}
```

このエラーメッセージは、アクセストークンがExperience Platform統合から生成されていない場合に表示されます。 Experience Platform統合用の新し [いアクセストークンの生成手順については](../tutorials/authentication.md) 、認証チュートリアルを参照してください。

### IDとネイティブXIDを取得する際の内部エラーと名前空間コード

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Invalid IMS Token/IMS Org | Internal error - when tried to get native XID from identity and namespace code"
}
```

IDサービスがIDを保持する場合、IDのIDと関連付けられた名前空間IDには、XIDと呼ばれる一意の識別子が割り当てられます。 このメッセージは、特定のID値と名前空間のXIDを見つけるプロセス中にエラーが発生した場合に表示されます。

### IMS組織は、IDサービスの使用のためにプロビジョニングされていません

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "The IMS Org. {IMS_ORG_NAME} is not provisioned for Identity Service usage"
}
```

このエラーメッセージは、IMS組織がIDサービスに対する適切な権限を持つプロビジョニングされていない場合に表示されます。 この問題を解決するには、システム管理者に問い合わせてください。

### 内部サーバーエラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Server Error. There was a problem processing your request"
}
```

このエラーは、Platformサービス呼び出しの実行で予期しない例外が発生した場合に表示されます。 ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。 問題が解決しない場合は、システム管理者に問い合わせてください。

## バッチ取り込みのエラーコード

IDサービスは、バッチインジェストを使用してプラットフォームにアップロードされたレコードや時系列のデータからIDデータを取り込みます。 バッチ取り込みは非同期的なプロセスなので、バッチの詳細を表示エラーに表示する必要があります。 バッチが完了するまで、バッチの進行に合わせてエラーが蓄積されます。

以下は、 [Data Ingestion APIの使用時に発生する可能性のあるIDサービスに関するエラーメッセージのリストです](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。

### 不明なXDMスキーマ

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Unknown XDM schema"
}
```

IDサービスは、それぞれExperienceEventクラスまたはExperienceEventクラスに準拠するレコードデータまたは時系列プロファイルのIDのみを使用します。 どちらのクラスにも準拠していないIDサービスのデータを取り込もうとすると、このエラーが発生します。

### 処理されたバッチの最初の100行に0個の有効なIDがありました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There were 0 valid identities in the first 100 rows of the processed batch"
}
```

このエラーは、バッチの最初の100行にIDが表示されない場合に表示されます。 ただし、このエラーは、後続のレコードでIDが見つからなかったことを最終的に示すものではありません。

### XDMレコード1つにつきIDが1つしかないので、レコードをスキップしました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Skipped {NUMBER_OF_RECORDS} records as they had only 1 identity per XDM record"
}
```

IDサービスは、1つのレコードに複数のID値が存在する場合にのみIDをリンクします。 このエラーメッセージは、取り込まれたバッチごとに1回ずつ発生し、1つのIDしか見つからず、IDグラフに変更が生じなかったレコードの数を表示します。

### 名前空間コードがこのIMS組織に登録されていません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Namespace Code {ERRONEOUS_CODE} is not registered for this IMS Org"
}
```

このエラーは、取り込んだレコードに関連付けられた名前空間が存在しないか、IMS組織からアクセスできないIDが存在する場合に表示されます。

### IMS組織がプライベートIDグラフ用にプロビジョニングされていないため、バッチ取り込みをスキップします

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "Skipping batch ingestion as IMS Org is not provisioned for Private Identity Graph"
}
```

バッチデータを取り込むと、IMS組織がIDサービスに対する適切な権限を持つプロビジョニングされていない場合に、このエラーメッセージが表示されます。 この問題を解決するには、システム管理者に問い合わせてください。

### 内部エラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Error. There was a problem during the ingestion"
}
```

このエラーは、バッチ取り込み中に予期しない例外が発生した場合に表示されます。 ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに一定の時間間隔でリクエストを数回再試行することです。 問題が解決しない場合は、システム管理者に問い合わせてください。
