---
keywords: Experience Platform；ホーム；人気のあるトピック；セグメント評価；セグメント化サービス；セグメント化；セグメント化；セグメントの評価；セグメント結果へのアクセス；セグメントの評価；セグメントの評価とアクセス；
solution: Experience Platform
title: セグメント結果の評価とアクセス
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform Segmentation Service API を使用してセグメントを評価し、セグメント結果にアクセスする方法について説明します。
exl-id: 47702819-f5f8-49a8-a35d-034ecac4dd98
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '1548'
ht-degree: 64%

---

# セグメント結果の評価とアクセス

このドキュメントでは、[[!DNL Segmentation API]](../api/getting-started.md) を使用してセグメントを評価し、セグメント結果にアクセスするためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関わる様々な [!DNL Adobe Experience Platform] サービスに関する十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):データからオーディエンスセグメントを作成 [!DNL Real-time Customer Profile] できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md):Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### 必須ヘッダー

また、このチュートリアルでは、 API の呼び出しを正常におこなうために、[認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了している必要があります。[!DNL Platform]次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{IMS_ORG}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Platform] API へのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name： `{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## セグメントの評価

セグメント定義を開発、テスト、保存したら、スケジュールされた評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。[オンデマンド評価](#on-demand-evaluation)では、オーディエンスを即座に構築するセグメントジョブを作成できます。それぞれの評価の手順を以下に示します。

まだセグメント化 API](./create-a-segment.md) を使用した [ セグメントの作成チュートリアルを完了していない場合、または [ セグメントビルダー ](../ui/overview.md) を使用してセグメント定義を作成していない場合は、このチュートリアルに進む前に作成してください。

## スケジュールされた評価 {#scheduled-evaluation}

IMS 組織では、スケジュールされた評価を通じて、エクスポートジョブを自動的に実行する定期的なスケジュールを作成できます。

>[!NOTE]
>
>[!DNL XDM Individual Profile] の結合ポリシーが最大 5 個あるサンドボックスに対して、スケジュールされた評価を有効にすることができます。 1 つのサンドボックス環境内に [!DNL XDM Individual Profile] の結合ポリシーが 6 つ以上ある場合、スケジュールされた評価を使用することはできません。

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

このエンドポイントの使用に関する詳細は、『[ スケジュールエンドポイントガイド ](../api/schedules.md#create)』を参照してください。

### スケジュールの有効化

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

このエンドポイントの使用に関する詳細は、『[ スケジュールエンドポイントガイド ](../api/schedules.md#update-state)』を参照してください。

### スケジュール時間の更新

`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、スケジュールの ID をパスに含めることで、スケジュールのタイミングを更新できます。

このエンドポイントの使用に関する詳細は、『[ スケジュールエンドポイントガイド ](../api/schedules.md#update-schedule)』を参照してください。

## オンデマンド評価

オンデマンド評価を使用すると、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。スケジュールされた評価とは異なり、セグメントの生成はリクエストされた場合にのみ発生し、定期的に実行されません。

### セグメントジョブの作成

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。セグメント定義と、[!DNL Real-time Customer Profile] によって複数のプロファイルフラグメント間で重なり合う属性を結合する方法を制御する結合ポリシーを参照します。 セグメントジョブが正常に完了したら、処理中に発生したエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

[!DNL Real-time Customer Profile] API の `/segment/jobs` エンドポイントにPOSTリクエストを送信して、新しいセグメントジョブを作成できます。

このエンドポイントの使用に関する詳細については、『[ セグメントジョブエンドポイントガイド ](../api/segment-jobs.md#create)』を参照してください。


### セグメントジョブステータスの検索

特定のセグメントジョブの `id` を使用して、ルックアップリクエスト（GET）を実行すると、ジョブの現在のステータスを表示できます。

このエンドポイントの使用に関する詳細については、『[ セグメントジョブエンドポイントガイド ](../api/segment-jobs.md#get)』を参照してください。

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメント内に含まれる各プロファイルの `segmentMembership` マップが更新されます。`segmentMembership` には、に取り込まれた評価済みのオーディエンスセグメントも格納さ [!DNL Platform]れ、などの他のソリューションと統合できま [!DNL Adobe Audience Manager]す。

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

アクセスする特定のプロファイルがわかっている場合は、[!DNL Real-time Customer Profile] API を使用してアクセスできます。 個々のプロファイルにアクセスするための完全な手順については、[プロファイル API を使用したリアルタイム顧客プロファイルデータへのアクセス](../../profile/api/entities.md)のチュートリアルで説明しています。

## セグメントのエクスポート {#export}

セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。

オーディエンスをエクスポートするには、次の手順を実行する必要があります。

- [ターゲットデータセットの作成](#create-a-target-dataset) - データセットを作成して、オーディエンスメンバーを保持します。
- [データセットでオーディエンスプロファイルを生成](#generate-profiles-for-audience-members) - セグメントジョブの結果に基づいて、XDM 個別プロファイルをデータセットに入力します。
- [エクスポートの進行状況を監視](#monitor-export-progress) - エクスポートプロセスの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) - オーディエンスのメンバーを表している、結果の XDM 個別プロファイルを取得します。

### ターゲットデータセットの作成

オーディエンスをエクスポートする場合は、まずターゲットデータセットを作成する必要があります。データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。セグメントをエクスポートするには、データセットが [!DNL XDM Individual Profile Union Schema] (`https://ns.adobe.com/xdm/context/profile__union`) に基づいている必要があります。 和集合スキーマは、システム生成された読み取り専用のスキーマであり、同じクラス（この場合は XDM 個別プロファイルクラス）を共有するスキーマのフィールドを集計します。和集合表示スキーマについて詳しくは、「スキーマレジストリ開発者ガイド」の「[リアルタイム顧客プロファイル](../../xdm/api/getting-started.md)」の節を参照してください。

必要なデータセットを作成する方法は 2 つあります。

- **API の使用：** このチュートリアルの以降の手順では、API を使用してを参照するデータセットを作成する方法につ [!DNL XDM Individual Profile Union Schema] いて説明 [!DNL Catalog] します。
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

和集合永続化データセットを作成したら、[!DNL Real-time Customer Profile] API の `/export/jobs` エンドポイントにPOSTリクエストを送信し、書き出すセグメントのデータセット ID とセグメント情報を指定して、オーディエンスメンバーをデータセットに保持する書き出しジョブを作成できます。

このエンドポイントの使用に関する詳細については、『[ エクスポートジョブエンドポイントガイド ](../api/export-jobs.md#create)』を参照してください。

### エクスポートの進行状況の監視

エクスポートジョブを処理するときに、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めることで、エクスポートジョブのステータスを監視することができます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

このエンドポイントの使用に関する詳細については、『[ エクスポートジョブエンドポイントガイド ](../api/export-jobs.md#get)』を参照してください。

## 次の手順

エクスポートが正常に完了すると、[!DNL Experience Platform] の [!DNL Data Lake] 内でデータを使用できます。 その後、[[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して、エクスポートに関連付けられた `batchId` を使用してデータにアクセスできます。 セグメントのサイズに応じて、データがチャンクに格納され、バッチが複数のファイルで構成される場合があります。

[!DNL Data Access] API を使用してバッチファイルにアクセスしてダウンロードする手順については、[ データアクセスのチュートリアル ](../../data-access/tutorials/dataset-data.md) を参照してください。

[!DNL Adobe Experience Platform Query Service] を使用して、正常にエクスポートされたセグメントデータにアクセスすることもできます。 [!DNL Query Service] は、UI または RESTful API を使用して、[!DNL Data Lake] 内のデータに対してクエリの書き込み、検証、実行をおこなえます。

オーディエンスデータに対してクエリを実行する方法の詳細については、[[!DNL Query Service]](../../query-service/home.md) のドキュメントを参照してください。
