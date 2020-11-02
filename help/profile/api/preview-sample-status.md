---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;preview;sample
title: プロファイルプレビュー — リアルタイム顧客プロファイルAPI
description: Adobe Experience Platformでは、複数のソースから顧客データを取り込んで、個々の顧客に対して堅牢な統合プロファイルを構築できます。 リアルタイム顧客プロファイルを有効にしたデータは、プラットフォームに取り込まれると、プロファイルのデータストア内に保存されます。 プロファイルストアのレコード数が増減すると、データストア内のプロファイルフラグメントと結合プロファイルの数に関する情報を含むサンプルジョブが実行されます。 プロファイルAPIを使用して、最新の成功したサンプルや、データセット別、ID名前空間別にリストプロファイルの配布をプレビューできます。
topic: guide
translation-type: tm+mt
source-git-commit: 47c65ef5bdd083c2e57254189bb4a1f1d9c23ccc
workflow-type: tm+mt
source-wordcount: '1608'
ht-degree: 5%

---


# プレビューサンプルステータスエンドポイント(プロファイルプレビュー)

Adobe Experience Platformでは、複数のソースから顧客データを取り込んで、個々の顧客に対して堅牢な統合プロファイルを構築できます。 リアルタイム顧客プロファイルが有効なデータがに取り込まれ [!DNL Platform]ると、そのデータはプロファイルデータストア内に保存されます。

プロファイルストアへのレコードの取り込みが、総プロファイル数を5%以上増減すると、ジョブがトリガされ、カウントが更新される。 ストリーミングデータワークフローの場合、5%増減のしきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 ジョブが存在する場合は、そのジョブが自動的にトリガされ、カウントが更新されます。 バッチ取り込みの場合、バッチをプロファイルストアに正常に取り込んでから15分以内に、5%の増減のしきい値に達すると、ジョブが実行され、カウントが更新されます。 プロファイルAPIを使用して、最新の成功したサンプルジョブをプレビューできるほか、リストプロファイルの配布をデータセット別、ID名前空間別に測定できます。

これらの指標は、Experience PlatformUIの [!UICONTROL プロファイル] セクション内でも使用できます。 UIを使用してプロファイルデータにアクセスする方法については、 [[!DNL Profile] ユーザガイドを参照してください](../ui/user-guide.md)。

## はじめに

The API endpoint used in this guide is part of the [[!DNL Real-time Customer Profile] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml). 先に進む前に、 [はじめに](getting-started.md)[!DNL Experience Platform] 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合されたプロファイル

このガイドは、「プロファイルフラグメント」と「結合されたプロファイル」の両方を参照しています。 先に進む前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、複数のプロファイルフラグメントで構成され、それらが結合されてその顧客の単一の表示が形成されます。 例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。 これらのフラグメントがプラットフォームに取り込まれると、結合ポリシーに基づいて結合され、そのお客様用の単一のプロファイルが作成されます。 したがって、各プロファイルは複数のフラグメントで構成されるので、プロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

## 表示の最後のサンプル状態 {#view-last-sample-status}

エンドポイントに対してGETリクエストを実行し、IMS組織で最後に正常に実行されたサンプルジョブの詳細を表示できます。 `/previewsamplestatus` これには、サンプル内のプロファイルの合計数、プロファイル数指標、または組織がExperience Platform内に持つプロファイルの合計数が含まれます。 プロファイル数は、プロファイルフラグメントを結合して個々の顧客に対する単一のプロファイルを形成した後に生成されます。 つまり、様々なチャネルでブランドとやり取りする 1 人の顧客に関連する複数のプロファイルフラグメントが組織に存在する場合でも、これらのフラグメントは、1 個人に関連しているため（デフォルトの結合ポリシーに従って）結合され、プロファイルの数が「1」として返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analyticsプロファイルなどの時系列(イベント)データのみを含むプロファイルの両方が含まれます。 プラットフォーム内の最新のプロファイル総数を提供するために、プロファイルデータが取り込まれる際に、サンプルジョブは定期的に更新されます。

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

この応答には、IMS組織に対して実行された最後に成功したサンプルジョブの詳細が含まれます。

>[!NOTE]
>
>この例の応答では、 `numRowsToRead` とは互いに等し `totalRows` い。 Experience Platform内に組織が持つプロファイルの数によっては、この場合があります。 ただし、これら2つの数字は異なります。これ `numRowsToRead` は、サンプルがプロファイル総数(`totalRows`)のサブセットとして表されるので、小さい方の数字になります。

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
| `numRowsToRead` | サンプル内のマージされたプロファイルの合計数。 |
| `sampleJobRunning` | サンプルジョブが進行中 `true` の場合に返すboolean値です。 バッチファイルがアップロードされた時点からプロファイルストアに実際に追加された時点までの待ち時間に対して、透明度を提供します。 |
| `cosmosDocCount` | Cosmosでのドキュメントの合計数です。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数です。 |
| `lastSuccessfulBatchTimestamp` | 前回成功したバッチ取り込みのタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨となり、応答に意義はありません。* |
| `totalRows` | エクスペリエンスプラットフォーム内の結合されたプロファイルの合計数です。「プロファイル数」とも呼ばれます。 |
| `lastBatchId` | 最後のバッチ取り込みID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | サンプリングされた結合プロファイル(`numRowsToRead`)と結合された合計プロファイル(`totalRows`)の比率。10進数形式で表されます。 |
| `mergeStrategy` | サンプルで使用されるマージ方法。 |
| `lastSampledTimestamp` | 最後に成功したサンプルタイムスタンプ。 |

## データセット別リストプロファイル配布

プロファイルセット別のGETの分布を確認するには、エンドポイントに対してリクエストを実行し `/previewsamplestatus/report/dataset` ます。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 複数のレポートが実行された日付の場合は、その日付の最新のレポートが返されます。 指定した日付に対してレポートが存在しない場合は、404エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストでは、 `date` パラメーターを使用して、指定した日付の最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset?date=2020-08-01 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

この応答には、データセットオブジェクトのリストを含む `data` 配列が含まれます。 表示された応答は、3つのデータセットを表示するように切り捨てられました。

>[!NOTE]
>
>日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付にデータセットレポートが存在しなかった場合は、HTTPステータス404（エラー）が返されます。

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
| `sampleCount` | このデータセットIDを持つ、サンプリングされた結合プロファイルの合計数。 |
| `samplePercentage` | サンプリングさ `sampleCount` れた結合プロファイルの合計数( `numRowsToRead` 最後のサンプルステータスで返される [値](#view-last-sample-status))に対する割合（百進数形式）。 |
| `fullIDsCount` | このデータセットIDとマージされたプロファイルの合計数。 |
| `fullIDsPercentage` | 結合さ `fullIDsCount` れたプロファイルの総数( `totalRows` 最後のサンプルステータスで返される [値](#view-last-sample-status))に対する割合（小数形式）。 |
| `name` | データセットの作成時に提供される、データセットの名前。 |
| `description` | データセットの作成時に提供される、データセットの説明。 |
| `value` | データセットのID。 |
| `streamingIngestionEnabled` | データセットのストリーミング取り込みが有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザーID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に `date` パラメーターが指定された場合、レポートは指定された日付に対して返されます。 パラメーターを指定しな `date` い場合は、最新のレポートが返されます。 |



## 名前空間別リストプロファイル配布

エンドポイントに対してGETリクエストを実行し、プロファイルストア内の結合されたすべてのプロファイルにわたるID名前空間別の分類を表示できます。 `/previewsamplestatus/report/namespace` ID名前空間は、顧客データが関連付けられるコンテキストのインジケータとして機能する、Adobe Experience PlatformIDサービスの重要なコンポーネントです。 To learn more, visit the [identity namespace overview](../../identity-service/namespaces.md).

>[!NOTE]
>
>1つのプロファイルが複数の名前空間に関連付けられる可能性があるので、名前空間別のプロファイルの合計数(各名前空間に表示される値を合計)は、常にプロファイル数指標より多くなります。 例えば、ある顧客が複数のチャネルで自社のブランドとやり取りした場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 複数のレポートが実行された日付の場合は、その日付の最新のレポートが返されます。 指定した日付に対してレポートが存在しない場合は、404エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD。 例：`date=2024-12-31` |

**リクエスト**

次のリクエストは `date` パラメーターを指定しないので、最新のレポートを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答** 

応答には `data` 配列が含まれ、各名前空間の詳細が含まれる個々のオブジェクトが含まれます。 4つの名前空間が表示されるように、表示される応答は切り捨てられました。

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
| `sampleCount` | 名前空間内のサンプル結合プロファイルの合計数。 |
| `samplePercentage` | サンプリング `sampleCount` された結合プロファイルの割合( `numRowsToRead` 最後のサンプルステータスで返された [値](#view-last-sample-status))。10進数形式で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に `date` パラメーターが指定された場合、レポートは指定された日付に対して返されます。 パラメーターを指定しな `date` い場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間内のマージされたプロファイルの合計数。 |
| `fullIDsPercentage` | 結合さ `fullIDsCount` れたプロファイルの合計( `totalRows` 最後のサンプルステータスで返される [値](#view-last-sample-status))に対する割合（パーセンテージ）。表現は10進数形式です。 |
| `code` | 名前空間 `code` の。 これは、 [Adobe Experience PlatformIDサービスAPIを使用して名前空間を操作する場合に見られます](../../identity-service/api/list-namespaces.md) 。また、Experience PlatformUIでは [!UICONTROL ID記号] とも呼ばれます。 To learn more, visit the [identity namespace overview](../../identity-service/namespaces.md). |
| `value` | 名前空間の `id` 値。 これは、 [IDサービスAPIを使用して名前空間を操作する場合に見つかります](../../identity-service/api/list-namespaces.md)。 |

## 次の手順

同様の予測とプレビューを使用して、セグメント定義に関する表示の概要レベルの情報に対して、期待されるオーディエンスを確実に分離することもできます。 APIを使用したセグメントプレビューと予測の操作に関する詳細な手順については、 [!DNL Adobe Experience Platform Segmentation Service] API開発者ガイドの一部、 [プレビューと予測エンドポイントガイド](../../segmentation/api/previews-and-estimates.md)[!DNL Segmentation] を参照してください。

