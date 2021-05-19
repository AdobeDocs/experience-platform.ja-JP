---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API;プレビュー；サンプル
title: プレビューサンプルステータス(プロファイルプレビュー)APIエンドポイント
description: Real-time Customer Commenta Endpoint APIの一部であるプレビューサンプルステータスエンドポイントを使用すると、プロファイルデータの最新の成功したサンプル、リストプロファイルのデータセット別およびID別のプレビュー、データセット重複レポートを生成できます。
exl-id: a90a601e-629e-417b-ac27-3d69379bb274
source-git-commit: 459eb626101b7382b8fe497835cc19f7d7adc6b2
workflow-type: tm+mt
source-wordcount: '2066'
ht-degree: 1%

---

# プレビューサンプルステータスエンドポイント(プロファイルプレビュー)

Adobe Experience Platformでは、複数のソースから顧客データを取り込んで、個々の顧客に対して堅牢で統合されたプロファイルを構築できます。 データがプラットフォームに取り込まれると、サンプルジョブが実行され、プロファイル数およびその他のプロファイル関連指標が更新されます。

このサンプルジョブの結果は、リアルタイム顧客プロファイルAPIの`/previewsamplestatus`エンドポイントを使用して表示できます。 また、このエンドポイントは、データセットとID名前空間の両方によるリストプロファイルの配分に使用でき、また、データセットの重複レポートを生成して組織のプロファイルストアの構成を把握することもできます。 このガイドでは、`/previewsamplestatus` APIエンドポイントを使用してこれらの指標を表示するために必要な手順について説明します。

>[!NOTE]
>
>Adobe Experience Platform Segmentation Service APIの一部として、予測およびプレビューエンドポイントが用意されています。このエンドポイントを使用すると、セグメント定義に関する概要レベルの情報を表示し、期待されるオーディエンスを確実に分離できます。 セグメントプレビューと予測エンドポイントを使用する詳細な手順については、[!DNL Segmentation] API開発者ガイドの[プレビューおよび予測エンドポイントガイド](../../segmentation/api/previews-and-estimates.md)を参照してください。

## はじめに

このガイドで使用されるAPIエンドポイントは、[[!DNL Real-time Customer Profile] API](https://www.adobe.com/go/profile-apis-en)の一部です。 先に進む前に、[はじめにガイド](getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しの読み方、および任意の[!DNL Experience Platform] APIの呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## プロファイルフラグメントと結合されたプロファイル

このガイドは、「プロファイルフラグメント」と「結合されたプロファイル」の両方を参照しています。 先に進む前に、これらの用語の違いを理解することが重要です。

個々の顧客プロファイルは、複数のプロファイルフラグメントで構成され、それらが結合されてその顧客の1つの表示が形成されます。 例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りする場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示される可能性があります。

プロファイルフラグメントがプラットフォームに取り込まれると、その顧客用の単一のプロファイルを作成するために、それらが（結合ポリシーに基づいて）結合されます。 したがって、各プロファイルは複数のフラグメントで構成されるので、プロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

Experience Platform内でのプロファイルとその役割についての詳細は、まず[リアルタイムカスタマープロファイルの概要](../home.md)を参照してください。

## サンプルジョブのトリガー方法

リアルタイム顧客プロファイルが有効なデータは[!DNL Platform]に取り込まれるので、プロファイルデータストア内に保存されます。 プロファイルストアへのレコードの取り込みが、総プロファイル数を5%以上増減する場合、サンプリングジョブをトリガしてカウントを更新する。 サンプルのトリガー方法は、使用されている取り込みのタイプによって異なります。

* **ストリーミングデータワークフロー**&#x200B;の場合、5%の増減しきい値に達したかどうかを判断するために、1時間ごとにチェックが行われます。 サンプルジョブが存在する場合は、サンプルジョブが自動的にトリガーされ、カウントが更新されます。
* **バッチインジェスト**&#x200B;の場合、バッチをプロファイルストアに正常に取り込んでから15分以内に、5%の増減のしきい値に達すると、ジョブが実行され、カウントが更新されます。 プロファイルAPIを使用して、最新の成功したサンプルジョブをプレビューできるほか、リストプロファイルの配布をデータセット別、ID名前空間別に測定できます。

名前空間指標別のプロファイル数とプロファイルは、Experience PlatformUIの[!UICONTROL プロファイル]セクション内でも利用できます。 UIを使用したプロファイルデータへのアクセス方法については、[[!DNL Profile] UIガイド](../ui/user-guide.md)を参照してください。

## 表示の最後のサンプル状態{#view-last-sample-status}

`/previewsamplestatus`エンドポイントに対してGETリクエストを実行し、IMS組織で実行された最後に成功したサンプルジョブの詳細を表示できます。 これには、サンプル内のプロファイルの合計数、プロファイル数指標、または組織がExperience Platform内に持つプロファイルの合計数が含まれます。

プロファイル数は、プロファイルフラグメントを結合して個々の顧客に対する単一のプロファイルを形成した後に生成されます。 つまり、プロファイルフラグメントを結合すると、すべて同じ個人に関連しているので、「1」プロファイルのカウントが返されます。

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

応答には、組織で最後に成功したサンプルジョブの詳細が含まれます。

>[!NOTE]
>
>この例の応答では、`numRowsToRead`と`totalRows`は互いに等しくなります。 Experience Platform内に組織が持つプロファイルの数によっては、この場合があります。 ただし、これら2つの数字は異なります。`numRowsToRead`は、プロファイルの総数(`totalRows`)のサブセットとしてサンプルを表すので、通常は小さい方の数字になります。

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
| `sampleJobRunning` | サンプルジョブが進行中の場合に`true`を返すboolean値です。 バッチファイルがアップロードされた時点からプロファイルストアに実際に追加された時点までの待ち時間に対して、透明度を提供します。 |
| `cosmosDocCount` | Cosmosでのドキュメントの合計数です。 |
| `totalFragmentCount` | プロファイルストア内のプロファイルフラグメントの合計数です。 |
| `lastSuccessfulBatchTimestamp` | 前回成功したバッチ取り込みのタイムスタンプ。 |
| `streamingDriven` | *このフィールドは非推奨となり、応答に意義はありません。* |
| `totalRows` | Experience Platform内のマージされたプロファイルの合計数。「プロファイル数」とも呼ばれます。 |
| `lastBatchId` | 最後のバッチ取り込みID。 |
| `status` | 最後のサンプルのステータス。 |
| `samplingRatio` | サンプリングした結合プロファイル(`numRowsToRead`)と結合された合計プロファイル(`totalRows`)の比率。10進数形式で表されます。 |
| `mergeStrategy` | サンプルで使用されるマージ方法。 |
| `lastSampledTimestamp` | 最後に成功したサンプルタイムスタンプ。 |

## データセット別リストプロファイル配布

プロファイルセットごとのGETの分布を確認するには、`/previewsamplestatus/report/dataset`エンドポイントに対してデータリクエストを実行します。

**API 形式**

```http
GET /previewsamplestatus/report/dataset
GET /previewsamplestatus/report/dataset?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 複数のレポートが実行された日付の場合は、その日付の最新のレポートが返されます。 指定した日付に対してレポートが存在しない場合は、404エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD。 例：`date=2024-12-31` |

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

この応答には、データセットオブジェクトのリストを含む`data`配列が含まれます。 表示された応答は、3つのデータセットを表示するように切り捨てられました。

>[!NOTE]
>
>日付に対して複数のレポートが存在する場合は、最新のレポートのみが返されます。 指定した日付にデータセットレポートが存在しない場合は、HTTPステータス404（エラー）が返されます。

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
| `samplePercentage` | `sampleCount`は、サンプルされた結合プロファイルの合計数（[最後のサンプル状態](#view-last-sample-status)で返された`numRowsToRead`値）に対する割合で、10進数形式で表されます。 |
| `fullIDsCount` | このデータセットIDとマージされたプロファイルの合計数。 |
| `fullIDsPercentage` | 結合されたプロファイルの合計数（[最後のサンプル状態](#view-last-sample-status)で返される`totalRows`値）に対する`fullIDsCount`のパーセンテージ。10進数形式で表します。 |
| `name` | データセットの作成時に提供される、データセットの名前。 |
| `description` | データセットの作成時に提供される、データセットの説明。 |
| `value` | データセットのID。 |
| `streamingIngestionEnabled` | データセットのストリーミング取り込みが有効になっているかどうか。 |
| `createdUser` | データセットを作成したユーザーのユーザーID。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト時に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターが指定されない場合は、最新のレポートが返されます。 |

## 名前空間別リストプロファイル配布

`/previewsamplestatus/report/namespace`エンドポイントに対してGETリクエストを実行し、プロファイルストア内の結合されたすべてのプロファイルのID名前空間別の分類を表示できます。

ID名前空間は、顧客データが関連付けられるコンテキストのインジケーターとして機能する、Adobe Experience PlatformIDサービスの重要なコンポーネントです。 詳しくは、まず[ID名前空間の概要](../../identity-service/namespaces.md)を読んでください。

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

次の要求では`date`パラメーターが指定されていないので、最新のレポートが返されます。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/previewsamplestatus/report/namespace \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

応答には`data`配列が含まれ、各名前空間の詳細が個々のオブジェクトに含まれます。 4つの名前空間が表示されるように、表示される応答は切り捨てられました。

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
| `samplePercentage` | `sampleCount`は、サンプリングされた結合プロファイルの割合（[最後のサンプルステータス](#view-last-sample-status)で返される`numRowsToRead`値）で、10進数形式で表されます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト時に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターが指定されない場合は、最新のレポートが返されます。 |
| `fullIDsFragmentCount` | 名前空間内のプロファイルフラグメントの合計数です。 |
| `fullIDsCount` | 名前空間内のマージされたプロファイルの合計数。 |
| `fullIDsPercentage` | 結合されたプロファイルの合計（[最後のサンプルステータス](#view-last-sample-status)で返される`totalRows`値）に対する`fullIDsCount`の割合（10進数形式）。 |
| `code` | 名前空間の`code`。 これは、[Adobe Experience PlatformIDサービスAPI](../../identity-service/api/list-namespaces.md)を使用して名前空間を操作する場合に見つかります。また、Experience PlatformUIでは[!UICONTROL ID記号]とも呼ばれます。 詳しくは、[ID名前空間の概要](../../identity-service/namespaces.md)を参照してください。 |
| `value` | 名前空間の`id`値。 これは、[IDサービスAPI](../../identity-service/api/list-namespaces.md)を使用する名前空間を操作する場合に見つかります。 |

## データセットの重複レポートの生成

データセット重複レポートは、アドレス指定可能なオーディエンス(プロファイル)に最も貢献するデータセットを公開することで、組織のプロファイルストアの構成を明確に示します。 このレポートは、データに洞察を提供するだけでなく、特定のデータセットのTTLの設定など、ライセンスの使用を最適化するための処置をとるのに役立ちます。

`/previewsamplestatus/report/dataset/overlap`エンドポイントに対してGETリクエストを実行することで、データセット重複レポートを生成できます。

コマンドラインまたはPostman UIを使用してデータセットの重複レポートを生成する手順を順を追って説明しています。[データセットの重複レポートの生成チュートリアル](../tutorials/dataset-overlap-report.md)を参照してください。

**API 形式**

```http
GET /previewsamplestatus/report/dataset/overlap
GET /previewsamplestatus/report/dataset/overlap?{QUERY_PARAMETERS}
```

| パラメーター | 説明 |
|---|---|
| `date` | 返されるレポートの日付を指定します。 同じ日付で複数のレポートが実行された場合、その日付の最新のレポートが返されます。 指定した日付に対してレポートが存在しない場合は、404（見つかりません）エラーが返されます。 日付を指定しない場合は、最新のレポートが返されます。 形式：YYYY-MM-DD。 例：`date=2024-12-31` |

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
| `data` | `data`オブジェクトには、データセットのカンマ区切りのリストと、それぞれのプロファイル数が含まれます。 |
| `reportTimestamp` | レポートのタイムスタンプ。 リクエスト時に`date`パラメーターが指定された場合、指定された日付のレポートが返されます。 `date`パラメーターが指定されない場合は、最新のレポートが返されます。 |

レポートの結果は、応答内のデータセットとプロファイル数から解釈できます。 次に示す例では、`data`オブジェクトをレポートします。

```json
  "5d92921872831c163452edc8,5da7292579975918a851db57,5eb2cdc6fa3f9a18a7592a98": 123,
  "5d92921872831c163452edc8,5eb2cdc6fa3f9a18a7592a98": 454412,
  "5eeda0032af7bb19162172a7": 107
```

このレポートには次の情報が表示されます。
* 以下のデータセットからのデータで構成されるプロファイルは123です。`5d92921872831c163452edc8`、`5da7292579975918a851db57`、`5eb2cdc6fa3f9a18a7592a98`。
* 次の2つのデータセットからのデータから構成されるプロファイルは454,412個です。`5d92921872831c163452edc8`と`5eb2cdc6fa3f9a18a7592a98`。
* データセット`5eeda0032af7bb19162172a7`のデータのみから構成されるプロファイルは107個です。
* 同組織には、合計454,642人のプロファイルが存在する。

## 次の手順

これで、プロファイルストアでサンプルデータをプレビューし、データセット重複レポートを実行する方法がわかったので、Segmentation Service APIの見積もりとプレビューエンドポイントを使用して、セグメント定義に関する表示の概要レベル情報を得ることもできます。 この情報は、セグメント内の期待されるオーディエンスを確実に分離するのに役立ちます。 セグメント化APIを使用したセグメントプレビューと予測の操作について詳しくは、[プレビューおよび予測エンドポイントガイド](../../segmentation/api/previews-and-estimates.md)を参照してください。

