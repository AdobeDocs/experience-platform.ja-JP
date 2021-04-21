---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API
title: リアルタイム顧客プロファイルトラブルシューティングガイド
topic-legacy: guide
type: Documentation
description: このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問と、Adobe Experience Platformを使用したプロファイルデータの操作時の一般的なエラーのトラブルシューティングガイドに回答します。
exl-id: 0b340025-093b-41e4-8053-969a8e80e889
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 3%

---

# リアルタイム顧客プロファイルトラブルシューティングガイド

このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問と、一般的なエラーのトラブルシューティングガイドについて回答します。 Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

[!DNL Real-time Customer Profile]を使うと、オンライン、オフライン、CRM、サードパーティなど複数のチャネルからのデータを組み合わせることで、各顧客の全体的な表示を見ることができます。 これにより、マーケターは、複数のチャネルにわたって、顧客に対して、調整され、一貫性のある、関連性のあるエクスペリエンスを提供できます。

## FAQ

以下は、リアルタイム顧客プロファイルに関するよくある質問に対する回答のリストです。

### リアルタイム顧客プロファイルで受け入れられるデータの種類は何ですか。

プロファイルは、対象のデータに一意の個人とデータを関連付ける1つ以上のID値が含まれている限り、**レコード**&#x200B;と&#x200B;**時系列**&#x200B;の両方のデータを受け入れます。

すべてのプラットフォームサービスと同様に、プロファイルのデータは、エクスペリエンスデータモデル(XDM)スキーマ下でセマンティックに構造化される必要があります。 その上、このスキーマには、**プライマリID**&#x200B;が定義され、プロファイルでの使用が有効になっている必要があります。

XDMに慣れていない場合は、[XDM overview](../xdm/home.md)に開始して詳しく調べてください。 次に、[IDフィールド](../xdm/tutorials/create-schema-ui.md#identity-field)と[プロファイル](../xdm/tutorials/create-schema-ui.md#profile)のスキーマを有効にする手順については、XDMユーザーガイドを参照してください。

### プロファイルデータはどこに保存されますか。

リアルタイムのお客様のプロファイルは、取り込んだ他のプラットフォームデータを含むData Lakeとは別に、独自のデータストア(「プロファイルストア」と呼ばれます)を維持します。

### データを既にプラットフォームに取り込んでいる場合は、プロファイルストアで入手できますか。

データがプロファイル以外のデータセットに取り込まれた場合は、プロファイルストアでデータを利用できるように、そのデータをプロファイル対応のデータセットに再度取り込む必要があります。 既存のデータセットのプロファイルを有効にすることはできますが、その設定より前に取り込まれたデータはプロファイルストアには表示されません。

以前に取り込んだデータをプロファイルストアに追加する場合は、[データセット設定チュートリアル](./tutorials/dataset-configuration.md)に従って新しいデータセットを作成するか、既存のデータセットをプロファイル可能に変換し、必要なデータをそのデータセットに再取り込みします。

### 取り込んだプロファイルデータを表示する方法を教えてください。

APIとUIのどちらを使用しているかに応じて、プロファイルデータを表示する方法は複数あります。

#### API の使用

アクセスするプロファイルエンティティのIDがわかっている場合は、プロファイルAPIの`/entities` (プロファイルアクセス)エンドポイントを使用して、それらのエンティティを検索できます。 詳しくは、開発者ガイドの[entities](./api/entities.md)の節を参照してください。

また、Adobe Experience PlatformセグメントサービスAPIを使用して、セグメントのメンバーシップの資格を持つ顧客の個々のプロファイルにアクセスすることもできます。 詳しくは、[Segmentation Serviceの概要](../segmentation/home.md)を参照してください。

#### UI の使用

Experience PlatformUIの&#x200B;**[!UICONTROL プロファイル]**&#x200B;ワークスペースの&#x200B;**[!UICONTROL 「参照]**」タブでは、プロファイルの合計数を表示し、個々のプロファイルをそのID値で検索できます。 詳しくは、[プロファイルユーザーガイド](./ui/user-guide.md)を参照してください。

また、**[!UICONTROL セグメント]**&#x200B;ワークスペースの「**[!UICONTROL 参照]**」タブで、セグメントのリストを表示することもできます。 セグメントを選択すると、そのセグメントに適したプロファイルのサンプルが表示されます。 その後、リストに表示された任意のプロファイルを選択して、詳細を表示できます。 詳しくは、[セグメント化UIの概要](../segmentation/ui/overview.md)を参照してください。

## エラーコード

以下は、リアルタイム顧客プロファイルAPIを使用する際に発生する可能性があるエラーメッセージのリストです。 発生したエラーがここに記載されていない場合は、[一般的なプラットフォームトラブルシューティングガイド](../landing/troubleshooting.md)に記載されている可能性があります。

### 指定されたパスの計算済み属性のスキーマを参照できませんでした

```json
{
  "code": "400",
  "message": "Could not lookup schema of the computed attribute for the provided path"
}
```

新しい計算済み属性を作成する場合、このエラーは、要求ペイロードで提供されたスキーマが見つからなかった場合に発生します。 ペイロードの`path`プロパティに正しいテナントIDが指定されていて、`schema.name`の値が有効なスキーマ名であることを確認してください。

テナントIDがわからない場合は、[スキーマレジストリ開発者ガイド](../xdm/api/getting-started.md)の手順に従ってIDを取得できます。

### 指定したスキーマまたはdefinedOnに同じ名前の関数が既に存在します

```json
{
  "code": "400",
  "message": "Function with the same name already exists for the specified schema or definedOn"
}
```

新しい計算済み属性を作成する場合、このエラーは、指定された`name`プロパティが`schema.name`で示されるスキーマに既に使用されている場合に発生します。 値を一意の名前に置き換えてから、やり直してください。

### 式の戻りスキーマは、XDMスキーマの計算済み属性のスキーマと同じではありません

```json
{
  "code": "400",
  "message": "Return schema of the expression is not same as the schema of the computed attribute in the XDM schema"
}
```

新しい計算済み属性を作成する場合、このエラーは、指定された`name`プロパティが`schema.name`で示されるスキーマに既に使用されている場合に発生します。 値を一意の名前に置き換えてから、やり直してください。

### 無効な削除要求(プロファイルシステムジョブ)

```json
{
  "code": "400",
  "message": "Invalid delete request (Profile System Job)"
}
```

このエラーは、削除システムジョブに無効なペイロードが指定された場合に発生します。 ペイロードの`dataSetID`プロパティまたは`batchID`プロパティの下に、それぞれ有効なデータセットまたはバッチIDを指定していることを確認します。 詳しくは、プロファイル開発者ガイドの[削除リクエストの作成](./api/profile-system-jobs.md#create-a-delete-request)の節を参照してください。

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

このエラーは、`POST /config/projections`要求で指定された`destinationId`が無効な場合に発生します。 再試行する前に、有効な宛先IDが指定されていることを重複確認してください。 新しい宛先を作成するには、『[プロファイル開発ガイド](./api/edge-projections.md#create-a-destination)』に記載されている手順に従います。

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
