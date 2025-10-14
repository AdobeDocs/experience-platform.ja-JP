---
keywords: Experience Platform；クエリサービス；クエリサービス；クエリ
title: Adobe Experience Platform クエリサービスの使用例
description: Adobe Experience Platform クエリサービスの汎用性とメリットを示すエンドツーエンドの例です。
exl-id: 00bdae47-71b7-44ea-9365-a1d64c88d2bf
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 0%

---

# Adobe Experience Platform [!DNL Query Service] の使用例

このドキュメントと付属のビデオプレゼンテーションでは、Adobe Experience Platform [!DNL Query Service] が組織の戦略的なビジネスインサイトにどのように役立つかを示す高度なエンドツーエンドワークフローを提供します。 このガイドでは、参照放棄のユースケースを例として、次の主な概念について説明します。

* Adobe Experience Platformの可能性を最大限に引き出すためのデータ処理が重要です。
* 既存のデータアーキテクチャに基づいてクエリを作成する方法を説明します。
* ニーズを満たすデータ品質と、不足を軽減する方法を確保します。
* パーソナライゼーションのセグメント化と宛先でダウンストリームに使用するために、設定された頻度で実行するようにクエリをスケジュールするプロセス。
* [!DNL Query Service] の機能を通じて、マーケターが派生データセットをオーディエンスに簡単に含めることができます。

## 目標 {#objectives}

このワークフローデモは、いくつかのAdobe Experience Platform サービスに依存しています。 次の機能やサービスについて理解を深めることをお勧めします。

* [Experience Data Model （XDM）スキーマ構成の基本 &#x200B;](../../xdm/schema/composition.md)
* [&#x200B; データセットの作成とデータの取り込み &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja) 方法
* [Adobe Analytics ソースコネクタを使用してデータを取り込む &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html?lang=ja) 法
* [セグメント化](../../segmentation/home.md)
* [宛先](../../destinations/home.md)

参照放棄の例では、Adobe [!DNL Analytics] データを使用して、特定の実用的なオーディエンスを作成することに重点を置いています。 オーディエンスは、過去 4 日間に web サイトを閲覧したものの、購入しなかったすべての顧客を含めるように絞り込まれます。 次に、オーディエンス内の各プロファイルは、顧客の行動パターンから生じた最も高い価格の SKU をターゲットにします。

クエリ自体は非常に規範的で、セグメント定義のユースケース条件を満たすデータのみが含まれます。 これにより、処理されるデータの量を最小限に抑え [!DNL Analytics] ことで、パフォーマンスが向上します。 また、データを価格別で最高から最低の順に注文し、ユーザーが閲覧していた最高価格の SKU を選択します。

プレゼンテーションで使用されるクエリを次に示します。

```sql
INSERT INTO summit_adv_data_prep_dataset
SELECT STRUCT(
    customerId AS crmCustomerId, struct(sku AS sku, price AS sku_price, abandonTS AS abandonTS) AS abandonBrowse) AS _pfreportingonprod
FROM
(SELECT
B.personKey.sourceId,
A.productListItems[0].SKU AS sku,
max(A.timestamp) AS abandonTS,
max(c._pfreportingonprod.price) AS price
FROM summit_adobe_analytics_dataset A,profile_attribute_14adf268_2a20_4dee_bee6_a6b0e34616a9 B,summit_product_dataset c
WHERE A._experience.analytics.customDimension.evars.evar1 = B.personKey.sourceID
AND productListItems[0].SKU = C._pfreportingonprod.sku
AND A.web.webpagedetails.URL NOT LIKE '%orderconfirmation%'
AND timestamp > current_date - interval '4 day'
GROUP BY customerId,sku
order by price desc)D;
```

## adobe analytics[!DNL Query Service] 使用した閲覧放棄の例 {#video-example}

以下に示すビデオプレゼンテーションでは、[!DNL Query Service] とAdobe Analytics の統合に焦点を当てたExperience Platform データの全体的で実際のユースケースを示します。

>[!VIDEO](https://video.tv.adobe.com/v/3454936?quality=12&learn=on&captions=jpn)

## [!DNL Query Service] の利点 {#benefits}

[!DNL Query Service] が提供する機能は、多くの目的に役立ちます。 セグメント化の複雑なロジックに対応したり、ダウンストリームで使用するためにパーソナライズされた様々な属性を計算したり、オーディエンスの作成方法を大幅に簡略化したりするために使用できます。

[!DNL Query Service] を使用すると、クエリに制約を含めて、オーディエンス構築プロセスを簡略化できます。 これにより、適切なデータがオーディエンスに適合するようになり、データ品質が向上します。 クエリの品質を維持することで、正確なオーディエンスが得られ、データの信頼性に役立ちます。 また、クエリから派生した属性に基づいてスキーマとカスタムテーブルを作成することで、オーディエンスを保存することもできます。 プロファイルに対してカスタムテーブルを有効にし、これらのデータポイントをセグメント化とパーソナライゼーションに使用できます。 この機能は、マーケターがユーザーの明確なオーディエンスを作成する際に役立ちます。

また、繰り返し条件や静的条件を満たすロジックをクエリに含めること [!DNL Query Service]、複雑なセグメント化の負担を抽出します。

Adobe Experience Platformは、データリポジトリと、効率的かつ信頼性の高い方法でデータをアクティブ化するために必要なツールを提供します。 Experience Platform内にデータを保持することで、他のプロセスを実行しながら属性を取得でき、操作や処理のためにサードパーティのツールにデータを書き出す必要がなくなります。 このような処理のオーバーヘッドは、数百の属性やキャンペーンを処理する際に、プロジェクトタイムラインに大きな影響を与える可能性があります。 これにより、マーケターは、データにアクセスしてキャンペーンを構築するための単一の場所に加え、メッセージをセグメント化およびパーソナライズする非常に動的な手段を利用できます。

## 次の手順

このドキュメントを読むことで、[!DNL Query Service] がデータの品質やセグメント化の容易さだけでなく、エンドツーエンドのワークフロー全体におけるデータアーキテクチャ内の重要性にどのような影響を与えるかを理解できるようになりました。 [!DNL Query Service] でAdobe Analyticsを使用する該当する SQL の例については、[Adobe Analytics マーチャンダイジング変数のユースケース &#x200B;](./merchandising-variables.md) を参照してください。

組織の戦略的ビジネスインサイトに対する [!DNL Query Service] の利点を示すその他のドキュメントには、[&#x200B; ボットフィルタリングのユースケース &#x200B;](./bot-filtering.md) があります。

また、次のドキュメントは、[!DNL Query Service] の機能を理解するうえで役に立ちます。

* [クエリ実行のガイダンス](../best-practices/writing-queries.md)
* [&#x200B; データアセット組織のガイダンス &#x200B;](../best-practices/organize-data-assets.md)。


