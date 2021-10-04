---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: リアルタイム顧客プロファイルトラブルシューティングガイド
topic-legacy: guide
type: Documentation
description: このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問と、Adobe Experience Platformを使用してプロファイルデータを操作する際の一般的なエラーのトラブルシューティングガイドに回答します。
exl-id: 0b340025-093b-41e4-8053-969a8e80e889
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 5%

---

# リアルタイム顧客プロファイルトラブルシューティングガイド

このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

[!DNL Real-time Customer Profile] では、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。これにより、マーケターは、複数のチャネルにわたって顧客に対して、調整され、一貫性のある、関連性の高いエクスペリエンスを提供できます。

## FAQ

次に、リアルタイム顧客プロファイルに関するよくある質問に対する回答のリストを示します。

### リアルタイム顧客プロファイルに受け入れられるデータの種類を教えてください。

プロファイルは、問題のデータにデータを個人に関連付ける ID 値が 1 つ以上含まれている限り、**レコード** と **時系列** の両方のデータを受け入れます。

すべての Platform サービスと同様、プロファイルでは、データがエクスペリエンスデータモデル (XDM) スキーマの下で意味的に構造化されている必要があります。 次に、このスキーマには **プライマリ ID** が定義され、プロファイルでの使用を有効にする必要があります。

XDM に慣れていない場合は、まず [XDM の概要 ](../xdm/home.md) を参照してください。 次に、[ID フィールド ](../xdm/tutorials/create-schema-ui.md#identity-field) と [ を設定して、プロファイル ](../xdm/tutorials/create-schema-ui.md#profile) のスキーマを有効にする手順については、XDM ユーザーガイドを参照してください。

### プロファイルデータはどこに保存されますか？

リアルタイム顧客プロファイルは、取り込んだ他の Platform データを含むデータレイクとは別に、独自のデータストア（「プロファイルストア」と呼ばれます）を維持します。

### データを既に Platform に取り込んでいる場合、プロファイルストアで使用できますか。

データが非プロファイルデータセットに取り込まれた場合、プロファイルストアで使用できるようにするには、そのデータをプロファイル対応のデータセットに再取り込みする必要があります。 プロファイルの既存のデータセットを有効にすることはできますが、その設定より前に取り込まれたデータは、引き続きプロファイルストアに表示されません。

以前に取得したデータをプロファイルストアに追加する場合は、『[ データセット設定のチュートリアル ](./tutorials/dataset-configuration.md)』に従って新しいデータセットを作成するか、既存のデータセットを変換してプロファイルに対して有効にし、必要なデータを再取得します。

### 取り込んだプロファイルデータを表示するにはどうすればよいですか？

API と UI のどちらを使用しているかに応じて、プロファイルデータを表示する方法は複数あります。

#### API の使用

アクセスするプロファイルエンティティの ID がわかっている場合は、Profile API の `/entities`（プロファイルアクセス）エンドポイントを使用して、それらのエンティティを検索できます。 詳しくは、開発者ガイドの [ エンティティ ](./api/entities.md) の節を参照してください。

また、Adobe Experience Platformセグメント化サービス API を使用して、セグメントメンバーシップの資格を持つ顧客の個々のプロファイルにアクセスすることもできます。 詳しくは、「[ セグメント化サービスの概要 ](../segmentation/home.md)」を参照してください。

#### UI の使用

Experience PlatformUI の **[!UICONTROL プロファイル]** ワークスペースの「**[!UICONTROL 参照]**」タブで、合計プロファイル数を表示し、ID 値を使用して個々のプロファイルを検索できます。 詳しくは、[ プロファイルユーザーガイド ](./ui/user-guide.md) を参照してください。

また、**[!UICONTROL セグメント]** ワークスペースの「**[!UICONTROL 参照]**」タブで、セグメントのリストを表示することもできます。 セグメントを選択すると、そのセグメントに適合するプロファイルのサンプルが表示されます。 次に、リストに表示されたこれらのプロファイルのいずれかを選択して、その詳細を表示できます。 詳しくは、[ セグメント化 UI の概要 ](../segmentation/ui/overview.md) を参照してください。

## エラーコード

以下は、リアルタイム顧客プロファイル API の操作時に発生する可能性のあるエラーメッセージのリストです。 発生したエラーがここに記載されていない場合は、一般的な [Platform トラブルシューティングガイド ](../landing/troubleshooting.md) に記載されています。

### 指定されたパスの計算済み属性のスキーマを参照できませんでした

```json
{
  "code": "400",
  "message": "Could not lookup schema of the computed attribute for the provided path"
}
```

新しい計算済み属性を作成する場合、このエラーは、リクエストペイロードで提供されたスキーマが見つからなかった場合に発生します。 ペイロードの `path` プロパティに正しいテナント ID が指定され、`schema.name` の値が有効なスキーマ名であることを確認します。

テナント ID が不明な場合は、『[ スキーマレジストリ開発者ガイド ](../xdm/api/getting-started.md)』の手順に従って、ID を取得できます。

### 指定したスキーマまたは definedOn に同じ名前の関数が既に存在します

```json
{
  "code": "400",
  "message": "Function with the same name already exists for the specified schema or definedOn"
}
```

新しい計算済み属性を作成すると、このエラーは、指定された `name` プロパティが `schema.name` で示されたスキーマに既に使用されている場合に発生します。 再試行する前に、値を一意の名前に置き換えてください。

### 式の戻りスキーマが XDM スキーマ内の計算済み属性のスキーマと同じではありません

```json
{
  "code": "400",
  "message": "Return schema of the expression is not same as the schema of the computed attribute in the XDM schema"
}
```

新しい計算済み属性を作成すると、このエラーは、指定された `name` プロパティが `schema.name` で示されたスキーマに既に使用されている場合に発生します。 再試行する前に、値を一意の名前に置き換えてください。

### 無効な削除要求（プロファイルシステムジョブ）

```json
{
  "code": "400",
  "message": "Invalid delete request (Profile System Job)"
}
```

このエラーは、削除システムジョブに無効なペイロードが指定された場合に発生します。 ペイロードの `dataSetID` プロパティまたは `batchID` プロパティの下に、それぞれ有効なデータセット ID またはバッチ ID を指定していることを確認します。 詳しくは、プロファイル開発者ガイドの [ 削除リクエスト ](./api/profile-system-jobs.md#create-a-delete-request) の作成に関する節を参照してください。

### プロファイルデータセットのバッチが見つかりません

```json
{
  "requestId":"LlTmQkhgHKFGHGHnIkmUxcIL4YTFSpQw",
  "errors":{
    "400":[
      {
        "code":"400",
        "message":"Batch not found for profile dataset '5da688d2c4e60518ad25b7b1'"
      }
    ]
  }
}
```

このエラーは、プロファイルデータの削除要求を作成しようとしたときに有効なバッチが見つからなかった場合に発生します。 再試行する前に、プロファイル対応データセットの正しい ID を入力していることを確認してください。

### 投影先はまだ作成されていません

```json
{
  "status":404,
  "title":"The projection destination has not yet been created.",
  "type":"http://ns.adobe.com/adobecloud/problem/missing-entity"
}
```

このエラーは、`POST /config/projections` リクエストで指定された `destinationId` が無効な場合に発生します。 再試行する前に、有効な宛先 ID が指定されていることを再確認してください。 新しい宛先を作成するには、『[ プロファイル開発者ガイド ](./api/edge-projections.md#create-a-destination)』で説明されている手順に従います。

### サポートされていないメディアの種類

```json
{
  "status": 415,
  "title": "HTTP 415 Unsupported Media Type",
  "type": "http://ns.adobe.com/adobecloud/problem/unsupported-media-type"
}
```

このエラーは、無効な Content-Type ヘッダーを持つPOSTまたはPUTリクエストを送信する際に発生します。 使用しているエンドポイントに有効な Content-Type 値を指定していることを再確認します。

ほとんどのプロファイルエンドポイントは、Content-Type ヘッダーに「application/json」を受け入れますが、次の例外があります。

| エンドポイント | Content-Type |
| --- | --- |
| `/config/projections` | application/vnd.adobe.platform.projectionConfig+json;version=1 |
| `/config/destinations` | application/vnd.adobe.platform.projectionDestination+json;version=1 |
