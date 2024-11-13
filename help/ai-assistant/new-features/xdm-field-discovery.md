---
title: AI アシスタントによる XDM フィールド検出
description: エクスペリエンスデータモデル（XDM）フィールド検出用の AI アシスタントの使用方法については、このドキュメントを参照してください。
badge: アルファ版
hide: true
hidefromtoc: true
source-git-commit: 2348001facd7ae3a95254130ae377b4ef3f2a749
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 1%

---

# AI アシスタントを使用したの XDM フィールド検出

>[!AVAILABILITY]
>
>この機能はAlpha中のため、ご利用いただけない場合があります。 Alphaプログラムに参加してこの機能にアクセスするには、Adobeアカウントチームにお問い合わせください。

AI アシスタントを使用して、Experience Data Model （XDM）フィールドを検索および検出し、Experience Platform内でターゲットオーディエンスを作成するために使用できます。

AI Assistant での XDM フィールド検出でサポートされるクエリおよびプロンプトパターンについては、次の表を参照してください。

>[!TIP]
>
>クエリパターンとプロンプトパターンは異なるユースケースで同じかもしれませんが、質問の正確な形式は、特定のサンドボックスに使用される XDM フィールドやスキーマによって異なります。

| クエリタイプ | クエリ/プロンプトパターン | 例 |
| --- | --- | --- |
| データドメインまたはエリアによる XDM フィールド検出 | {DATA_DOMAIN/AREA} を表すために使用する XDM フィールドを表示します。 | <ul><li>同意データを表すために使用される XDM フィールドを表示</li><li>メール購読に関する情報を表すために使用される XDM フィールドを表示する</li></ul> |
| 一般フィールド名による XDM フィールド検出 | <ul><li>{DATA_DOMAIN/AREA} に関連する XDM フィールドを表示します。</li><li>{GENERAL_FIELD_NAME} を含む XDM フィールド。</li></ul> | <ul><li>注文に関連する XDM フィールドを表示する</li><li>インタラクションの詳細に関連する XDM フィールドを表示</li><li>訪問者 ID を含む XDM フィールドはどれですか？</li><li>製品カテゴリを含む XDM フィールドはどれですか？</li></ul> |
| データモデル系列による XDM フィールド検出 | <ul><li>{GENERAL_FIELD_NAME} を含む {FIELD_GROUP/CLASS_NAME} のすべてのフィールドを表示します。</li><li>XDM フィールド {GENERAL_FIELD_NAME} の {FIELD_GROUP/CLASS_NAME} は何ですか？</li></ul> | <ul><li>製品データを含むフィールドグループのすべてのフィールドを表示します。</li><li>分析データを含むフィールドグループのすべてのフィールドを表示します。</li><li>XDM フィールドの名のクラスは何ですか？</li><li>XDM フィールドのメール購読のクラスは何ですか？</li></ul> |
| フィールド名と共に説明の強化機能を取得するプロンプト | {FIELD_DISCOVERY_QUERY}。 拡張説明も含めます。 | <ul><li>同意データを表すために使用される XDM フィールドを表示 また、フィールドの拡張説明も含めます。</li><li>インタラクションの詳細に関連する XDM フィールドを表示 また、フィールドの拡張説明も含めます。</li></ul> |

{style="table-layout:auto"}