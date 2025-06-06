---
title: AI アシスタントに関するよくある質問
description: AI アシスタントに関するよくある質問への回答を説明します
exl-id: 17a07c11-7bc6-4cba-be0a-d75fdb567053
source-git-commit: 74734711c4c7561523f4e93c5c85c261c2d31afa
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---

# FAQ

以下は、AI アシスタントに関するよくある質問に対する回答のリストです。

## AI アシスタントの情報はリアルタイムで提供されますか？

AI アシスタントの応答で表示されるデータは、毎日更新されます。 つまり、応答内のデータは、応答時にExperience Platform ユーザーインターフェイスに表示されるデータよりも最大 24 時間古くなる可能性があります。

同じ原則がアクセス制御にも当てはまります。 AI アシスタントがアクセス制御および権限設定に加えられた変更を反映するには、最大 24 時間かかる場合があります。 詳しくは、[AI アシスタントのプライバシー ](./privacy.md) に関するガイドを参照してください。

## AI アシスタントがサポートするAdobe アプリケーションは何ですか？

現在、AI アシスタントの範囲は次のとおりです。

* [ 製品ナレッジ ](./home.md#product-knowledge):AI アシスタントは、Experience Platform、Real-Time Customer Data Platform、Adobe Journey Optimizerの製品ナレッジに関する質問に回答できます。 また、Customer Journey Analyticsの UI を通じてのみ、Customer Journey Analyticsの製品知識のトピックを掘り下げることもできます。
* [ 運用インサイト ](./home.md#operational-insights)：属性、オーディエンス、データフロー、データセット、宛先、ジャーニー、スキーマおよびソースのデータオブジェクトに関する運用インサイトについて、AI アシスタントに質問できます。

## AI アシスタントの機能とは？

AI アシスタントは、Adobeの製品ナレッジクエリに対応し、データオブジェクトのオペレーショナルインサイトに関連する質問に答えることができます。 （例えば、「アクティブ化されているオーディエンスの数。」）

## AI アシスタントはプロファイルデータに関する情報を提供できますか？

いいえ。AI アシスタントはプロファイルレベルのデータにはアクセスできないので、AI アシスタントでは参照または使用されません。

## 個人情報は AI アシスタントのトレーニングデータに利用されますか？

AI アシスタントは、トレーニング目的で個人情報を使用しません。 AI アシスタントには、ご自身に関する個人情報（氏名、連絡先情報を含む）やその他の関係者に関する情報を提供しないでください。

## 回答の検証時に AI アシスタントから返される SQL クエリを実行できますか？

いいえ。この SQL クエリは読み取り専用で、AI アシスタントまたはクエリサービスでは実行できません。