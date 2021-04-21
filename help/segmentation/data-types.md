---
keywords: Experience Platform；ホーム；人気のあるトピック；データ型；データ型；データ型；データ型；データ型；Segmentationデータ型；Segmentation;Segmentation Service;Segmentationサービスのデータ型；
solution: Experience Platform
title: Segmentation Serviceでサポートされるデータタイプ
topic-legacy: overview
description: Experience Data Model(XDM)のすべてのデータ型は、AdobeSegmentation Service内でサポートされます。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。
exl-id: 73f932a7-f864-4566-ade7-c148a12dc83c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '514'
ht-degree: 63%

---

# Segmentation Serviceでサポートされるデータタイプ

Experience Data Model(XDM)のすべてのデータタイプは、Adobe Experience Platformセグメントサービス内でサポートされます。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。

## 文字列データ

セグメント定義では、文字列データを使用して、「国名」や「ロイヤルティプログラムレベル」など、セグメントオーディエンスの数値以外の制約を定義します。

文字列データは、論理ステートメント、包含／排他ステートメント、比較ステートメントを使用して、セグメント定義に含まれます。文字列属性をセグメント定義に追加したら、文字列関連ステートメントを使用して、他の文字列フィールドと比較して評価できます。

| ステートメントのタイプ | 例 |
| -------------- | -------- |
| 論理 | `and`、`or`、`not` |
| 包含／排他 | `include`,  `must` `exist`,  `exclude`  `must not exist` |
| 比較 | `equals`、`does not equal`、`contains`、`starts with` |

## 日付データ

日付データを使用すると、特定の開始／終了日を使用するか、以下の表に示す日付関連ステートメントを使用して、セグメント定義に時間ベースのコンテキストを割り当てることができます。*今年*&#x200B;いつもブランドとの関わりを持ち、過去数日&#x200B;*以内*&#x200B;もアクティブであった顧客オーディエンスを作成するといった場合が 1 つの実装例です。

| フィールドの例 | 日付関連ステートメント | タイムライン |
| ------------- | ------------------------ | --------- |
| person.firstPurchase | `today`、`yesterday`、`this month`、`this year` | セグメントが作成された日に関連します。 |
| person.lastPurchase | `in last`、`during`、`before`、`after`、`within` | 任意の週内／月内に関連します。 |

## エクスペリエンスイベント

[!DNL XDM ExperienceEvents]は、Adobe Experience Platformスキーマとして、[!DNL Platform]統合アプリケーションとの明示的で暗黙的な顧客の対話を記録します。このアプリケーションには、対話が行われた時点のシステムのスナップショットが含まれます。 [!DNL ExperienceEvents] は事実の記録ですしたがって、セグメント定義時に使用できるデータソースになります。

次の表に示すように、イベントデータは、イベント動作の絞り込みやイベント属性の指定に役立つキーワードを使用して表現されます。

| キーワード | 用途 |
| ------- | --- |
| 含む／除く | データを含めるまたは省略してイベントの動作を記述します。 |
| 任意／すべて | 適格なセグメントの数の判断に役立ちます。 |
| 「時間ルールを適用」切り替えボタン | 日付データを組み込みます。 |
| 等しい、等しくない、～で開始する、～で開始しない、～で終わる、～で終わらない、含む、含まない、存在する、存在しない | 文字列データを組み込みます。 |

### オーディエンスの共有

外部オーディエンスは、新しいセグメント定義のコンポーネントとしても使用でき、属性ルールを新しいセグメントに追加できます。

現在、外部オーディエンスとしてサポートされているのはAdobe Audience Managerのみです。今後、追加のソースが有効になる予定です。 プラットフォームでのAdobe Audience Managerオーディエンスの使用に関する詳細は、Adobe Audience Managerのドキュメント](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html)内の[オーディエンス共有ガイドを参照してください。

### セグメントの共有

プラットフォームで作成されたセグメントは、他の[Adobe Experience Cloudコアサービス](https://docs.adobe.com/content/help/ja-JP/core-services/interface/experience-cloud.html)内で使用できます。 この機能を有効にするには、ソリューションアーキテクトまたは担当のコンサルタントに連絡する必要があります。

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
