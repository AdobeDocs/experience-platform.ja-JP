---
solution: Experience Platform
title: ' Platform ダッシュボードを機能させる未加工データセットの調査と処理'
type: Documentation
description: クエリサービスを使用して、Experience Platform でプロファイル、セグメント、宛先ダッシュボードを機能させる未加工データセットを調査し、処理する方法を説明します。
exl-id: 0087dcab-d5fe-4a24-85f6-587e9ae74fb8
source-git-commit: b9dd7584acc43b5946f8c0669d7a81001e44e702
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 100%

---

# クエリサービスを使用したダッシュボードデータセットの調査、検証、処理

Adobe Experience Platform は、Experience Platform UI 内で使用可能なダッシュボードを通じて、組織のプロファイル、セグメント、宛先データに関する重要な情報を提供します。その後、Adobe Experience Platform クエリサービスを使用して、データレイクでこれらのダッシュボードを機能させる未加工データセットを調査、検証、処理できます。

## クエリサービスの概要

Adobe Experience Platform クエリサービスは、標準の SQL を使用してデータレイクのデータをクエリできるようにすることで、マーケターがデータからのインサイトを得るのをサポートします。クエリサービスは、データレイク内の任意のデータセットを結合し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、リアルタイム顧客プロファイルへの取り込みに使用できるユーザーインターフェイスと API を提供します。

クエリサービスと Experience Platform 内での役割について詳しくは、[クエリサービスの概要](../query-service/home.md)を参照してください。

## 使用可能なデータセット

クエリサービスを使用して、プロファイル、セグメント、宛先の各ダッシュボードに対して未加工データセットをクエリできます。次のセクションでは、データレイクで見つけられる未加工データセットについて説明します。

### プロファイル属性データセット

プロファイルダッシュボードのインサイトは、組織で定義された結合ポリシーに結び付けられます。アクティブな結合ポリシーごとに、データレイクで使用できるプロファイル属性データセットがあります。

これらのデータセットの命名規則は、**Profile-Snapshot-Export** の後に、システムで生成されるランダムなアルファ値が続きます。例：`Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f`。

各プロファイルスナップショット書き出しデータセットの完全なスキーマを理解するには、Experience Platform UI の[データセットビューアを使用](../catalog/datasets/user-guide.md)して、 データセットをプレビューし、調査します。

![](images/query/profile-attribute.png)

#### プロファイル属性データセットと結合ポリシー ID のマッピング

各プロファイル属性データセットには、**プロファイルスナップショットエクスポート**&#x200B;の後に、システムで生成されるランダムな英数字の値が続きます。例：`Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f`。

この英数字値は、組織が作成したいずれかの結合ポリシーの結合ポリシー ID にマッピングされる、システム生成のランダムな文字列です。各結合ポリシー ID と関連するプロファイル属性データセット文字列とのマッピングは、`adwh_dim_merge_policies` データセットで保持されます。

`adwh_dim_merge_policies` データセットには、次のフィールドが含まれます。

* `merge_policy_name`
* `merge_policy_id`
* `merge_policy`
* `dataset_id`

このデータセットは、クエリエディター UI を使用して Experience Platform で確認できます。クエリエディターの使用について詳しくは、[クエリエディターの UI ガイド](../query-service/ui/user-guide.md)を参照してください。

### セグメントメタデータデータセット

組織の各セグメントのメタデータを含むデータレイクで使用できるセグメントメタデータデータセットがあります。

このデータセットの命名規則は、**Segmentdefinition-Snapshot-Export** の後に英数字が続きます。例：`Segmentdefinition-Snapshot-Export-acf28952-2b6c-47ed-8f7f-016ac3c6b4e7`

各セグメント定義のスナップショット書き出しデータセットの完全なスキーマを理解するには、[Experience Platform UI のデータセットビューアを使用して、 ](../catalog/datasets/user-guide.md)データセットをプレビューし、調査します。

![](images/query/segment-metadata.png)

### 宛先メタデータデータセット

組織でアクティブ化されたすべての宛先のメタデータは、データレイクで未加工データセットとして使用できます。

このデータセットの命名規則は **DIM_Destination** です。

DIM の宛先データセットの完全なスキーマを理解するには、Experience Platform UI の[データセットビューアを使用して、](../catalog/datasets/user-guide.md)データセット をプレビューし、調査します。

![](images/query/destinations-metadata.png)

## クエリの例

次のクエリの例には、ダッシュボードを機能させる未加工データセットを調査、検証、処理するためにクエリサービスで使用できる SQL のサンプルが含まれています。

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

### セグメント別のプロファイルの数

このオーディエンスインサイトは、データセット内の各セグメント内の結合プロファイルの合計数を提供します。この数は、プロファイルフラグメントを結合してセグメント内の個々のプロファイルを 1 つ形成するために、セグメント結合ポリシーをプロファイルデータに適用した結果です。

```sql
Select          
        concat_ws('-', key, source_namespace) segment_id,
        count(1) count_of_profiles
      from
        (
            Select
              Upper(key) as source_namespace,
              explode(value)
            from
              (
                  Select
                    explode(Segmentmembership)
                  from
                    Profile-Snapshot-Export-abbc7093-80f4-4b49-b96e-e743397d763f
              )
        )
      group by
      segment_id
```

## 次の手順

このガイドを読むと、クエリサービスを使用して、プロファイル、セグメント、宛先の各ダッシュボードを動作させる未加工データセットを調査および処理するためのクエリを実行できるようになります。

各ダッシュボードとその指標について詳しくは、ドキュメントのナビゲーションで使用可能なダッシュボードのリストからダッシュボードを選択してください。
