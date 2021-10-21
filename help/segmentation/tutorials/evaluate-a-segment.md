---
keywords: 経験のあるプラットホーム、home、人気のある話題。セグメンテーションサービス、セグメンテーション、セグメンテーションセグメンテーション、セグメントの評価、セグメントの検索、セグメントへのアクセス
solution: Experience Platform
title: セグメント結果の評価とアクセス
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe エクスペリエンスプラットフォームセグメンテーションサービス API を使用して、セグメントを評価し、セグメント結果にアクセスする方法について説明します。
exl-id: 47702819-f5f8-49a8-a35d-034ecac4dd98
source-git-commit: 8325ae6fd7d0013979e80d56eccd05b6ed6f5108
workflow-type: tm+mt
source-wordcount: '1572'
ht-degree: 63%

---

# セグメント結果の評価とアクセス

このドキュメントでは、を使用してセグメントを評価し、セグメント結果にアクセスする方法を説明して [[!DNL Segmentation API]](../api/getting-started.md) います。

## はじめに

このチュートリアルでは、対象となるセグメントを作成するための様々なサービスについて、十分に理解しておく必要があり [!DNL Adobe Experience Platform] ます。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md): 複数のソースから集められたデータに基づいて、統合されたカスタマープロファイルをリアルタイムに提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md)「」: データから対象ユーザーセグメントを作成できます [!DNL Real-time Customer Profile] 。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md): プラットフォームによって顧客の体験データが整理される標準化されたフレームワーク。 セグメンテーションを最大限に活用するには、データモデリングのベストプラクティスに従い、データをプロファイルとイベントとして ingested にしておく必要が [ ](../../xdm/schema/best-practices.md) あります。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必須ヘッダー

また、このチュートリアルでは、 API の呼び出しを正常におこなうために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。Api に対する要求 [!DNL Platform] には、操作が行われるサンドボックスの名前を指定するヘッダーが必要になります。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## セグメントの評価

セグメント定義を開発、テスト、保存したら、スケジュールされた評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。[オンデマンド評価](#on-demand-evaluation)では、オーディエンスを即座に構築するセグメントジョブを作成できます。それぞれの評価の手順を以下に示します。

セグメントの作成を完了していない場合 [ は、セグメンテーション API のチュートリアルを使用してセグメントを作成する ](./create-a-segment.md) か、または segment Builder を使用してセグメント定義を作成してから、 [ ](../ui/overview.md) このチュートリアルに進む前に、この操作を行ってください。

## スケジュールされた評価 {#scheduled-evaluation}

IMS 組織では、スケジュールされた評価を通じて、エクスポートジョブを自動的に実行する定期的なスケジュールを作成できます。

>[!NOTE]
>
>最大5個の結合ポリシーが設定されたサンドボックスについては、定期的な評価を有効にすることができ [!DNL XDM Individual Profile] ます。 1つのサンドボックス環境内に5個以上の統合ポリシーがある場合 [!DNL XDM Individual Profile] 、定期的な評価を使用することはできません。

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

このエンドポイントの使用について詳しくは、 [ スケジュールエンドポイントガイドを参照してください。](../api/schedules.md#create)

### スケジュールの有効化

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

このエンドポイントの使用について詳しくは、 [ スケジュールエンドポイントガイドを参照してください。](../api/schedules.md#update-state)

### スケジュール時間の更新

`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、スケジュールの ID をパスに含めることで、スケジュールのタイミングを更新できます。

このエンドポイントの使用について詳しくは、 [ スケジュールエンドポイントガイドを参照してください。](../api/schedules.md#update-schedule)

## オンデマンド評価

オンデマンド評価を使用すると、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。スケジュールされた評価とは異なり、セグメントの生成はリクエストされた場合にのみ発生し、定期的に実行されません。

### セグメントジョブの作成

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。これにより、セグメント定義が参照されます。また、複数のマージポリシーを使用して、 [!DNL Real-time Customer Profile] プロファイルフラグメント間で重複する属性を制御することもできます。 セグメントジョブが正常に完了したら、処理中に発生したエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

新しいセグメントジョブを作成するには、API 内のエンドポイントに対して POST 要求を行い `/segment/jobs` [!DNL Real-time Customer Profile] ます。

このエンドポイントの使用に関する詳細情報については、 [ segment jobs エンドポイントガイドを参照してください。](../api/segment-jobs.md#create)


### セグメントジョブステータスの検索

特定のセグメントジョブの `id` を使用して、ルックアップリクエスト（GET）を実行すると、ジョブの現在のステータスを表示できます。

このエンドポイントの使用に関する詳細情報については、 [ segment jobs エンドポイントガイドを参照してください。](../api/segment-jobs.md#get)

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメント内に含まれる各プロファイルの `segmentMembership` マップが更新されます。`segmentMembership` には、ingested という事前に評価された対象ユーザーセグメントが格納され [!DNL Platform] ます。また、などの他のソリューションとの統合にも使用でき [!DNL Adobe Audience Manager] ます。

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

使用するプロファイルがわかっている場合は、この API を使用してアクセスでき [!DNL Real-time Customer Profile] ます。 個々のプロファイルにアクセスするための完全な手順については、[プロファイル API を使用したリアルタイム顧客プロファイルデータへのアクセス](../../profile/api/entities.md)のチュートリアルで説明しています。

## セグメントのエクスポート {#export}

セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。

オーディエンスをエクスポートするには、次の手順を実行する必要があります。

- [ターゲットデータセットの作成](#create-a-target-dataset) - データセットを作成して、オーディエンスメンバーを保持します。
- [データセットでオーディエンスプロファイルを生成](#generate-profiles-for-audience-members) - セグメントジョブの結果に基づいて、XDM 個別プロファイルをデータセットに入力します。
- [エクスポートの進行状況を監視](#monitor-export-progress) - エクスポートプロセスの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) - オーディエンスのメンバーを表している、結果の XDM 個別プロファイルを取得します。

### ターゲットデータセットの作成

オーディエンスをエクスポートする場合は、まずターゲットデータセットを作成する必要があります。データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。セグメントを書き出すためには、データセットが () に基づいている必要があり [!DNL XDM Individual Profile Union Schema] `https://ns.adobe.com/xdm/context/profile__union` ます。 和集合スキーマは、システム生成された読み取り専用のスキーマであり、同じクラス（この場合は XDM 個別プロファイルクラス）を共有するスキーマのフィールドを集計します。和集合表示スキーマについて詳しくは、「スキーマレジストリ開発者ガイド」の「[リアルタイム顧客プロファイル](../../xdm/api/getting-started.md)」の節を参照してください。

必要なデータセットを作成する方法は 2 つあります。

- **Api を使用する:** このチュートリアルでは、 [!DNL XDM Individual Profile Union Schema] api を使用してを参照するデータセットを作成する方法について説明して [!DNL Catalog] います。
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

ユニオンが保存されていないデータセットがある場合は、API のエンドポイントに対して POST 要求を実行 `/export/jobs` [!DNL Real-time Customer Profile] し、書き出すセグメントのデータセット ID とセグメント情報を指定することによって、データセットに対象メンバーを永続化するための書き出しジョブを作成できます。

このエンドポイントの使用について詳しくは、「 [ ジョブの輸出エンドポイントガイド」を参照してください。](../api/export-jobs.md#create)

### エクスポートの進行状況の監視

エクスポートジョブを処理するときに、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めることで、エクスポートジョブのステータスを監視することができます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

このエンドポイントの使用について詳しくは、「 [ ジョブの輸出エンドポイントガイド」を参照してください。](../api/export-jobs.md#get)

## 次の手順

エクスポートが正常に完了すると、内でデータを使用できるように [!DNL Data Lake] [!DNL Experience Platform] なります。 これにより、を使用し [[!DNL Data Access API] ](https://www.adobe.io/experience-platform-apis/references/data-access/) て、関連付けられたファイルを使用してデータにアクセスでき `batchId` ます。セグメントのサイズに応じて、データがチャンクに格納され、バッチが複数のファイルで構成される場合があります。

この API を使用してバッチファイルにアクセスし、ダウンロードする方法については [!DNL Data Access] 、データアクセスのチュートリアルを参照して [ ](../../data-access/tutorials/dataset-data.md) ください。

を使用して、正常に書き出されたセグメントのデータにアクセスすることもでき [!DNL Adobe Experience Platform Query Service] ます。 UI または RESTful API を使用すると、 [!DNL Query Service] 内のデータに対してクエリーを記述、検証および実行でき [!DNL Data Lake] ます。

対象ユーザーデータのクエリー方法について詳しくは、のドキュメントを参照してください [[!DNL Query Service]](../../query-service/home.md) 。
