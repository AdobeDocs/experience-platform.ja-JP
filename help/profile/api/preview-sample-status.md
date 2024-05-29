---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；プレビュー；サンプル
title: プレビューサンプルのステータス（プロファイルプレビュー） API エンドポイント
description: リアルタイム顧客プロファイル API のプレビューサンプルステータスエンドポイントを使用すると、プロファイルデータの最新の成功したサンプルをプレビューしたり、データセット別および ID 別のプロファイル分布をリストしたり、データセットの重複、ID の重複、ステッチされていないプロファイルを示すレポートを生成したりできます。
role: Developer
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '2906'
ht-degree: 5%

---

# プレビューサンプルのステータスエンドポイント（プロファイルプレビュー）

Adobe Experience Platformでは、複数のソースから顧客データを取り込み、個々の顧客ごとに堅牢で統一されたプロファイルを作成できます。 データが Platform に取り込まれると、サンプルジョブが実行されて、プロファイル数やその他のリアルタイム顧客プロファイルデータ関連指標が更新されます。

このサンプルジョブの結果は、 `/previewsamplestatus` リアルタイム顧客プロファイル API の一部であるエンドポイント。 また、このエンドポイントを使用して、データセットと ID 名前空間の両方でプロファイル配布をリスト表示したり、複数のレポートを生成して組織のプロファイルストアの構成を可視化したりできます。 このガイドでは、を使用してこれらの指標を表示するために必要な手順について説明します。 `/previewsamplestatus` API エンドポイント。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service API の一部として使用できる推定およびプレビューエンドポイントがあり、セグメント定義に関する概要レベルの情報を表示して、期待されるオーディエンスを確実に分離できるようにします。 プレビューおよび推定エンドポイントを使用する詳細な手順については、を参照してください。 [プレビューと予測エンドポイントガイド](../../segmentation/api/previews-and-estimates.md)、の一部 [!DNL Segmentation] API デベロッパーガイド。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile] API](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合プロファイル

このガイドは、「プロファイルフラグメント」と「結合プロファイル」の両方を参照します。 続行する前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたいで自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示される可能性があります。

プロファイルフラグメントが Platform に取り込まれると、その顧客用に単一のプロファイルを作成するために、（結合ポリシーに基づいて）結合されます。 したがって、各プロファイルは複数のフラグメントで構成され、プロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

プロファイルとそのExperience Platform内での役割について詳しくは、まず次をお読みください [リアルタイム顧客プロファイルの概要](../home.md).

## サンプルジョブがトリガーされる方法

リアルタイム顧客プロファイルに対して有効なデータはに取り込まれます。 [!DNL Platform]を選択すると、プロファイルデータストア内に保存されます。 プロファイルストアへのレコードの取り込みが、合計プロファイル数を 5% 以上増加または減少させると、サンプリングジョブがトリガーされて数が更新されます。 サンプルをトリガーする方法は、使用する取り込みのタイプによって異なります。

* の場合 **ストリーミングデータワークフロー**&#x200B;の場合は、5% の増加または減少のしきい値に達したかどうかを判断するために、1 時間ごとにチェックが行われます。 サンプルジョブが自動的にトリガーされ、カウントが更新されます。
* の場合 **バッチ取り込み**&#x200B;では、プロファイルストアにバッチを正常に取り込んでから 15 分以内に、5% の増減のしきい値に達した場合は、カウントを更新するジョブを実行します。 プロファイル API を使用すると、最新の成功したサンプルジョブをプレビューできるほか、データセット別および ID 名前空間別のプロファイル配布をリストできます。

内では、プロファイル数および名前空間別プロファイル指標も使用できます [!UICONTROL プロファイル] Experience PlatformUI のセクション。 UI を使用してプロファイルデータにアクセスする方法について詳しくは、を参照してください。 [[!DNL Profile] UI ガイド](../ui/user-guide.md).

## 最後のサンプルステータスを表示 {#view-last-sample-status}

に対してGETリクエストを実行できます。 `/previewsamplestatus` 組織で最後に実行され、成功したサンプルジョブの詳細を表示するエンドポイント。 これには、サンプル内のプロファイルの合計数や、プロファイル数指標、組織がExperience Platform内に持つプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客の単一のプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントが結合されると、すべてが同じ個人に関連しているので、「1」のプロファイルのカウントが返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analytics プロファイルなどの時系列（イベント）データのみを含むプロファイルの両方も含まれます。 Platform 内に最新のプロファイルの合計数を提供するために、プロファイルデータが取り込まれると、サンプルジョブは定期的に更新されます。

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

応答には、組織に対して実行され、最後に成功したサンプルジョブの詳細が含まれます。

>[!NOTE]
>
>この例の応答では、 `numRowsToRead` および `totalRows` 互いに等しい。 組織がExperience Platformに持っているプロファイルの数に応じて、このケースが当てはまります。 ただし、通常、これら 2 つの数値は異なり、 `numRowsToRead` プロファイルの合計数のサブセットとしてサンプルを表すので小さい数（`totalRows`）に設定します。

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
| `numRowsToRead` | サンプル内の結合プロファイルの合計数。 |
| `sampleJobRunning` | を返すブール値 `true` サンプルジョブが進行中の場合。 バッチファイルがアップロードされてからプロファイルストアに実際に追加されるまでの待ち時間に対して、透明性を提供します。 |
| `cosmosDocCount` | Cosmos での合計ドキュメント数。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数。 |
| `lastSuccessfulBatchTimestamp` | 前回成功したバッチ取り込みタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨（廃止予定）となり、応答に重要な意味はありません。* |
| `totalRows` | Experience Platformでの結合プロファイルの合計数。「プロファイル数」とも呼ばれます。 |
| `lastBatchId` | 前回のバッチ取得 ID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | サンプリングされた結合プロファイルの割合（`numRowsToRead`）から結合プロファイルの合計（`totalRows`）に設定します。小数点形式でパーセンテージとして表します。 |
| `mergeStrategy` | サンプルで使用されている結合方法。 |
| `lastSampledTimestamp` | 前回成功したサンプルタイムスタンプ。 |

## データセット別のプロファイル分布のリスト

データセット別のプロファイルの分布を確認するには、に対してGETリクエストを実行します。 `/previewsamplestatus/report/dataset` エンドポイント。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD 例：`date=2024-12-31` |

**リクエスト**

次のリクエストは、 `date` パラメーターを指定して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset?date=2020-08-01 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には次が含まれます `data` データセットオブジェクトのリストを含む配列。 表示される応答は、3 つのデータセットを表示するために切り捨てられています。

>[!NOTE]
>
>日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付のデータセットレポートが存在しない場合は、HTTP ステータス 404 （見つかりません）が返されます。

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
| `sampleCount` | このデータセット ID を持つサンプリングされた結合プロファイルの合計数。 |
| `samplePercentage` | この `sampleCount` サンプリングされた結合プロファイルの合計数の割合（ `numRowsToRead` で返される値 [前回のサンプルステータス](#view-last-sample-status)）、10 進数形式で表されます。 |
| `fullIDsCount` | このデータセット ID を持つ結合プロファイルの合計数。 |
| `fullIDsPercentage` | この `fullIDsCount` 結合プロファイルの合計数の割合（ `totalRows` で返される値 [前回のサンプルステータス](#view-last-sample-status)）、10 進数形式で表されます。 |
| `name` | データセットの作成時に指定したデータセットの名前。 |
| `description` | データセットの作成時に提供される、データセットの説明。 |
| `value` | データセットの ID。 |
| `streamingIngestionEnabled` | データセットがストリーミング取得に対して有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザー ID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 If a `date` リクエスト中にパラメーターが指定されました。返されたレポートは指定された日付のです。 ない場合 `date` パラメーターを指定すると、最新のレポートが返されます。 |

## ID 名前空間別のプロファイル配分のリスト

に対してGETリクエストを実行できます。 `/previewsamplestatus/report/namespace` プロファイルストアにあるすべての結合済みプロファイルで ID 名前空間による分類を表示するエンドポイント。 これには、Adobeが提供する標準 ID と、組織が定義するカスタム ID の両方が含まれます。

ID 名前空間は、顧客データが関連するコンテキストのインジケーターとして機能するAdobe Experience Platform ID サービスの重要なコンポーネントです。 詳細については、まず次を読んでください [id 名前空間の概要](../../identity-service/features/namespaces.md).

>[!NOTE]
>
>1 つのプロファイルが複数の名前空間に関連付けられている可能性があるので、名前空間別のプロファイルの合計数（各名前空間に表示される値をまとめたもの）は、プロファイル数指標よりも多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` およびを指定すると、最新のレポートが返されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には次が含まれます `data` 各名前空間の詳細を含む個々のオブジェクトを持つ配列。 表示された応答は、4 つの名前空間を表示するために切り捨てられました。

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
| `sampleCount` | 名前空間内のサンプリングされた結合プロファイルの合計数です。 |
| `samplePercentage` | この `sampleCount` サンプリングされた結合プロファイルの割合（ `numRowsToRead` で返される値 [前回のサンプルステータス](#view-last-sample-status)）、10 進数形式で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 If a `date` リクエスト中にパラメーターが指定されました。返されたレポートは指定された日付のです。 ない場合 `date` パラメーターを指定すると、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間での結合プロファイルの合計数。 |
| `fullIDsPercentage` | この `fullIDsCount` 合計結合プロファイルの割合（ `totalRows` で返される値 [前回のサンプルステータス](#view-last-sample-status)）、10 進数形式で表されます。 |
| `code` | この `code` 名前空間の場合。 このフォルダーは、 [Adobe Experience Platform Identity Service API](../../identity-service/api/list-namespaces.md) およびは、以下とも呼ばれます [!UICONTROL ID シンボル] Experience PlatformUI で変更できます。 詳しくは、 [id 名前空間の概要](../../identity-service/features/namespaces.md). |
| `value` | この `id` 名前空間の値。 このフォルダーは、 [ID サービス API](../../identity-service/api/list-namespaces.md). |

## データセットの重複レポートの生成

データセット重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も寄与するデータセットを公開することで、組織のプロファイルストアの構成を可視化します。 このレポートは、データへのインサイトを提供するだけでなく、特定のデータセットの有効期限の設定など、ライセンスの使用を最適化するアクションを実行するのに役立ちます。

に対してデータセットリクエストを実行することで、GETセット重複レポートを生成できます。 `/previewsamplestatus/report/dataset/overlap` エンドポイント。

コマンドラインまたはPostman UI を使用してデータセット重複レポートを生成する手順については、 [データセット重複レポートの生成に関するチュートリアル](../tutorials/dataset-overlap-report.md).

**API 形式**

```http
GET /previewsamplestatus/report/dataset/overlap
GET /previewsamplestatus/report/dataset/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付に複数のレポートが実行された場合は、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD 例：`date=2024-12-31` |

**リクエスト**

次のリクエストは、 `date` パラメーターを指定して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200 （OK）とデータセット重複レポートが返されます。

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
| `data` | この `data` オブジェクトには、データセットとそれぞれのプロファイル数のコンマ区切りリストが含まれています。 |
| `reportTimestamp` | レポートのタイムスタンプ。 If a `date` リクエスト中にパラメーターが指定されました。返されたレポートは指定された日付のです。 ない場合 `date` パラメーターを指定すると、最新のレポートが返されます。 |

### データセット重複レポートの解釈

レポートの結果は、応答のデータセットとプロファイル数から解釈できます。 次のレポート例について考えてみます `data` オブジェクト :

```json
  "5d92921872831c163452edc8,5da7292579975918a851db57,5eb2cdc6fa3f9a18a7592a98": 123,
  "5d92921872831c163452edc8,5eb2cdc6fa3f9a18a7592a98": 454412,
  "5eeda0032af7bb19162172a7": 107
```

このレポートには、次の情報が表示されます。

* 次のデータセットからのデータで構成される 123 のプロファイルがあります。 `5d92921872831c163452edc8`, `5da7292579975918a851db57`, `5eb2cdc6fa3f9a18a7592a98`.
* 次の 2 つのデータセットからのデータで構成される 454,412 のプロファイルがあります。 `5d92921872831c163452edc8` および `5eb2cdc6fa3f9a18a7592a98`.
* データセットのデータのみで構成されるプロファイルが 107 個あります `5eeda0032af7bb19162172a7`.
* 組織内には合計 454,642 のプロファイルがあります。

## ID 名前空間の重複レポートの生成 {#identity-overlap-report}

ID 名前空間の重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も貢献する ID 名前空間を公開することで、組織のプロファイルストアの構成を可視化します。 これには、Adobeが提供する標準 ID 名前空間と、組織が定義するカスタム ID 名前空間の両方が含まれます。

に対してGETリクエストを実行することで、ID 名前空間重複レポートを生成できます。 `/previewsamplestatus/report/namespace/overlap` エンドポイント。

**API 形式**

```http
GET /previewsamplestatus/report/namespace/overlap
GET /previewsamplestatus/report/namespace/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付に複数のレポートが実行された場合は、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD 例：`date=2024-12-31` |

**リクエスト**

次のリクエストは、 `date` パラメーターを指定して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200 （OK）と ID 名前空間の重複レポートが返されます。

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
| `data` | この `data` オブジェクトには、id 名前空間コードとそれぞれのプロファイル数の一意の組み合わせを持つ、コンマ区切りのリストが含まれています。 |
| 名前空間コード | この `code` は、ID 名前空間名ごとの短い形式です。 各ののマッピング `code` その `name` は、を使用して検索できます [Adobe Experience Platform Identity Service API](../../identity-service/api/list-namespaces.md). この `code` は [!UICONTROL ID シンボル] Experience PlatformUI で変更できます。 詳しくは、 [id 名前空間の概要](../../identity-service/features/namespaces.md). |
| `reportTimestamp` | レポートのタイムスタンプ。 If a `date` リクエスト中にパラメーターが指定されました。返されたレポートは指定された日付のです。 ない場合 `date` パラメーターを指定すると、最新のレポートが返されます。 |

### ID 名前空間の重複レポートの解釈

レポートの結果は、応答内の ID とプロファイル数から解釈できます。 各行の数値は、標準 ID 名前空間とカスタム ID 名前空間の正確な組み合わせで構成されるプロファイルの数を示します。

以下の抜粋を考えてみましょう。 `data` オブジェクト :

```json
  "AAID,ECID,Email,crmid": 142,
  "AVID,ECID": 24,
  "ECID": 6565
```

このレポートには、次の情報が表示されます。

* 次の要素で構成される 142 のプロファイルがあります `AAID`, `ECID`、および `Email` 標準 ID とカスタムからの ID `crmid` id 名前空間。
* 次の要素で構成される 24 のプロファイルがあります `AAID` および `ECID` id 名前空間。
* 次のみを含むプロファイルが 6,565 あります。 `ECID` id。

## ステッチされていないプロファイルレポートの生成

ステッチされていないプロファイルレポートを通じて、組織のプロファイルストアの構成をさらに可視化できます。 「未ステッチ」プロファイルは、プロファイルフラグメントが 1 つのみ含まれるプロファイルです。 「不明」プロファイルは、次のような偽名 ID 名前空間に関連付けられたプロファイルです `ECID` および `AAID`. 不明なプロファイルは非アクティブです。つまり、指定された期間、新しいイベントが追加されていません。 ステッチされていないプロファイル レポートは、7、30、60、90、120 日間のプロファイルの分類を提供します。

ステッチされていないプロファイルレポートを生成するには、にGETリクエストを行います。 `/previewsamplestatus/report/unstitchedProfiles` エンドポイント。

**API 形式**

```http
GET /previewsamplestatus/report/unstitchedProfiles
```

**リクエスト**

次のリクエストは、ステッチされていないプロファイルレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/unstitchedProfiles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

リクエストが成功すると、HTTP ステータス 200 （OK）と、ステッチされていないプロファイルレポートが返されます。

>[!NOTE]
>
>このガイドの目的上、レポートは次のみを含むように切り捨てられています `"120days"` と」`7days`「期間。 完全な未ステッチプロファイル レポートは、7 日、30 日、60 日、90 日および 120 日間のプロファイルの分類を提供します。

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
| `data` | この `data` オブジェクトには、ステッチされていないプロファイルレポートに返された情報が含まれています。 |
| `totalNumberOfProfiles` | プロファイルストア内の一意のプロファイルの合計数。 これは、アドレス可能なオーディエンス数と同等です。 これには、既知のプロファイルと未ステッチのプロファイルの両方が含まれます。 |
| `totalNumberOfEvents` | プロファイルストア内の ExperienceEvents の合計数。 |
| `unstitchedProfiles` | ステッチされていないプロファイルの時間帯別の分類を含むオブジェクト。 未関連付けプロファイルレポートは、7 日、30 日、60 日、90 日および 120 日の期間のプロファイルの分類を提供します。 |
| `countOfProfiles` | 期間中の未ステッチプロファイルの数、または名前空間の未ステッチプロファイルの数。 |
| `eventsAssociated` | 時間範囲の ExperienceEvents の数または名前空間のイベントの数。 |
| `nsDistribution` | ステッチされていないプロファイルとイベントが名前空間ごとに配布される、個々の ID 名前空間を含むオブジェクト。 メモ：合計を合計する `countOfProfiles` の ID 名前空間ごとに `nsDistribution` オブジェクトがと等しい `countOfProfiles` 期間に対して。 同じことが言えます `eventsAssociated` 名前空間ごとの合計 `eventsAssociated` 期間ごと。 |
| `reportTimestamp` | レポートのタイムスタンプ。 |

### ステッチされていないプロファイルレポートの解釈

レポートの結果は、組織がプロファイルストア内に持つ未ステッチおよび非アクティブのプロファイルの数に関するインサイトを提供できます。

以下の抜粋を考えてみましょう。 `data` オブジェクト :

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

* プロファイルフラグメントを 1 つだけ含み、過去 7 日間に新しいイベントがないプロファイルは 1,782 あります。
* ステッチされていないプロファイル 1,782 件に関連付けられている ExperienceEvents は 29,151 件です。
* ECID の ID 名前空間の 1 つのプロファイルフラグメントを含む 1,734 個の未ステッチプロファイルがあります。
* ECID の ID 名前空間の単一のプロファイルフラグメントを含む 1,734 個の未ステッチのプロファイルに関連付けられたイベントは 28,591 個あります。

## 次の手順

プロファイルストアのサンプルデータをプレビューする方法と、データに対して複数のレポートを実行する方法がわかったので、Segmentation Service API の推定エンドポイントとプレビューエンドポイントを使用して、セグメント定義に関する概要レベルの情報を表示することもできます。 この情報は、期待されるオーディエンスを確実に分離するのに役立ちます。 Segmentation API を使用したプレビューと予測の操作について詳しくは、以下を参照してください。 [プレビューと見積もりエンドポイントガイド](../../segmentation/api/previews-and-estimates.md).

