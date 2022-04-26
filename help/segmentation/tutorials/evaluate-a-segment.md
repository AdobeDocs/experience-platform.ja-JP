---
keywords: Experience Platform；ホーム；人気のトピック；セグメント評価；セグメント化サービス；セグメント化；セグメント化；セグメントの評価；セグメント結果へのアクセス；セグメントの評価とアクセス；
solution: Experience Platform
title: セグメント結果の評価とアクセス
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform Segmentation Service API を使用してセグメントを評価し、セグメント結果にアクセスする方法について説明します。
exl-id: 47702819-f5f8-49a8-a35d-034ecac4dd98
source-git-commit: 9d82065fdf1be087023284f153f1abedb7fdb67b
workflow-type: tm+mt
source-wordcount: '1595'
ht-degree: 62%

---

# セグメント結果の評価とアクセス

このドキュメントでは、 [[!DNL Segmentation API]](../api/getting-started.md).

## はじめに

このチュートリアルでは、 [!DNL Adobe Experience Platform] オーディエンスセグメントの作成に関係するサービス。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):以下からオーディエンスセグメントを作成できます： [!DNL Real-time Customer Profile] データ。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)： Platform が顧客体験データを整理するための標準的なフレームワーク。セグメント化を最適に利用するには、 [データモデリングのベストプラクティス](../../xdm/schema/best-practices.md).
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必須ヘッダー

また、このチュートリアルでは、 API の呼び出しを正常におこなうために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。[!DNL Platform]次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。へのリクエスト [!DNL Platform] API には、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## セグメントの評価 {#evaluate-a-segment}

セグメント定義を開発、テスト、保存したら、スケジュールされた評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。[オンデマンド評価](#on-demand-evaluation)では、オーディエンスを即座に構築するセグメントジョブを作成できます。それぞれの評価の手順を以下に示します。

まだ [セグメント化 API を使用してセグメントを作成する](./create-a-segment.md) チュートリアルを使用するか、 [セグメントビルダー](../ui/overview.md)を使用する場合は、このチュートリアルに進む前におこないます。

## スケジュールされた評価 {#scheduled-evaluation}

IMS 組織では、スケジュールされた評価を通じて、エクスポートジョブを自動的に実行する定期的なスケジュールを作成できます。

>[!NOTE]
>
>の結合ポリシーが最大 5 個あるサンドボックスに対して、スケジュールされた評価を有効にすることができます。 [!DNL XDM Individual Profile]. 組織に対する結合ポリシーが 6 つ以上ある場合 [!DNL XDM Individual Profile] 単一のサンドボックス環境内では、スケジュールされた評価を使用できません。

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

このエンドポイントの使用に関する詳細については、 [スケジュールエンドポイントガイド](../api/schedules.md#create)

### スケジュールの有効化

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

このエンドポイントの使用に関する詳細については、 [スケジュールエンドポイントガイド](../api/schedules.md#update-state)

### スケジュール時間の更新

`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、スケジュールの ID をパスに含めることで、スケジュールのタイミングを更新できます。

このエンドポイントの使用に関する詳細については、 [スケジュールエンドポイントガイド](../api/schedules.md#update-schedule)

## オンデマンド評価

オンデマンド評価を使用すると、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。スケジュールされた評価とは異なり、セグメントの生成はリクエストされた場合にのみ発生し、定期的に実行されません。

### セグメントジョブの作成

セグメントジョブは、オーディエンスセグメントをオンデマンドで作成する非同期プロセスです。 セグメント定義と、その方法を制御する結合ポリシーを参照します [!DNL Real-time Customer Profile] は、複数のプロファイルフラグメントで重複している属性を結合します。 セグメントジョブが正常に完了したら、処理中に発生したエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。セグメント定義に現在適合しているオーディエンスを更新するたびに、セグメントジョブを実行する必要があります。

新しいセグメントジョブを作成するには、 `/segment/jobs` エンドポイント [!DNL Real-time Customer Profile] API

このエンドポイントの使用に関する詳細については、 [セグメントジョブエンドポイントガイド](../api/segment-jobs.md#create)

### セグメントジョブステータスの検索

特定のセグメントジョブの `id` を使用して、ルックアップリクエスト（GET）を実行すると、ジョブの現在のステータスを表示できます。

このエンドポイントの使用に関する詳細については、 [セグメントジョブエンドポイントガイド](../api/segment-jobs.md#get)

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメント内に含まれる各プロファイルの `segmentMembership` マップが更新されます。`segmentMembership` また、には、 [!DNL Platform]を使用して、 [!DNL Adobe Audience Manager].

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

アクセスする特定のプロファイルがわかっている場合は、 [!DNL Real-time Customer Profile] API 個々のプロファイルにアクセスするための完全な手順については、[プロファイル API を使用したリアルタイム顧客プロファイルデータへのアクセス](../../profile/api/entities.md)のチュートリアルで説明しています。

## セグメントのエクスポート {#export}

セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。

オーディエンスをエクスポートするには、次の手順を実行する必要があります。

- [ターゲットデータセットの作成](#create-a-target-dataset) - データセットを作成して、オーディエンスメンバーを保持します。
- [データセットでオーディエンスプロファイルを生成](#generate-profiles) - セグメントジョブの結果に基づいて、XDM 個別プロファイルをデータセットに入力します。
- [エクスポートの進行状況を監視](#monitor-export-progress) - エクスポートプロセスの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) - オーディエンスのメンバーを表している、結果の XDM 個別プロファイルを取得します。

### ターゲットデータセットの作成 {#create-dataset}

オーディエンスをエクスポートする場合は、まずターゲットデータセットを作成する必要があります。データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。セグメントをエクスポートするには、データセットが [!DNL XDM Individual Profile Union Schema] (`https://ns.adobe.com/xdm/context/profile__union`) をクリックします。 和集合スキーマは、システム生成された読み取り専用のスキーマであり、同じクラス（この場合は XDM 個別プロファイルクラス）を共有するスキーマのフィールドを集計します。和集合表示スキーマについて詳しくは、「スキーマレジストリ開発者ガイド」の「[リアルタイム顧客プロファイル](../../xdm/api/getting-started.md)」の節を参照してください。

必要なデータセットを作成する方法は 2 つあります。

- **API の使用：** このチュートリアルの以降の手順では、 [!DNL XDM Individual Profile Union Schema] の使用 [!DNL Catalog] API
- **UI の使用：**[!DNL Adobe Experience Platform] のユーザーインターフェイスを使用して、和集合スキーマを参照するデータセットを作成するには、[UI のチュートリアル](../ui/overview.md)の手順を実行してから、このチュートリアルに戻り、[オーディエンスプロファイルを生成する](#generate-profiles)手順に進みます。

互換性のあるデータセットが既に存在し、その ID がわかっている場合は、[オーディエンスプロファイルを生成する](#generate-profiles)手順に直接進むことができます。

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
    }
}'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | データセットのわかりやすい名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示（スキーマ）の ID。 |

**応答** 

応答に成功した場合、新しく作成されたデータセットの読み取り専用のシステム生成された一意の ID を含む配列が返されます。オーディエンスメンバーを正常にエクスポートするには、適切に設定されたデータセット ID が必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### オーディエンスメンバーのプロファイルの生成 {#generate-profiles}

和集合永続化データセットを取得したら、に対してPOSTリクエストを実行することで、オーディエンスメンバーをデータセットに保持する書き出しジョブを作成できます `/export/jobs` エンドポイント [!DNL Real-time Customer Profile] API を使用し、書き出すセグメントのデータセット ID とセグメント情報を指定します。

このエンドポイントの使用に関する詳細については、 [書き出しジョブエンドポイントガイド](../api/export-jobs.md#create)

### エクスポートの進行状況の監視

エクスポートジョブを処理するときに、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めることで、エクスポートジョブのステータスを監視することができます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

このエンドポイントの使用に関する詳細については、 [書き出しジョブエンドポイントガイド](../api/export-jobs.md#get)

## 次の手順

エクスポートが正常に完了すると、データは [!DNL Data Lake] in [!DNL Experience Platform]. その後、 [[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用してデータにアクセスするには `batchId` をエクスポートに関連付けました。 セグメントのサイズに応じて、データがチャンクに格納され、バッチが複数のファイルで構成される場合があります。

の使用方法に関する詳細な手順については、 [!DNL Data Access] バッチファイルにアクセスしてダウンロードする API では、 [データアクセスのチュートリアル](../../data-access/tutorials/dataset-data.md).

また、 [!DNL Adobe Experience Platform Query Service]. UI または RESTful API の使用 [!DNL Query Service] では、 [!DNL Data Lake].

オーディエンスデータに対してクエリを実行する方法の詳細については、 [[!DNL Query Service]](../../query-service/home.md).
