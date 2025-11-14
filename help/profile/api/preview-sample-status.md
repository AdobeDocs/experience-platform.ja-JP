---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；プレビュー；サンプル
title: プレビューサンプルのステータス（プロファイルプレビュー） API エンドポイント
description: リアルタイム顧客プロファイル API のプレビューサンプルステータスエンドポイントを使用すると、プロファイルデータの最新の成功したサンプルをプレビューしたり、データセット別および ID 別のプロファイル分布をリストしたり、データセットの重複、ID の重複、ステッチされていないプロファイルを示すレポートを生成したりできます。
role: Developer
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: bb2cfb479031f9e204006ba489281b389e6c6c04
workflow-type: tm+mt
source-wordcount: '2306'
ht-degree: 6%

---

# プレビューサンプルのステータスエンドポイント（プロファイルプレビュー）

Adobe Experience Platformでは、複数のソースから顧客データを取り込み、個々の顧客ごとに堅牢で統一されたプロファイルを作成できます。 データがExperience Platformに取り込まれると、サンプルジョブが実行されて、プロファイル数やその他のリアルタイム顧客プロファイルデータ関連指標が更新されます。

このサンプルジョブの結果は、リアルタイム顧客プロファイル API の一部である `/previewsamplestatus` エンドポイントを使用して確認できます。 また、このエンドポイントを使用して、データセットと ID 名前空間の両方でプロファイル配布をリスト表示したり、複数のレポートを生成して組織のプロファイルストアの構成を可視化したりできます。 このガイドでは、`/previewsamplestatus` API エンドポイントを使用してこれらの指標を表示するために必要な手順について説明します。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service API の一部として使用できる推定およびプレビューエンドポイントがあり、セグメント定義に関する概要レベルの情報を表示して、期待されるオーディエンスを確実に分離できるようにします。 プレビューおよび予測エンドポイントを使用する手順について詳しくは、[ API デベロッパーガイドの ](../../segmentation/api/previews-and-estimates.md) プレビューおよび予測エンドポイントガイド [!DNL Segmentation] を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile] API](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合プロファイル

このガイドは、「プロファイルフラグメント」と「結合プロファイル」の両方を参照します。 続行する前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたいで自社のブランドとやり取りを行う場合、1 人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示される可能性があります。

プロファイルフラグメントがExperience Platformに取り込まれると、その顧客用に単一のプロファイルを作成するために、（結合ポリシーに基づいて）結合されます。 したがって、各プロファイルは複数のフラグメントで構成され、プロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

プロファイルとそのExperience Platform内での役割について詳しくは、まず [ リアルタイム顧客プロファイルの概要 ](../home.md) をお読みください。

## サンプルジョブがトリガーされる方法

リアルタイム顧客プロファイルに対して有効なデータは [!DNL Experience Platform] に取り込まれるので、プロファイルデータストア内に保存されます。 プロファイルストアへのレコードの取り込みが、合計プロファイル数を 3% 以上増加または減少させると、サンプリングジョブがトリガーされて数が更新されます。 サンプルをトリガーする方法は、使用する取り込みのタイプによって異なります。

* **ストリーミングデータワークフロー** の場合は、3% の増加または減少しきい値に達したかどうかを判断するために、1 時間ごとにチェックが行われます。 サンプルジョブが自動的にトリガーされ、カウントが更新されます。
* **バッチ取り込み** の場合、プロファイルストアにバッチを正常に取り込んでから 15 分以内に、3% の増減しきい値に達した場合、回数を更新するジョブが実行されます。 プロファイル API を使用すると、最新の成功したサンプルジョブをプレビューできるほか、データセット別および ID 名前空間別のプロファイル配布をリストできます。

プロファイル数および名前空間別プロファイル指標は、Experience Platform UI の [!UICONTROL Profiles] セクション内でも使用できます。 UI を使用してプロファイルデータにアクセスする方法について詳しくは、[[!DNL Profile] UI ガイド ](../ui/user-guide.md) を参照してください。

## 最後のサンプルステータスを表示 {#view-last-sample-status}

`/previewsamplestatus` エンドポイントにGET リクエストを送信することで、組織で最後に実行され、成功したサンプルジョブの詳細を表示できます。 このレポートには、サンプル内のプロファイルの合計数、およびプロファイル数指標、つまり組織がExperience Platform内に持つプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客の単一のプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントが結合されると、すべてが同じ個人に関連しているので、「1」のプロファイルのカウントが返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analytics プロファイルなどの時系列（イベント）データのみを含むプロファイルの両方も含まれます。 Experience Platform内に最新のプロファイルの合計数を提供するために、プロファイルデータが取り込まれると、サンプルジョブは定期的に更新されます。

**API 形式**

```http
GET /previewsamplestatus
```

**リクエスト**

+++ 最後のサンプルステータスを表示するサンプルリクエスト。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が返され、組織に対して実行された最後の成功したサンプルジョブの詳細が含まれます。

+++ 最後のサンプルステータスを含むサンプル応答。

>[!NOTE]
>
>この応答の例では、`numRowsToRead` と `totalRows` は互いに等しくなります。 Experience Platform内のプロファイルの数に応じて、このケースが当てはまります。 ただし、通常、これら 2 つの数値は異なります。`numRowsToRead` れは、プロファイルの合計数（`totalRows`）のサブセットとしてサンプルを表すので、小さい方の数値です。

```json
{
  "numRowsToRead": "41003",
  "sampleJobRunning": {
    "status": true,
    "submissionTimestamp": "2020-08-01 17:57:57.0"
  },
  "docCount": "\"300803\"",
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
| -------- | ----------- |
| `numRowsToRead` | サンプル内の結合プロファイルの合計数。 |
| `sampleJobRunning` | サンプルジョブが進行中の場合に `true` を返すブール値です。 バッチファイルがアップロードされてからプロファイルストアに実際に追加されるまでの待ち時間に対して、透明性を提供します。 |
| `docCount` | データベース内のドキュメント総数。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数。 |
| `lastSuccessfulBatchTimestamp` | 前回成功したバッチ取り込みタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨となり、応答に重要な意味はありません。* |
| `totalRows` | Experience Platformでの結合プロファイルの合計数（プロファイル数とも呼ばれます）。 |
| `lastBatchId` | 前回のバッチ取得 ID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | 合計の結合プロファイル（`numRowsToRead`）に対する、サンプリングされた結合プロファイル（`totalRows`）の割合（10 進数形式でパーセンテージで表されます）。 |
| `mergeStrategy` | サンプルで使用されている結合方法。 |
| `lastSampledTimestamp` | 前回成功したサンプルタイムスタンプ。 |

+++

## データセット別のプロファイル分布のリスト

`/previewsamplestatus/report/dataset` エンドポイントにGET リクエストをおこなうと、データセット別のプロファイルの分布を確認できます。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `date` | 返されるレポートの日付を指定します。 その日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD | `date=2024-12-31` |

**リクエスト**

次のリクエストは、`date` パラメーターを使用して、指定された日付の最新のレポートを返します。

+++ データセット別のプロファイル分布を取得するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset?date=2020-08-01 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

>[!NOTE]
>
>日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付のデータセットレポートが存在しない場合は、HTTP ステータス 404 （見つかりません）が返されます。

応答が成功すると、HTTP ステータス 200 が返され、データセットオブジェクトのリストを含む `data` 配列が含まれています。

+++ 最新のデータセットオブジェクトを含んだサンプル応答。

>[!NOTE]
>
>表示される次の応答は、3 つのデータセットを表示するために切り捨てられています。

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
| -------- | ----------- |
| `sampleCount` | このデータセット ID を持つサンプリングされた結合プロファイルの合計数。 |
| `samplePercentage` | `sampleCount` は、サンプリングされた結合プロファイルの合計数のパーセンテージで表されます（`numRowsToRead` 最後のサンプルステータス [）で返された ](#view-last-sample-status) 値です。 |
| `fullIDsCount` | このデータセット ID を持つ結合プロファイルの合計数。 |
| `fullIDsPercentage` | `fullIDsCount` は、結合プロファイルの合計数のパーセンテージで表されます（`totalRows` 最後のサンプルステータス [）で返される ](#view-last-sample-status) 値）。 |
| `name` | データセットの作成時に指定したデータセットの名前。 |
| `description` | データセットの作成時に提供される、データセットの説明。 |
| `value` | データセットの ID。 |
| `streamingIngestionEnabled` | データセットがストリーミング取得に対して有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザー ID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に `date` パラメーターが指定された場合、返されるレポートは指定された日付のです。 `date` パラメーターを指定しない場合は、最新のレポートが返されます。 |

+++

## ID 名前空間別のプロファイル配分のリスト

`/previewsamplestatus/report/namespace` エンドポイントに対してGET リクエストを実行し、プロファイルストア内のすべての結合済みプロファイルで ID 名前空間別の分類を表示できます。 これには、Adobeで提供される標準 ID と、組織で定義されるカスタム ID の両方が含まれます。

ID 名前空間は、顧客データが関連するコンテキストのインジケーターとして機能するAdobe Experience Platform ID サービスの重要なコンポーネントです。 詳しくは、まず [ID 名前空間の概要 ](../../identity-service/features/namespaces.md) を読んでください。

>[!NOTE]
>
>1 つのプロファイルが複数の名前空間に関連付けられている可能性があるので、名前空間別のプロファイルの合計数（各名前空間に表示される値をまとめたもの）は、プロファイル数指標よりも多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| クエリーパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `date` | 返されるレポートの日付を指定します。 その日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：`YYYY-MM-DD`。 | `date=2025-6-20` |

**リクエスト**

次のリクエストでは、`date` パラメーターが指定されておらず、最新のレポートが返されます。

+++ 名前空間別のプロファイル配布の最新レポートを返すサンプルリクエスト。 

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が返され、`data` 配列と、各名前空間の詳細を含む個々のオブジェクトが含まれます。 表示された応答は、4 つの名前空間を表示するために切り捨てられました。

+++ サンプル応答には、名前空間別のプロファイル配布に関する情報が含まれています。

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
| -------- | ----------- |
| `sampleCount` | 名前空間内のサンプリングされた結合プロファイルの合計数です。 |
| `samplePercentage` | `sampleCount` は、10 進数形式で表された、サンプリングされた結合プロファイルの割合（`numRowsToRead` 最後のサンプルステータス [ で返された ](#view-last-sample-status) 値）です。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に `date` パラメーターが指定された場合、返されるレポートは指定された日付のです。 `date` パラメーターを指定しない場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間での結合プロファイルの合計数。 |
| `fullIDsPercentage` | 10 進数形式で表された、結合プロファイルの合計に対する割合としての `fullIDsCount` ータ `totalRows` 最後のサンプルステータス [ で返された ](#view-last-sample-status) 値）。 |
| `code` | 名前空間の `code`。 これは、[Adobe Experience Platform ID サービス API を使用して名前空間を操作する際に見つかり ](../../identity-service/api/list-namespaces.md)Experience Platform UI では [!UICONTROL Identity symbol] とも呼ばれます。 詳しくは、「[ID 名前空間の概要 ](../../identity-service/features/namespaces.md)」を参照してください。 |
| `value` | 名前空間の `id` 値。 これは、[ID サービス API](../../identity-service/api/list-namespaces.md) を使用して名前空間を操作する際に見つかります。 |

+++

## データセット統計のリスト {#dataset-stats}

`/previewsamplestatus/report/dataset_stats` エンドポイントに対してGET リクエストをおこなうと、データセットに関する統計を提供するレポートを生成できます。

**API 形式**

```http
GET /previewsamplestatus/report/dataset_stats
```

**リクエスト**

+++ データセット統計レポートを生成するためのリクエスト例。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset_stats \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 と、データセットの統計に関する情報が返されます。

+++ データセットの統計に関する情報を含むサンプル応答。

>[!NOTE]
>
>次の応答は、3 つのデータセットを表示するために切り捨てられています。

```json
{
    "data": [
        {
            "120days": 4,
            "14days": 4,
            "30days": 4,
            "365days": 4,
            "60days": 4,
            "7days": 4,
            "90days": 4,
            "datasetId": "{DATASET_ID}",
            "datasetType": "ExperienceEvents",
            "percentEvents": 0.0,
            "percentProfiles": 0.0,
            "profileFragments": 1,
            "records": 4,
            "totalProfiles": 1
        },
        {
            "120days": 155435837,
            "14days": 32888631,
            "30days": 66496282,
            "365days": 155435837,
            "60days": 116433804,
            "7days": 18202004,
            "90days": 155435837,
            "datasetId": "{DATASET_ID}",
            "datasetType": "ExperienceEvents",
            "percentEvents": 16.0,
            "percentProfiles": 0.0,
            "profileFragments": 5410745,
            "records": 155435837,
            "totalProfiles": 4524723
        },
        {
            "120days": 0,
            "14days": 0,
            "30days": 0,
            "365days": 0,
            "60days": 0,
            "7days": 0,
            "90days": 0,
            "datasetId": "{DATASET_ID}",
            "datasetType": "Profiles",
            "percentEvents": 0.0,
            "percentProfiles": 0.0,
            "profileFragments": 3589,
            "records": 3589,
            "totalProfiles": 3589
        }
    ],
    "reportTimestamp": "2025-10-29T16:20:18.956"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `120days` | データの有効期限が 120 日になった後にデータセットに残るレコードの数。 |
| `14days` | データの有効期限が 14 日間になった後にデータセットに残るレコードの数。 |
| `30days` | データの有効期限が 30 日になった後にデータセットに残るレコードの数。 |
| `365days` | データの有効期限が 365 日になった後にデータセットに残るレコードの数。 |
| `60days` | データの有効期限が 60 日になった後にデータセットに残るレコードの数。 |
| `7days` | データの有効期限が 7 日間になった後にデータセットに残るレコードの数。 |
| `90days` | データの有効期限が 90 日になった後にデータセットに残るレコードの数。 |
| `datasetId` | データセットの ID。 |
| `datasetType` | データセットタイプ。 この値は `Profiles` または `ExperienceEvents` です。 |
| `percentEvents` | データセット内にあるエクスペリエンスイベントレコードの割合。 |
| `percentProfiles` | データセット内にあるプロファイルレコードの割合。 |
| `profileFragments` | データセットに存在するプロファイルフラグメントの合計数。 |
| `records` | データセットに取り込まれたプロファイルレコードの合計数。 |
| `totalProfiles` | データセットに取り込まれたプロファイルの合計数。 |

+++

## データセットサイズの取得 {#character-count}

このエンドポイントを使用すると、データセットのサイズをバイト単位で週単位で取得できます。

**API 形式**

```http
GET /previewsamplestatus/report/character_count
```

**リクエスト**

+++文字数カウントレポートを生成するリクエストのサンプル。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/character_count \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 が、週全体でのデータセットのサイズに関する情報と共に返されます。

+++ データ有効期限が切れた後のデータセットのサイズに関する情報を含むサンプル応答。

>[!NOTE]
>
>次の応答は、3 つのデータセットを表示するために切り捨てられています。

```json
{
    "data": [
        {
            "datasetIds": [
                {
                    "datasetId": "67aba91a453f7d298cd2a643",
                    "recordType": "keyvalue",
                    "weeks": [
                        {
                            "size": 107773533894,
                            "week": "2025-10-26"
                        }
                    ]
                },
                {
                    "datasetId": "67aa6c867c3110298b017f0e",
                    "recordType": "timeseries",
                    "weeks": [
                        {
                            "size": 242902062440,
                            "week": "2025-10-26"
                        },
                        {
                            "size": 837539413062,
                            "week": "2025-10-19"
                        },
                        {
                            "size": 479253986484,
                            "week": "2025-10-12"
                        },
                        {
                            "size": 358911988990,
                            "week": "2025-10-05"
                        },
                        {
                            "size": 349701073042,
                            "week": "2025-09-28"
                        }
                    ]
                },
                {
                    "datasetId": "680c043667c0d7298c9ea275",
                    "recordType": "keyvalue",
                    "weeks": [
                        {
                            "size": 18392459832,
                            "week": "2025-10-26"
                        }
                    ]
                }
            ],
            "modelName": "_xdm.context.profile",
            "reportTimestamp": "2025-10-30T00:28:30.069Z"
        }
    ],
    "reportTimestamp": "2025-10-30T00:28:30.069Z"
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `datasetId` | データセットの ID。 |
| `recordType` | データセット内のデータのタイプ。 レコードタイプは、`weeks` 変数の値に影響します。 サポートされる値は `keyvalue` と `timeseries` です。 |
| `weeks` | データセットに関するサイズ情報を含む配列。 レコードタイプ `keyvalue` のデータセットの場合、これには、最新の週とデータセットの合計サイズ（バイト単位）が含まれます。 レコードタイプ `timeseries` のデータセットの場合、データセットの取り込みから最新の週までの毎週と、それらの各週のデータセットの合計サイズ（バイト単位）が含まれます。 |
| `modelName` | データセットのモデルの名前。 使用可能な値は `_xdm.context.profile` および `_xdm.context.experienceevent` です。 |
| `reportTimestamp` | レポートが生成された日時。 |

+++

## 次の手順

プロファイルストアのサンプルデータをプレビューする方法と、データに対して複数のレポートを実行する方法がわかったので、Segmentation Service API の推定エンドポイントとプレビューエンドポイントを使用して、セグメント定義に関する概要レベルの情報を表示することもできます。 この情報は、期待されるオーディエンスを確実に分離するのに役立ちます。 Segmentation API を使用したプレビューと予測の操作について詳しくは、[ プレビューと予測エンドポイントガイド ](../../segmentation/api/previews-and-estimates.md) を参照してください。

