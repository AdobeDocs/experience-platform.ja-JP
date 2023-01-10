---
keywords: Experience Platform；ホーム；人気のトピック；データタイプ；データタイプ；データタイプ；データタイプ；データタイプ；セグメント化データタイプ；セグメント化；セグメント化サービス；セグメント化サービスデータタイプ；
solution: Experience Platform
title: セグメント化サービスでサポートされるデータタイプ
description: Experience Data Model(XDM) のすべてのデータタイプは、Segmentation Service 内でサポートされています。Adobe セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。
exl-id: 73f932a7-f864-4566-ade7-c148a12dc83c
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 62%

---

# セグメント化サービスでサポートされるデータタイプ

すべての Experience Data Model(XDM) データタイプがAdobe Experience Platform Segmentation Service 内でサポートされています。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。

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

Adobe Experience Platformスキーマとして、 [!DNL XDM ExperienceEvents] 明示的かつ暗黙的な顧客インタラクションを記録する [!DNL Platform] — 統合アプリケーション（インタラクションが発生した時点のシステムのスナップショットを含む）。 [!DNL ExperienceEvents] は事実記録です。 したがって、セグメント定義時に使用できるデータソースになります。

次の表に示すように、イベントデータは、イベント動作の絞り込みやイベント属性の指定に役立つキーワードを使用して表現されます。

| キーワード | 用途 |
| ------- | --- |
| 含む／除く | データを含めるまたは省略してイベントの動作を記述します。 |
| 任意／すべて | 適格なセグメントの数の判断に役立ちます。 |
| 「時間ルールを適用」切り替えボタン | 日付データを組み込みます。 |
| 等しい、等しくない、～で開始する、～で開始しない、～で終わる、～で終わらない、含む、含まない、存在する、存在しない | 文字列データを組み込みます。 |

### オーディエンスの共有

外部オーディエンスは、新しいセグメント定義のコンポーネントとしても使用でき、属性ルールを新しいセグメントに追加できます。

現在、外部オーディエンスとしてはAdobe Audience Managerのみがサポートされており、今後、追加のソースが有効になる予定です。 Platform でのAdobe Audience Managerオーディエンスの使用について詳しくは、 [Adobe Audience Managerドキュメント内のオーディエンス共有ガイド](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja).

### セグメントの共有

Platform で作成されたセグメントは、他の [Adobe Experience Cloud Core Services](https://experienceleague.adobe.com/docs/core-services/interface/experience-cloud.html?lang=ja). この機能を有効にするには、ソリューションアーキテクトまたは担当のコンサルタントにお問い合わせください。

## その他のデータタイプ

サポートされるデータタイプには、上記のデータタイプに加えて、次のデータタイプも含まれます。

- URI(Uniform Resource Identifier)
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
