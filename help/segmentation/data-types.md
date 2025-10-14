---
solution: Experience Platform
title: セグメント化サービスでサポートされるデータタイプ
description: Adobe セグメント化サービス内では、すべてのエクスペリエンスデータモデル（XDM）データタイプがサポートされます。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。
exl-id: 73f932a7-f864-4566-ade7-c148a12dc83c
source-git-commit: 0a9028beca36b46d6228c0038366bbac5d32603c
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 52%

---

# セグメント化サービスでサポートされるデータタイプ

Adobe Experience Platform セグメント化サービス内では、すべてのエクスペリエンスデータモデル（XDM）データタイプがサポートされます。 セグメント定義を構成するルールは、次のデータタイプによってコンテキスト化されます。

## 文字列データ

セグメント定義では、文字列データを使用して、「国名」や「ロイヤルティプログラムレベル」など、オーディエンスの非数値制約を定義します。

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
| person.firstPurchase | `today`、`yesterday`、`this month`、`this year` | セグメント定義が作成された日に関連します。 |
| person.lastPurchase | `in last`, `during`, `before`, `after`, `within` | 任意の週内／月内に関連します。 |

## エクスペリエンスイベント

Adobe Experience Platform スキーマとして、Experience-Platform 統合アプリケーションとの明示的および暗黙的な顧客インタラクション（インタラクションが発生した時点でのシステムのスナップショットなど）を記録で [!DNL XDM ExperienceEvents] ます。 [!DNL ExperienceEvents] れはファクトレコードです。 したがって、セグメント定義時に使用できるデータソースになります。

次の表に示すように、イベントデータは、イベント動作の絞り込みやイベント属性の指定に役立つキーワードを使用して表現されます。

| キーワード | 用途 |
| ------- | --- |
| 含む／除外する | データを含めるまたは省略してイベントの動作を記述します。 |
| 任意／すべて | 選定されるセグメント定義の数を判断するのに役立ちます。 |
| 「時間ルールを適用」切り替えボタン | 日付データを組み込みます。 |
| 等しい、等しくない、～で開始する、～で開始しない、～で終わる、～で終わらない、含む、含まない、存在する、存在しない | 文字列データを組み込みます。 |

### オーディエンスの共有

また、外部オーディエンスは、新しいセグメント定義のコンポーネントとして使用し、その属性ルールを新しいセグメント定義に追加することもできます。

現在、外部オーディエンスとしてサポートされているのはAdobe Audience Managerのみで、今後、有効になるソースも追加されます。 Experience PlatformでのAdobe Audience Manager オーディエンスの使用について詳しくは、[Adobe Audience Manager ドキュメント内のオーディエンス共有ガイド &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html?lang=ja) を参照してください。

### セグメント定義の共有

Experience Platformで作成したセグメント定義は、他の [Adobe Experience Cloud コアサービス &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/experience-cloud.html?lang=ja) 内で使用できます。 この機能を有効にするには、ソリューションアーキテクトまたはコンサルタントに問い合わせる必要があります。

## その他のデータタイプ

上記のデータタイプに加えて、サポートされるデータタイプのリストには次も含まれます。

- Uniform Resource Identifier （URI; ユニフォーム リソース識別子）
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
