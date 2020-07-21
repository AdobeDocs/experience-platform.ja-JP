---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: セグメントの評価
topic: tutorial
translation-type: tm+mt
source-git-commit: 6a0a9b020b0dc89a829c557bdf29b66508a10333
workflow-type: tm+mt
source-wordcount: '1519'
ht-degree: 0%

---


# セグメント結果の評価とアクセス

このドキュメントでは、セグメントを評価し、を使用してセグメントの結果にアクセスするためのチュートリアルを提供し [!DNL Segmentation API](../api/getting-started.md)ます。

## はじめに

このチュートリアルでは、オーディエンスセグメントの作成に関わる様々な [!DNL Adobe Experience Platform] サービスについて、十分に理解している必要があります。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
- [!DNL Adobe Experience Platform Segmentation Service](../home.md): データからオーディエンスセグメントを作成でき [!DNL Real-time Customer Profile] ます。
- [!DNL Experience Data Model (XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

### 必須ヘッダー

また、このチュートリアルでは、APIの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があり [!DNL Platform] ます。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへの要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

すべてのPOST、PUT、およびPATCHリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## セグメントの評価

セグメント定義を開発、テストおよび保存したら、予定された評価またはオンデマンド評価を通じてセグメントを評価できます。

[スケジュール評価](#scheduled-evaluation) （「スケジュール済みセグメント化」とも呼ばれます）では、特定の時間にエクスポートジョブを実行するための定期的なスケジュールを作成できます。 [オンデマンド評価では、オーディエンスを即座に作成するセグメントジョブを作成します](#on-demand-evaluation) 。 それぞれの手順を以下に示します。

まだセグメント化APIのチュートリアルを使用してセグメントを [作成していない場合、または](./create-a-segment.md) セグメントビルダーを使用してセグメント定義を作成していない場合は、このチュートリアルに進む前に作成してください [](../ui/overview.md)。

## 評価の予定 {#scheduled-evaluation}

定期評価を通じて、IMS組織は定期的なスケジュールを作成し、エクスポートジョブを自動的に実行できます。

>[!NOTE]
>
>スケジュールされた評価は、最大5つのマージポリシーを持つサンドボックスに対して有効にでき [!DNL XDM Individual Profile]ます。 1つのSandbox環境内に5つを超えるマージポリシーがある場合、 [!DNL XDM Individual Profile] 予定された評価を使用できません。

### スケジュールの作成

エンドポイントにPOSTリクエストを行うことで、スケジュールを作成し、スケジュールをトリガーする特定の時間を含めることがで `/config/schedules` きます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#create)

### スケジュールの有効化

デフォルトでは、スケジュールは作成時(POST)のリクエスト本文で `state` プロパティがに設定されていない限り、作成時 `active` には非アクティブになります。 エンドポイントにPATCHリクエストを行い、 `state` パスにスケジュールのIDを含めることで、スケジュールを有効にする( `active`toに設定する `/config/schedules` )ことができます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#update-state)

### スケジュール時刻の更新

スケジュールのタイミングは、エンドポイントにPATCHリクエストを行い、 `/config/schedules` パスにスケジュールのIDを含めることで更新できます。

このエンドポイントの使用に関する詳細は、 [スケジュールエンドポイントガイドを参照してください](../api/schedules.md#update-schedule)

## オンデマンド評価

オンデマンド評価では、必要に応じてオーディエンスセグメントを生成するためのセグメントジョブを作成できます。 スケジュールされた評価とは異なり、これはリクエストされた場合にのみ発生し、定期的な評価とは異なります。

### セグメントジョブの作成

セグメントジョブは、新しいオーディエンスセグメントを作成する非同期プロセスです。 セグメント定義と、プロファイルフラグメント間で重なり合う属性を結合する方法を制御する結合ポリシーを参照します。 [!DNL Real-time Customer Profile] セグメントジョブが正常に完了したら、処理中に発生した可能性のあるエラーやオーディエンスの最大サイズなど、セグメントに関する様々な情報を収集できます。

APIのエンドポイントにPOSTリクエストを作成して、新しいセグメントジョブを作成 `/segment/jobs` でき [!DNL Real-time Customer Profile] ます。

このエンドポイントの使用に関する詳細は、『 [セグメントジョブエンドポイントガイド』を参照してください。](../api/segment-jobs.md#create)


### セグメントジョブステータスの検索

特定のセグメントジョブ `id` に対してを使用して、ルックアップリクエスト(GET)を実行し、ジョブの現在のステータスを表示できます。

このエンドポイントの使用に関する詳細は、『 [セグメントジョブエンドポイントガイド』を参照してください。](../api/segment-jobs.md#get)

## セグメント結果の解釈

セグメントジョブが正常に実行されると、セグメントに含まれるプロファイルごとに `segmentMembership` マップが更新されます。 `segmentMembership` また、に取り込まれた評価済みオーディエンスセグメントが保存され [!DNL Platform]、などの他のソリューションと統合でき [!DNL Adobe Audience Manager]ます。

次の例は、個々のプロファイルレコードの `segmentMembership` 属性がどのように表示されるかを示しています。

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
| `lastQualificationTime` | セグメントのメンバーシップがアサーションされ、プロファイルがセグメントに入るか出るときのタイムスタンプ。 |
| `status` | 現在のリクエストの一部としてのセグメントパーティシペーションのステータス。 次の既知の値のいずれかと等しい必要があります。 <ul><li>`existing`: エンティティが引き続きセグメント内にあります。</li><li>`realized`: エンティティがセグメントを入力しています。</li><li>`exited`: エンティティがセグメントを終了しています。</li></ul> |

## セグメント結果へのアクセス

セグメントジョブの結果は、次の2つの方法のいずれかでアクセスできます。 個々のプロファイルにアクセスしたり、オーディエンス全体をデータセットにエクスポートしたりできます。

次の節で、これらのオプションについて詳しく説明します。

## プロファイルの検索

アクセスする特定のプロファイルがわかっている場合は、 [!DNL Real-time Customer Profile] APIを使用してアクセスできます。 個々のプロファイルにアクセスするための完全な手順は、プロファイルAPIのチュートリアルの「リアルタイム [プロファイルデータにアクセスする](../../profile/api/entities.md) 」で確認できます。

## セグメントのエクスポート {#export}

セグメント化ジョブが正常に完了したら( `status` 属性の値は「SUCCEEDED」です)、オーディエンスをデータセットにエクスポートし、データセットにアクセスして処理できます。

オーディエンスを書き出すには、次の手順が必要です。

- [ターゲットデータセットの作成](#create-a-target-dataset) -オーディエンスメンバーを格納するデータセットを作成します。
- [データセットでのオーディエンスプロファイルの生成](#generate-profiles-for-audience-members) — セグメントジョブの結果に基づいて、XDM Individualプロファイルをデータセットに組み込みます。
- [書き出しの進行状況を監視](#monitor-export-progress) — 書き出し処理の現在の進行状況を確認します。
- [オーディエンスデータの読み取り](#next-steps) -オーディエンスのメンバを表すXDM個々のプロファイルを取得します。

### ターゲットデータセットの作成

オーディエンスを書き出す場合は、まずターゲットデータセットを作成する必要があります。 データセットを正しく設定して、エクスポートが正常に完了するようにすることが重要です。

重要な考慮事項の1つは、データセットのベースとなるスキーマです(以下のAPIサンプルリクエスト`schemaRef.id` を参照)。 セグメントをエクスポートするには、 [!DNL XDM Individual Profile Union Schema] (`https://ns.adobe.com/xdm/context/profile__union`)に基づくデータセットが必要です。 和集合スキーマは、システム生成の読み取り専用スキーマで、同じクラス(この場合はXDM Individualプロファイルクラス)を共有するスキーマのフィールドを集計します。 和集合表示のスキーマの詳細については、『スキーマレジストリ開発ガイド』の「 [リアルタイム顧客プロファイル」の節を参照してください](../../xdm/api/getting-started.md)。

必要なデータセットを作成する方法は2つあります。

- **APIの使用：** このチュートリアルで説明する手順は、 [!DNL XDM Individual Profile Union Schema][!DNL Catalog] APIを使用して、データセットを参照するデータセットを作成する方法を示しています。
- **UIの使用：** 和集合スキーマを参照するデータセットを作成するには、 [!DNL Adobe Experience Platform] UIチュートリアルの手順に従い [、このチュートリアルに戻って、オーディエンスプロファイルの](../ui/overview.md) 生成手順に進みます [](#generate-xdm-profiles-for-audience-members)。

互換性のあるデータセットが既に存在し、そのIDがわかっている場合は、オーディエンスプロファイルの [生成手順に直接進むことができます](#generate-xdm-profiles-for-audience-members)。

**API形式**

```http
POST /dataSets
```

**リクエスト**

次のリクエストは、ペイロードに設定パラメーターを提供する新しいデータセットを作成します。

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
| `name` | データセットを説明する名前。 |
| `schemaRef.id` | データセットが関連付けられる和集合表示(スキーマ)のID。 |
| `fileDescription.persisted` | に設定した場合、和集合セットが表示内で保持され `true`るようにするBoolean値です。 |

**応答**

正常に完了すると、新たに作成されたデータセットの読み取り専用の、システム生成の一意のIDを含む配列が返されます。 オーディエンスメンバーを正常にエクスポートするには、適切に設定されたデータセットIDが必要です。

```json
[
  "@/datasets/5b020a27e7040801dedba61b"
] 
```

### オーディエンスメンバのプロファイルの生成 {#generate-profiles}

和集合永続性データセットを取得したら、APIのエンドポイントにPOSTリクエストを送信し、エクスポートするセグメントのデータセットIDとセグメント情報を提供することで、オーディエンスをデータセットに永続化するエクスポートジョブを作成できます。 `/export/jobs`[!DNL Real-time Customer Profile]

このエンドポイントの使用に関する詳細は、『 [書き出しジョブエンドポイントガイド』を参照してください。](../api/export-jobs.md#create)

### 書き出しの進行状況の監視

書き出しジョブの処理時に、エンドポイントにGETリクエストを行い、書き出しジョブのパスを含めることで、 `/export/jobs` そ `id` のステータスを監視できます。 エクスポートジョブは、 `status` フィールドが値「SUCCEEDEDED」を返すと完了します。

このエンドポイントの使用に関する詳細は、『 [書き出しジョブエンドポイントガイド』を参照してください。](../api/export-jobs.md#get)

## 次の手順

書き出しが正常に完了すると、の内でデータを使用で [!DNL Data Lake] き [!DNL Experience Platform]ます。 その後、を使用して、エクスポート [!DNL Data Access API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/data-access-api.yaml) に関連付けられたを使用してデータにアクセス `batchId` できます。 セグメントのサイズに応じて、データはチャンクになり、バッチは複数のファイルで構成される場合があります。

APIを使用してバッチファイルにアクセスしてダウンロードする手順については、「 [!DNL Data Access] データアクセス [](../../data-access/tutorials/dataset-data.md)」チュートリアルに従ってください。

また、を使用して、正常にエクスポートされたセグメントデータにアクセスすることもでき [!DNL Adobe Experience Platform Query Service]ます。 UIまたはRESTful APIを使用して、内のデータに関するクエリの書き込み、検証、実行を [!DNL Query Service] 行うことができ [!DNL Data Lake]ます。

オーディエンスデータをクエリする方法の詳細については、のドキュメントを参照してくだ [!DNL Query Service](../../query-service/home.md)さい。
