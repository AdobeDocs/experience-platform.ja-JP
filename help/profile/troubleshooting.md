---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
title: リアルタイム顧客プロファイルトラブルシューティングガイド
topic: guide
translation-type: tm+mt
source-git-commit: 59cf089a8bf7ce44e7a08b0bb1d4562f5d5104db
workflow-type: tm+mt
source-wordcount: '983'
ht-degree: 9%

---


# リアルタイム顧客プロファイルトラブルシューティングガイド

このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

リアルタイム顧客プロファイルは、様々な企業データアセットのデータを結合し、個々の顧客プロファイルおよび関連する時系列イベントの形でそのデータにアクセスできる汎用参照エンティティストアです。この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。

## FAQ

以下は、リアルタイム顧客プロファイルに関するよくある質問に対する回答のリストです。

### リアルタイム顧客プロファイルで受け入れられるデータの種類は何ですか。

プロファイルは、 **レコード** と **** 時系列の両方のデータを受け取ります。ただし、対象のデータに、データを一意の個人に関連付ける1つ以上の識別値が含まれている場合に限ります。

すべてのプラットフォームサービスと同様に、プロファイルのデータは、エクスペリエンスデータモデル(XDM)スキーマ下でセマンティックに構造化される必要があります。 そのため、このスキーマは **プライマリID** を定義し、プロファイルでの使用を有効にする必要があります。

XDMに慣れていない場合は、 [XDMの概要に関する開始を参照し](../xdm/home.md) 、詳細を確認してください。 次に、XDMプロファイルガイドを参照して、IDフィールドの [設定とスキーマの](../xdm/tutorials/create-schema-ui.md#identity-field) 有効化の手順を確認します [](../xdm/tutorials/create-schema-ui.md#profile)。

### プロファイルデータはどこに保存されますか。

リアルタイムのお客様のプロファイルは、取り込んだ他のプラットフォームデータを含むData Lakeとは別に、独自のデータストア(「プロファイルストア」と呼ばれます)を維持します。

### データを既にプラットフォームに取り込んでいる場合は、プロファイルストアで入手できますか。

データがプロファイル以外のデータセットに取り込まれた場合は、プロファイルストアでデータを利用できるように、そのデータをプロファイル対応のデータセットに再度取り込む必要があります。 既存のデータセットのプロファイルを有効にすることはできますが、その設定より前に取り込まれたデータはプロファイルストアには表示されません。

以前に取り込んだデータをプロファイルストアに追加する場合は、 [データセット設定のチュートリアルに従って新しいデータセットを作成するか](./tutorials/dataset-configuration.md) 、プロファイル可能な既存のデータセットを変換し、必要なデータをそのデータセットに再度取り込みます。

### 取り込んだプロファイルデータを表示する方法を教えてください。

APIとUIのどちらを使用しているかに応じて、プロファイルデータを表示する方法は複数あります。

#### API の使用

アクセスするプロファイルエンティティのIDがわかっている場合は、プロファイルAPIの `/entities` (プロファイルアクセス)エンドポイントを使用して、これらのエンティティを検索できます。 詳しくは、『開発者ガイド』の [entitiesに関する節を参照してください](./api/entities.md) 。

また、Adobe Experience PlatformセグメントサービスAPIを使用して、セグメントのメンバーシップの資格を持つ顧客の個々のプロファイルにアクセスすることもできます。 See the [Segmentation Service overview](../segmentation/home.md) for more information.

#### UI の使用

Experience PlatformUIの「 **[!UICONTROL 参照]** 」タブ( **[!UICONTROL プロファイル]** ワークスペース)では、プロファイルの合計数を表示し、個々のプロファイルをそのID値で検索できます。 See the [Profile user guide](./ui/user-guide.md) for more information.

セグメントワークスペースの「 **[!UICONTROL 参照]** 」タブで、セグメントのリストを表示することもでき **[!UICONTROL ます]** 。 セグメントを選択すると、そのセグメントに適したプロファイルのサンプルが表示されます。 その後、リストに表示された任意のプロファイルを選択して、詳細を表示できます。 See the [Segmentation UI overview](../segmentation/ui/overview.md) for more information.

## エラーコード

以下は、リアルタイム顧客プロファイルAPIを使用する際に発生する可能性があるエラーメッセージのリストです。 発生したエラーがここに記載されていない場合は、代わりに [Platformの一般的なトラブルシューティングガイド](../landing/troubleshooting.md) （英語）に記載されている可能性があります。

### 指定されたパスの計算済み属性のスキーマを参照できませんでした

```json
{
  "code": "400",
  "message": "Could not lookup schema of the computed attribute for the provided path"
}
```

新しい計算済み属性を作成する場合、このエラーは、要求ペイロードで提供されたスキーマが見つからなかった場合に発生します。 ペイロードの `path` プロパティに正しいテナントIDが指定されていて、の値が有効なスキーマ名であるこ `schema.name` とを確認してください。

テナントIDがわからない場合は、『 [スキーマレジストリ開発者ガイド](../xdm/api/getting-started.md)』の手順に従ってIDを取得できます。

### 指定したスキーマまたはdefinedOnに同じ名前の関数が既に存在します

```json
{
  "code": "400",
  "message": "Function with the same name already exists for the specified schema or definedOn"
}
```

新しい計算済み属性を作成する場合、このエラーは、指定された `name` プロパティがに示すスキーマに既に使用されている場合に発生し `schema.name`ます。 値を一意の名前に置き換えてから、やり直してください。

### 式の戻りスキーマは、XDMスキーマの計算済み属性のスキーマと同じではありません

```json
{
  "code": "400",
  "message": "Return schema of the expression is not same as the schema of the computed attribute in the XDM schema"
}
```

新しい計算済み属性を作成する場合、このエラーは、指定された `name` プロパティがに示すスキーマに既に使用されている場合に発生し `schema.name`ます。 値を一意の名前に置き換えてから、やり直してください。

### 無効な削除要求(プロファイルシステムジョブ)

```json
{
  "code": "400",
  "message": "Invalid delete request (Profile System Job)"
}
```

このエラーは、削除システムジョブに無効なペイロードが指定された場合に発生します。 ペイロードのプロパティまたは `dataSetID``batchID` プロパティの下に、それぞれ有効なデータセットまたはバッチIDを提供していることを確認してください。 詳しくは、『プロファイル開発ガイド [』の削除リクエストの](./api/profile-system-jobs.md#create-a-delete-request) 作成に関する節を参照してください。

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

このエラーは、プロファイルデータの削除要求を作成しようとしたときに、有効なバッチが見つからなかった場合に発生します。 プロファイル対応データセットに正しいIDを入力したことを確認してから、やり直してください。

### 投影先がまだ作成されていません

```json
{
  "status":404,
  "title":"The projection destination has not yet been created.",
  "type":"http://ns.adobe.com/adobecloud/problem/missing-entity"
}
```

このエラーは、リク `destinationId``POST /config/projections` エストに入力されたが無効な場合に発生します。 再試行する前に、有効な宛先IDが指定されていることを重複確認してください。 新しい保存場所を作成するには、『 [プロファイル開発ガイド](./api/edge-projections.md#create-a-destination)』に説明されている手順に従います。

### サポートされないメディアタイプ

```json
{
  "status": 415,
  "title": "HTTP 415 Unsupported Media Type",
  "type": "http://ns.adobe.com/adobecloud/problem/unsupported-media-type"
}
```

このエラーは、無効なContent-Typeヘッダーを含むPOSTまたはPUT要求を送信するときに発生します。 使用するエンドポイントに対して有効なContent-Type値を指定していることを重複が確認します。

ほとんどのプロファイルエンドポイントでは、Content-Typeヘッダーに「application/json」を使用できますが、以下の例外があります。

| エンドポイント | Content-Type |
| --- | --- |
| `/config/projections` | application/vnd.adobe.platform.projectionConfig+json;version=1 |
| `/config/destinations` | application/vnd.adobe.platform.projectionDestination+json;version=1 |