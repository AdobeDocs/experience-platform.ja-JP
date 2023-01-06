---
keywords: Experience Platform；ホーム；人気の高いトピック；取り込んだデータ；トラブルシューティング；faq；取り込み；バッチ取り込み；バッチ取り込み；
solution: Experience Platform
title: バッチ取得トラブルシューティングガイド
description: このドキュメントは、Adobe Experience Platform バッチデータ取得 API に関するよくある質問に答えるのに役立ちます。
exl-id: 0a750d7e-a4ee-4a79-a697-b4b732478b2b
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 87%

---

# バッチ取得トラブルシューティングガイド

このドキュメントは、Adobe Experience Platformに関するよくある質問への回答に役立ちます [!DNL Batch Data Ingestion] API

## バッチ API 呼び出し

### バッチは、CompleteBatch API から HTTP 200 OK を受け取った後すぐにアクティブになりますか？

API からの `200 OK` の応答は、バッチの処理が受理されたことを意味します。バッチは、「アクティブ」や「失敗」などの最終状態にトランジションするまでアクティブになりません。

### 失敗した後で CompleteBatch API 呼び出しを再試行しても問題はありませんか？

はい — API 呼び出しを再試行しても安全です。失敗したにもかかわらず、操作が実際に成功し、バッチが正常に受け入れられた可能性があります。ただし、クライアントには API に障害が発生した場合に備えた再試行メカニズムが必要となり、実際に再試行は推奨されています。操作が実際に成功した場合、API は再試行後も成功を返します。

### Large File Upload API は、いつ使用する必要がありますか？

Large File Upload API を使用する場合の推奨ファイルサイズは 256 MB 以上です。Large File Upload API の使用方法について詳しくは、[こちら](./api-overview.md#ingest-large-parquet-files)を参照してください。

### Large File Complete API 呼び出しが失敗するのはなぜですか？

大きなファイルのチャンクが重なっているか、見つからない場合、サーバーは HTTP 400（不正なリクエスト）を返します。これは、ファイルチャンクが繋がり合わされる際に、範囲検証がファイルの完了時に行われるので、重なり合うチャンクをアップロードできる場合があるためです。

## 取得サポート

### サポートされている取得形式を教えてください。

現在、Parquet と JSON の両方がサポートされています。CSV はレガシーベースでサポートされています。データはマスターにプロモーションされ、事前確認が行われますが、コンバージョン、パーティション化、行の検証などの最新の機能はサポートされません。

### バッチ入力形式はどこで指定しますか。

入力形式は、ペイロード内でのバッチ作成時に指定する必要があります。バッチ入力形式の指定方法の例を次に示します。

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json"
           }
    }'
```

### アップロードされたデータがデータセットに表示されないのはなぜですか？

データをデータセットに表示するには、バッチが完了とマークされている必要があります。 取り込むすべてのファイルをアップロードしてから、バッチが完了とマークする必要があります。 バッチを完了としてマークする例を次に示します。

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches/{BATCH_ID}?action=COMPLETE" \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

### 複数行の JSON はどのように取得されますか？

複数行の JSON を取得するには、バッチ作成時に `isMultiLineJson` フラグを設定する必要があります。この例を次に示します。

```shell
curl -X POST "https://platform.adobe.io/data/foundation/import/batches" \
  -H "accept: application/json" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
  -d '{
          "datasetId": "{DATASET_ID}",
           "inputFormat": {
                "format": "json",
                "isMultiLineJson": true
           }
      }'
```

### JSON 行（1 行 JSON）と複数行 JSON の違いを教えてください。

JSON 行の場合、1 行に 1 つの JSON オブジェクトがあります。以下に例を示します。

```json
{"string":"string1","int":1,"array":[1,2,3],"dict": {"key": "value1"}}
{"string":"string2","int":2,"array":[2,4,6],"dict": {"key": "value2"}}
{"string":"string3","int":3,"array":[3,6,9],"dict": {"key": "value3", "extra_key": "extra_value3"}}
```

複数行の JSON の場合、1 つのオブジェクトが複数行を占め、すべてのオブジェクトが JSON 配列に含まれます。以下に例を示します。

```json
[
    {"string":"string1","int":1,"array":[1,2,3],"dict": {"key": "value1"}},
    {"string":"string2","int":2,"array":[2,4,6],"dict": {"key": "value2"}},
    {
        "string": "string3",
        "int": 3,
        "array": [
            3,
            6,
            9
        ],
        "dict": {
            "key": "value3",
            "extra_key": "extra_value3"
        }
    }
]
```

デフォルトでは、 [!DNL Batch Data Ingestion] は 1 行の JSON を使用します。

### CSV の取得はサポートされていますか？

CSV の取得は、フラットなスキーマのみサポートされています。現在、CSV での階層データの取り込みはサポートされていません。

すべてのデータ取得機能を使用するには、JSON または Parquet 形式が必要です。

### データに対して実行される検証のタイプを教えてください。

データに対して実行される検証には、次の 3 つのレベルがあります。

- スキーマ - バッチ取得を使用すると、取得済みデータのスキーマがデータセットのスキーマと一致するようになります。
- データタイプ — バッチ取得では、取得された各フィールドのタイプが、データセットのスキーマで定義されたタイプと一致するようになります。
- 制約 — バッチ取得では、「必須」、「列挙」、「形式」などの制約がスキーマ定義で適切に定義されるようにします。

### 既に取得されたバッチを置き換える方法を教えてください。

既に取得されたバッチは、バッチ再生機能を使用して置き換えることができます。バッチ再生の詳細については、[こちら](./api-overview.md#replay-a-batch)を参照してください。

### バッチ取得はどのように監視されますか？

バッチがバッチプロモーション用に通知されたら、次のリクエストでバッチ取得の進行状況を監視できます。

```shell
curl -X GET "https://platform.adobe.io/data/foundation/catalog/batches/{BATCH_ID}" \
  -H "x-gw-ims-org-id: {ORG_ID}" \
  -H "Authorization: Bearer {ACCESS_TOKEN}" \
  -H "x-api-key: {API_KEY}"
```

このリクエストを使用すると、次のような応答が返されます。

```http
200 OK
```

```json
{
    "{BATCH_ID}":{
        "imsOrg":"{ORG_ID}",
        "created":1494349962314,
        "createdClient":"{API_KEY}",
        "createdUser":"{USER_ID}",
        "updatedUser":"{USER_ID}",
        "completed":1494349963467,
        "externalId":"{EXTERNAL_ID}",
        "status":"staging",
        "errors":[],
    }
}
```

## バッチ状態

### 可能なバッチ状態を教えてください。

バッチは、そのライフサイクルで、次のステータスを経由できます。

| ステータス | マスターに書き込まれたデータ | 説明 |
| ------ | ---------------------- | ----------- |
| Abandoned |  | クライアントは、予想された時間枠でバッチを完了できませんでした。 |
| Aborted |  | クライアントが、 [!DNL Batch Data Ingestion] API：指定したバッチの中止操作。 Loaded 状態になったバッチは中止することはできません。 |
| Active／Success | x | バッチはステージからマスターに正常にプロモートされ、ダウンストリーム消費で使用できるようになりました。**注意**：「Active」と「Success」は同じ意味で使用されます。 |
| Archived |  | バッチはコールドストレージにアーカイブされています。 |
| Failed／Failure |  | 不良な設定または不正なデータ、あるいはその両方から生じる端末の状態。クライアントがデータを修正して再送信できるように、実行可能なエラーはバッチと共に記録されます。**注意**：「Failed」と「Failure」は同じ意味で使用されます。 |
| Inactive | x | バッチは正常にプロモーションされましたが、元に戻されたか、期限が切れています。バッチはダウンストリーム消費に使用できなくなりますが、基になるデータは、保持、アーカイブまたはその他の方法で削除されるまでマスターに残ります。 |
| Loading |  | クライアントは現在、バッチのデータを書き込んでいます。この時点では、バッチはプロモーションの準備ができて&#x200B;**いません**。 |
| Loaded |  | クライアントはバッチのデータの書き込みを完了しました。バッチはプロモーションの準備ができました。 |
| Retained |  | データはマスターから取り出され、Adobe Data Lake の指定されたアーカイブに保存されます。 |
| Staging |  | クライアントはプロモーション用のバッチに正常に署名しました。データはダウンストリームでの消費用にステージングされています。 |
| Retrying |  | クライアントはプロモーション用のバッチに署名しましたが、エラーが原因で、バッチ監視サービスによってバッチが再試行されています。この状態を使用して、データの取り込みに遅延が生じる可能性があることをクライアントに通知できます。 |
| Stalled |  | クライアントがプロモーション用のバッチに署名しましたが、バッチ監視サービスによる `n` 回の再試行の後、バッチプロモーションが停止しました。 |

### バッチの「Staging」とは何を意味しますか？

バッチが「ステージング」の場合、それは、バッチがプロモーションのために正常に通知され、データがダウンストリームで消費するためにステージングされていることを意味します。

### バッチの「Retrying」とは何を意味しますか？

バッチが「再試行中」の場合は、断続的な問題が原因でバッチのデータ取得が一時的に停止しています。この場合、顧客の介入は必要ありません。

### バッチの「Stalled」とは何を意味しますか？

バッチが「Stalled」の場合は、 [!DNL Data Ingestion Services] バッチの取り込みが困難で、すべての再試行が使い果たされています。

### バッチの「Loading」とは何を意味しますか？

バッチが「読み込み中」の場合、CompleteBatch API が呼び出されていないので、バッチをプロモートします。

### バッチが正常に取得されたかどうかを知る方法はありますか？

バッチのステータスが「Active」であれば、バッチは正常に取得されています。バッチのステータスを確認するには、[前述](#how-is-batch-ingestion-monitored)の手順に従います。

### バッチが失敗した後はどうなりますか？

バッチが失敗した場合、失敗の理由はペイロードの `errors` セクションで識別できます。エラーの例を次に示します。

```json
    "errors":[
        {
            "code":"106",
            "description":"Dataset file is empty. Please upload file with data.",
            "rows":[]
        },
        {
            "code":"118",
            "description":"CSV file contains empty header row.",
            "rows":[]
        }
    ]
```

エラーが修正されると、バッチを再度アップロードできます。

## バッチサポート

### バッチを削除する方法を教えてください。

から直接削除する代わりに、 [!DNL Catalog]、バッチは、次のいずれかの方法を使用して削除する必要があります。

1. バッチが進行中の場合は、バッチを中止する必要があります。
2. バッチが正常にマスターされた場合は、バッチを元に戻す必要があります。

### 使用できるバッチレベルの指標を教えてください。

Active／Success 状態のバッチに対しては、次のバッチレベルの指標を使用できます。

| 指標 | 説明 |
| ------ | ----------- |
| inputByteSize | に対してステージングされたバイトの合計数です [!DNL Data Ingestion Services] 処理する |
| inputRecordSize | に対してステージングされた行の合計数 [!DNL Data Ingestion Services] 処理する |
| outputByteSize | が出力したバイトの合計数 [!DNL Data Ingestion Services] から [!DNL Data Lake]. |
| outputRecordSize | が出力した行の合計数 [!DNL Data Ingestion Services] から [!DNL Data Lake]. |
| partitionCount | に書き込まれたパーティションの合計数 [!DNL Data Lake]. |

### 指標が一部のバッチで使用できないのはなぜですか？

指標がバッチで使用できない理由は 2 つあります。

1. バッチは、Active／Success の状態に正常に到達していません。
2. バッチは、CSV 取得などの従来のプロモーションパスを使用してプロモーションされました。

### 異なるステータスコードの意味を教えてください。

| ステータスコード | 説明 |
| ----------- | ----------- |
| 106 | データセットファイルが空です。 |
| 118 | CSV ファイルに空のヘッダー行が含まれています。 |
| 200 | バッチの処理が受け入れられ、「Active」や「Failure」などの最終的な状態にトランジションされます。送信されたバッチは、`GetBatch` エンドポイントを使用して監視できます。 |
| 400 | 不正なリクエストです。バッチ内に不足しているか、重複しているチャンクがある場合に返されます。 |

[large-file-upload]: batch_data_ingestion_developer_guide.md#how-to-ingest-large-parquet-files
