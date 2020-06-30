---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience PlatformIDサービストラブルシューティングガイド
topic: troubleshooting
translation-type: tm+mt
source-git-commit: 6ffdcc2143914e2ab41843a52dc92344ad51bcfb
workflow-type: tm+mt
source-wordcount: '2225'
ht-degree: 1%

---


# IDサービストラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問への回答 [!DNL Identity Service]と、一般的なエラーのトラブルシューティングガイドを提供します。 APIに関する一般的な質問とトラブルシューティングについては、 [!DNL Platform] Adobe Experience PlatformAPIトラブルシューティングガイドを参照してください [](../landing/troubleshooting.md)。

1人の顧客を識別するデータは、多くの場合、自社ブランドとの関わり合いに使用する様々なデバイスやシステムに断片化されます。 [!DNL Identity Service] これらの断片化されたIDをまとめて収集し、顧客の行動を完全に把握し、効果的なデジタルエクスペリエンスをリアルタイムで提供できます。 詳しくは、「 [IDサービスの概要](./home.md)」を参照してください。

## FAQ

次に、に関するよくある質問への回答のリストを示し [!DNL Identity Service]ます。

## IDデータとは

IDデータは、個人の特定に使用できる任意のデータです。 組織内でのデータの使用方法のコンテキストに応じて、IDデータには、ユーザー名、電子メールアドレス、CRMシステムのIDが含まれます。 匿名ユーザーはデバイスやcookie IDで識別できるので、IDデータはWebサイトやサービスの登録ユーザーに制限されません。

## データフィールドをIDとしてラベル付けするメリットは何ですか。

特定のデータフィールドをレコードおよび時系列データのIDとしてラベル付けすると、データの自然構造内でIDの関係をマッピングし、重複データをチャネル間で調整できます。 See the [Identity Service overview](./home.md) for more information.

## 匿名IDとは何ですか。

既知のIDとは、個人を識別、連絡、または特定するために、その個人または他の情報と共に使用できるID値のことです。 既知のIDの例としては、電子メールアドレス、電話番号、CRM IDなどがあります。

匿名IDとは、個人（cookie IDなど）を識別、連絡、または特定するために、その匿名ID自体または他の情報と共に使用できないID値を指します。

## プライベートIDグラフとは

プライベートIDグラフは、ステッチされたIDとリンクされたIDの間の関係のプライベートマップで、組織にのみ表示されます。

ストリーミングエンドポイントから取り込まれたデータ、または有効なデータセットに送信されたデータに複数のIDが含まれている場合 [!DNL Identity Service]、それらのIDはプライベートIDグラフにリンクされます。 [!DNL Identity Service] このグラフを利用して、特定のコンシューマーまたはエンティティのIDを収集し、IDのステッチとプロファイルの結合を可能にします。

## XDMスキーマ内に複数のIDフィールドを作成する方法を教えてください。

[エクスペリエンスデータモデル(XDM)](../xdm/home.md) スキーマは、複数のIDフィールドをサポートしています。 XDM IndividualプロファイルまたはXDM ExperienceEventクラスを実装するスキーマ内の任意のタイプのデータフィールドは、IDフィールドとしてラベル付けできます。 `string` ラベル付けが行われると、これらのフィールドに含まれるすべてのデータがプロファイルのIDマップに追加されます。

ユーザーインターフェイスを使用してXDMフィールドにIDフィールドのラベルを付ける手順については、スキーマエディタのチュートリアルの [IDセクション](../xdm/tutorials/create-schema-ui.md) を参照してください。 APIを使用している場合は、「スキーマレジストリAPIチュートリアル [」の](../xdm/tutorials/create-schema-api.md) ID記述子の節を参照してください。

## 一部のフィールドにIDのラベルを付けるべきでないコンテキストがあるか。

IDフィールドは、個々のユーザーに固有の値に対して予約する必要があります。 例えば、顧客忠誠度プログラムのデータセットを考えてみましょう。 「忠誠度レベル」フィールド（ゴールド、シルバー、ブロンズ）は有用なIDフィールドではありませんが、一意の値である忠誠度IDは有用なIDフィールドです。

郵便番号やIPアドレスなどのフィールドは、個人のIDとしてラベルを付けないでください。個人のIDとしてはラベルを付けないでください。これらの値は、複数の個人に適用される場合があります。 これらのタイプのフィールドは、家庭レベルのマーケティング戦略のIDとしてのみラベルを付ける必要があります。

## 自分のIDフィールドが期待どおりにリンクされないのはなぜですか。

IDサービスAPIの [`/cluster/members` エンドポイント](./api/list-cluster-identites.md) を使用して、1つ以上のIDフィールドに関連付けられたIDを表示できます。 応答が、期待するリンク付きIDを返さない場合は、XDMデータに適切なID情報を提供していることを確認します。 詳しくは、「IDサービスの概要」の「XDMデータ [](./home.md) をIDサービスに提供する」の節を参照してください。

## ID名前空間とは

「ID名前空間」は、IDフィールドが顧客のIDにどのように関連付けられるかに関するコンテキストを提供します。 例えば、「Email」名前空間のIDフィールドは標準E メールフォーマット(name<span>@emailprovider.com)に準拠する必要があり、「Phone」名前空間を使用するフィールドは標準電話番号（北米では987-555-1234など）に準拠する必要があります。

名前空間は、異なるCRMシステム間で類似したID値を区別します。 例えば、会社の報酬プログラムに関連付けられた数値の忠誠度IDを含むプロファイルについて考えてみましょう。 「忠誠度」を名前空間すると、同じプロファイルにも表示されるeCommerceシステムの類似した数値IDからこの値が分離されます。

See the [identity namespace overview](./home.md) for more information.

## IDをID名前空間に関連付ける方法を教えてください。

「ID」フィールドは、作成時に既存のID名前空間に関連付ける必要があります。 新しい名前空間をIDフィールドに関連付ける前に、API [(API)を使用して](#how-do-i-create-a-custom-namespace-for-my-organization) 作成する必要があります。

APIを使用してID記述子を作成する際に名前空間を定義する詳しい手順については、スキーマレジストリ開発者ガイドの記述子の [作成に関する節を参照してください](../xdm/tutorials/create-schema-ui.md) 。 スキーマフィールドをUIでIDとしてマークするには、 [スキーマエディタのチュートリアルの手順に従います](../xdm/tutorials/create-schema-api.md)。

## Experience Platformが提供する標準ID名前空間とは何ですか。

次の標準名前空間は、Experience Platform内のすべての組織で使用できるように用意されています。

| 表示名 | ID | コード | 説明 |
| ------------ | --- | --- | ----------- |
| コア | 0 | コア | レガシー名： &quot;Adobe AudienceManager&quot; |
| ECID | 4 | ECID | alias: &quot;Adobe Marketing CloudID&quot;、&quot;Adobe Experience Cloud ID&quot;、&quot;Adobe Experience PlatformID&quot; |
| 電子メール | 6 | 電子メール |  |
| 電子メール（SHA256、小文字） | 11 | 電子メール | 事前にハッシュされた電子メールの標準名前空間。 この名前空間で指定する値は、SHA-256でハッシュする前に小文字に変換されます。 |
| 電話番号 | 7 | 電話番号 |  |
| Windows AID | 8 | WAID |  |
| AdCloud | 411 | AdCloud | alias: Ad Cloud |
| Adobe Target | 9 | TNTID | TargetID |
| Google広告ID | 20914 | ガイド | ガイド |
| Apple IDFA | 20915 | IDFA | 広告主のID |

## 組織で使用できるID名前空間のリストはどこで見つけられますか？

[IDサービスAPIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)、エンドポイントにGET要求を行うことで、組織で使用可能なすべてのID名前空間をリストでき `/idnamespace/identities` ます。 詳しくは、IDサービスAPIの概要で使用可能な [名前空間のリストに関する節を参照してください](./api/list-namespaces.md) 。

## 組織用のカスタム名前空間を作成する方法を教えてください。

[IDサービスAPIを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/id-service-api.yaml)、エンドポイントにPOSTリクエストを行うことで、組織のカスタムID名前空間を作成でき `/idnamespace/identities` ます。 詳しくは、IDサービスAPIの概要でのカスタム名前空間の [作成に関する節を参照してください](./api/create-custom-namespace.md) 。

## 複合IDとXIDとは何ですか。

IDは、API呼び出しで複合IDまたはXIDによって参照されます。 **複合ID** とは、ID値と名前空間を含むIDの表現です。 「 **XID** 」は、複合ID(IDと名前空間)と同じ構成体を表す単一値の識別子で、Identity Serviceで保持される場合は新しいIDに自動的に割り当てられます。 詳しくは、 [IDサービスAPIの概要](./home.md) （英語）を参照してください。

## IDサービスは個人識別情報(PII)をどのように処理しますか。

IDサービスは、値を永続化する前に、PIIの強力な一方向暗号化ハッシュを作成します。 「電話」および「電子メール」名前空間のIDデータは、SHA-256を使用して自動的にハッシュされ、「電子メール」値はハッシュ前に自動的に小文字に変換されます。

## Platformに送信する前に、すべてのPIIを暗号化する必要があるか。

PIIデータをPlatformに取り込む前に、PIIデータを手動で暗号化する必要はありません。 Platformは、該当するすべてのデータフィールドに `I1` データ使用ラベルを適用することで、これらのフィールドを取り込み時に自動的にハッシュID値に変換します。

データ使用量ラベルを適用および管理する手順については、「 [データ使用量ラベルのチュートリアル](../data-governance/labels/user-guide.md)」を参照してください。

## PIIベースのIDをハッシュする際に考慮すべき点はありますか？

ハッシュ化されたPII値をIDサービスに送信する場合は、データセットに対して同じ暗号化方法を使用する必要があります。 これにより、データセット間で同じID値が同じハッシュ値を生成し、IDグラフ内で適切に照合およびリンクできるようになります。

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

次の節では、 [!DNL Identity Service] APIの操作中に発生する可能性のある特定のエラーコードと予期しない動作に関するトラブルシューティングの推奨事項を示します。

## [!DNL Identity Service] エラーメッセージ

以下は、 [!DNL Identity Service] APIの使用時に発生する可能性があるエラーメッセージのリストです。

### 必要なクエリパラメータがありません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Missing required query parameter - namespace"
}
```

このエラーは、要求されたクエリパラメーターが要求パスに含まれていない場合に表示されます。 エラーメッセージ `detail` のは、見つからないパラメーターの名前を示します。 このエラーメッセージの例を次に示します。

- 必要なクエリパラメータがありません — nsId
- 必要なクエリパラメーターがありません — ID
- 必要なクエリパラメーターがありません — xidまたは(nsid,id)
- 必要なクエリパラメーターがありません — targetNs
- 必要なクエリパラメータがありません — xidsまたはcompositeXids

再試行する前に、指定されたパラメーターが要求パスに正しく含まれていることを確認してください。

### タイムスタンプは過去180日以内にする必要があります

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Timestamp should be within last 180 days"
}
```

[!DNL Identity Service] 180日を超える古いデータを削除します。 このエラーメッセージは、これより古いデータにアクセスしようとすると表示されます。

### 1回の呼び出しで最大1000個のXID

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit of 1000 XIDs in a single call"
}
```

このエラーメッセージは、1回のAPI呼び出しで許可される [XIDの最大数を超えるID情報を取得しようとした場合に表示されます](#what-are-composite-identities-and-xids) 。 この問題を解決するには、リクエスト内のXIDの数を、表示される制限を下回る数に減らしてください。


### 1回の呼び出しで1000 compositeXidsに制限があります

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There is a limit for 1000 compositeXids in a single call"
}
```

このエラーメッセージは、1回のAPI呼び出しで許可される [複合IDの最大数を超えるID情報を取得しようとした場合に表示されます](#what-are-composite-identities-and-xids) 。 この問題を解決するには、リクエスト内の複合IDの数を、表示される制限を下回るように減らします。

### 指定したグラフの種類は無効です

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "The graph-type abc specified is invalid. Please provide a valid graph-type"
}
```

このエラーメッセージは、 `graph-type` クエリパラメーターにリクエストパスで無効な値が与えられた場合に表示されます。 サポートされているグラフのタイプについては、 [概要の「](./home.md) IDグラフ [!DNL Identity Service] 」の節を参照してください。

### サービストークンに有効なスコープがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Service token does not have valid scope. Either acp.core.identity or acp.foundation is required"
}
```

このエラーメッセージは、IMS組織がに対する適切な権限を持つプロビジョニングを行っていない場合に表示され [!DNL Identity Service]ます。 この問題を解決するには、システム管理者に問い合わせてください。

### ゲートウェイサービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Gateway service token is not valid"
}
```

このエラーの場合、アクセストークンは無効です。 アクセストークンは24時間ごとに期限が切れます。APIを使用し続けるには、再生成する必要があり [!DNL Platform] ます。 新しい [アクセストークンの生成手順については、](../tutorials/authentication.md) 認証のチュートリアルを参照してください。

### 認証サービストークンが無効です

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Authorization service token is not valid"
}
```

このエラーの場合、アクセストークンは無効です。 アクセストークンは24時間ごとに期限が切れます。APIを使用し続けるには、再生成する必要があり [!DNL Platform] ます。 新しい [アクセストークンの生成手順については、](../tutorials/authentication.md) 認証のチュートリアルを参照してください。

### ユーザートークンに有効な製品コンテキストがありません

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "User token does not have valid product context"
}
```

このエラーメッセージは、アクセストークンが [!DNL Experience Platform] 統合から生成されていない場合に表示されます。 [統合用の新しいアクセストークンを生成する手順については、](../tutorials/authentication.md) 認証のチュートリアル [!DNL Experience Platform] を参照してください。

### IDと名前空間コードからネイティブXIDを取得する際に内部エラーが発生しました

```json
{
    "title": "UnauthorizedAccess",
    "status": 401,
    "detail": "Invalid IMS Token/IMS Org | Internal error - when tried to get native XID from identity and namespace code"
}
```

IDを [!DNL Identity Service] 保持する場合、IDのIDと関連付けられた名前空間IDには、XIDと呼ばれる一意の識別子が割り当てられます。 このメッセージは、指定したID値と名前空間のXIDを見つけるプロセス中にエラーが発生した場合に表示されます。

### IMS組織が [!DNL Identity Service] 使用のためにプロビジョニングされていない

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "The IMS Org. {IMS_ORG_NAME} is not provisioned for Identity Service usage"
}
```

このエラーメッセージは、IMS組織がに対する適切な権限を持つプロビジョニングを行っていない場合に表示され [!DNL Identity Service]ます。 この問題を解決するには、システム管理者に問い合わせてください。

### 内部サーバーエラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Server Error. There was a problem processing your request"
}
```

このエラーは、サー [!DNL Platform] ビス呼び出しの実行中に予期しない例外が発生した場合に表示されます。 ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに、時間を指定してリクエストを数回再試行することです。 問題が解決しない場合は、システム管理者に問い合わせてください。

## バッチ取り込みのエラーコード

[!DNL Identity Service] バッチインジェストを [!DNL Platform] 使用してアップロードされたレコードおよび時系列データからIDデータを取り込みます。 バッチ取り込みは非同期的な処理なので、バッチの詳細を表示エラーに表示する必要があります。 バッチが完了するまで、バッチの進行に従ってエラーが累積します。

以下は、 [!DNL Identity Service] データ取り込みAPIの使用時に発生する可能性があるエラーメッセージ [のリストです](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。

### 不明なXDMスキーマ

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Unknown XDM schema"
}
```

[!DNL Identity Service] IDは、それぞれまたはクラスに適合するレコードまたは時系列データに対して [!DNL Profile] のみ [!DNL ExperienceEvent] 使用します。 どちらのクラスにも準拠し [!DNL Identity Service] ていないデータを取り込もうとすると、このエラーが発生します。

### 処理されたバッチの最初の100行に、有効なIDが0件ありました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "There were 0 valid identities in the first 100 rows of the processed batch"
}
```

このエラーは、バッチの最初の100行にIDが表示されない場合に表示されます。 ただし、このエラーは、以降のレコードでIDが見つからなかったことを最終的に示すものではありません。

### XDMレコード1つにつきIDが1つしかないので、レコードをスキップしました

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Skipped {NUMBER_OF_RECORDS} records as they had only 1 identity per XDM record"
}
```

[!DNL Identity Service] 単一のレコードに2つ以上のID値が存在する場合は、IDのみがリンクされます。 このエラーメッセージは取り込むバッチごとに1回発生し、1つのIDしか見つからず、IDグラフに変更がなかったレコードの数を表示します。

### 名前空間コードがこのIMS組織に登録されていません

```json
{
    "title": "InvalidInput",
    "status": 400,
    "detail": "Namespace Code {ERRONEOUS_CODE} is not registered for this IMS Org"
}
```

このエラーは、取り込んだレコードに関連付けられた名前空間が存在しないか、お使いのIMS組織からアクセスできないIDが表示された場合に表示されます。

### IMS組織がプライベートIDグラフ用にプロビジョニングされていないため、バッチ取り込みをスキップします

```json
{
    "title": "AccountNotProvisioned",
    "status": 403,
    "detail": "Skipping batch ingestion as IMS Org is not provisioned for Private Identity Graph"
}
```

バッチデータを取り込むと、このエラーメッセージは、IMS組織がに対する適切な権限をプロビジョニングしていない場合に表示され [!DNL Identity Service]ます。 この問題を解決するには、システム管理者に問い合わせてください。

### 内部エラー

```json
{
    "title": "InternalError",
    "status": 500,
    "detail": "Internal Error. There was a problem during the ingestion"
}
```

このエラーは、バッチ取り込み中に予期しない例外が発生した場合に表示されます。 ベストプラクティスは、自動呼び出しをプログラムして、このエラーを受け取ったときに、時間を指定してリクエストを数回再試行することです。 問題が解決しない場合は、システム管理者に問い合わせてください。
