---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；プレビュー；サンプル
title: サンプルステータスのプレビュー（プロファイルプレビュー） API エンドポイント
description: Real-Time Customer Profile APIのプレビューサンプルステータスエンドポイントを使用すると、プロファイルデータの最新の成功したサンプルをプレビューし、データセット別およびID別にプロファイル配布を一覧表示し、データセットの重複、IDの重複、結合されていないプロファイルを示すレポートを生成できます。
role: Developer
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 6%

---

# サンプルステータスエンドポイントのプレビュー（プロファイルプレビュー）

Adobe Experience Platformなら、複数のソースから顧客データを取り込み、個々の顧客一人ひとりに対して堅牢な統合プロファイルを構築できます。 データがExperience Platformに取り込まれると、サンプルジョブが実行され、プロファイル数およびその他のリアルタイム顧客プロファイルデータ関連の指標が更新されます。

このサンプルジョブの結果は、Real-Time Customer Profile APIの一部である`/previewsamplestatus` エンドポイントを使用して表示できます。 このエンドポイントは、データセットとID名前空間の両方によるプロファイル配布のリストを作成したり、組織のプロファイルストアの構成を可視化するために複数のレポートを生成したりするためにも使用できます。 このガイドでは、`/previewsamplestatus` API エンドポイントを使用してこれらの指標を表示するために必要な手順について説明します。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service APIの一部として、セグメント定義に関する概要レベルの情報を表示して、想定されるオーディエンスを確実に分離するのに役立つ推定エンドポイントとプレビューエンドポイントがあります。 プレビューと見積もりのエンドポイントを操作するための詳細な手順については、[&#x200B; API開発者ガイドの一部である](../../segmentation/api/previews-and-estimates.md) プレビューと見積もりエンドポイント ガイド [!DNL Segmentation]を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile] API](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合プロファイル

このガイドでは、「プロファイルフラグメント」と「結合プロファイル」の両方について説明します。 先に進む前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、その顧客に対する単一のビューを形成するように結合された複数のプロファイルフラグメントで構成されます。たとえば、顧客が複数のチャネルをまたいで企業とやり取りする場合、その顧客に関連する複数のプロファイルフラグメントが、複数のデータセットに表示される可能性があります。

プロファイルフラグメントがExperience Platformに取り込まれると、その顧客の単一のプロファイルを作成するために、（結合ポリシーに基づいて）結合されます。 したがって、プロファイルフラグメントの総数は、各プロファイルが複数のフラグメントで構成されているので、結合されたプロファイルの総数よりも常に高くなる可能性があります。

Experience Platform内でのプロファイルとその役割について詳しくは、[&#x200B; リアルタイム顧客プロファイルの概要](../home.md)を参照してください。

## サンプルジョブのトリガー方法

リアルタイム顧客プロファイルに対して有効になっているデータが[!DNL Experience Platform]に取り込まれると、プロファイルデータストア内に保存されます。 プロファイルストアにレコードを取り込むと、合計プロファイル数が3%以上増加または減少すると、サンプリングジョブがトリガーされて数が更新されます。 サンプルがトリガーされる方法は、使用する取り込みのタイプによって異なります。

* **ストリーミングデータワークフロー**&#x200B;の場合、3%の増減しきい値が満たされているかどうかを判断するために、時間単位でチェックが行われます。 が設定されている場合、サンプルジョブは自動的にトリガーされ、カウントが更新されます。
* **バッチ取り込み**&#x200B;の場合、プロファイルストアにバッチを正常に取り込んでから15分以内に、3%の増減しきい値を満たした場合、ジョブが実行され、カウントが更新されます。 Profile APIを使用すると、成功した最新のサンプルジョブをプレビューしたり、データセット別およびID名前空間別にプロファイル配布を一覧表示したりできます。

プロファイル数と名前空間指標によるプロファイルも、Experience Platform UIの[!UICONTROL Profiles] セクション内で使用できます。 UIを使用してプロファイルデータにアクセスする方法について詳しくは、[[!DNL Profile] UI ガイド &#x200B;](../ui/user-guide.md)を参照してください。

## 最後のサンプルステータスを表示 {#view-last-sample-status}

`/previewsamplestatus` エンドポイントに対してGET リクエストを行うことで、自社に対して実行された最後に成功したサンプルジョブの詳細を確認できます。 このレポートには、サンプル内のプロファイルの合計数、プロファイル数メトリック、または組織がExperience Platform内に保持するプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客ごとに単一のプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントが結合されると、すべて同じ個人に関連しているため、「1」プロファイルのカウントが返されます。

プロファイル数には、属性（レコードデータ）を持つプロファイルと、Adobe Analytics プロファイルなどの時系列（イベント）データのみを含むプロファイルの両方も含まれます。 サンプルジョブは、プロファイルデータが取り込まれると、Experience Platform内の最新のプロファイルの合計数を提供するために定期的に更新されます。

**API 形式**

```http
GET /previewsamplestatus
```

**リクエスト**

+++ 最後のサンプルステータスを表示するためのサンプルリクエスト。

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

応答が成功すると、HTTP ステータス 200が返され、組織で実行された最後に成功したサンプルジョブの詳細が含まれます。

+++ 最後のサンプルステータスを含むサンプル応答。

>[!NOTE]
>
>この応答の例では、`numRowsToRead`と`totalRows`は互いに等しくなります。 Experience Platformに自社が保有するプロファイルの数によっては、このケースが考えられます。 ただし、一般に、これら2つの数値は異なり、`numRowsToRead`はプロファイルの総数（`totalRows`）のサブセットとしてサンプルを表すため、小さい数値です。

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
| `sampleJobRunning` | サンプルジョブの処理中に`true`を返すブール値。 バッチファイルがアップロードされたときから、実際にプロファイルストアに追加されたときまで発生する待ち時間の透明性を提供します。 |
| `docCount` | データベース内のドキュメントの合計数。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数。 |
| `lastSuccessfulBatchTimestamp` | 最後に成功したバッチ取り込みタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨（廃止予定）であり、応答に対する意味が含まれていません。* |
| `totalRows` | Experience Platformで結合されたプロファイルの合計数。プロファイル数とも呼ばれます。 |
| `lastBatchId` | 最後のバッチ取得ID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | 結合されたプロファイル （`numRowsToRead`）と結合されたプロファイルの合計（`totalRows`）の比率（小数点形式の割合）。 |
| `mergeStrategy` | サンプルで使用されるマージ戦略。 |
| `lastSampledTimestamp` | 最後に成功したサンプルタイムスタンプ。 |

+++

## データセット別プロファイル分布のリスト

GET リクエストを`/previewsamplestatus/report/dataset` エンドポイントに行うことで、データセット別のプロファイルの配布を確認できます。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| クエリパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `date` | 返すレポートの日付を指定します。 日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付にレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD。 | `date=2024-12-31` |

**リクエスト**

次のリクエストは、`date` パラメーターを使用して、指定された日付の最新レポートを返します。

+++ データセットでプロファイル分布を取得するためのサンプルリクエスト。

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
>日付に複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定された日付にデータセットレポートが存在しない場合は、HTTP ステータス 404 （見つかりません）が返されます。

応答が成功すると、HTTP ステータス 200が返され、データセット オブジェクトのリストを含む`data`配列が含まれます。

+++ 最新のデータセットオブジェクトを含むサンプル応答。

>[!NOTE]
>
>次に示す応答は、3つのデータセットを示すために切り捨てられています。

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
| `sampleCount` | このデータセット IDを持つ結合されたプロファイルのサンプルの合計数です。 |
| `samplePercentage` | 結合されたプロファイルの合計サンプル数に対する割合としての`sampleCount` （`numRowsToRead`最後のサンプルステータス [で返される](#view-last-sample-status)値）は、小数点形式で表されます。 |
| `fullIDsCount` | このデータセット IDを持つ結合プロファイルの合計数。 |
| `fullIDsPercentage` | 結合されたプロファイルの合計数に対する割合（`fullIDsCount`最後のサンプルステータス `totalRows`で返される[値）の](#view-last-sample-status)は、小数点形式で表されます。 |
| `name` | データセットの作成時に指定された、データセットの名前。 |
| `description` | データセットの作成時に提供されるデータセットの説明。 |
| `value` | データセットのID。 |
| `streamingIngestionEnabled` | データセットがストリーミング取り込みに対して有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザーID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date` パラメーターが指定された場合、返されたレポートは、指定された日付のものです。 `date` パラメーターが指定されていない場合は、最新のレポートが返されます。 |

+++

## ID名前空間によるプロファイル配布のリスト

`/previewsamplestatus/report/namespace` エンドポイントに対してGET リクエストを実行し、プロファイルストア内のすべての結合プロファイルのID名前空間による分類を確認できます。 これには、Adobeが提供する標準IDと、企業が定義したカスタム IDの両方が含まれます。

ID名前空間は、Adobe Experience Platform Identity Serviceの重要なコンポーネントであり、顧客データが関連するコンテキストの指標として機能します。 詳しくは、[ID名前空間の概要](../../identity-service/features/namespaces.md)を参照してください。

>[!NOTE]
>
>名前空間別のプロファイルの合計数（各名前空間に表示される値を合計）は、1つのプロファイルが複数の名前空間に関連付けられる可能性があるため、プロファイル数の指標よりも多くなる可能性があります。 例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**API 形式**

```http
GET /previewsamplestatus/report/namespace
GET /previewsamplestatus/report/namespace?{QUERY_PARAMETERS}
```

| クエリパラメーター | 説明 | 例 |
| --------------- | ----------- | ------- |
| `date` | 返すレポートの日付を指定します。 日付に複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付にレポートが存在しない場合は、404 （見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：`YYYY-MM-DD`。 | `date=2025-6-20` |

**リクエスト**

次のリクエストは`date` パラメーターを指定せず、最新のレポートを返します。

+++ 名前空間によるプロファイル配布の最新レポートを返すリクエストのサンプル。 

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答が成功すると、HTTP ステータス 200が返され、各名前空間の詳細を含む個々のオブジェクトを含む`data`配列が含まれます。 表示される応答は、4つの名前空間を示すように切り捨てられています。

+++ サンプル応答には、名前空間によるプロファイル配布に関する情報が含まれます。

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
| `sampleCount` | 名前空間内の結合されたプロファイルのサンプルの合計数です。 |
| `samplePercentage` | サンプリングされた結合プロファイルの割合としての`sampleCount` （`numRowsToRead`最後のサンプルステータス [で返される](#view-last-sample-status)値）は、小数点形式で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト中に`date` パラメーターが指定された場合、返されたレポートは、指定された日付のものです。 `date` パラメーターが指定されていない場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数。 |
| `fullIDsCount` | 名前空間内の結合されたプロファイルの合計数です。 |
| `fullIDsPercentage` | 統合されたプロファイルの合計に対する割合（`fullIDsCount`最後のサンプルステータス `totalRows`で返される[値）である](#view-last-sample-status)は、小数点形式で表されます。 |
| `code` | 名前空間の`code`です。 これは、[Adobe Experience Platform Identity Service API](../../identity-service/api/list-namespaces.md)を使用して名前空間を操作する場合に見つかり、Experience Platform UIでは[!UICONTROL Identity symbol]とも呼ばれます。 詳しくは、[ID名前空間の概要](../../identity-service/features/namespaces.md)を参照してください。 |
| `value` | 名前空間の`id`値。 これは、[ID サービス API](../../identity-service/api/list-namespaces.md)を使用して名前空間を操作する際に見つかります。 |

+++

## データセット統計のリスト {#dataset-stats}

GET リクエストを`/previewsamplestatus/report/dataset_stats` エンドポイントに送信することで、データセットに関する統計情報を提供するレポートを生成できます。

**API 形式**

```http
GET /previewsamplestatus/report/dataset_stats
```

**リクエスト**

+++ データセット統計レポートを生成するためのサンプルリクエスト。

```shell
curl -X GET https://platform.adobe.io/data/core/ups/previewsamplestatus/report/dataset_stats \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

+++

**応答**

応答が成功すると、データセットの統計に関する情報を含むHTTP ステータス 200が返されます。

+++ データセットの統計に関する情報を含むサンプル応答。

>[!NOTE]
>
>次の応答は、3つのデータセットを示すために切り捨てられました。

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
| `120days` | 120日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `14days` | 14日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `30days` | 30日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `365days` | 365日のデータ有効期限の後にデータセットに残るレコードの数。 |
| `60days` | 60日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `7days` | 7日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `90days` | 90日間のデータ有効期限の後にデータセットに残るレコードの数。 |
| `datasetId` | データセットのID。 |
| `datasetType` | データセットの種類。 この値は`Profiles`または`ExperienceEvents`のいずれかです。 |
| `percentEvents` | データセット内にあるエクスペリエンスイベントレコードの割合。 |
| `percentProfiles` | データセット内にあるプロファイルレコードの割合。 |
| `profileFragments` | データセットに存在するプロファイルフラグメントの合計数。 |
| `records` | データセットに取り込まれたプロファイルレコードの合計数。 |
| `totalProfiles` | データセットに取り込まれたプロファイルの合計数です。 |

+++

## 次の手順

プロファイルストアでサンプルデータをプレビューし、そのデータに関する複数のレポートを実行する方法を理解したので、Segmentation Service APIの推定エンドポイントとプレビューエンドポイントを使用して、セグメント定義に関する概要レベルの情報を表示することもできます。 この情報は、想定されるオーディエンスを確実に分離するのに役立ちます。 セグメント化APIを使用したプレビューと見積もりの操作について詳しくは、[&#x200B; エンドポイントのプレビューと見積もりガイド &#x200B;](../../segmentation/api/previews-and-estimates.md)を参照してください。

