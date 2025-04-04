---
title: TTL を使用してデータレイクでのエクスペリエンスイベントデータセット保持を管理
description: Adobe Experience Platform API の Time-To-Live （TTL）設定を使用してデータレイクでのエクスペリエンスイベントデータセット保持を評価、設定、管理する方法について説明します。 このガイドでは、TTL 行レベルの有効期限がデータ保持ポリシーをサポートする方法、ストレージ効率を最適化する方法、効果的なデータライフサイクル管理を確実に行う方法について説明します。 また、TTL を効果的に適用するのに役立つユースケースとベストプラクティスも提供します。
exl-id: d688d4d0-aa8b-4e93-a74c-f1a1089d2df0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2341'
ht-degree: 1%

---

# TTL を使用してデータレイクでのエクスペリエンスイベントデータセット保持を管理

効率的なデータ管理は、最適なパフォーマンス、コスト管理、データの整合性を実現するために重要です。 エクスペリエンスイベントデータセット保持の有効期間（TTL）を使用して、行レベルの有効期限を適用し、データレイク内のデータセットから古いレコードを自動的に削除すると同時に、最適なストレージ効率とデータ関連性を確保します。

このガイドでは、Catalog Service API を使用して TTL を評価、設定、管理する方法について説明します。 TTL を適用するタイミングと理由、API 呼び出しを使用して TTL 値を設定および更新する方法、効果的な実装を確実に行うためのベストプラクティスについて説明します。

>[!IMPORTANT]
>
> TTL は、データのライフサイクル管理とストレージ効率を最適化するように設計されています。 これはコンプライアンスツールではないので、規制要件に当てるべきではありません。 コンプライアンスには、多くの場合、より広範なデータガバナンス戦略が必要です。

## 行レベルのデータ管理に TTL を使用する理由

データセットが増加するにつれて、パフォーマンスの維持、コストの制御、データの関連性の維持を実現するために、効率的なデータ管理がますます重要になります。 TTL ベースの行レベルのデータ有効期限は、手動の介入なしで古いレコードを削除することでデータクリーンアップを自動化し、ストレージの最適化とシステム効率の向上を支援します。

TTL は、時間の経過と共に関連性が失われる時間依存データを管理する場合に役立ちます。 次の必要がある場合は、TTL の実装を検討してください。

- 古いレコードを自動的に削除することで、ストレージ・コストを削減します。
- 無関係なデータを最小限に抑えることで、クエリのパフォーマンスを向上させます。
- 関連情報のみを保持することで、データハイジーンを維持します。
- データ保持を最適化してビジネス目標をサポートします。

>[!NOTE]
>
>エクスペリエンスイベントデータセット保持は、データレイクに保存されたイベントデータに適用されます。 Real-Time Customer Data Platformでリテンションを管理している場合は、データレイクのリテンション設定と共に [ エクスペリエンスイベントの有効期限 ](../../profile/event-expirations.md) および [ 偽名プロファイルの有効期限 ](../../profile/pseudonymous-profiles.md) を使用することを検討してください。
>
>TTL 設定は、使用権限に基づいてストレージを最適化するのに役立ちます。 （Real-Time CDPで使用される）プロファイルストアデータは古いと見なされ、30 日後に削除される可能性がありますが、Data Lake 内の同じイベントデータは、Analytics と Data Distillerのユースケースで 12 ～ 13 か月間（または使用権限に基づいて、それより長い期間）使用できます。

### ある業界の例 {#industry-example}

例えば、ビデオビュー、検索、レコメンデーションなど、ユーザーのインタラクションを追跡するビデオストリーミングサービスについて考えてみます。 最近のエンゲージメントデータはパーソナライゼーションにとって重要ですが、古いアクティビティログ（1 年以上のインタラクションなど）は関連性を失います。 行レベルの有効期限を使用すると、Experience Platformは古いログを自動的に削除し、現在の意味のあるデータのみが分析とレコメンデーションに使用されるようにします。

## TTL の適合性の評価

保持ポリシーを適用する前に、データセットが行レベルの有効期限の適切な候補であるかどうかを評価します。 次の点に留意してください。

- データの関連性の推移：古いデータは価値を提供しますか、それとも古くなりますか。
- ダウンストリームプロセスへの影響：データを削除すると、レポート、分析または統合に影響がありますか？
- ストレージコストと保存コストの比較：古いデータの価値によって、保存コストは正当化されますか？

履歴レコードが長期的な分析やビジネスオペレーションに不可欠な場合、TTL は適切なアプローチではない可能性があります。 これらの要因を確認することで、データの可用性に悪影響を与えることなく、TTL がデータ保持のニーズに確実に対応できるようになります。

## クエリの計画

TTL を適用する前に、クエリを使用してデータセットのサイズと関連性を分析します。 ターゲットクエリの実行は、様々な TTL 設定で保持または削除されるデータの量を決定するのに役立ちます。

例えば、次の SQL クエリは、過去 30 日間に作成されたレコードの数をカウントします。

```sql
SELECT COUNT(1) FROM [datasetName] WHERE timestamp > date_sub(now(), INTERVAL 30 DAY);
```

異なる時間間隔で同様のクエリを実行すると、TTL 設定を検証し、ストレージの効率とデータのアクセシビリティのバランスを確保できます。

## TTL 管理の基本を学ぶ

Catalog Service API を使用してエクスペリエンスイベントデータセット保持を評価、設定、管理する前に、リクエストを正しくフォーマットする方法を理解する必要があります。 これには、API パスの把握、必要なヘッダーの提供、リクエストペイロードの書式設定が含まれます。 この重要な情報については、[Catalog Service API 入門ガイド ](../api/getting-started.md) を参照してください。

>[!NOTE]
>
>このドキュメントでは、データセット自体はそのままの状態で、データセット内の期限切れの行を個別に削除する、行レベルの有効期限について説明します。 データセットの有効期限には適用されません。この有効期限は、データセット全体を削除し、別の機能によって管理されます。 データセットレベルの有効期限については、[ データセット有効期限 API ドキュメント ](../../hygiene/api/dataset-expiration.md) を参照してください。

### 現在の TTL 設定の確認方法

TTL 管理を開始するには、まず、現在の TTL 設定を確認します。 `/ttl/{datasetId}` エンドポイントに対してGET リクエストを実行し、データセットのデフォルト、最大、最小 TTL 設定を取得します。 TTL ルールはデータセットタイプに応じて異なる可能性があるので、この手順は必須です。

>[!TIP]
>
>Catalog Service API のExperience Platform Gateway URL とベースパスは `https://platform.adobe.io/data/foundation/catalog` です。

**API 形式**

```http
GET /ttl/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | データセットを一意に識別する、システムで生成された文字列。 データセット ID を見つけるには、`/datasets` エンドポイントを使用します。 関連するデータセットの応答をフィルタリングする手順については、[list catalog objects API ガイド ](../api/list-objects.md) を参照してください。 |

**リクエスト**

次のリクエストでは、特定のデータセットに関する組織の TTL 設定を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/ttl/5ba9452f7de80408007fc52a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -H 'x-sandbox-id: {SANDBOX_ID}'
```

**応答**

正常な応答では、`adobe_lakeHouse` ストレージと `adobe_unifiedProfile` ストレージの両方のデフォルト、最大、最小 TTL 値を含む、データセットの TTL 設定が返されます。

+++選択して応答を表示します

```json
{
    "67976f0b4878252ab887ccd9": {
        "name": "Acme Sales Data",
        "description": "This dataset contains sales transaction records for Acme Corporation.",
        "imsOrg": "{ORG_ID}",
        "sandboxId": "{SANDBOX_ID}",
        "tags": {
            "adobe/pqs/table": [
                "acme_sales_20250127_113331_106"
            ],
            "adobe/siphon/table/format": [
                "delta"
            ]
        },
        "extensions": {
            "adobe_lakeHouse": {  
                "rowExpiration": {
                    "defaultValue": "P12M",
                    "maxValue": "P12M",
                    "minValue": "P30D"
                }
            },
            "adobe_unifiedProfile": {  
                "rowExpiration": {
                    "defaultValue": "P12M",
                    "maxValue": "P12M",
                    "minValue": "P7D"
                }
            }
        },
        "version": "1.0.0",
        "created": 1737977611118,
        "updated": 1737977611118,
        "createdClient": "acme_data_pipeline",
        "createdUser": "john.snow@acmecorp.com",
        "updatedUser": "arya.stark@acmecorp.com",
        "classification": {
            "managedBy": "CUSTOMER"
        }
    }
}
```

+++

| プロパティ | 説明 |
|--------------|-------------|
| `defaultValue` | カスタム TTL が設定されていない場合に適用されるデフォルトの TTL 期間。 |
| `maxValue` | データセットで許可される最長 TTL。 null の場合、上限はありません。 |
| `minValue` | システムポリシーへの準拠を確保するための最短の TTL。 |

<!-- Q) what is the default Max and Min values and are they system-imposed? -->

### データセットの TTL の設定方法 {#set-ttl}

>[!IMPORTANT]
>
>行の有効期限は、時系列スキーマを使用するイベントデータセットにのみ適用できます。 TTL を設定する前に、API リクエストが正常に完了するように、データセットのスキーマが `https://ns.adobe.com/xdm/data/time-series` を拡張していることを確認します。 スキーマレジストリ API を使用して、スキーマの詳細を取得し、`meta:extends` プロパティを確認します。 これを行う方法のガイダンスについては、[ スキーマエンドポイントのドキュメント ](../../xdm/api/schemas.md#lookup) を参照してください。

データセットのエクスペリエンスイベントデータセット保持を設定するには、`/v2/datasets/{ID}` エンドポイントにPATCH リクエストを実行して、新しい TTL 値を設定します。

**API 形式**

```http
PATCH /v2/datasets/{DATASET_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{DATASET_ID}` | TTL 値を更新するデータセットの ID。 |

**リクエスト**

以下のリクエストの例では、`ttlValue` が `P3M` に設定されています。 これにより、3 か月を超えるレコードが自動的に削除されます。 保持期間は、6 か月間は `P6M`、1 年間は `P12M` などの値を使用して、ビジネスニーズに合わせて調整できます。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/catalog/v2/datasets/{DATASET_ID}' \
  -h 'Authorization: Bearer {ACCESS_TOKEN}' \
  -h 'Content-Type: application/json' \
  -h 'x-api-key: {API_KEY}' \
  -h 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
    "extensions": {
        "adobe_lakeHouse": {
            "rowExpiration": {
                "ttlValue": "P3M"  // A 3 month retention period
            }
        }
    }
}
```

**応答**

正常な応答には、データセットの TTL 設定が表示されます。 `adobe_lakeHouse` ストレージと `adobe_unifiedProfile` ストレージの両方の行レベルの有効期限設定に関する詳細が含まれます。

+++選択して応答を表示します

```JSON
{
    "67976f0b4878252ab887ccd9": {
        "name": "Acme Sales Data",
        "description": "This dataset contains sales transaction records for Acme Corporation.",
        "imsOrg": "{ORG_ID}",
        "sandboxId": "{SANDBOX_ID}",
        "tags": {
            "adobe/pqs/table": [
                "acme_sales_20250127_113331_106"
            ],
            "adobe/siphon/table/format": [
                "delta"
            ]
        },
        "extensions": {
            "adobe_lakeHouse": {
                "rowExpiration": {
                "ttlValue": "P3M",
                    "valueStatus": "custom",
                    "setBy": "user",
                    "updated": 1737977766499
                }
            },
            "adobe_unifiedProfile": {  
                "rowExpiration": {
                    "ttlValue": "P3M",
                    "valueStatus": "custom",
                    "setBy": "user",
                    "updated": 1737977766499
                }
            }
        },
        "version": "1.0.0",
        "created": 1737977611118,
        "updated": 1737977611118,
        "createdClient": "acme_data_pipeline",
        "createdUser": "john.snow@acmecorp.com",
        "updatedUser": "arya.stark@acmecorp.com",
        "classification": {
            "managedBy": "CUSTOMER"
        }
    }
}
```

+++

| プロパティ | 説明 |
|----------------------------------|-------------|
| `extensions` | データセットに関連する追加のメタデータのコンテナ。 |
| `extensions.adobe_lakeHouse` | 行レベルの有効期限設定など、ストレージアーキテクチャに関連する設定を指定します |
| `rowExpiration` | オブジェクトには、データセットの保持期間を定義する TTL 設定が含まれています。 |
| `rowExpiration.ttlValue` | データセット内のレコードが自動的に削除されるまでの期間を定義します。 ISO-8601 の期間形式を使用します（例：3 か月の場合は `P3M`、1 週間の場合は `P30D`）。 |
| `rowExpiration.valueStatus` | 文字列は、TTL 設定がデフォルトのシステム値か、ユーザーが設定したカスタム値かを示します。 使用可能な値：`default`、`custom` |
| `rowExpiration.setBy` | TTL 設定を最後に変更したユーザーを指定します。 使用可能な値：`user` （手動で設定）または `service` （自動的に割り当て）。 |
| `rowExpiration.updated` | 前回の TTL 更新のタイムスタンプ。 この値は、TTL 設定が最後に変更された日時を示します。 |

### TTL の更新方法 {#update-ttl}

TTL を調整することで、ビジネスニーズに合わせて保持期間を延長または短縮します。 例えば、前述のビデオストリーミングプラットフォームを検討する場合、プラットフォームは、パーソナライゼーション用に新しいエンゲージメントデータを確保するために、最初に TTL を 3 か月に設定する場合があります。 ただし、分析で 3 か月を超えるインタラクションパターンが依然として有益なインサイトを提供することが示された場合は、より優れたレコメンデーションモデルを実現するために、古いレコードを保持するように TTL 期間を 6 か月に拡張できます。

既存の TTL 値を変更するには、`/v2/datasets/{DATASET_ID}` エンドポイントで `PATCH` メソッドを使用します。

#### API 形式

```http
PATCH /v2/datasets/{DATASET_ID}
```

**リクエスト**

次のリクエストでは、自動削除の前に、レコード保持期間を延長して TTL が 6 か月（`P6M`）に更新されます。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/catalog/v2/datasets/{DATASET_ID}' \
  -h 'Authorization: Bearer {ACCESS_TOKEN}' \
  -h 'Content-Type: application/json' \
  -h 'x-api-key: {API_KEY}' \
  -h 'x-gw-ims-org-id: {ORG_ID}' \
  -d '{
    "extensions": {
        "adobe_lakeHouse": {
            "rowExpiration": {
                "ttlValue": "P6M"  // Extend to 6 months
            }
        }
    }
}
```

<!-- Q) For Clarity, should this example show both data stores being updated by expanding the example payload above? -->

**応答**

```JSON
{  "extensions": {
        "adobe_lakeHouse": {
            "rowExpiration": {
              "ttlValue": "P6M",
              "valueStatus": "custom",
              "setBy": "user",
              "updated": "1737977766499"
            }
        },
        "adobe_unifiedProfile": {
            "rowExpiration": {
                "ttlValue": "P3M",
                "valueStatus": "custom",
                "setBy": "user",
                "updated": "17379754766355"
            }
        }
    }
}
```

## TTL 設定のベストプラクティス {#best-practices}

適切な TTL 値を選択することは、エクスペリエンスイベントデータセット保持ポリシーで、データ保持、ストレージ効率、分析のニーズのバランスを取るために重要です。 TTL が短すぎるとデータが失われる可能性がありますが、長すぎると、ストレージコストや不要なデータ蓄積が増加する可能性があります。 データのアクセス頻度と関連性の高いデータを保持する期間を考慮して、TTL がデータセットの目的に合っていることを確認します。

次の表に、データセットタイプと使用パターンに基づいた、一般的な TTL 推奨事項を示します。

| データセットタイプ | 推奨 TTL | 典型的なユースケース |
|-----------------------------|------------------------|-------------------|
| 頻繁にアクセスするデータセット | 30 ～ 90 日 | ユーザーエンゲージメントログ、web サイトクリックストリームデータ、短期キャンペーンパフォーマンスデータ。 |
| アーカイブデータセット | 一年以上 | 金融取引ログ、コンプライアンスデータ、長期トレンド分析、機械学習トレーニングデータセット。 |
| アプリ管理データセット | 最大 13 か月 | システム管理データセットには事前に定義された TTL 制限があり、システムが課した制限に準拠するように自動的に適用されます。 |
| 顧客管理データセット | 30 日 – 最大 TTL | UI、API または Data Distillerで作成したデータセット。 TTL は、少なくとも 30 日で、定義された最大 TTL 内である必要があります。 |

TTL 設定を定期的に確認して、ストレージポリシー、分析ニーズ、ビジネス要件に引き続き合致していることを確認します。

### TTL を設定する際の主な考慮事項

<!-- What are the default TTL limits for system-generated Profile Store and data lake datasets? -->

<!-- Q) Are the limits: 90 days for data in the Profile store and 13 months for data in the data lake? This is true for Journey Optimizer. -->

TTL 設定をデータ保持戦略に確実に合致させるには、次のベストプラクティスに従います。

- TTL の変更を定期的に監査します。 すべての TTL 更新トリガーに監査イベントが発生します。 監査ログを使用して、コンプライアンス、データガバナンスおよびトラブルシューティングの目的で TTL の変更をトラックします。
- データを無期限に保持する必要がある場合は、TTL を削除します。 TTL を無効にするには、`ttlValue` を `null` に設定します。 これにより、自動期限切れを防ぎ、すべてのレコードを永続的に保持します。 この変更を行う前に、ストレージの影響を考慮します。

<!-- Q) Are there any specific system constraints or impacts of setting TTL to null? -->

## TTL の制限 {#limitations}

TTL を使用する場合は、次の制限事項に注意してください。

- **TTL を使用したエクスペリエンスイベントデータセットの保持は、データセットの削除ではなく、行レベルの有効期限に適用**。 TTL は、定義された保持期間に基づいてレコードを削除しますが、データセット全体は削除しません。 データセットを削除するには、[ データセットの有効期限エンドポイント ](../../hygiene/api/dataset-expiration.md) を使用するか、手動で削除します。
- **TTL は削除できません**、更新のみ。 適用した TTL は削除できませんが、[ 保持期間を変更 ](#update-ttl) して拡張または短縮できます。 データを無期限に保持するには、削除を試みるのではなく、十分に長い TTL を設定します。
- **TTL はコンプライアンスツールではありません**。 TTL は、ストレージとデータのライフサイクル管理を最適化しますが、規制のデータ保持要件を満たしていません。 コンプライアンスのために、より広範なデータガバナンス戦略を導入します。

## データセット保持ポリシーに関する FAQ {#faqs}

この節では、Adobe Experience Platformのデータセット保持ポリシーに関するよくある質問に対する回答を示します。

### 保持ポリシールールを適用できるデータセットのタイプを教えてください。

+++回答
XDM ExperienceEvent クラスを使用して作成されたデータセットに保持ポリシーを適用できます。 プロファイルサービスの場合、保持ポリシーは、プロファイルが有効になっているエクスペリエンスイベントデータセットにのみ適用されます。
+++

### データセット保持ジョブによって Data Lake Services からデータはどのくらいの時間で削除されますか？

+++回答
データセット TTL は毎週評価および処理され、期限切れのレコードがすべて削除されます。 イベントが 30 日以上前（取り込み日 > 30 日）にExperience Platformに取り込まれ、そのイベントの日付が定義された保持期間（TTL）を超えている場合、イベントは期限切れと見なされます。
+++

### データセット保持ジョブは、どれくらいの期間でプロファイルサービスからデータを削除しますか？

+++回答
リテンション・ポリシーを設定すると、イベント・タイムスタンプがリテンション期間（TTL）を超えた場合、Experience Platform内の既存のイベントは直ちに削除されます。 新しいイベントは、そのタイムスタンプが保持期間を超えると削除されます。

例えば、5 月 15 日に 30 日間の有効期限ポリシーを適用すると、次のようになります。

- 新しいイベントは、取り込まれると 30 日間の有効期限が切れます。
- 4 月 15 日より古いタイムスタンプを持つ既存のイベントは、すぐに削除されます。
- 4 月 15 日より後のタイムスタンプを持つ既存のイベントは、タイムスタンプから 30 日後に期限切れになるように設定されています（例えば、4 月 18 日からのイベントは 5 月 18 日に削除されます）。
+++

### データレイクとプロファイルサービスに異なる保持ポリシーを設定できますか。

+++回答
はい。データレイクとプロファイルサービスに対して異なる保持ポリシーを設定できます。 ただし、プロファイルの保持期間は、データレイクに設定された期間より短くすることはできません。
+++

### 現在のデータセットの使用状況を確認するにはどうすればよいですか？

+++回答
データレイクとプロファイルストアの最新のデータセットストレージサイズを、[!UICONTROL  データセット ] インベントリワークスペースで別の指標として確認できます。 最大のデータセットを識別するために列を並べ替え、保存ポリシーが適用されていることを確認します。

サンドボックスレベルの使用については、ライセンス使用状況ダッシュボードを参照してください。 詳しくは、[ ライセンスの使用に関するドキュメント ](../../dashboards/guides/license-usage.md) を参照してください。
+++

### データ保持ジョブが成功したかどうかを確認する方法

+++回答
最後のデータ保持ジョブを検証するには、[ データセット保持設定 UI](./user-guide.md#data-retention-policy) またはデータインベントリページでそのタイムスタンプを確認します。

データセットの使用状況に関する履歴レポートは現在ご利用いただけません。
+++

### 削除したデータを復元できますか？

+++回答
いいえ。保存ポリシーが適用されると、保存期間より古いデータは完全に削除され、リカバリできなくなります。
+++

### データレイクのエクスペリエンスイベントデータセットで設定できる最小 TTL は何ですか？

+++回答
データレイクエクスペリエンスイベントデータセットの最小 TTL は 30 日です。 データレイクは、最初の取り込みおよび処理中に、処理バックアップおよびリカバリシステムとして機能します。 その結果、データは、有効期限が切れる前に、取り込み後 30 日以上データレイクに残る必要があります。
+++

### TTL ポリシーで許可されている期間より長くデータレイクフィールドを保持する必要がある場合はどうすればよいですか。

+++回答
Data Distillerを使用すると、稼働率の制限内に収まりながら、データセットの TTL を超える特定のフィールドを保持できます。 必要なフィールドのみを派生データセットに定期的に書き込むジョブを作成します。 このワークフローにより、長期間使用するために重要なデータを保持しながら、より短い TTL に確実に準拠できます。

詳しくは、[SQL での派生データセットの作成 ](../../query-service/data-distiller/derived-datasets/create-derived-datasets-with-sql.md) ガイドを参照してください。
+++

## 次の手順 {#next-steps}

これで、行レベルの有効期限の TTL 設定を管理する方法を学びました。次のドキュメントを参照して、TTL 管理をさらに理解してください。

- 保持ジョブ：[ データライフサイクル UI ガイド ](../../hygiene/ui/dataset-expiration.md) を使用して、Experience Platform UI でデータセットの有効期限をスケジュールおよび自動化する方法を説明します。または、データセットの保持の設定を確認し、期限切れのレコードが削除されていることを確認します。
- [Dataset Expiration API エンドポイントガイド ](../../hygiene/api/dataset-expiration.md)：行だけでなく、データセット全体を削除する方法を説明します。 API を使用してデータセットの有効期限をスケジュール、管理、自動化し、効率的なデータ保持を確保する方法を説明します。
- [ データ使用ポリシーの概要 ](../../data-governance/policies/overview.md)：データ保持戦略を、より広範なコンプライアンス要件やマーケティング上の使用制限に合わせる方法について説明します。
