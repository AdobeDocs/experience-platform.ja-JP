---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Segmentation Service開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: c0eacfba2feea66803e63ed55ad9d0a97e9ae47c
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---


# Getting started with [!DNL Segmentation Service] {#getting-started}

Adobe Experience Platformセグメントサービスを使用すると、セグメントを作成し、 [!DNL Real-time Customer Profile] データをAdobe Experience Platformしてオーディエンスを生成できます。

開発者ガイドでは、の使用に関連する様々なExperience Platformサービスについて、作業を理解している必要があり [!DNL Segmentation Service]ます。

- [!DNL Segmentation](../home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
- [!DNL Experience Data Model (XDM) System](../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
- [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、 [!DNL Segmentation] APIを正しく操作するために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

APIドキュメントには、リクエストをフォーマットする方法を示すAPI呼び出しの例が含まれています。 [!DNL Segmentation Service] 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

## 必須ヘッダー

また、APIドキュメントでは、Platformエンドポイントの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があります。 次に示すように、Experience PlatformAPI呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>でのサンドボックスの操作について詳し [!DNL Experience Platform]くは、 [サンドボックスの概要ドキュメント](../../sandboxes/home.md)を参照してください。

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

For more information on using this endpoint, please read the [schedules developer guide](./schedules.md). -->

## セグメント定義

セグメント定義は、どのプロファイルがどのオーディエンスセグメントに含まれるかを定義します。 エンドポイントを使用して、セグメント定義のリストの取得、新しいセグメント定義の作成、特定のセグメント定義の詳細の取得、特定のセグメント定義の削除または特定のセグメント定義の詳細の上書きを行うことができます。 `/segment/definitions`

このエンドポイントの使用方法の詳細については、『 [セグメント定義開発者ガイド](./segment-definitions.md)』を参照してください。

## セグメントジョブ

セグメントジョブは、以前に確立されたセグメント定義を処理して、オーディエンスセグメントを生成します。 エンドポイントを使用して、セグメントジョブのリストの取得、新しいセグメントジョブの作成、特定のセグメントジョブの詳細の取得または特定のセグメントジョブの削除を行うことができます。 `/segment/jobs`

このエンドポイントの使用方法の詳細については、『 [セグメントジョブ開発者ガイド](./segment-jobs.md)』を参照してください。

## セグメント検索

セグメント検索は、様々なデータソースに含まれる設定可能なフィールドを検索およびインデックス付けして、ほぼリアルタイムで返すために使用します。 セグメント検索の使用を開始するには、 [検索開発者ガイドを参照してください](segment-search.md)

## 次の手順

APIを使用して呼び出しを行うには、左側のナビゲーションまたは [!DNL Segmentation Service][開発者ガイドの概要で、使用可能なエンドポイントガイドの1つを選択します](./overview.md)