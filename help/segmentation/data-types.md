---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformセグメントサービスのデータタイプ
topic: overview
translation-type: tm+mt
source-git-commit: 96b6f820e5d372446c4927e7719aedadb2b11bc9
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 6%

---


# Adobe Experience Platform [!DNL Segmentation Service] でサポートされるデータ型

では、すべてのXDMデータ型がサポートされてい [!DNL Segmentation Service]ます。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。

## 文字列データ

セグメント定義では、文字列データを使用して、「国名」や「忠誠度プログラムレベル」など、セグメントオーディエンスの数値以外の制約を定義します。

文字列データは、論理、包含/排他および比較文を使用して、セグメント定義に含めます。 セグメント定義に文字列属性が追加されたら、文字列に関連するステートメントを使用して、他の文字列フィールドと比較して評価できます。

| 明細書タイプ | 例 |
| -------------- | -------- |
| 論理 | `and`、`or`、`not` |
| 包括的/排他的 | `include`, `must` `exist`, `exclude`, `must not exist` |
| 比較 | `equals`、`does not equal`、`contains`、`starts with` |

## 日付データ

日付データを使用すると、特定の開始/終了日を使用するか、次の表に示す日付に関連する文を使用して、時間に基づくコンテキストをセグメント定義に割り当てることができます。 1つの実装は、 *今年のいつでもブランドとの関わりを示し、過去数日間* にわたってアクティブだった顧客のオーディエンスを作成することが考えられます ** 。

| サンプルフィールド | 日付に関連する明細書 | タイムライン |
| ------------- | ------------------------ | --------- |
| person.firstPurchase | `today`、`yesterday`、`this month`、`this year` | セグメントが作成された日に関連します。 |
| person.lastPurchase | `in last`、`during`、`before`、`after`、`within` | 任意の週/月内で関連性があります。 |

## エクスペリエンスイベント

Adobe Experience Platformスキーマとして、インタラクションが発生した時点のシステムのスナップショットを含む、 [!DNL XDM ExperienceEvents][!DNL Platform]統合アプリケーションを使用した明示的および暗黙的な顧客インタラクションを記録します。 [!DNL ExperienceEvents] は事実の記録です したがって、これらはセグメント定義時に使用できるデータソースです。

次の表に示すように、イベントデータは、イベントの動作を絞り込み、イベント属性を指定するのに役立つキーワードを使用してレンダリングされます。

| キーワード | 使用方法 |
| ------- | --- |
| 含む/除外する | データの組み込みまたは欠落によるイベントの動作を説明します。 |
| 任意/すべて | 条件を満たすセグメントの数の判断に役立ちます。 |
| 「タイムルールを適用」切り替えボタン | 日付データを組み込みます。 |
| 等しい、等しくない、開始、次の値を含まない、次の値を含む、次の値を含まない、次の値で終わらない、次の値を含まない、次の値を含まない、次の値を含まない、が存在しない | 文字列データを組み込みます。 |

### オーディエンスの共有

外部オーディエンスは、新しいセグメント定義のコンポーネントとして使用し、新しいセグメントに属性ルールを追加することもできます。

現在、外部オーディエンスとしてAdobe Audience Managerのみがサポートされ、今後、追加のソースが有効になります。 PlatformでのAdobe Audience Managerオーディエンスの使用について詳しくは、Adobe Audience Managerドキュメント内の [オーディエンス共有ガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html)。

### セグメントの共有

Platformで作成されたセグメントは、他の [Adobe Experience Cloudコアサービス内で使用できます](https://docs.adobe.com/content/help/ja-JP/core-services/interface/experience-cloud.html)。 この機能を有効にするには、ソリューションアーキテクトまたは担当のコンサルタントに連絡する必要があります。

## その他のデータタイプ

上記のデータタイプに加えて、サポートされるデータタイプのリストには、次のものも含まれます。

- Uniform Resource Identifier(URI)
- 列挙
- 数値
- ロング
- 整数
- Short
- バイト
- Boolean
- 配列
- オブジェクト
- マップ