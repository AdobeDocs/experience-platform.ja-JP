---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、プレビュー、サンプル
title: サンプルステータスのプレビュー（プロファイルプレビュー）API エンドポイント
description: リアルタイム顧客プロファイル API のプレビューサンプルステータスエンドポイントを使用すると、プロファイルデータの最新の成功例をプレビューし、データセットと ID 別にプロファイル配分をリストし、データセットの重複、ID の重複、未関連付けプロファイルを示すレポートを生成できます。
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: a6173860adda4bd71c94750e5cce6dd4cbe820c6
workflow-type: tm+mt
source-wordcount: '2874'
ht-degree: 5%

---

# サンプルステータスエンドポイントをプレビュー（プロファイルプレビュー）

Adobe Experience Platformを使用すると、複数のソースから顧客データを取り込んで、個々の顧客に対して堅牢で統合されたプロファイルを作成できます。 データが Platform に取り込まれると、サンプルジョブが実行され、プロファイル数やその他のリアルタイム顧客プロファイルのデータ関連指標が更新されます。

このサンプルジョブの結果は、 `/previewsamplestatus` エンドポイント（リアルタイム顧客プロファイル API の一部） このエンドポイントは、データセットと ID 名前空間の両方によるプロファイル配分のリストを作成したり、組織のプロファイルストアの構成を可視化できる複数のレポートを生成したりする場合にも使用できます。 このガイドでは、 `/previewsamplestatus` API エンドポイント。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service API の一部として使用できる推定およびプレビューエンドポイントがあり、期待されるオーディエンスを確実に特定するのに役立つ、セグメント定義に関する概要レベルの情報を表示できます。 セグメントのプレビューおよび推定エンドポイントを使用する手順の詳細については、 [プレビューおよび予測エンドポイントガイド](../../segmentation/api/previews-and-estimates.md)( [!DNL Segmentation] API 開発者ガイド。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile] API](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合プロファイル

このガイドでは、「プロファイルフラグメント」と「結合プロファイル」の両方を参照します。 先に進む前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたいでブランドとやり取りする場合、組織では、その 1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示される可能性があります。

プロファイルフラグメントが Platform に取り込まれると、その顧客の単一のプロファイルを作成するために、プロファイルフラグメントが（結合ポリシーに基づいて）結合されます。 したがって、各プロファイルは複数のフラグメントで構成されるので、プロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

Experience Platform内でのプロファイルとその役割の詳細については、まず [リアルタイム顧客プロファイルの概要](../home.md).

## サンプルジョブのトリガー方法

リアルタイム顧客プロファイルで有効なデータがに取り込まれると、 [!DNL Platform]の場合は、プロファイルデータストア内に保存されます。 レコードのプロファイルストアへの取り込みがプロファイルの合計数の 5%以上増加または減少すると、サンプリングジョブがトリガーされ、カウントが更新されます。 サンプルのトリガー方法は、使用する取り込みのタイプによって異なります。

* の場合 **ストリーミングデータワークフロー**&#x200B;に設定されている場合、5%の増減しきい値に達したかどうかを確認するチェックが 1 時間ごとにおこなわれます。 存在する場合は、サンプルジョブが自動的にトリガーされ、カウントが更新されます。
* の場合 **バッチ取得**、バッチをプロファイルストアに正常に取り込んだ 15 分以内に、5%の増減しきい値に達した場合は、ジョブが実行されてカウントが更新されます。 プロファイル API を使用すると、成功した最新のサンプルジョブのほか、データセット別、ID 名前空間別のプロファイル配布のリストを表示できます。

プロファイル数と名前空間別のプロファイルは、 [!UICONTROL プロファイル] Experience PlatformUI の UI を使用してプロファイルデータにアクセスする方法について詳しくは、 [[!DNL Profile] UI ガイド](../ui/user-guide.md).

## 最後のサンプルステータスを表示 {#view-last-sample-status}

に対してGETリクエストを実行できます `/previewsamplestatus` エンドポイント：IMS 組織で最後に実行された成功したサンプルジョブの詳細を表示します。 これには、サンプル内のプロファイルの合計数、プロファイル数指標、またはExperience Platform内に組織が持つプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客ごとに 1 つのプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントを結合すると、すべて同じ個人に関連しているので、「1」プロファイルの数が返されます。

また、プロファイル数には、属性（レコードデータ）を持つプロファイルと、時系列（イベント）データのみを含むプロファイル (Adobe Analyticsプロファイルなど ) の両方が含まれます。 Platform 内の最新の合計プロファイル数を提供するために、サンプルジョブは、プロファイルデータが取り込まれると定期的に更新されます。

**API 形式**

```http
GET /previewsamplestatus
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、組織で最後に実行された正常なサンプルジョブの詳細が含まれます。

>[!NOTE]
>
>この例の応答では、 `numRowsToRead` および `totalRows` は互いに等しい。 組織がExperience Platformするプロファイルの数に応じて、このような場合があります。 ただし、通常、これら 2 つの数は異なり、 `numRowsToRead` は小さい数です。これは、サンプルをプロファイルの合計数のサブセットとして表す (`totalRows`) をクリックします。

```json
{
  "numRowsToRead": "41003",
  "sampleJobRunning": {
    "status": true,
    "submissionTimestamp": "2020-08-01 17:57:57.0"
  },
  "cosmosDocCount": "\"300803\"",
  "totalFragmentCount": 47429,
  "lastSuccessfulBatchTimestamp": "\"null\"",
  "streamingDriven": "\"false\"",
  "totalRows": "41003",
  "lastBatchId": "\"null\"",
  "status": "TASK_FINISHED",
  "samplingRatio": 1.0,
  "mergeStrategy": "timestampOrdered_auto",
  "lastSampledTimestamp": "2020-08-01 17:57:57.0"
}
```

| プロパティ | 説明 |
|---|---|
| `numRowsToRead` | サンプル内の結合プロファイルの合計数です。 |
| `sampleJobRunning` | を返す boolean 値です。 `true` サンプルジョブが進行中の場合。 バッチファイルが実際にプロファイルストアに追加された時点からに発生する遅延の透明性を提供します。 |
| `cosmosDocCount` | Cosmos のドキュメント数の合計です。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数。 |
| `lastSuccessfulBatchTimestamp` | 前回成功したバッチ取り込みのタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨となり、応答に対する重要性はありません。* |
| `totalRows` | Experience Platform内の結合プロファイルの合計数（「プロファイル数」とも呼ばれます）。 |
| `lastBatchId` | 前回のバッチ取り込み ID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | サンプリングされた結合プロファイルの割合 (`numRowsToRead`) を結合済みプロファイルの合計 (`totalRows`) に割合を表します。 |
| `mergeStrategy` | サンプルで使用される結合方法。 |
| `lastSampledTimestamp` | 前回成功したサンプルタイムスタンプ。 |

## データセット別のプロファイル配布のリスト

データセットごとのプロファイルの分布を確認するには、次の場所で `/previewsamplestatus/report/dataset` endpoint.

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日に複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（見つかりません）エラーが返されます。 日付が指定されていない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD です。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` 指定した日付の最新のレポートを返すためのパラメーター。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset?date=2020-08-01 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、 `data` 配列に含まれるデータセットオブジェクトのリスト。 表示される応答は、3 つのデータセットを表示するために切り捨てられました。

>[!NOTE]
>
>その日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付のデータセットレポートが存在しない場合は、HTTP ステータス 404（見つかりません）が返されます。

```json
{
  "data": [
    {
      "sampleCount": 12577,
      "samplePercentage": 0.306734,
      "fullIDsCount": 20988,
      "fullIDsPercentage": 0.511865,
      "name": "CRM Profiles",
      "description": "Profiles from the CRM.",
      "value": "5f160106be34361915754b9c",
      "streamingIngestionEnabled": "",
      "createdUser": "{CREATED_USER}",
      "reportTimestamp": "2020-08-01T17:57:58.697",
    },
    {
      "sampleCount": 12938,
      "samplePercentage": 0.315538,
      "fullIDsCount": 21796,
      "fullIDsPercentage": 0.531571,
      "name": "AAM Authenticated Profiles",
      "description": "This data set contains AAM authenticated profiles.",
      "value": "5dc77ec6eed47f18a796ca90",
      "streamingIngestionEnabled": "",
      "createdUser": "{CREATED_USER}",
      "reportTimestamp": "2020-08-01T17:57:58.697"
    },
    {
      "sampleCount": 22725,
      "samplePercentage": 0.554228,
      "fullIDsCount": 41003,
      "fullIDsPercentage": 1.0,
      "name": "Loyalty Program Members",
      "description": "Members of the loyalty program at all levels.",
      "value": "5d0fda92274e55144d4de620",
      "streamingIngestionEnabled": "",
      "createdUser": "{CREATED_USER}",
      "reportTimestamp": "2020-08-01T17:57:58.697"
    }
  ],
  "reportTimestamp": "2020-08-01T17:57:58.697"
}
```

| プロパティ | 説明 |
|---|---|
| `sampleCount` | このデータセット ID を持つサンプル済み結合プロファイルの合計数。 |
| `samplePercentage` | この `sampleCount` を、サンプル済み結合プロファイルの合計数 ( `numRowsToRead` 返される値 [最終サンプルステータス](#view-last-sample-status)) で表されます。 |
| `fullIDsCount` | このデータセット ID を持つ結合プロファイルの合計数です。 |
| `fullIDsPercentage` | この `fullIDsCount` を、結合されたプロファイルの合計数 ( `totalRows` 返される値 [最終サンプルステータス](#view-last-sample-status)) で表されます。 |
| `name` | データセットの名前。データセットの作成時に指定されます。 |
| `description` | データセットの説明。データセットの作成時に指定されます。 |
| `value` | データセットの ID。 |
| `streamingIngestionEnabled` | データセットのストリーミング取り込みが有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザー ID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 次の場合、 `date` パラメーターがリクエスト中に指定された場合、返されるレポートは指定された日付です。 指定しない場合 `date` パラメーターが指定された場合は、最新のレポートが返されます。 |

## ID 名前空間別のプロファイル配分のリスト

に対してGETリクエストを実行できます `/previewsamplestatus/report/namespace` エンドポイントを使用して、プロファイルストア内のすべての結合プロファイルの id 名前空間による分類を表示できます。 これには、Adobeが提供する標準 ID と、組織が定義するカスタム ID の両方が含まれます。

ID 名前空間は、Adobe Experience Platform ID サービスの重要なコンポーネントで、顧客データが関連するコンテキストのインジケーターとして機能します。 詳しくは、まず [id 名前空間の概要](../../identity-service/namespaces.md).

>[!NOTE]
>
>1 つのプロファイルが複数の名前空間に関連付けられている可能性があるので、名前空間別のプロファイルの合計数（名前空間ごとに表示される値を加算）は、プロファイル数指標より多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日に複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（見つかりません）エラーが返されます。 日付が指定されていない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD です。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` パラメーターと同じ値を持つため、最新のレポートが返されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、 `data` 配列を作成します。各名前空間の詳細が格納される個々のオブジェクトが含まれます。 表示される応答は、4 つの名前空間を表示するために切り捨てられました。

```json
{
  "data": [
    {
      "sampleCount": 12148,
      "samplePercentage": 0.296271,
      "reportTimestamp": "2020-08-01T17:57:58.697",
      "fullIDsFragmentCount": 13141,
      "fullIDsCount": 12631,
      "fullIDsPercentage": 0.308051,
      "code": "Email",
      "value": "6"
    },
    {
      "sampleCount": 6989,
      "samplePercentage": 0.170451,
      "reportTimestamp": "2020-08-01T17:57:58.697",
      "fullIDsFragmentCount": 7543,
      "fullIDsCount": 7042,
      "fullIDsPercentage": 0.171744,
      "code": "ECID",
      "value": "4"
    },
    {
      "sampleCount": 888,
      "samplePercentage": 0.021657,
      "reportTimestamp": "2020-08-01T17:57:58.697",
      "fullIDsFragmentCount": 3801,
      "fullIDsCount": 3206,
      "fullIDsPercentage": 0.078189,
      "code": "AAID",
      "value": "10"
    },
    {
      "sampleCount": 21809,
      "samplePercentage": 0.531888,
      "reportTimestamp": "2020-08-01T17:57:58.697",
      "fullIDsFragmentCount": 27023,
      "fullIDsCount": 21936,
      "fullIDsPercentage": 0.534985,
      "code": "Phone",
      "value": "7"
    }
  ],
  "reportTimestamp": "2020-08-01T17:57:58.697"
}
```

| プロパティ | 説明 |
|---|---|
| `sampleCount` | 名前空間内のサンプル済み結合プロファイルの合計数です。 |
| `samplePercentage` | この `sampleCount` を、サンプルされた結合プロファイル ( `numRowsToRead` 返される値 [最終サンプルステータス](#view-last-sample-status)) で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 次の場合、 `date` パラメーターがリクエスト中に指定された場合、返されるレポートは指定された日付です。 指定しない場合 `date` パラメーターが指定された場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間内の結合プロファイルの合計数です。 |
| `fullIDsPercentage` | この `fullIDsCount` 合計結合プロファイル ( `totalRows` 返される値 [最終サンプルステータス](#view-last-sample-status)) で表されます。 |
| `code` | この `code` 名前空間用。 これは、 [Adobe Experience Platform ID サービス API](../../identity-service/api/list-namespaces.md) また、 [!UICONTROL ID シンボル] Experience PlatformUI 詳しくは、 [id 名前空間の概要](../../identity-service/namespaces.md). |
| `value` | この `id` 名前空間の値。 これは、 [ID サービス API](../../identity-service/api/list-namespaces.md). |

## データセットの重複レポートの生成

データセット重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も貢献するデータセットを公開することで、組織のプロファイルストアの構成を可視化できます。 このレポートは、データに関するインサイトを提供するだけでなく、特定のデータセットの有効期限を設定するなど、ライセンスの使用状況を最適化するためのアクションを実行するのに役立ちます。

データセットの重複レポートは、 `/previewsamplestatus/report/dataset/overlap` endpoint.

コマンドラインまたはPostman UI を使用してデータセットの重複レポートを生成する手順については、 [データセット重複レポートの生成に関するチュートリアル](../tutorials/dataset-overlap-report.md).

**API 形式**

```http
GET /previewsamplestatus/report/dataset/overlap
GET /previewsamplestatus/report/dataset/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付で複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（見つかりません）エラーが返されます。 日付が指定されていない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD です。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` 指定した日付の最新のレポートを返すためのパラメーター。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200(OK) とデータセット重複レポートが返されます。

```json
{
    "data": {
        "5d92921872831c163452edc8,5da7292579975918a851db57,5eb2cdc6fa3f9a18a7592a98": 123,
        "5d92921872831c163452edc8,5eb2cdc6fa3f9a18a7592a98": 454412,
        "5eeda0032af7bb19162172a7": 107
    },
    "reportTimestamp": "2021-12-29T19:55:31.147"
}
```

| プロパティ | 説明 |
|---|---|
| `data` | この `data` オブジェクトには、データセットのコンマ区切りリストと、それぞれのプロファイル数が含まれます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 次の場合、 `date` パラメーターがリクエスト中に指定された場合、返されるレポートは指定された日付です。 指定しない場合 `date` パラメーターが指定された場合は、最新のレポートが返されます。 |

### データセットの重複レポートの解釈

レポートの結果は、応答のデータセットとプロファイル数から解釈できます。 次のサンプルレポートについて考えてみましょう。 `data` オブジェクト：

```json
  "5d92921872831c163452edc8,5da7292579975918a851db57,5eb2cdc6fa3f9a18a7592a98": 123,
  "5d92921872831c163452edc8,5eb2cdc6fa3f9a18a7592a98": 454412,
  "5eeda0032af7bb19162172a7": 107
```

このレポートには、次の情報が表示されます。

* 次のデータセットからのデータで構成される 123 個のプロファイルがあります。 `5d92921872831c163452edc8`, `5da7292579975918a851db57`, `5eb2cdc6fa3f9a18a7592a98`.
* 次の 2 つのデータセットからのデータで構成される 454,412 個のプロファイルがあります。 `5d92921872831c163452edc8` および `5eb2cdc6fa3f9a18a7592a98`.
* データセットのデータのみから構成される 107 個のプロファイルがあります `5eeda0032af7bb19162172a7`.
* 組織には、合計 454,642 個のプロファイルがあります。

## ID 名前空間の重複レポートの生成 {#identity-overlap-report}

ID 名前空間重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も貢献する ID 名前空間を公開することで、組織のプロファイルストアの構成を視覚的に確認できます。 これには、Adobeが提供する標準 ID 名前空間と、組織が定義したカスタム ID 名前空間の両方が含まれます。

ID 名前空間の重複レポートは、 `/previewsamplestatus/report/namespace/overlap` endpoint.

**API 形式**

```http
GET /previewsamplestatus/report/namespace/overlap
GET /previewsamplestatus/report/namespace/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付で複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（見つかりません）エラーが返されます。 日付が指定されていない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD です。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` 指定した日付の最新のレポートを返すためのパラメーター。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200(OK) と ID 名前空間の重複レポートが返されます。

```json
{
    "data": {
        "Email,crmid,loyal": 2,
        "ECID,Email,crmid": 7,
        "ECID,Email,mobilenr": 12,
        "AAID,ECID,loyal": 1,
        "mobilenr": 25,
        "AAID,ECID": 1508,
        "ECID,crmid": 1,
        "AAID,ECID,crmid": 2,
        "Email,crmid": 328,
        "CORE": 49,
        "AAID": 446,
        "crmid,loyal": 20988,
        "Email": 10904,
        "crmid": 249,
        "ECID,Email": 74,
        "Phone": 40,
        "Email,Phone,loyal": 48,
        "AAID,AVID,ECID": 85,
        "Email,loyal": 1002,
        "AAID,ECID,Email,Phone,crmid": 5,
        "AAID,ECID,Email,crmid,loyal": 23,
        "AAID,AVID,ECID,Email,crmid": 2,
        "AVID": 3,
        "AAID,ECID,Phone": 1,
        "loyal": 43,
        "ECID,Email,crmid,loyal": 6,
        "AAID,ECID,Email,Phone,crmid,loyal": 1,
        "AAID,ECID,Email": 2,
        "AAID,ECID,Email,crmid": 142,
        "AVID,ECID": 24,
        "ECID": 6565
    },
    "reportTimestamp": "2021-12-29T16:55:03.624"
}
```

| プロパティ | 説明 |
|---|---|
| `data` | この `data` オブジェクトには、id 名前空間コードの一意の組み合わせと、それぞれのプロファイル数を含む、コンマ区切りのリストが含まれます。 |
| 名前空間コード | この `code` は、各 id 名前空間名の短い形式です。 各 `code` その `name` は [Adobe Experience Platform ID サービス API](../../identity-service/api/list-namespaces.md). この `code` は、 [!UICONTROL ID シンボル] Experience PlatformUI 詳しくは、 [id 名前空間の概要](../../identity-service/namespaces.md). |
| `reportTimestamp` | レポートのタイムスタンプ。 次の場合、 `date` パラメーターがリクエスト中に指定された場合、返されるレポートは指定された日付です。 指定しない場合 `date` パラメーターが指定された場合は、最新のレポートが返されます。 |

### ID 名前空間の重複レポートの解釈

レポートの結果は、応答の ID とプロファイル数から解釈できます。 各行の数値は、標準 ID 名前空間とカスタム ID 名前空間の正確な組み合わせで構成されるプロファイルの数を示します。

以下の抜粋を考えてみましょう。 `data` オブジェクト：

```json
  "AAID,ECID,Email,crmid": 142,
  "AVID,ECID": 24,
  "ECID": 6565
```

このレポートには、次の情報が表示されます。

* 次の要素で構成される 142 個のプロファイルがあります `AAID`, `ECID`、および `Email` 標準 ID とカスタム ID `crmid` id 名前空間。
* 次の要素で構成される 24 個のプロファイルがあります `AAID` および `ECID` ID 名前空間。
* 6,565 個のプロファイルに、 `ECID` id.

## 未関連付けプロファイルレポートの生成

未関連付けプロファイルレポートを使用すると、組織のプロファイルストアの構成をより明確に把握できます。 「未関連付け」プロファイルは、1 つのプロファイルフラグメントのみを含むプロファイルです。 「不明」プロファイルは、偽名の ID 名前空間（例： ）に関連付けられたプロファイルです。 `ECID` および `AAID`. 不明なプロファイルは非アクティブで、指定された期間、新しいイベントが追加されていないことを意味します。 未関連付けプロファイルレポートは、7 日、30 日、60 日、90 日、120 日のプロファイルの分類を提供します。

未関連付けプロファイルレポートを生成するには、 `/previewsamplestatus/report/unstitchedProfiles` endpoint.

**API 形式**

```http
GET /previewsamplestatus/report/unstitchedProfiles
```

**リクエスト**

次のリクエストは、未関連付けプロファイルレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/unstitchedProfiles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200(OK) と未関連付けプロファイルレポートが返されます。

>[!NOTE]
>
>このガイドの目的のため、レポートは以下のみを含むように切り捨てられました： `"120days"` および」`7days`」の期間が含まれます。 未関連付けプロファイルの完全なレポートは、7 日、30 日、60 日、90 日、120 日のプロファイルの分類を表示します。

```json
{
  "data": {
      "totalNumberOfProfiles": 63606,
      "totalNumberOfEvents": 130977,
      "unstitchedProfiles": {
          "120days": {
              "countOfProfiles": 1644,
              "eventsAssociated": 26824,
              "nsDistribution": {
                  "Email": {
                      "countOfProfiles": 18,
                      "eventsAssociated": 95
                  },
                  "loyal": {
                      "countOfProfiles": 26,
                      "eventsAssociated": 71
                  },
                  "ECID": {
                      "countOfProfiles": 1600,
                      "eventsAssociated": 26658
                  }
              }
          },
          "7days": {
              "countOfProfiles": 1782,
              "eventsAssociated": 29151,
              "nsDistribution": {
                  "Email": {
                      "countOfProfiles": 19,
                      "eventsAssociated": 97
                  },
                  "ECID": {
                      "countOfProfiles": 1734,
                      "eventsAssociated": 28591
                  },
                  "loyal": {
                      "countOfProfiles": 29,
                      "eventsAssociated": 463
                  }
              }
          }
      }
  },
  "reportTimestamp": "2025-08-25T22:14:55.186"
}
```

| プロパティ | 説明 |
|---|---|
| `data` | この `data` オブジェクトには、未関連付けプロファイルレポートに対して返される情報が含まれます。 |
| `totalNumberOfProfiles` | プロファイルストア内の一意のプロファイルの合計数です。 これは、アドレス可能なオーディエンスの数と同じです。 これには、既知のプロファイルと未関連付けプロファイルの両方が含まれます。 |
| `totalNumberOfEvents` | プロファイルストア内の ExperienceEvents の合計数です。 |
| `unstitchedProfiles` | 未関連付けプロファイルの時間別分類を含むオブジェクト。 未関連付けプロファイルレポートは、7 日、30 日、60 日、90 日、120 日の期間のプロファイルの分類を表示します。 |
| `countOfProfiles` | その期間中の未関連付けプロファイルの数または名前空間の未関連付けプロファイルの数。 |
| `eventsAssociated` | 時間範囲の ExperienceEvents の数または名前空間のイベント数。 |
| `nsDistribution` | 個々の ID 名前空間と、各名前空間の未関連付けプロファイルとイベントの分布を含むオブジェクト。 注意：合計の追加 `countOfProfiles` ( `nsDistribution` オブジェクトが `countOfProfiles` を設定します。 同じことが～にも当てはまる `eventsAssociated` 名前空間ごとおよび合計 `eventsAssociated` 期間ごと。 |
| `reportTimestamp` | レポートのタイムスタンプ。 |

### 未関連付けプロファイルレポートの解釈

レポートの結果から、組織がプロファイルストア内に持つ未関連付けおよび非アクティブなプロファイルの数に関するインサイトを得ることができます。

以下の抜粋を考えてみましょう。 `data` オブジェクト：

```json
  "7days": {
    "countOfProfiles": 1782,
    "eventsAssociated": 29151,
    "nsDistribution": {
      "Email": {
        "countOfProfiles": 19,
        "eventsAssociated": 97
      },
      "ECID": {
        "countOfProfiles": 1734,
        "eventsAssociated": 28591
      },
      "loyal": {
        "countOfProfiles": 29,
        "eventsAssociated": 463
      }
    }
  }
```

このレポートには、次の情報が表示されます。

* 1,782 個のプロファイルに、1 つのプロファイルフラグメントのみが含まれ、過去 7 日間新しいイベントを持たない。
* 1,782 個の未関連付けプロファイルに関連付けられた 29,151 個の ExperienceEvents があります。
* ECID の ID 名前空間の単一のプロファイルフラグメントを含む、1,734 個の未関連付けプロファイルがあります。
* 1,734 個の未関連付けプロファイルに関連付けられた 28,591 イベントがあり、ECID の ID 名前空間の単一のプロファイルフラグメントを含んでいます。

## 次の手順

これで、プロファイルストアでサンプルデータをプレビューし、データに関する複数のレポートを実行する方法がわかったので、Segmentation Service API の推定エンドポイントとプレビューエンドポイントを使用して、セグメント定義に関する概要レベルの情報を表示できます。 この情報は、セグメント内の期待されるオーディエンスを確実に分離するのに役立ちます。 セグメント化 API を使用したセグメントプレビューと推定の操作について詳しくは、 [エンドポイントガイドのプレビューと推定](../../segmentation/api/previews-and-estimates.md).

