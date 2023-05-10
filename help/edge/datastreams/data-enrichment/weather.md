---
title: DNL The Weather Channel からの天気データの使用
description: DNL の気象データを使用する気象チャネルは、データストリームを通じて収集するデータを強化するために使用します。
exl-id: 548dfca7-2548-46ac-9c7e-8190d64dd0a4
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 4%

---

# 次の天気データを使用： [!DNL The Weather Channel]

Adobeが～と提携した [!DNL [The Weather Company]](https://www.ibm.com/weather) データストリームを介して収集されたデータに米国の天気の追加コンテキストを取り込みます。 このデータは、分析、ターゲティングおよびセグメントのExperience Platform作成に使用できます。

次の 3 種類のデータを使用できます。 [!DNL The Weather Channel]:

* **[!UICONTROL 現在の天気]**:場所に基づく、ユーザーの現在の気象条件。 これには、現在の温度、権限、雲の範囲などが含まれます。
* **[!UICONTROL 予測天気]**:この予測には、ユーザーの所在地の 1 日、2 日、3 日、5 日、7 日および 10 日の予測が含まれます。
* **[!UICONTROL トリガー]**:トリガーは、様々な意味的気象条件にマッピングする特定の組み合わせです。 次の 3 種類の天気トリガーがあります。

   * **[!UICONTROL 気象トリガー]**:意味的に意味のある条件（寒い、雨の日など）。 これらの定義は、様々な気候間で異なる場合があります。
   * **[!UICONTROL 製品トリガー]**:様々なタイプの製品の購入につながる条件。 例：寒波予報は、雨のコートの購入の可能性が高いことを意味する可能性があります。
   * **[!UICONTROL 厳しい天候トリガー]**:冬の嵐やハリケーンの警告など、厳しい天気の警告。

## 前提条件 {#prerequisites}

天気データを使用する前に、次の前提条件を満たしていることを確認してください。

* 使用する天気データのライセンスを取得する必要があります。 [!DNL The Weather Channel]. その後、お客様のアカウントで有効にします。
* 気象データは、データストリームを通じてのみ使用できます。 天気データを使用するには、 [!DNL Web SDK], [!DNL Mobile Edge Extension] または [サーバー API](../../../server-api/overview.md) を使用してこのデータを活用できます。
* データストリームには [[!UICONTROL 地域]](../configure.md#advanced-options) 有効。
* を [気象場群](#schema-configuration) を使用しているスキーマに追加します。

## プロビジョニング {#provisioning}

データのライセンスを取得したら、 [!DNL The Weather Channel]を使用すると、アカウントがデータにアクセスできるようになります。 次に、データストリームでデータを有効にするには、Adobeカスタマーケアに問い合わせる必要があります。 有効にすると、データが自動的に追加されます。

デバッガーでエッジトレースを実行するか、アシュランスを使用して [!DNL Edge Network].

### スキーマ設定 {#schema-configuration}

データストリームで使用するExperience Platformデータセットに対応するイベントスキーマに、気象フィールドグループを追加する必要があります。 次の 5 つのフィールドグループを使用できます。

* [!UICONTROL 予測された天気]
* [!UICONTROL 現在の天気]
* [!UICONTROL 製品トリガー]
* [!UICONTROL 相対トリガー]
* [!UICONTROL 厳しい天候トリガー]

## 天気データにアクセスする {#access-weather-data}

データのライセンスが取得され、使用可能になると、Adobe サービス全体で様々な方法でデータにアクセスできます。

### Adobe Analytics {#analytics}

In [!DNL Adobe Analytics]の場合、天候データは、処理ルールを介してマッピングするために、他の [!DNL XDM] スキーマ。

マッピングできるフィールドのリストは、 [気象基準](weather-reference.md) ページ。 すべてと同様 [!DNL XDM] スキーマの場合、キーには `a.x`. 例えば、 `weather.current.temperature.farenheit` 次の場所に現れるだろう [!DNL Analytics] as `a.x.weather.current.temperature.farenheit`.

![処理ルールインターフェイス](../../assets/datastreams/data-enrichment/weather/processing-rules.png)

### Adobe Customer Journey Analytics {#cja}

In [!DNL Adobe Customer Journey Analytics]に値を入力すると、データストリームで指定されたデータセット内の天気データを利用できます。 天気属性が [をスキーマに追加しました](#prerequisites-prerequisites)を使用する場合、 [データビューに追加](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-dataviews/create-dataview.html?lang=ja) in [!DNL Customer Journey Analytics].

### Real-Time Customer Data Platform {#rtcdp}

天気データは [Real-time Customer Data Platform](../../../rtcdp/overview.md)（セグメントで使用） 気象データはイベントに添付されます。

![気象イベントを表示するセグメントビルダー](../../assets/datastreams/data-enrichment/weather/schema-builder.png)

天気の状況は頻繁に変化するので、Adobeでは、上の例に示すように、セグメントに時間制約を設定することをお勧めします。 6 ヶ月前の寒い日よりも、最後の日か 2 日の方がずっと効果的です。

詳しくは、 [気象基準](weather-reference.md) 」をクリックします。

### Adobe Target {#target}

In [!DNL Adobe Target]を使用すると、天気データを使用してパーソナライゼーションをリアルタイムで実行できます。 天気データがに渡される [!DNL Target] as [!UICONTROL mbox] パラメーターを使用し、カスタム [!UICONTROL mbox] パラメーター。

![Target Audience Builder](../../assets/datastreams/data-enrichment/weather/target-audience-builder.png)

パラメーターは [!DNL XDM] 特定のフィールドへのパス。 詳しくは、 [気象基準](weather-reference.md) を返します。

## 次の手順 {#next-steps}

このドキュメントを読むと、様々なAdobeソリューションでの天気データの使用方法に関する理解が深まりました。 天気データフィールドのマッピングについて詳しくは、 [フィールドマッピング参照](weather-reference.md).
