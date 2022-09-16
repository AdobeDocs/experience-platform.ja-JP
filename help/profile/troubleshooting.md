---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: リアルタイム顧客プロファイルトラブルシューティングガイド
topic-legacy: guide
type: Documentation
description: このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問に対する回答と、Adobe Experience Platformを使用してプロファイルデータを操作する際の一般的なエラーのトラブルシューティングガイドを示します。
exl-id: 0b340025-093b-41e4-8053-969a8e80e889
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 5%

---

# リアルタイム顧客プロファイルトラブルシューティングガイド

このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問に対する回答と、一般的なエラーのトラブルシューティングガイドを提供します。 Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

[!DNL Real-time Customer Profile] では、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。これにより、マーケターは、複数のチャネルにわたって顧客に対して調整され、一貫性のある、関連性の高いエクスペリエンスを提供できます。

## FAQ

次に、リアルタイム顧客プロファイルに関するよくある質問に対する回答のリストを示します。

### リアルタイム顧客プロファイルで受け入れられるデータの種類は何ですか？

プロファイルは両方を受け入れます **レコード** および **時系列** データ（該当するデータに、データを一意の個人に関連付ける id 値が 1 つ以上含まれている場合）

すべての Platform サービスと同様に、プロファイルのデータは、エクスペリエンスデータモデル (XDM) スキーマの下で、意味的に構造化されている必要があります。 次に、このスキーマには **プライマリ ID** 定義済みで、プロファイルで使用できるようになります。

XDM に慣れていない場合は、まず [XDM の概要](../xdm/home.md) を参照してください。 次に、XDM ユーザーガイドで、 [id フィールドを設定](../xdm/tutorials/create-schema-ui.md#identity-field) および [プロファイルのスキーマを有効にする](../xdm/tutorials/create-schema-ui.md#profile).

### プロファイルデータはどこに保存されますか？

リアルタイム顧客プロファイルは、他に取り込んだ Platform データを含むデータレイクとは別に、独自のデータストア（「プロファイルストア」と呼ばれます）を維持します。

### データを既に Platform に取り込んでいる場合、Profile ストアで使用できるようにできますか。

データが非プロファイルデータセットに取り込まれている場合、プロファイルストアで使用できるようにするには、そのデータをプロファイル対応のデータセットに再取り込みする必要があります。 プロファイルの既存のデータセットを有効にすることはできますが、その設定以前に取り込まれたデータは、プロファイルストアには表示されません。

以前に取り込んだデータをプロファイルストアに追加する場合は、 [データセット設定のチュートリアル](./tutorials/dataset-configuration.md) を使用して、新しいデータセットを作成するか、既存のデータセットを変換してプロファイルに対して有効にし、目的のデータをそのデータセットに再取り込みします。

### 取り込んだプロファイルデータを表示するにはどうすればよいですか？

API と UI のどちらを使用しているかに応じて、プロファイルデータを表示する複数の方法があります。

#### API の使用

アクセスするプロファイルエンティティの ID がわかっている場合は、 `/entities` （プロファイルアクセス）エンドポイントを使用してエンティティを検索する必要があります。 詳しくは、 [エンティティ](./api/entities.md) （開発者ガイド）を参照してください。

また、Adobe Experience Platform Segmentation Service API を使用して、セグメントメンバーシップの対象として認定された顧客の個々のプロファイルにアクセスすることもできます。 詳しくは、 [セグメント化サービスの概要](../segmentation/home.md) を参照してください。

#### UI の使用

Experience PlatformUI で、 **[!UICONTROL 参照]** 」タブをクリックします。 **[!UICONTROL プロファイル]** workspace では、合計プロファイル数を表示し、id 値で個々のプロファイルを検索できます。 詳しくは、 [プロファイルユーザーガイド](./ui/user-guide.md) を参照してください。

また、セグメントのリストは、 **[!UICONTROL 参照]** 」タブをクリックします。 **[!UICONTROL セグメント]** ワークスペース。 セグメントを選択すると、そのセグメントに適合するプロファイルのサンプルが表示されます。 その後、リストに表示された任意のプロファイルを選択して、その詳細を表示できます。 詳しくは、 [セグメント化 UI の概要](../segmentation/ui/overview.md) を参照してください。

## エラーコード

次に、リアルタイム顧客プロファイル API を使用する際に発生する可能性のあるエラーメッセージのリストを示します。 発生したエラーがここに記載されていない場合は、一般に [Platform トラブルシューティングガイド](../landing/troubleshooting.md) 代わりに、

### 指定されたパスの計算済み属性のスキーマを参照できませんでした

```json
{
  "code": "400",
  "message": "Could not lookup schema of the computed attribute for the provided path"
}
```

新しい計算済み属性を作成する場合、このエラーは、リクエストペイロードで提供されたスキーマがシステムで見つからなかった場合に発生します。 ペイロードの `path` プロパティに含まれ、 `schema.name` は有効なスキーマ名です。

テナント ID が不明な場合は、 [スキーマレジストリ開発者ガイド](../xdm/api/getting-started.md).

### 指定したスキーマまたは definedOn に同じ名前の関数が既に存在します

```json
{
  "code": "400",
  "message": "Function with the same name already exists for the specified schema or definedOn"
}
```

新しい計算済み属性を作成する際に、指定した `name` プロパティは、次の場所に示されるスキーマに既に使用されています： `schema.name`. 再試行する前に、値を一意の名前に置き換えてください。

### 式の戻り値のスキーマが XDM スキーマ内の計算済み属性のスキーマと同じではありません

```json
{
  "code": "400",
  "message": "Return schema of the expression is not same as the schema of the computed attribute in the XDM schema"
}
```

新しい計算済み属性を作成する際に、指定した `name` プロパティは、次の場所に示されるスキーマに既に使用されています： `schema.name`. 再試行する前に、値を一意の名前に置き換えてください。

### 無効な削除リクエスト（プロファイルシステムジョブ）

```json
{
  "code": "400",
  "message": "Invalid delete request (Profile System Job)"
}
```

このエラーは、削除システムジョブに無効なペイロードが指定された場合に発生します。 ペイロードの `dataSetID` または `batchID` プロパティに設定されます。 詳しくは、 [削除リクエストの作成](./api/profile-system-jobs.md#create-a-delete-request) （プロファイル開発者ガイド）を参照してください。

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

このエラーは、プロファイルデータの削除リクエストを作成しようとしたときに有効なバッチが見つからなかった場合に発生します。 再試行する前に、プロファイル対応データセットの正しい ID を入力していることを確認してください。

### 投影先がまだ作成されていません

```json
{
  "status":404,
  "title":"The projection destination has not yet been created.",
  "type":"http://ns.adobe.com/adobecloud/problem/missing-entity"
}
```

このエラーは、 `destinationId` 提供される `POST /config/projections` リクエストが無効です。 再試行する前に、有効な宛先 ID を指定したことを再確認してください。 新しい宛先を作成するには、 [プロファイル開発者ガイド](./api/edge-projections.md#create-a-destination).

### サポートされていないメディアタイプ

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
