---
keywords: Experience Platform;home;popular topics;segment evaluation;Segmentation Service;segmentation;Segmentation;evaluate a segment;access segment results;evaluate and access segment;
solution: Experience Platform
title: セグメントの評価
topic: tutorial
type: Tutorial
description: このドキュメントでは、セグメント化 API を使用してセグメントを評価したり、セグメント結果にアクセスしたりするためのチュートリアルを提供します。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '1535'
ht-degree: 63%

---


# セグメント結果の評価とアクセス

This document provides a tutorial for evaluating segments and accessing segment results using the [[!DNL Segmentation API]](../api/getting-started.md).

## はじめに

This tutorial requires a working understanding of the various [!DNL Adobe Experience Platform] services involved in creating audience segments. このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):プラットフォームが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

### 必須ヘッダー

また、このチュートリアルでは、 API の呼び出しを正常におこなうために、[認証に関するチュートリアル](../../tutorials/authentication.md)を完了している必要があります。[!DNL Platform]Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

- Authorization: Bearer `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform] are isolated to specific virtual sandboxes. Requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>For more information on sandboxes in [!DNL Platform], see the [sandbox overview documentation](../../sandboxes/home.md).

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## セグメントの評価

セグメント定義を開発、テスト、保存したら、スケジュールされた評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。[オンデマンド評価](#on-demand-evaluation)では、オーディエンスを即座に構築するセグメントジョブを作成できます。それぞれの評価の手順を以下に示します。

If you have not yet completed the [create a segment using the Segmentation API](./create-a-segment.md) tutorial or created a segment definition using [Segment Builder](../ui/overview.md), please do so before proceeding with this tutorial.

## スケジュールされた評価 {#scheduled-evaluation}

IMS 組織では、スケジュールされた評価を通じて、エクスポートジョブを自動的に実行する定期的なスケジュールを作成できます。

>[!NOTE]
>
>Scheduled evaluation can be enabled for sandboxes with a maximum of five (5) merge policies for [!DNL XDM Individual Profile]. If your organization has more than five merge policies for [!DNL XDM Individual Profile] within a single sandbox environment, you will not be able to use scheduled evaluation.

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#create)

### スケジュールの有効化

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#update-state)

### スケジュール時間の更新

`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、スケジュールの ID をパスに含めることで、スケジュールのタイミングを更新できます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#update-schedule)

## オンデマンド評価

オンデマンド評価を使用すると、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。スケジュールされた評価とは異なり、セグメントの生成はリクエストされた場合にのみ発生し、定期的に実行されません。

### セグメントジョブの作成

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。It references a segment definition, as well as any merge policies controlling how [!DNL Real-time Customer Profile] merges overlapping attributes across your profile fragments. セグメントジョブが正常に完了したら、処理中に発生したエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

You can create a new segment job by making a POST request to the `/segment/jobs` endpoint in the [!DNL Real-time Customer Profile] API.

このエンドポイントの使用に関する詳細は、『 [セグメントジョブエンドポイントガイド』を参照してください。](../api/segment-jobs.md#create)


### セグメントジョブステータスの検索

特定のセグメントジョブの `id` を使用して、ルックアップリクエスト（GET）を実行すると、ジョブの現在のステータスを表示できます。

このエンドポイントの使用に関する詳細は、『 [セグメントジョブエンドポイントガイド』を参照してください。](../api/segment-jobs.md#get)

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメント内に含まれる各プロファイルの `segmentMembership` マップが更新されます。`segmentMembership` また、に取り込まれた評価済みオーディエンスセグメントが保存され [!DNL Platform]、などの他のソリューションと統合でき [!DNL Adobe Audience Manager]ます。

次の例は、個々のプロファイルレコードの `segmentMembership` 属性がどのように記述されるかを示しています。

```json
{
  "segmentMembership": {
    "UPS": {
      "04a81716-43d6-4e7a-a49c-f1d8b3129ba9": {
        "timestamp": "2018-04-26T15:52:25+00:00",
        "status": "existing"
      },
      "53cba6b2-a23b-454a-8069-fc41308f1c0f": {
        "lastQualificationTime": "2018-04-26T15:52:25+00:00",
        "status": "realized"
      }
    },
    "Email": {
      "abcd@adobe.com": {
        "lastQualificationTime": "2017-09-26T15:52:25+00:00",
        "status": "exited"
      }
    }
  }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `lastQualificationTime` | セグメントメンバーシップのアサーションが行われて、プロファイルがセグメントに出入りしたときのタイムスタンプ。 |
| `status` | 現在のリクエストの一部としてのセグメント参加のステータス。次の既知の値のいずれかと等しい必要があります。 <ul><li>`existing`：エンティティが引き続きセグメント内に存在します。</li><li>`realized`：エンティティがセグメントに入っています。</li><li>`exited`：エンティティがセグメントから退出しています。</li></ul> |

## セグメント結果へのアクセス

セグメントジョブの結果には、2 つの方法のいずれかでアクセスできます。1 つの方法では、個々のプロファイルにアクセスし、もう 1 つの方法では、オーディエンス全体をデータセットにエクスポートします。

以下の節では、これらのオプションの概要を詳しく説明します。

## プロファイルの検索

If you know the specific profile that you would like to access, you can do so using the [!DNL Real-time Customer Profile] API. 個々のプロファイルにアクセスするための完全な手順については、[プロファイル API を使用したリアルタイム顧客プロファイルデータへのアクセス](../../profile/api/entities.md)のチュートリアルで説明しています。

## セグメントのエクスポート {#export}

セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。

オーディエンスをエクスポートするには、次の手順を実行する必要があります。

- [ターゲットデータセットの作成](#create-a-target-dataset) - データセットを作成して、オーディエンスメンバーを保持します。
- [データセットでオーディエンスプロファイルを生成](#generate-profiles-for-audience-members) - セグメントジョブの結果に基づいて、XDM 個別プロファイルをデータセットに入力します。
- [エクスポートの進行状況を監視](#monitor-export-progress) - エクスポートプロセスの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) - オーディエンスのメンバーを表している、結果の XDM 個別プロファイルを取得します。

### ターゲットデータセットの作成

オーディエンスをエクスポートする場合は、まずターゲットデータセットを作成する必要があります。データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。セグメントをエクスポートするには、 [!DNL XDM Individual Profile Union Schema] (`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、システム生成された読み取り専用のスキーマであり、同じクラス（この場合は XDM 個別プロファイルクラス）を共有するスキーマのフィールドを集計します。和集合表示スキーマについて詳しくは、「スキーマレジストリ開発者ガイド」の「[リアルタイム顧客プロファイル](../../xdm/api/getting-started.md)」の節を参照してください。

必要なデータセットを作成する方法は 2 つあります。

- **APIの使用：** このチュートリアルで説明する手順は、 [!DNL XDM Individual Profile Union Schema][!DNL Catalog] APIを使用して、データセットを参照するデータセットを作成する方法を示しています。
- **UI の使用：**[!DNL Adobe Experience Platform] のユーザーインターフェイスを使用して、和集合スキーマを参照するデータセットを作成するには、[UI のチュートリアル](../ui/overview.md)の手順を実行してから、このチュートリアルに戻り、[オーディエンスプロファイルを生成する](#generate-xdm-profiles-for-audience-members)手順に進みます。

互換性のあるデータセットが既に存在し、その ID がわかっている場合は、[オーディエンスプロファイルを生成する](#generate-xdm-profiles-for-audience-members)手順に直接進むことができます。

**API 形式**

```http
POST /dataSets
```

**リクエスト**

次のリクエストは、新しいデータセットを作成し、ペイロードに設定パラメーターを提供します。

```shell
curl -X POST \
  https://platform.adobe.io/data/foundation/catalog/dataSets \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "name": "Segment Export",
    "schemaRef": {
        "id": "https://ns.adobe.com/xdm/context/profile__union",
        "contentType": "application/vnd.adobe.xed+json;version=1"
    },
    "fileDescription": {
        "persisted": true,
        "containerFormat": "parquet",
        "format": "parquet"
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | データセットのわかりやすい名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示（スキーマ）の ID。 |
| `fileDescription.persisted` | ブール値。`true` に設定した場合、データセットが和集合表示に保持されます。 |

**応答** 

応答に成功した場合、新しく作成されたデータセットの読み取り専用のシステム生成された一意の ID を含む配列が返されます。オーディエンスメンバーを正常にエクスポートするには、適切に設定されたデータセット ID が必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### オーディエンスメンバーのプロファイルの生成 {#generate-profiles}

Once you have a union-persisting dataset, you can create an export job to persist the audience members to the dataset by making a POST request to the `/export/jobs` endpoint in the [!DNL Real-time Customer Profile] API and providing the dataset ID and the segment information for the segments that you wish to export.

このエンドポイントの使用に関する詳細は、『 [書き出しジョブエンドポイントガイド』を参照してください。](../api/export-jobs.md#create)

### エクスポートの進行状況の監視

エクスポートジョブを処理するときに、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めることで、エクスポートジョブのステータスを監視することができます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

このエンドポイントの使用に関する詳細は、『 [書き出しジョブエンドポイントガイド』を参照してください。](../api/export-jobs.md#get)

## 次の手順

Once the export has completed successfully, your data is available within the [!DNL Data Lake] in [!DNL Experience Platform]. You can then use the [[!DNL Data Access API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) to access the data using the `batchId` associated with the export. セグメントのサイズに応じて、データがチャンクに格納され、バッチが複数のファイルで構成される場合があります。

For step-by-step instructions on how to use the [!DNL Data Access] API to access and download batch files, follow the [Data Access tutorial](../../data-access/tutorials/dataset-data.md).

また、を使用して、正常にエクスポートされたセグメントデータにアクセスすることもでき [!DNL Adobe Experience Platform Query Service]ます。 Using the UI or RESTful API, [!DNL Query Service] allows you to write, validate, and run queries on data within the [!DNL Data Lake].

For more information on how to query audience data, please review the documentation on [[!DNL Query Service]](../../query-service/home.md).
