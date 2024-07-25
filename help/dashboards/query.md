---
solution: Experience Platform
title: クエリサービスを使用したダッシュボードデータセットの調査、検証、処理
type: Documentation
description: クエリサービスを使用して、Experience Platformでプロファイル、オーディエンス、宛先ダッシュボードを機能させる未加工データセットを調査し、処理する方法を説明します。
exl-id: 0087dcab-d5fe-4a24-85f6-587e9ae74fb8
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 37%

---

# [!DNL Query Service] を使用したダッシュボードデータセットの調査、検証、処理

Adobe Experience Platformは、Experience PlatformUI 内で使用可能なダッシュボードを通じて、組織のプロファイル、オーディエンス、宛先データに関する重要な情報を提供します。 その後、Adobe Experience Platform [!DNL Query Service] を使用して、データレイクでこれらのダッシュボードを機能させる未加工データセットを調査、検証、処理できます。

## [!DNL Query Service] 入門

Adobe Experience Platform [!DNL Query Service] は、標準の SQL を使用してデータレイクのデータに対してクエリを実行できるようにすることで、マーケターがデータからのインサイトを得るのをサポートしています。 [!DNL Query Service] は、データレイク内のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりして、レポートや機械学習で使用したり、リアルタイム顧客プロファイルに取り込んだりできる、ユーザーインターフェイスおよび API を提供します。

[!DNL Query Service] とExperience Platform内での役割について詳しくは、[[!DNL Query Service]  概要 ](../query-service/home.md) を参照してください。

## 使用可能なデータセットへのアクセス

[!DNL Query Service] を使用して、プロファイル、オーディエンス、宛先の各ダッシュボードに対して未加工データセットをクエリできます。 使用可能なデータセットを表示するには、Experience PlatformUI の左側のナビゲーションで「**データセット**」を選択して、データセットダッシュボードを開きます。 ダッシュボードリストは、組織で使用可能なすべてのデータセットを管理します。リストに表示された各データセットに関する詳細（名前、データセットが準拠するスキーマ、最新の取得実行のステータスなど）が表示されます。

![ 左側のナビゲーションで「データセット」タブがハイライト表示されたデータセット参照ダッシュボード。](./images/query/browse-datasets.png)

### システム生成データセット {#system-generated-datasets}

>[!IMPORTANT]
>
>システム生成データセットは、デフォルトでは非表示です。 デフォルトでは、「[!UICONTROL  参照 ] タブには、データの取り込み先のデータセットのみが表示されます。

システム生成データセットを表示するには、フィルターアイコン（![A フィルターアイコン](/help/images/icons/filter.png)）は、検索バーの左側にあります。

![ フィルターアイコンがハイライト表示されたデータセットの「参照」タブ。](./images/query/filter-datasets.png)

サイドバーに、「プロファイルに含める [!UICONTROL  と「システムデータセットを表示 ] という 2 つのトグルが表示されます。[!UICONTROL  が表示され ] す。 [!UICONTROL  システムデータセットを表示 ] の切替スイッチを選択して、システム生成データセットをデータセットの参照可能なリスト内に含めます。

![ 「システムデータセットを表示」切替スイッチがハイライト表示されたデータセット「参照」タブ。](./images/query/show-system-datasets.png)

### プロファイル属性データセット {#profile-attribute-datasets}

プロファイルダッシュボードのインサイトは、組織で定義された結合ポリシーに結び付けられます。アクティブな結合ポリシーごとに、データレイクで使用できるプロファイル属性データセットがあります。

これらのデータセットの命名規則は、**Profile-Snapshot-Export** の後に、システムで生成されるランダムなアルファ値が続きます。例：`Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f`。

各プロファイルスナップショット書き出しデータセットの完全なスキーマを理解するには、Experience Platform UI の[データセットビューアを使用](../catalog/datasets/user-guide.md)して、 データセットをプレビューし、調査します。

![Profile-Snapshot-Export データセットのプレビュー。](images/query/profile-attribute.png)

#### プロファイル属性データセットと結合ポリシー ID のマッピング

各システム生成プロファイル属性データセットに割り当てられる英数字の値は、組織が作成したいずれかの結合ポリシーの結合ポリシー ID にマッピングされるランダムな文字列です。 各結合ポリシー ID と関連するプロファイル属性データセット文字列とのマッピングは、`adwh_dim_merge_policies` データセットで保持されます。

`adwh_dim_merge_policies` データセットには、次のフィールドが含まれます。

* `merge_policy_name`
* `merge_policy_id`
* `merge_policy`
* `dataset_id`

このデータセットは、クエリエディター UI を使用して Experience Platform で確認できます。クエリエディターの使用について詳しくは、[クエリエディターの UI ガイド](../query-service/ui/user-guide.md)を参照してください。

### オーディエンスメタデータデータセット

組織の各オーディエンスのメタデータを含むデータレイクで使用できるオーディエンスメタデータデータセットがあります。

このデータセットの命名規則は、**Segmentdefinition-Snapshot-Export** の後に英数字が続きます。例：`Segmentdefinition-Snapshot-Export-acf28952-2b6c-47ed-8f7f-016ac3c6b4e7`

各セグメント定義のスナップショット書き出しデータセットの完全なスキーマを理解するには、[Experience Platform UI のデータセットビューアを使用して、 ](../catalog/datasets/user-guide.md)データセットをプレビューし、調査します。

### 宛先メタデータデータセット

組織でアクティブ化されたすべての宛先のメタデータは、データレイクで未加工データセットとして使用できます。

このデータセットの命名規則は **DIM_Destination** です。

DIM の宛先データセットの完全なスキーマを理解するには、Experience Platform UI の[データセットビューアを使用して、](../catalog/datasets/user-guide.md)データセット をプレビューし、調査します。

![DIM_Destination データセットのプレビュー。](images/query/destinations-metadata.png)

## 顧客データプラットフォーム（CDP）インサイトレポート

CDP インサイトデータモデル機能は、様々なプロファイル、宛先、セグメント化ウィジェットに関するインサイトを強化する SQL を公開します。 これらの SQL クエリテンプレートをカスタマイズして、マーケティングおよび KPI のユースケースに関する CDP レポートを作成できます。

CDP レポートは、プロファイルデータと、そのオーディエンスおよび宛先との関係に関するインサイトを提供します。 [CDP インサイトデータモデルを特定の KPI 使用例に適用する ](./data-models/cdp-insights-data-model-b2c.md) 方法について詳しくは、CDP インサイトデータモデルのドキュメントを参照してください。

## クエリの例

次のクエリの例には、ダッシュボードを機能させる未加工データセットを調査、検証、処理するた [!DNL Query Service] に使用できる SQL のサンプルが含まれています。

### ID 別のプロファイルの数

このプロファイルインサイトは、データセット内のすべての結合プロファイルにわたって ID の分類を提供します。

>[!NOTE]
>
>1 つのプロファイルに複数の名前空間が関連付けられている可能性があるので、ID 別のプロファイルの合計数（各名前空間に表示される値をまとめたもの）は、結合されたプロファイルの合計数より多くなる場合があります。例えば、顧客が複数のチャネルでブランドとやり取りする場合、複数の名前空間がその個々の顧客に関連付けられます。

**クエリ**

```sql
Select
        Key namespace,
        count(1) count_of_profiles
     from
        (
           Select
               explode(identitymap)
           from
              Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f
        )
     group by
        namespace;
```

### オーディエンス別のプロファイルの数

このオーディエンスインサイトは、データセット内の各オーディエンス内の結合プロファイルの合計数を提供します。 この数は、プロファイルフラグメントを結合してオーディエンス内の個々のプロファイルを 1 つ形成するために、オーディエンス結合ポリシーをプロファイルデータに適用した結果です。

```sql
Select          
        concat_ws('-', key, source_namespace) audience_id,
        count(1) count_of_profiles
      from
        (
            Select
              Upper(key) as source_namespace,
              explode(value)
            from
              (
                  Select
                    explode(Audiencemembership)
                  from
                    Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f
              )
        )
      group by
      audience_id
```

## 次の手順

このガイドを読むと、[!DNL Query Service] を使用して、プロファイル、オーディエンス、宛先の各ダッシュボードを動作させる未加工データセットを調査および処理するクエリを実行できるようになります。

各ダッシュボードとその指標について詳しくは、ドキュメントのナビゲーションで使用可能なダッシュボードのリストからダッシュボードを選択してください。
