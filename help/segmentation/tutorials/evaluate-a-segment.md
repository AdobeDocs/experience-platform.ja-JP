---
solution: Experience Platform
title: セグメント結果の評価とアクセス
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform Segmentation Service API を使用してセグメント定義を評価し、セグメント化結果にアクセスする方法について説明します。
exl-id: 47702819-f5f8-49a8-a35d-034ecac4dd98
source-git-commit: f6d700087241fb3a467934ae8e64d04f5c1d98fa
workflow-type: tm+mt
source-wordcount: '1594'
ht-degree: 47%

---

# セグメント定義結果の評価とアクセス

このドキュメントでは、セグメント定義を評価し、[[!DNL Segmentation API]](../api/getting-started.md) を使用してこれらの結果にアクセスするためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、オーディエンスの作成に関連する様々な [!DNL Adobe Experience Platform] サービスについての十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [[!DNL Real-Time Customer Profile]](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
- [[!DNL Adobe Experience Platform Segmentation Service]](../home.md):[!DNL Real-Time Customer Profile] データからオーディエンスを作成できます。
- [[!DNL Experience Data Model (XDM)]](../../xdm/home.md): Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。 セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。
- [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

### 必須ヘッダー

また、このチュートリアルでは、API を正しく呼び出すために、[ 認証に関するチュートリアル ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) を完了している必要 [!DNL Experience Platform] あります。 次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- Authorization： Bearer `{ACCESS_TOKEN}`
- x-api-key： `{API_KEY}`
- x-gw-ims-org-id： `{ORG_ID}`

[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されています。[!DNL Experience Platform] API へのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name：`{SANDBOX_NAME}`

>[!NOTE]
>
>[!DNL Experience Platform] のサンドボックスについて詳しくは、[サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)を参照してください。

すべての POST、PUT、および PATCH リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## セグメント定義の評価 {#evaluate-a-segment}

セグメント定義を開発、テスト、保存したら、スケジュールに沿った評価またはオンデマンド評価を行って、セグメント定義を評価できます。

[スケジュールされた評価](#scheduled-evaluation) （「スケジュールされたセグメント化」とも呼ばれる）では、特定の時間にエクスポートジョブを実行する定期的なスケジュールを作成できます。[オンデマンド評価](#on-demand-evaluation)では、オーディエンスを即座に構築するセグメントジョブを作成できます。それぞれの評価の手順を以下に示します。

[Segmentation API を使用したセグメント定義の作成 ](./create-a-segment.md) チュートリアルをまだ完了していない場合、または [ セグメントビルダー ](../ui/segment-builder.md) を使用してセグメント定義を作成した場合は、このチュートリアルに進む前に行ってください。

## スケジュールされた評価 {#scheduled-evaluation}

スケジュールされた評価を通じて、組織は繰り返しスケジュールを作成して、書き出しジョブを自動的に実行できます。

>[!NOTE]
>
>[!DNL XDM Individual Profile] の最大 5 つの結合ポリシーを備えたサンドボックスに対して、スケジュールされた評価を有効にできます。組織で、1 つのサンドボックス環境内に [!DNL XDM Individual Profile] の結合ポリシーが 6 つ以上ある場合は、スケジュールされた評価を使用できません。

### スケジュールの作成

`/config/schedules` エンドポイントに対して POST リクエストを実行することにより、スケジュールを作成し、スケジュールをトリガーする必要がある特定の時間を指定することができます。

このエンドポイントの使用について詳しくは、[ スケジュールエンドポイントガイドを参照してください ](../api/schedules.md#create)

### スケジュールの有効化

デフォルトでは、作成（POST）リクエスト本文で `state` プロパティが `active` に設定されていない限り、スケジュールは作成時に非アクティブになります。`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、パスにスケジュールの ID を含めることで、スケジュールを有効にする（`state` を `active` に設定する）ことができます。

このエンドポイントの使用について詳しくは、[ スケジュールエンドポイントガイドを参照してください ](../api/schedules.md#update-state)

### スケジュール時間の更新

`/config/schedules` エンドポイントに対して PATCH リクエストを実行し、スケジュールの ID をパスに含めることで、スケジュールのタイミングを更新できます。

このエンドポイントの使用について詳しくは、[ スケジュールエンドポイントガイドを参照してください ](../api/schedules.md#update-schedule)

## オンデマンド評価

オンデマンド評価を使用すると、セグメントジョブを作成して、必要なときにいつでもオーディエンスを生成できます。 スケジュールされた評価とは異なり、セグメントの生成はリクエストされた場合にのみ発生し、定期的に実行されません。

### セグメントジョブの作成

セグメントジョブは、オーディエンスセグメントをオンデマンドで作成する非同期プロセスです。 セグメント定義と、プロファイルフラグメント間で重複する属性の結合方法を制御する結合ポリシー [!DNL Real-Time Customer Profile] 参照します。 セグメントジョブが正常に完了すると、処理中に発生した可能性のあるエラーやオーディエンスの最終的なサイズなど、セグメント定義に関する様々な情報を収集できます。 セグメントジョブは、そのセグメント定義が現在該当するオーディエンスを更新するたびに実行する必要があります。

[!DNL Real-Time Customer Profile] API の `/segment/jobs` エンドポイントに POST リクエストをおこなうことで、新しいセグメントジョブを作成できます。

このエンドポイントの使用について詳しくは、[ セグメントジョブエンドポイントガイド ](../api/segment-jobs.md#create) を参照してください。

### セグメントジョブステータスの検索

特定のセグメントジョブの `id` を使用して、ルックアップリクエスト（GET）を実行すると、ジョブの現在のステータスを表示できます。

このエンドポイントの使用について詳しくは、[ セグメントジョブエンドポイントガイド ](../api/segment-jobs.md#get) を参照してください。

## セグメントジョブの結果の解釈

セグメントジョブが正常に実行されると、セグメント定義内に含まれる各プロファイルの `segmentMembership` マップが更新されます。 `segmentMembership` には、[!DNL Experience Platform] に取り込まれた事前評価済みオーディエンスも保存され、[!DNL Adobe Audience Manager] などの他のソリューションと統合できます。

次の例は、個々のプロファイルレコードの `segmentMembership` 属性がどのように記述されるかを示しています。

```json
{
  "segmentMembership": {
    "UPS": {
      "04a81716-43d6-4e7a-a49c-f1d8b3129ba9": {
        "timestamp": "2018-04-26T15:52:25+00:00",
        "status": "realized"
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
| `lastQualificationTime` | セグメントメンバーシップのアサーションが行われ、プロファイルがセグメント定義にエントリまたは離脱した際のタイムスタンプ。 |
| `status` | 現在のリクエストの一部としてのセグメント定義のパーティシペーションのステータス。 次の既知の値のいずれかと等しい必要があります。 <ul><li>`realized`：エンティティがセグメント定義の対象になります。</li><li>`exited`：エンティティがセグメント定義から離脱しています。</li></ul> |

>[!NOTE]
>
>`lastQualificationTime` に基づき、30 日を超えて「`exited`」ステータスにあるセグメントメンバーシップは、削除の対象となります。

## セグメントジョブの結果へのアクセス

セグメントジョブの結果には、2 つの方法のいずれかでアクセスできます。1 つの方法では、個々のプロファイルにアクセスし、もう 1 つの方法では、オーディエンス全体をデータセットにエクスポートします。

以下の節では、これらのオプションの概要を詳しく説明します。

## プロファイルの検索

アクセスする特定のプロファイルがわかっている場合は、[!DNL Real-Time Customer Profile] API を使用してアクセスできます。 個々のプロファイルにアクセスする完全な手順については、チュートリアル [ プロファイル API を使用したリアルタイム顧客プロファイルデータへのアクセス ](../../profile/api/entities.md) を参照してください。

## セグメントのエクスポート {#export}

セグメントジョブが正常に完了したら（`status` 属性の値は「SUCCEEDED」）、オーディエンスをデータセットにエクスポートして、オーディエンスにアクセスし、処理をおこなうことができます。

オーディエンスをエクスポートするには、次の手順を実行する必要があります。

- [ターゲットデータセットの作成](#create-a-target-dataset) - データセットを作成して、オーディエンスメンバーを保持します。
- [データセットでオーディエンスプロファイルを生成](#generate-profiles) - セグメントジョブの結果に基づいて、XDM 個別プロファイルをデータセットに入力します。
- [エクスポートの進行状況を監視](#monitor-export-progress) - エクスポートプロセスの現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) - オーディエンスのメンバーを表している、結果の XDM 個別プロファイルを取得します。

### ターゲットデータセットの作成 {#create-dataset}

オーディエンスをエクスポートする場合は、まずターゲットデータセットを作成する必要があります。データセットを正しく設定して、エクスポートが正常に行われるようにすることが重要です。

重要な考慮事項の 1 つは、データセットのベースとなるスキーマ（以下の API サンプルリクエストの `schemaRef.id`）です。セグメント定義を書き出すには、データセットが [!DNL XDM Individual Profile Union Schema] （`https://ns.adobe.com/xdm/context/profile__union`）に基づいている必要があります。 和集合スキーマは、システム生成された読み取り専用のスキーマであり、同じクラス（この場合は XDM 個別プロファイルクラス）を共有するスキーマのフィールドを集計します。和集合表示スキーマについて詳しくは、[ スキーマレジストリ開発者ガイドのリアルタイム顧客プロファイル ](../../xdm/api/getting-started.md) の節を参照してください。

必要なデータセットを作成する方法は 2 つあります。

- **API の使用：** このチュートリアルの手順では、[!DNL Catalog] API を使用して [!DNL XDM Individual Profile Union Schema] を参照するデータセットを作成する方法の概要を説明します。
- **UI の使用：** [!DNL Adobe Experience Platform] ユーザーインターフェイスを使用して和集合スキーマを参照するデータセットを作成するには、[UI チュートリアル ](../ui/overview.md) の手順に従い、このチュートリアルに戻って [ オーディエンスプロファイルの生成 ](#generate-profiles) 手順を進めます。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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

和集合を保持するデータセットが用意できたら、[!DNL Real-Time Customer Profile] API の `/export/jobs` エンドポイントに POST リクエストを行い、書き出すセグメント定義のデータセット ID とセグメント定義情報を指定することで、データセットにオーディエンスメンバーを保持する書き出しジョブを作成できます。

このエンドポイントの使用について詳しくは、[ 書き出しジョブエンドポイントガイド ](../api/export-jobs.md#create) を参照してください。

### エクスポートの進行状況の監視

エクスポートジョブを処理するときに、`/export/jobs` エンドポイントに対して GET リクエストを実行し、エクスポートジョブの `id` をパスを含めることで、エクスポートジョブのステータスを監視することができます。`status` フィールドによって値「SUCCEEDED」が返されると、エクスポートジョブが完了します。

このエンドポイントの使用について詳しくは、[ 書き出しジョブエンドポイントガイド ](../api/export-jobs.md#get) を参照してください。

## 次の手順

書き出しが正常に完了すると、[!DNL Experience Platform] の [!DNL Data Lake] 内でデータを使用できるようになります。 その後、[[!DNL Data Access API]](https://www.adobe.io/experience-platform-apis/references/data-access/) を使用して、書き出しに関連付けられた `batchId` を使用してデータにアクセスできます。 セグメント定義のサイズによっては、データがチャンクにまとめられ、バッチが複数のファイルで構成される場合があります。

[!DNL Data Access] API を使用してバッチファイルにアクセスし、ダウンロードする手順については、[ データアクセスのチュートリアル ](../../data-access/tutorials/dataset-data.md) に従ってください。

また、[!DNL Adobe Experience Platform Query Service] を使用して、正常に書き出されたセグメント定義データにアクセスすることもできます。 UI または RESTful API を使用す [!DNL Query Service] と、[!DNL Data Lake] 内のデータに対するクエリを記述、検証および実行できます。

オーディエンスデータのクエリ方法について詳しくは、[[!DNL Query Service]](../../query-service/home.md) のドキュメントを参照してください。
