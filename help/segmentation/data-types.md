---
keywords: Experience Platform;home;popular topics;data type;data types;Data types;Data type;Segmentation data types;Segmentation;segmentation;Segmentation Service;Segmentation service data types;
solution: Experience Platform
title: Adobe Experience Platformセグメントサービスのデータタイプ
topic: overview
description: すべての XDM データ型がセグメント化サービス内でサポートされています。セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。
translation-type: tm+mt
source-git-commit: 17ef6c1c6ce58db2b65f1769edf719b98d260fc6
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 69%

---


# Adobe Experience Platform [!DNL Segmentation Service] でサポートされるデータ型

All XDM data types are supported within [!DNL Segmentation Service]. セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。

## 文字列データ

セグメント定義では、文字列データを使用して、「国名」や「ロイヤルティプログラムレベル」など、セグメントオーディエンスの数値以外の制約を定義します。

文字列データは、論理ステートメント、包含／排他ステートメント、比較ステートメントを使用して、セグメント定義に含まれます。文字列属性をセグメント定義に追加したら、文字列関連ステートメントを使用して、他の文字列フィールドと比較して評価できます。

| ステートメントのタイプ | 例 |
| -------------- | -------- |
| 論理 | `and`、`or`、`not` |
| 包含／排他 | `include`, `must` `exist`, `exclude`, `must not exist` |
| 比較 | `equals`、`does not equal`、`contains`、`starts with` |

## 日付データ

日付データを使用すると、特定の開始／終了日を使用するか、以下の表に示す日付関連ステートメントを使用して、セグメント定義に時間ベースのコンテキストを割り当てることができます。*今年*&#x200B;いつもブランドとの関わりを持ち、過去数日&#x200B;*以内*&#x200B;もアクティブであった顧客オーディエンスを作成するといった場合が 1 つの実装例です。

| フィールドの例 | 日付関連ステートメント | タイムライン |
| ------------- | ------------------------ | --------- |
| person.firstPurchase | `today`、`yesterday`、`this month`、`this year` | セグメントが作成された日に関連します。 |
| person.lastPurchase | `in last`、`during`、`before`、`after`、`within` | 任意の週内／月内に関連します。 |

## エクスペリエンスイベント

As an Adobe Experience Platform schema, [!DNL XDM ExperienceEvents] record explicit and implicit customer interactions with [!DNL Platform]-integrated applications, including a snapshot of the system at the time the interaction took place. [!DNL ExperienceEvents] は事実の記録です したがって、セグメント定義時に使用できるデータソースになります。

次の表に示すように、イベントデータは、イベント動作の絞り込みやイベント属性の指定に役立つキーワードを使用して表現されます。

| キーワード | 用途 |
| ------- | --- |
| 含む／除く | データを含めるまたは省略してイベントの動作を記述します。 |
| 任意／すべて | 適格なセグメントの数の判断に役立ちます。 |
| 「時間ルールを適用」切り替えボタン | 日付データを組み込みます。 |
| 等しい、等しくない、～で開始する、～で開始しない、～で終わる、～で終わらない、含む、含まない、存在する、存在しない | 文字列データを組み込みます。 |

### オーディエンスの共有

外部オーディエンスは、新しいセグメント定義のコンポーネントとしても使用でき、属性ルールを新しいセグメントに追加できます。

現在、外部オーディエンスとしてサポートされているのはAdobe Audience Managerのみです。今後、追加のソースが有効になる予定です。 プラットフォームでのAdobe Audience Managerオーディエンスの使用について詳しくは、Adobe Audience Managerのドキュメント内の [オーディエンス共有ガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html)。

### セグメントの共有

プラットフォームで作成したセグメントは、他の [Adobe Experience Cloudコアサービス内で使用できます](https://docs.adobe.com/content/help/ja-JP/core-services/interface/experience-cloud.html)。 この機能を有効にするには、ソリューションアーキテクトまたは担当のコンサルタントに連絡する必要があります。

## その他のデータタイプ

上記のデータタイプに加えて、サポートされるデータタイプのリストには、次のものも含まれます。

- Uniform Resource Identifier(URI)
- 列挙
- 数値
- 長整数
- 整数
- 短整数
- バイト
- Boolean
- 配列
- オブジェクト
- Map