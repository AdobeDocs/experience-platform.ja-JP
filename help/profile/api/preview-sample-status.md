---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、プレビュー、サンプル
title: プレビューサンプルステータス（プロファイルプレビュー）APIエンドポイント
description: リアルタイム顧客プロファイルAPIの一部であるプレビューサンプルステータスエンドポイントを使用すると、最新の成功したプロファイルデータをプレビューし、データセット別およびID別にプロファイル配分をリストし、データセットの重複、IDの重複、不明なプロファイルを示すレポートを生成できます。
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: 8b1ba51f1f59b88a85d103cc40c18ac15d8648f6
workflow-type: tm+mt
source-wordcount: '2882'
ht-degree: 5%

---

# サンプルステータスエンドポイントのプレビュー（プロファイルプレビュー）

Adobe Experience Platformを使用すると、複数のソースから顧客データを取り込み、個々の顧客に対して堅牢で統合されたプロファイルを作成できます。 データがPlatformに取り込まれると、サンプルジョブが実行され、プロファイル数やその他のリアルタイム顧客プロファイルデータ関連指標が更新されます。

このサンプルジョブの結果は、リアルタイム顧客プロファイルAPIの一部である`/previewsamplestatus`エンドポイントを使用して表示できます。 このエンドポイントは、データセットとID名前空間の両方によるプロファイル配分のリストを作成したり、組織のプロファイルストアの構成を把握できるように、複数のレポートを生成したりするのにも使用できます。 このガイドでは、`/previewsamplestatus` APIエンドポイントを使用してこれらの指標を表示するために必要な手順について説明します。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service APIの一部として使用できる推定およびプレビューエンドポイントがあり、予想されるオーディエンスを確実に特定するのに役立つ、セグメント定義に関する概要レベルの情報を表示できます。 セグメントのプレビューと推定のエンドポイントを使用する詳細な手順を見つけるには、『[!DNL Segmentation] API開発者ガイド』の一部である、[プレビューと推定のエンドポイントに関するガイド](../../segmentation/api/previews-and-estimates.md)を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-time Customer Profile] API](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合プロファイル

このガイドでは、「プロファイルフラグメント」と「結合されたプロファイル」の両方を参照します。 先に進む前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、その顧客に対する単一のビューを形成する複数のプロファイルフラグメントで構成されます。例えば、顧客が複数のチャネルをまたいでブランドとやり取りする場合、組織には、その1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示される可能性があります。

プロファイルフラグメントがPlatformに取り込まれると、その顧客の単一のプロファイルを作成するために、プロファイルフラグメントが結合ポリシーに基づいて結合されます。 したがって、各プロファイルは複数のフラグメントで構成されるので、プロファイルフラグメントの合計数は、常に結合されたプロファイルの合計数よりも多くなる可能性が高くなります。

Experience Platform内でのプロファイルとその役割について詳しくは、まず「[リアルタイム顧客プロファイルの概要](../home.md)」を参照してください。

## サンプルジョブのトリガー方法

リアルタイム顧客プロファイルに対して有効なデータが[!DNL Platform]に取り込まれると、そのデータはプロファイルデータストアに保存されます。 プロファイルストアへのレコードの取り込みで、プロファイルの合計数が5%を超えるまたは減少すると、サンプリングジョブがトリガーされ、カウントが更新されます。 サンプルのトリガー方法は、使用する取り込みのタイプによって異なります。

* **ストリーミングデータワークフロー**&#x200B;の場合、5%の増減しきい値に達したかどうかを判断するために、1時間ごとにチェックが実行されます。 ある場合は、サンプルジョブが自動的にトリガーされ、カウントが更新されます。
* **バッチ取得**&#x200B;の場合、15分以内にプロファイルストアにバッチを正常に取り込みます。5%の増減しきい値に達すると、ジョブが実行されてカウントが更新されます。 プロファイルAPIを使用すると、最新の成功したサンプルジョブをプレビューし、データセット別およびID名前空間別にプロファイル配分をリストできます。

プロファイル数と名前空間別のプロファイルは、Experience PlatformUIの[!UICONTROL Profiles]セクション内でも使用できます。 UIを使用してプロファイルデータにアクセスする方法について詳しくは、[[!DNL Profile] UIガイド](../ui/user-guide.md)を参照してください。

## 最後のサンプルステータスの表示 {#view-last-sample-status}

`/previewsamplestatus`エンドポイントに対してGETリクエストを実行し、IMS組織に対して最後に正常に実行されたサンプルジョブの詳細を表示できます。 これには、サンプル内のプロファイルの合計数、プロファイル数指標、またはExperience Platform内に組織が持つプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客ごとに1つのプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントを結合すると、すべて同じ個人に関連しているので、「1」プロファイルの数が返されます。

また、プロファイル数には、属性（レコードデータ）を持つプロファイルと、時系列（イベント）データのみを含むプロファイル(Adobe Analyticsプロファイルなど)の両方が含まれます。 サンプルジョブは、Platform内の最新の合計プロファイル数を提供するために、プロファイルデータが取り込まれると定期的に更新されます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、組織に対して最後に正常に実行されたサンプルジョブの詳細が含まれます。

>[!NOTE]
>
>この応答の例では、`numRowsToRead`と`totalRows`は互いに等しくなります。 組織がExperience Platformしているプロファイルの数に応じて、このような場合があります。 ただし、通常、これら2つの数値は異なり、`numRowsToRead`はプロファイルの合計数(`totalRows`)のサブセットとしてサンプルを表すので、小さい数になります。

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
| `numRowsToRead` | サンプル内の結合済みプロファイルの合計数です。 |
| `sampleJobRunning` | サンプルジョブが進行中の場合に`true`を返すboolean値。 バッチファイルが実際にプロファイルストアに追加された時点からに発生する遅延に対して透明性を提供します。 |
| `cosmosDocCount` | Cosmosのドキュメント数の合計。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数。 |
| `lastSuccessfulBatchTimestamp` | 最後に成功したバッチ取得のタイムスタンプ。 |
| `streamingDriven` | *このフィールドは廃止され、応答に対する重要性はありません。* |
| `totalRows` | Experience Platform内の結合済みプロファイルの合計数。「プロファイル数」とも呼ばれます。 |
| `lastBatchId` | 最後のバッチ取得ID。 |
| `status` | 最後のサンプルの状態。 |
| `samplingRatio` | 合計結合プロファイル(`numRowsToRead`)に対する結合済みプロファイルの比率(`totalRows`)。小数形式の割合で表されます。 |
| `mergeStrategy` | サンプルで使用される結合方法。 |
| `lastSampledTimestamp` | 最後に成功したサンプルタイムスタンプ。 |

## データセット別のプロファイル配分のリスト

プロファイルのデータセット別の分布を確認するには、`/previewsamplestatus/report/dataset`エンドポイントに対してGETリクエストを実行します。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日に複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（未検出）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DDです。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、`date`パラメーターを使用して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset?date=2020-08-01 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には、データセットオブジェクトのリストを含む`data`配列が含まれます。 表示される応答は、3つのデータセットを表示するように切り捨てられました。

>[!NOTE]
>
>その日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付のデータセットレポートが存在しない場合は、HTTPステータス404（未検出）が返されます。

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
| `sampleCount` | このデータセットIDを持つサンプリング済み結合プロファイルの合計数。 |
| `samplePercentage` | サンプリングされた結合プロファイルの合計数（[最後のサンプルステータス](#view-last-sample-status)で返される`numRowsToRead`値）の割合(`sampleCount`)。10進数形式で表されます。 |
| `fullIDsCount` | このデータセットIDを持つ結合プロファイルの合計数です。 |
| `fullIDsPercentage` | 結合されたプロファイルの合計数（[最後のサンプルステータス](#view-last-sample-status)で返される`totalRows`値）に対する割合(`fullIDsCount`)。小数形式で表されます。 |
| `name` | データセットの名前。データセットの作成時に指定されます。 |
| `description` | データセットの説明。データセットの作成時に提供されます。 |
| `value` | データセットのID。 |
| `streamingIngestionEnabled` | データセットのストリーミング取り込みが有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザーID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターを指定しない場合は、最新のレポートが返されます。 |

## ID名前空間別のプロファイル配分のリスト

`/previewsamplestatus/report/namespace`エンドポイントに対してGETリクエストを実行し、プロファイルストア内のすべての結合プロファイルにわたるID名前空間による分類を表示できます。 これには、Adobeが提供する標準IDと、組織が定義するカスタムIDの両方が含まれます。

ID名前空間は、Adobe Experience Platform IDサービスの重要なコンポーネントで、顧客データが関連するコンテキストのインジケーターとして機能します。 詳しくは、まず[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。

>[!NOTE]
>
>1つのプロファイルが複数の名前空間に関連付けられる可能性があるので、名前空間別のプロファイルの合計数（各名前空間に表示される値を加算）は、プロファイル数指標より多くなる場合があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 その日に複数のレポートが実行された場合は、その日の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（未検出）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DDです。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、`date`パラメーターが指定されていないので、最新のレポートが返されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には`data`配列が含まれ、個々のオブジェクトに各名前空間の詳細が含まれます。 4つの名前空間を表示するために、表示される応答が切り捨てられました。

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
| `samplePercentage` | サンプリングされた結合プロファイルの割合（[最後のサンプルステータス](#view-last-sample-status)で返される`numRowsToRead`値）としての`sampleCount`。10進数形式で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターを指定しない場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間内の結合済みプロファイルの合計数です。 |
| `fullIDsPercentage` | `fullIDsCount`は、合計結合プロファイル（[最後のサンプルステータス](#view-last-sample-status)で返される`totalRows`値）の割合で、10進数形式で表されます。 |
| `code` | 名前空間の`code`。 これは、[Adobe Experience Platform IDサービスAPI](../../identity-service/api/list-namespaces.md)を使用して名前空間を操作する際に見つかります。Experience PlatformUIでは、[!UICONTROL ID記号]とも呼ばれます。 詳しくは、[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |
| `value` | 名前空間の`id`値。 これは、[IDサービスAPI](../../identity-service/api/list-namespaces.md)を使用して名前空間を操作する際に見つかります。 |

## データセットの重複レポートの生成

データセット重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も貢献するデータセットを公開することで、組織のプロファイルストアの構成を視覚的に確認できます。 このレポートは、データに関するインサイトを提供するだけでなく、特定のデータセットのTTLの設定など、ライセンスの使用を最適化するためのアクションを実行するのに役立ちます。

`/previewsamplestatus/report/dataset/overlap`エンドポイントに対してGETリクエストを実行すると、データセット重複レポートを生成できます。

コマンドラインまたはPostman UIを使用してデータセットの重複レポートを生成する手順については、[データセットの重複レポートの生成に関するチュートリアル](../tutorials/dataset-overlap-report.md)を参照してください。

**API 形式**

```http
GET /previewsamplestatus/report/dataset/overlap
GET /previewsamplestatus/report/dataset/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付で複数のレポートが実行された場合は、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（未検出）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DDです。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、`date`パラメーターを使用して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

リクエストが成功すると、HTTPステータス200(OK)とデータセット重複レポートが返されます。

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
| `data` | `data`オブジェクトには、データセットのコンマ区切りリストと、それぞれのプロファイル数が含まれます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターを指定しない場合は、最新のレポートが返されます。 |

### データセットの重複レポートの解釈

レポートの結果は、応答のデータセットとプロファイル数から解釈できます。 次の例のレポート`data`オブジェクトについて考えてみましょう。

```json
  "5d92921872831c163452edc8,5da7292579975918a851db57,5eb2cdc6fa3f9a18a7592a98": 123,
  "5d92921872831c163452edc8,5eb2cdc6fa3f9a18a7592a98": 454412,
  "5eeda0032af7bb19162172a7": 107
```

このレポートには次の情報が表示されます。

* 次のデータセットからのデータで構成される123個のプロファイルがあります。`5d92921872831c163452edc8`、`5da7292579975918a851db57`、`5eb2cdc6fa3f9a18a7592a98`。
* 次の2つのデータセットから得られるデータで構成される454,412件のプロファイルがあります。`5d92921872831c163452edc8`と`5eb2cdc6fa3f9a18a7592a98`が表示されます。
* データセット`5eeda0032af7bb19162172a7`のデータのみから構成される107個のプロファイルがあります。
* 組織には、合計454,642件のプロファイルがあります。

## ID名前空間の重複レポートの生成

ID名前空間重複レポートは、アドレス可能なオーディエンス（結合プロファイル）に最も貢献するID名前空間を公開することで、組織のプロファイルストアの構成を可視化できます。 これには、Adobeが提供する標準ID名前空間と、組織が定義したカスタムID名前空間の両方が含まれます。

`/previewsamplestatus/report/namespace/overlap`エンドポイントに対してGETリクエストを実行すると、ID名前空間の重複レポートを生成できます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace/overlap
GET /previewsamplestatus/report/namespace/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付で複数のレポートが実行された場合は、その日付の最新のレポートが返されます。 指定した日付のレポートが存在しない場合は、404（未検出）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DDです。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、`date`パラメーターを使用して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace/overlap?date=2021-12-29 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

リクエストが成功すると、HTTPステータス200(OK)とID名前空間の重複レポートが返されます。

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
| `data` | `data`オブジェクトには、ID名前空間コードの一意の組み合わせとそれぞれのプロファイル数を含む、コンマ区切りのリストが含まれます。 |
| 名前空間コード | `code`は、各ID名前空間名の短い形式です。 各`code`の`name`へのマッピングは、[Adobe Experience Platform IDサービスAPI](../../identity-service/api/list-namespaces.md)を使用して見つかります。 `code`は、Experience PlatformUIでは[!UICONTROL ID記号]とも呼ばれます。 詳しくは、[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターを指定しない場合は、最新のレポートが返されます。 |

### ID名前空間の重複レポートの解釈

レポートの結果は、応答のIDとプロファイルの数から解釈できます。 各行の数値は、標準ID名前空間とカスタムID名前空間の正確な組み合わせで構成されるプロファイルの数を示します。

`data`オブジェクトの次の抜粋を考えてみましょう。

```json
  "AAID,ECID,Email,crmid": 142,
  "AVID,ECID": 24,
  "ECID": 6565
```

このレポートには次の情報が表示されます。

* `AAID`、`ECID`、`Email`の標準IDで構成される142個のプロファイルと、カスタムの`crmid` ID名前空間のプロファイルがあります。
* `AAID`と`ECID`のID名前空間で構成される24個のプロファイルがあります。
* `ECID` IDのみを含むプロファイルは6,565個あります。

## 不明なプロファイルレポートの生成

不明なプロファイルレポートを使用すると、組織のプロファイルストアの構成をさらに詳しく把握できます。 「不明なプロファイル」は、特定の期間、未関連付けおよび非アクティブの両方に該当するプロファイルを指します。 「未関連付け」プロファイルは、1つのプロファイルフラグメントのみを含むプロファイルで、「非アクティブ」プロファイルは、指定された期間、新しいイベントを追加しなかった任意のプロファイルです。 不明なプロファイルレポートは、7日、30日、60日、90日、120日のプロファイルの分類を表示します。

`/previewsamplestatus/report/unknownProfiles`エンドポイントに対してGETリクエストを実行すると、不明なプロファイルレポートを生成できます。

**API 形式**

```http
GET /previewsamplestatus/report/unknownProfiles
```

**リクエスト**

次のリクエストは、不明なプロファイルレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/unknownProfiles \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

リクエストが成功すると、HTTPステータス200(OK)と不明なプロファイルレポートが返されます。

>[!NOTE]
>
>このガイドの目的上、レポートは`"120days"`と&quot;`7days`&quot;の期間のみを含むように切り捨てられています。 完全な不明なプロファイルレポートは、7日、30日、60日、90日、120日のプロファイルの分類を表示します。

```json
{
  "data": {
      "totalNumberOfProfiles": 63606,
      "totalNumberOfEvents": 130977,
      "unknownProfiles": {
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
| `data` | `data`オブジェクトには、不明なプロファイルレポートに対して返される情報が含まれます。 |
| `totalNumberOfProfiles` | プロファイルストア内の一意のプロファイルの合計数。 これは、アドレス可能なオーディエンスの数と同じです。 既知のプロファイルと不明なプロファイルの両方が含まれます。 |
| `totalNumberOfEvents` | プロファイルストア内のExperienceEventsの合計数。 |
| `unknownProfiles` | 期間別の不明なプロファイル（未関連付けおよび非アクティブ）の分類を含むオブジェクト。 不明なプロファイルレポートは、7日、30日、60日、90日、120日の期間のプロファイルの分類を表示します。 |
| `countOfProfiles` | その期間の不明なプロファイルの数または名前空間の不明なプロファイルの数。 |
| `eventsAssociated` | 時間範囲のExperienceEventsの数または名前空間のイベントの数。 |
| `nsDistribution` | 個々のID名前空間と、各名前空間の不明なプロファイルおよびイベントの配分を含むオブジェクト。 注意：`nsDistribution`オブジェクトの各ID名前空間の合計`countOfProfiles`を加算すると、その期間の`countOfProfiles`が等しくなります。 名前空間ごとの`eventsAssociated`と期間ごとの合計`eventsAssociated`についても同じことが言えます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 |

### 不明なプロファイルレポートの解釈

レポートの結果から、組織がプロファイルストア内に持つ未関連付けプロファイルと非アクティブなプロファイルの数に関するインサイトを得ることができます。

`data`オブジェクトの次の抜粋を考えてみましょう。

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

このレポートには次の情報が表示されます。

* 1,782個のプロファイルがあり、1つのプロファイルフラグメントのみが含まれ、過去7日間新しいイベントが発生していません。
* 1,782件の不明なプロファイルに関連付けられたExperienceEventsは29,151件あります。
* ECIDのID名前空間の単一のプロファイルフラグメントを含む不明なプロファイルは1,734件あります。
* 1,734件の不明なプロファイルに関連付けられた28,591件のイベントがあり、この中には、ECIDのID名前空間の1つのプロファイルフラグメントが含まれています。

## 次の手順

これで、プロファイルストアでサンプルデータをプレビューし、データに関する複数のレポートを実行する方法がわかったので、セグメント化サービスAPIの推定エンドポイントとプレビューエンドポイントを使用して、セグメント定義に関する概要レベルの情報を表示できます。 この情報は、セグメント内の予期されるオーディエンスを確実に分離するのに役立ちます。 セグメント化APIを使用したセグメントプレビューと推定の操作について詳しくは、『[プレビューと推定エンドポイントのガイド](../../segmentation/api/previews-and-estimates.md)』を参照してください。

