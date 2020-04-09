---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Segmentation Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 3fbacf57d5f6741726cb54fb55eab05042046f49

---


# Segmentation Service開発ガイド

セグメント化を使用すると、セグメントを作成し、リアルタイムの顧客オーディエンスデータからAdobe Experience Platformでプロファイルを生成できます。

## はじめに

このガイドでは、セグメント化の使用に関連する様々なAdobe Experience Platformサービスについて、実際に理解する必要があります。

- [セグメント](../home.md):リアルタイム顧客オーディエンスデータから顧客セグメントを作成できます。
- [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
- [リアルタイム顧客プロファイル](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムのプロファイルを顧客に提供します。
- [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

APIを使用してセグメントを正しく使用するために知っておく必要がある追加情報については、以下の節で説明します。

### サンプルAPI呼び出しの読み取り

Segmentation Service APIドキュメントには、リクエストの形式を設定する方法を示すAPI呼び出しの例が記載されています。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) （英語のみ）を参照してください。

### 必須ヘッダ

また、APIドキュメントでは、プラットフォームエンドポイントの呼び出しを正 [常に行うために](../../tutorials/authentication.md) 、認証のチュートリアルを完了している必要があります。 次に示すように、認証チュートリアルで、エクスペリエンスプラットフォームAPI呼び出しの必要な各ヘッダーの値を指定します。

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] エクスペリエンスプラットフォームでのサンドボックスの操作について詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

<!-- ## Estimates

Estimates provides statistical information for a segment definition, such as projected audience size and confidence interval. You can use the `/estimate` endpoint to view an estimate of a segment definition. 

For more information on using this endpoint, please read the [estimates developer guide](./estimates.md). 

## Export jobs

Export jobs are asynchronous processes that are used to persist audience segment members to datasets. You can use the `/export/jobs` endpoint to retrieve all export jobs, create a new export job, retrieve details of a specific export job, or cancel a specific export job.

For more information on using this endpoint, please read the [export jobs developer guide](./export-jobs.md).

## Previews

Previews provide a paginated list of qualifying profiles for a segment definition, allowing you to compare the results against what you expect. You can use the `/preview` endpoint to create a new preview job, look up results of a specific preview job, or delete a specific preview job.

For more information on using this endpoint, please read the [previews developer guide](./previews.md).

## PQL conversions

Profile Query Language (PQL) conversions allows you to convert your formatting between `pql/text` and `pql/json`. You can do this by using the `/segment/conversion` endpoint.

For more information on using this endpoint, please read the [PQL conversions developer guide](./pql-conversions.md).

## Schedules

Schedules are a tool that can be used to automatically run export jobs once a day. You can use the `/config/schedules` endpoint to retrieve a list of schedules, create a new schedule, retrieve details of a specific schedule, update a specific schedule, or delete a specific schedule. 

For more information on using this endpoint, please read the [schedules developer guide](./schedules.md).

## Segment definitions

Segment definitions define which profiles will be part of which audience segments. You can use the `/segment/definitions` endpoint to retrieve a list of segment definitions, create a new segment definition, retrieve details of a specific segment definition, delete a specific segment definition, or overwrite details of a specific segment definition.

For more information on using this endpoint, please read the [segment definitions developer guide](./segment-definitions.md). -->

## セグメントジョブ

セグメントジョブは、以前に確立されたセグメント定義を処理して、オーディエンスセグメントを生成します。 エンドポイントを使用して、 `/segment/jobs` セグメントジョブのリストの取得、新しいセグメントジョブの作成、特定のセグメントジョブの詳細の取得または特定のセグメントジョブの削除を行うことができます。

このエンドポイントの使用方法の詳細については、『セグメントジョブ開発ガ [イド』を参照してくださ](./segment-jobs.md)い。

## 次の手順

セグメントAPIを使用して呼び出しを開始するには、サブガイドの1つを選択して、特定のセグメント関連エンドポイントの使用方法を学びます。 プラットフォームUIを使用したセグメントの操作について詳しくは、セグメント化のユーザーガイドを [参照してくださ](../ui/overview.md)い。