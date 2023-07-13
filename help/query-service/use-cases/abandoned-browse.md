---
keywords: Experience Platform；クエリサービス；クエリサービス；クエリ
title: Adobe Experience Platformクエリサービスの使用例
description: Adobe Experience Platform Query Service の汎用性とメリットを示すエンドツーエンドの例です。
exl-id: 00bdae47-71b7-44ea-9365-a1d64c88d2bf
source-git-commit: 79966442f5333363216da17342092a71335a14f0
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 3%

---

# Adobe Experience Platformの使用例 [!DNL Query Service]

このドキュメントと付属のビデオプレゼンテーションは、Adobe Experience Platformがどのように機能するかを示す高レベルのエンドツーエンドのワークフローを提供します [!DNL Query Service] は、組織の戦略的ビジネスインサイトを活用します。 閲覧中断の使用例を例として使用したこのガイドでは、次の主要概念を説明します。

* Adobe Experience Platformの潜在能力を最大限に引き出すための、データ処理の重要な重要性。
* 既存のデータアーキテクチャに基づいてクエリを作成する方法。
* ニーズを満たすデータ品質、および不足を軽減する方法を確保する。
* パーソナライゼーションのためのセグメント化と宛先で、設定された頻度でクエリを実行するようにスケジュールするプロセスです。
* マーケターが、 [!DNL Query Service].

## 目標 {#objectives}

このワークフローのデモは、複数のAdobe Experience Platformサービスに基づいています。 続ける場合は、次の機能およびサービスについて十分に理解することをお勧めします。

* この [Experience Data Model(XDM) スキーマ構成の基本](../../xdm/schema/composition.md)
* 方法 [データセットの作成とデータの取り込み](https://experienceleague.adobe.com/docs/platform-learn/tutorials/data-ingestion/create-datasets-and-ingest-data.html?lang=ja)
* 方法 [Adobe Analyticsソースコネクタを使用したデータの取り込み](https://experienceleague.adobe.com/docs/platform-learn/tutorials/sources/ingest-data-from-adobe-analytics.html?lang=ja)
* [セグメント化](../../segmentation/home.md)
* [宛先](../../destinations/home.md)

閲覧中断の例では、Adobeの使用が中心です [!DNL Analytics] 特定のアクションにつながるオーディエンスを作成するためのデータ。 オーディエンスが絞り込まれ、過去 4 日間に Web サイトを閲覧したが購入しなかったすべての顧客が含まれます。 その後、オーディエンスの各プロファイルが、顧客の行動パターンに基づいて最も価格の高い SKU をターゲットに設定されます。

クエリ自体は非常に規範的で、セグメント定義の使用例の条件を満たすデータのみが含まれます。 これにより、 [!DNL Analytics] 処理中のデータ。 また、データを価格の高い順に並べ、ユーザーが閲覧していた最も価格の高い SKU を選択します。

プレゼンテーションで使用されるクエリは、次のように表示されます。

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

## [!DNL Query Service] adobe analytics を使用した閲覧中断の例 {#video-example}

以下に示すビデオプレゼンテーションは、に焦点を当てた、Experience Platformデータの全体的な実際の使用例です。 [!DNL Query Service] とAdobe分析の統合。

>[!VIDEO](https://video.tv.adobe.com/v/342533?quality=12&learn=on)

## のメリット [!DNL Query Service] {#benefits}

が提供する機能 [!DNL Query Service] 多くの目的を果たす これを使用して、セグメント化の複雑なロジックに対応したり、ダウンストリームで使用するためにパーソナライズされた様々な属性を計算したり、オーディエンスの構築を大幅に簡略化したりできます。

[!DNL Query Service] を使用すると、クエリに制約を含めて、オーディエンスの構築プロセスを簡略化できます。 これにより、適切なデータがオーディエンスに適したものとなるので、データの品質が向上します。 クエリの品質を維持すると、正確なオーディエンスが得られ、データの信頼性に役立ちます。 また、クエリから派生した属性に基づいてスキーマとカスタムテーブルを作成することで、オーディエンスを保存することもできます。 カスタムテーブルをプロファイルに対して有効にし、これらのデータポイントをセグメント化とパーソナライゼーションに使用できます。 この機能は、明確な顧客オーディエンスを作成したいマーケターを支援します。

また、繰り返し条件または静的条件を満たすロジックをクエリに含めることでも、 [!DNL Query Service] 精巧なセグメント化の負担を抽出します。

Adobe Experience Platformは、データリポジトリと、効率的で信頼性の高い方法でデータをアクティブ化するために必要なツールを提供します。 Platform 内にデータを保持することで、他のプロセスを実行しながら属性を取得でき、操作や処理をおこなうためにサードパーティのツールにデータを書き出す必要がなくなります。 このような処理のオーバーヘッドは、数百の属性やキャンペーンを処理する際に、プロジェクトのタイムラインに大きく影響する可能性があります。 これにより、マーケターは、データにアクセスしてキャンペーンを構築できるだけでなく、メッセージをセグメント化しパーソナライズする非常に動的な手段を提供できます。

## 次の手順

このドキュメントを読むと、次の内容を理解できます。 [!DNL Query Service] は、データの質とセグメント化の容易さに加え、エンドツーエンドのワークフロー全体に対するデータアーキテクチャ内での重要性にも影響を与えます。 Adobe Analyticsと [!DNL Query Service]を参照し、 [Adobe Analyticsマーチャンダイジング変数の使用例](./merchandising-variables.md).

の利点を示すその他のドキュメント [!DNL Query Service] 組織の戦略的ビジネスインサイトに対して、 [ボットフィルタリングの使用例](./bot-filtering.md) 例：

また、これらのドキュメントは、 [!DNL Query Service] 機能：

* [クエリ実行のガイダンス](../best-practices/writing-queries.md)
* [データアセット組織のガイダンス](../best-practices/organize-data-assets.md).


