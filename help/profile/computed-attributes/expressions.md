---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: 計算済み属性の PQL 式の例
topic-legacy: guide
type: Documentation
description: 計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数では、有効なプロファイルクエリ言語 (PQL) 式を使用する必要があります。 このガイドでは、計算済み属性で最もよく使用される PQL 式の一部を説明します。
exl-id: 7c80e2d3-919a-47f9-a59f-833a70f02a8f
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 4%

---

# （アルファ）計算済み属性の PQL 式の例

>[!IMPORTANT]
>
>計算済み属性機能は現在アルファ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformでは、計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。各計算済み属性は、名前や説明、値が保持されるフィールドのスキーマクラスとパス、計算済み属性に格納する値が計算済み値の式など、基本情報を使用して定義されます。

計算済み属性で使用される式は、 [!DNL Profile Query Language] (PQL) を使用して作成されます。PQL は、リアルタイム顧客プロファイルデータのクエリの定義と実行をサポートするように設計された、Experience Data Model(XDM) 準拠のクエリ言語です。

計算済み属性は現在、次の関数をサポートしています。sum、count、min、max および boolean。 このガイドでは、組織に独自の計算済み属性を定義する際に使用できる、最もよく使用される PQL 式の一部について説明します。 PQL の詳細と、サポートされるクエリのその他の書式設定のガイドラインおよびサンプルへのリンクについては、[PQL の概要 ](../../segmentation/pql/overview.md) を参照してください。

## ストリーミング式

次の表に、ストリーミングモードでサポートされる一般的に使用されるクエリ式の詳細を示します。

| 説明 | PQL 式 | 入力タイプ：<br/> プロファイルまたはエクスペリエンスイベント (EE[]) | 結果のタイプ |
|---|---|---|---|
| 過去 7 日間の画像のダウンロード数。 | xEvent[（タイムスタンプは現在よりも 7 日前に発生）および eventType=&quot;download&quot;および contentType= &quot;image&quot;].count() | プロファイルと EE [] | 整数 |
| 過去 7 日間のスポーツ用品に対する顧客支出の合計。 | xEvent[（タイムスタンプは現在より 7 日前に発生）および eventType=&quot;transaction&quot;および category = &quot;sportinggoods&quot;].sum(commerce.order.priceTotal) | プロファイルと EE [] | 整数または倍精度 |
| 過去 7 日間のスポーツ用品に対する平均顧客支出。<br/><br/>**注意：** 3 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[( タイムスタンプが発生しま &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>す。sum(commerce.order.priceTotal)<br/><br/>**ca2:** xEvent[( タイムスタンプが発生します。 &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>.count()<br/><br/>**ca3:** ca1 / ca2]] | プロファイルと EE [] | Double |
| 顧客は過去 7 日間、スポーツ用品に 100 ドル以上を費やしたか。<br/><br/>**注意：** 2 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[( タイムスタンプが発生 &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>する.sum(commerce.order.priceTotal)<br/><br/>**ca2:** ca1 > 100] | プロファイルと EE [] | Boolean |
| 顧客は過去 7 日間に購入したか。 | chain(xEvent, timestamp, [A:WHAT(eventType = &quot;transaction&quot;) WHEN（&lt; 7 日前）]) | プロファイルと EE [] | Boolean |
| 過去 7 日間で最も低いユーザーがスポーツ用品に費やした。 | xEvent[（タイムスタンプは現在より 7 日前に発生）および eventType=&quot;transaction&quot;および category = &quot;sportinggoods&quot;].min(commerce.order.priceTotal) | プロファイルと EE [] | 整数または倍精度 |
| 過去 7 日間で最も高いユーザーがスポーツ用品に費やした。 | xEvent[（タイムスタンプは現在より 7 日前に発生）および eventType=&quot;transaction&quot;および category = &quot;sportinggoods&quot;].max(commerce.order.priceTotal) | プロファイルと EE [] | 整数または倍精度 |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロード数。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.count()))) | プロファイルと EE [] | Map[String, Integer] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロード数に対する数値プロパティの合計。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal)))) | プロファイルと EE [] | Map[String、Integer] または Map[String、Double] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの平均。<br/><br/>**注意：** 3 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[( タイムスタンプが発生する &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>.groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal)))<br/><br/>**ca2:** xEvent[( タイムスタンプが発生する.groupBy(product).map((K, G) => map(K, G.count())) &lt; 7=&quot;&quot; days=&quot;&quot; before=&quot;&quot; now=&quot;&quot;>ca3:<br/><br/>**** ca2 / ca1]] | プロファイルと EE [] | Map[String, Double] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの最小値。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.min(commerce.order.priceTotal)))) | プロファイルと EE [] | Map[String、Integer] または Map[String、Double] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードを超えた数値プロパティの最大数。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.max(commerce.order.priceTotal)))) | プロファイルと EE [] | Map[String、Integer] または Map[String、Double] |
| プロファイルに対する数値式（イベントを参照しない）。 | if(person.gender = &quot;female&quot;, 60, 65) | プロファイル | 整数または倍精度 |
| プロファイルに対するブール式（イベントを参照しない）。 | person.birthYear >= 2000 | プロファイル | Boolean |
| プロファイルに対する文字列式（イベントを参照していません）。 | if(homeAddress.countryCode in [&quot;US&quot;,&quot;MX&quot;,&quot;CA&quot;], &quot;NA&quot;, &quot;ROW&quot;) | プロファイル | 文字列 |

## バッチ式

次の表に、バッチモードでのみ使用可能な、つまり現在ストリーミングでは使用できないクエリ式の詳細を示します。

| 説明 | PQL 式 | 入力タイプ：<br/> プロファイルまたはエクスペリエンスイベント (EE[]) | 結果のタイプ |
|---|---|---|---|
| 過去 7 日間の製品ダウンロードに対する数値式の合計が 100 を超えているかどうか（製品別インデックス）。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal) > 100)) | プロファイルと EE [] | Map[String, Boolean] |
| 過去 7 日間の製品のダウンロードに対する数値式の平均が 100 を超えているかどうか（製品別）。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map(((K, G) => mapEntry(K, G.average(commerce.order.priceTotal) > 100)) | プロファイルと EE [] | Map[String, Boolean] |
| 過去 7 日間の、ダウンロードした各製品の様々な指標の蓄積。製品別にインデックス化。 | xEvent[（タイムスタンプは今から 7 日前に発生します）および eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, {&quot;count&quot;:G.count(), &quot;sum&quot;:G.sum(commerce.order.priceTotal)}) | プロファイルと EE [] | Map[String, Object] （Object はカスタム XDM タイプ） |
