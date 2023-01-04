---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: 計算済み属性の PQL 式の例
topic-legacy: guide
type: Documentation
description: 計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数では、有効なプロファイルクエリ言語 (PQL) 式を使用する必要があります。 このガイドでは、計算済み属性で最もよく使用される PQL 式の一部を説明します。
exl-id: 7c80e2d3-919a-47f9-a59f-833a70f02a8f
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '965'
ht-degree: 4%

---

# （アルファ）計算済み属性の PQL 式の例

>[!IMPORTANT]
>
>計算済み属性機能は現在アルファ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

Adobe Experience Platformでは、計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。 これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。各計算済み属性は、名前や説明、値が保持されるフィールドのスキーマクラスとパス、計算済み属性に格納する値を表す式などの基本情報を使用して定義されます。

計算済み属性で使用される式は、 [!DNL Profile Query Language] (PQL):Experience Data Model(XDM) に準拠したクエリ言語で、リアルタイム顧客プロファイルデータのクエリの定義と実行をサポートするように設計されています。

計算済み属性は現在、次の関数をサポートしています。sum、count、min、max および boolean。 このガイドでは、組織に独自の計算済み属性を定義する際に使用できる、最もよく使用される PQL 式の一部の概要を説明します。 PQL の詳細と、サポートされるクエリの追加の書式設定のガイドラインやサンプルへのリンクについては、 [PQL の概要](../../segmentation/pql/overview.md).

## ストリーミング式

次の表に、ストリーミングモードでサポートされる一般的に使用されるクエリ式の詳細を示します。

| 説明 | PQL 式 | 入力タイプ：<br/>プロファイルまたはエクスペリエンスイベント (EE)[]) | 結果のタイプ |
|---|---|---|---|
| 過去 7 日間の画像のダウンロード数。 | xEvent[（タイムスタンプは今から 7 日未満に発生します）、eventType=&quot;download&quot;および contentType= &quot;image&quot;].count() | プロファイルと EE[] | 整数 |
| 過去 7 日間のスポーツ用品に対する顧客支出の合計。 | xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].sum(commerce.order.priceTotal) | プロファイルと EE[] | 整数または倍精度浮動小数点数 |
| 過去 7 日間のスポーツ用品に対する平均顧客支出。<br/><br/>**注意：** 3 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].sum(commerce.order.priceTotal)<br/><br/>**ca2:** xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].count()<br/><br/>**ca3:** ca1 / ca2 | プロファイルと EE[] | Double |
| 顧客は過去 7 日間にスポーツ用品に 100 ドル以上を費やしたか。<br/><br/>**注意：** 2 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].sum(commerce.order.priceTotal)<br/><br/>**ca2:** ca1 > 100 | プロファイルと EE[] | ブール値 |
| 顧客が過去 7 日間に購入したか。 | chain(xEvent, timestamp, [回答：WHAT(eventType = &quot;transaction&quot;) WHEN（&lt; 7 日前）]) | プロファイルと EE[] | ブール値 |
| 過去 7 日間で最も低いユーザーがスポーツ用品に費やした。 | xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].min(commerce.order.priceTotal) | プロファイルと EE[] | 整数または倍精度浮動小数点数 |
| 過去 7 日間で最も高いユーザーがスポーツ用品に費やした。 | xEvent[（タイムスタンプは今から 7 日未満で発生します）、eventType=&quot;transaction&quot;および category=&quot;sportinggoods&quot;].max(commerce.order.priceTotal) | プロファイルと EE[] | 整数または倍精度浮動小数点数 |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロード数。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.count())) | プロファイルと EE[] | マップ[文字列、整数] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの合計。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal))) | プロファイルと EE[] | マップ[文字列、整数] またはマップ[文字列、倍精度浮動小数点] |
| 過去 7 日間の、ダウンロードされた各製品のダウンロードに対する数値プロパティの、製品ごとにインデックス化された平均。<br/><br/>**注意：** 3 つの計算済み属性を作成する必要があります。 | **ca1:** xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal)))<br/><br/>**ca2:** xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.count()))<br/><br/>**ca3:** ca2 / ca1 | プロファイルと EE[] | マップ[文字列、倍精度浮動小数点] |
| 過去 7 日間に、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの最小数。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.min(commerce.order.priceTotal))) | プロファイルと EE[] | マップ[文字列、整数] またはマップ[文字列、倍精度浮動小数点] |
| 過去 7 日間の、製品ごとにインデックス付けされた、ダウンロードされた各製品のダウンロードに対する数値プロパティの最大数。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.max(commerce.order.priceTotal))) | プロファイルと EE[] | マップ[文字列、整数] またはマップ[文字列、倍精度浮動小数点] |
| イベントを参照していない、プロファイルに対する数値式。 | if(person.gender = &quot;female&quot;, 60, 65) | プロファイル | 整数または倍精度浮動小数点数 |
| イベントを参照していないプロファイルのブール式。 | person.birthYear >= 2000 | プロファイル | ブール値 |
| プロファイルの文字列式。イベントを参照していません。 | if(homeAddress.countryCode in [&quot;US&quot;,&quot;MX&quot;,&quot;CA&quot;], &quot;NA&quot;, &quot;ROW&quot;) | プロファイル | 文字列 |

## バッチ式

次の表に、バッチモードでのみ使用可能な、つまり現在ストリーミングでは使用できないクエリ式の詳細を示します。

| 説明 | PQL 式 | 入力タイプ：<br/>プロファイルまたはエクスペリエンスイベント (EE)[]) | 結果のタイプ |
|---|---|---|---|
| 過去 7 日間の製品ダウンロードに対する数値式の合計が 100 を超えているかどうか（製品別インデックス）。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.sum(commerce.order.priceTotal) > 100)) | プロファイルと EE[] | マップ[文字列、ブール値] |
| 過去 7 日間の製品のダウンロードに対する数値式の平均が 100 を超えているかどうか（製品ごとにインデックス付けされています）。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, G.average(commerce.order.priceTotal) > 100)) | プロファイルと EE[] | マップ[文字列、ブール値] |
| 過去 7 日間にダウンロードした各製品に対する様々な指標の蓄積。製品ごとにインデックスが作成されます。 | xEvent[（タイムスタンプは今から 7 日未満前に発生します）と eventType=&quot;download&quot;].groupBy(product).map((K, G) => mapEntry(K, {&quot;count&quot;:G.count(), &quot;sum&quot;:G.sum(commerce.order.priceTotal)}) | プロファイルと EE[] | マップ[文字列、オブジェクト] ここで、オブジェクトはカスタム XDM タイプです。 |
