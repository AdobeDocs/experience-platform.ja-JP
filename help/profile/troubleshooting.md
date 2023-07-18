---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: リアルタイム顧客プロファイルトラブルシューティングガイド
type: Documentation
description: このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問への回答のほか、Adobe Experience Platform を使用してプロファイルデータを操作する際に発生する一般的なエラーのトラブルシューティングガイドを示します。
exl-id: 0b340025-093b-41e4-8053-969a8e80e889
source-git-commit: 8ae18565937adca3596d8663f9c9e6d84b0ce95a
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 94%

---

# リアルタイム顧客プロファイルトラブルシューティングガイド

このドキュメントでは、リアルタイム顧客プロファイルに関するよくある質問への回答のほか、一般的なエラーのトラブルシューティングガイドを示します。Adobe Experience Platform の他のサービスに関する質問やトラブルシューティングについては、[Experience Platform のトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

[!DNL Real-Time Customer Profile] を使用すると、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルのデータを組み合わせて、個々の顧客の全体像を把握できます。これにより、マーケターは、複数のチャネルにわたって、連携の取れた一貫性のある適切なエクスペリエンスを顧客に提供できます。

## FAQ

以下は、リアルタイム顧客プロファイルに関するよくある質問への回答のリストです。

### どのような種類のデータをリアルタイム顧客プロファイルに使用できますか？

プロファイルには&#x200B;**レコード**&#x200B;データと&#x200B;**時系列**&#x200B;データの両方を使用できますが、対象となっているデータには、データを一意の個人に関連付ける ID 値が少なくとも 1 つ含まれている必要があります。

他のすべての Platform サービスと同様に、プロファイルでは、そのデータがエクスペリエンスデータモデル（XDM）スキーマの下で意味的に構造化されている必要があります。次に、このスキーマには&#x200B;**プライマリ ID** が定義されていて、プロファイルで使用できるようになっている必要があります。

XDM に慣れていない場合は、まず [XDM の概要](../xdm/home.md)を参照してください。次に、[ID フィールドの設定](../xdm/tutorials/create-schema-ui.md#identity-field)と[プロファイルへのスキーマの有効化](../xdm/tutorials/create-schema-ui.md#profile)の手順については、XDM ユーザーガイドを参照してください。

### プロファイルデータはどこに保存されますか？

リアルタイム顧客プロファイルは、取り込んだ他の Platform データを含んだデータレイクとは別に、独自のデータストア（「プロファイルストア」と呼ばれます）を維持管理します。

### データを既に Platform に取り込んでいる場合、それを Profile ストアで使用できるようにできますか？

データがプロファイル以外のデータセットに取り込まれている場合、プロファイルストアで使用できるようにするには、そのデータをプロファイル対応のデータセットに再度取り込む必要があります。プロファイルの既存のデータセットを有効にすることはできますが、その設定以前に取り込まれたデータは、プロファイルストアには表示されません。

以前に取り込んだデータをプロファイルストアに追加する場合は、[データセット設定のチュートリアル](./tutorials/dataset-configuration.md)に従って、新しいデータセットを作成するか、既存のデータセットをプロファイルで使用できるように変換してから、そのデータセットに目的のデータを再度取り込みます。

### 取り込んだプロファイルデータを表示するにはどうすればよいですか？

API と UI のどちらを使用しているかに応じて、プロファイルデータを表示する方法は複数あります。

#### API の使用

アクセスするプロファイルエンティティの ID がわかっている場合は、プロファイル API の `/entities`（プロファイルアクセス）エンドポイントを使用して、それらのエンティティを検索できます。詳しくは、開発者ガイドの[エンティティ](./api/entities.md)に関する節を参照してください。

また、Adobe Experience Platform Segmentation Service API を使用して、オーディエンスメンバーシップの資格を持つ顧客の個々のプロファイルにアクセスすることもできます。 詳しくは、[セグメント化サービスの概要](../segmentation/home.md)を参照してください。

#### UI の使用

Experience Platform UI では、**[!UICONTROL プロファイル]**&#x200B;ワークスペースの「**[!UICONTROL 参照]**」タブを使用して、合計プロファイル数を表示し、ID 値で個人プロファイルを検索できます。詳しくは、[プロファイルユーザーガイド](./ui/user-guide.md)を参照してください。

オーディエンスのリストを **[!UICONTROL 参照]** 」タブをクリックします。 **[!UICONTROL オーディエンス]** ワークスペース。 オーディエンスを選択すると、そのオーディエンスに適合するプロファイルのサンプルが表示されます。 次に、これらのリストされたプロファイルのいずれかを選択して、その詳細を表示できます。詳しくは、[クセグメント化 UI の概要](../segmentation/ui/overview.md)を参照してください。

## エラーコード

リアルタイム顧客プロファイル API を使用する際に発生する可能性のあるエラーメッセージのリストを以下に示します。発生したエラーがここに記載されていない場合は、代わりに一般的な [Platform トラブルシューティングガイド](../landing/troubleshooting.md)で見つかることもあります。

### 指定されたパスの計算属性のスキーマをルックアップできませんでした

```json
{
  "code": "400",
  "message": "Could not lookup schema of the computed attribute for the provided path"
}
```

新しい計算属性を作成するときに、リクエストのペイロードで指定したスキーマが見つからなかった場合に、このエラーが発生します。ペイロードの `path` プロパティに正しいテナント ID を指定したことと、`schema.name` の値が有効なスキーマ名であることを確認してください。

テナント ID が不明な場合は、[スキーマレジストリ開発者ガイド](../xdm/api/getting-started.md)に記載されている手順に従って取得できます。

### 指定されたスキーマまたは definedOn に対して、同じ名前の関数が既に存在します

```json
{
  "code": "400",
  "message": "Function with the same name already exists for the specified schema or definedOn"
}
```

新しい計算属性を作成するときに、指定した `name` プロパティが `schema.name` で示されるスキーマに既に使用されている場合、このエラーが発生します。値を一意の名前に置き換えてから、もう一度やり直してください。

### 式の戻りスキーマが XDM スキーマの計算属性のスキーマと同じではありません

```json
{
  "code": "400",
  "message": "Return schema of the expression is not same as the schema of the computed attribute in the XDM schema"
}
```

新しい計算属性を作成するときに、指定した `name` プロパティが `schema.name` で示されるスキーマに既に使用されている場合、このエラーが発生します。値を一意の名前に置き換えてから、もう一度やり直してください。

### 削除リクエスト (プロファイルシステムジョブ) が無効です

```json
{
  "code": "400",
  "message": "Invalid delete request (Profile System Job)"
}
```

このエラーは、削除システムジョブに無効なペイロードが指定された場合に発生します。ペイロードの `dataSetID` または `batchID` プロパティで、それぞれ有効なデータセットまたはバッチ ID を指定していることを確認してください。詳しくは、プロファイル開発者ガイドの[削除リクエストの作成](./api/profile-system-jobs.md#create-a-delete-request)に関する節を参照してください。

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

このエラーは、プロファイル データの削除リクエストを作成しようとしたときに、有効なバッチが見つからなかった場合に発生します。再試行する前に、プロファイル対応データセットの正しい ID を入力したことを確認してください。

### 投影先がまだ作成されていません

```json
{
  "status":404,
  "title":"The projection destination has not yet been created.",
  "type":"http://ns.adobe.com/adobecloud/problem/missing-entity"
}
```

このエラーは、`POST /config/projections` リクエストで指定した `destinationId` が無効な場合に発生します。再試行する前に、有効な宛先 ID を指定したことを再確認してください。新しい宛先を作成するには、[プロファイル開発者ガイド](./api/edge-projections.md#create-a-destination)に記載されている手順に従ってください。

### サポートされていないメディアタイプ

```json
{
  "status": 415,
  "title": "HTTP 415 Unsupported Media Type",
  "type": "http://ns.adobe.com/adobecloud/problem/unsupported-media-type"
}
```

このエラーは、無効な Content-Type ヘッダーを含む POST または PUT リクエストを送信したときに発生します。使用しているエンドポイントに有効な Content-Type 値を指定していることを再確認してください。

ほとんどのプロファイルエンドポイントは、Content-Type ヘッダーで「application/json」を使用できますが、次の例外があります。

| エンドポイント | Content-Type |
| --- | --- |
| `/config/projections` | application/vnd.adobe.platform.projectionConfig+json; version=1 |
| `/config/destinations` | application/vnd.adobe.platform.projectionDestination+json; version=1 |
