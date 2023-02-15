---
title: 天気データフィールドのマッピング
description: The Weather Channel との統合の一部として使用できる、使用可能な天気データフィールドの参照ページです。
source-git-commit: 3e5ca8fe67e5ad9ce0fe70d0c9f1e2fbc20cee17
workflow-type: tm+mt
source-wordcount: '12238'
ht-degree: 0%

---


# 天気データフィールドのマッピング

Adobeが～と提携した [!DNL [The Weather Company]](https://www.ibm.com/weather) データストリームを介して収集されたデータに米国の天気の追加コンテキストを取り込みます。 このデータは、分析、ターゲティングおよびセグメントのExperience Platform作成に使用できます。

このページには、オーディエンスデータのエンリッチメントに使用できるすべてのフィールドが一覧表示されます。

フィールドは、フィールドグループに合わせて 3 つの異なるグループに分類されます。

* [**[!UICONTROL 現在の天気]**](#current-weather):場所に基づく、ユーザーの現在の気象条件。 これには、現在の温度、権限、雲の範囲などが含まれます。
* [**[!UICONTROL 予測天気]**](#forecast):この予測には、ユーザーの所在地の 1 日、2 日、3 日、5 日、7 日および 10 日の予測が含まれます。
* [**[!UICONTROL トリガー]**](#triggers):トリガーは、様々な意味的気象条件にマッピングする特定の組み合わせです。 次の 3 種類の天気トリガーがあります。

   * **[!UICONTROL 相対トリガー]**:意味的に意味のある条件（寒い、雨の日など）。 これらの定義は、様々な気候間で異なる場合があります。
   * **[!UICONTROL 製品トリガー]**:様々なタイプの製品の購入につながる条件。 例：寒波予報は、雨のコートの購入の可能性が高いことを意味する可能性があります。
   * **[!UICONTROL 厳しい天候トリガー]**:冬の嵐やハリケーンの警告など、厳しい天気の警告。

## 現在の天気 {#current-weather}

場所に基づく、ユーザーの現在の気象条件。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL 温度露点摂氏] | 飽和状態に達するために一定の圧力で空気を冷却する必要がある温度。 露点は、空気の湿度の間接的な測定値でもあります。 露点が温度を超えることはありません。 Dewpoint と Temperature が等しい場合、通常は雲またはフォグが形成されます。 温度と露点の値が近いほど、相対湿度が高くなります。 -80 ～ 100 (°F) または —62 ～ 37 (°C) の範囲。 摂氏温度 | `weather.current.temperatureDewPoint.celsius` |
| [!UICONTROL 温度露点華氏] | 飽和状態に達するために一定の圧力で空気を冷却する必要がある温度。 露点は、空気の湿度の間接的な測定値でもあります。 露点が温度を超えることはありません。 Dewpoint と Temperature が等しい場合、通常は雲またはフォグが形成されます。 温度と露点の値が近いほど、相対湿度が高くなります。 -80 ～ 100 (°F) または —62 ～ 37 (°C) の範囲。 温度（華氏） | `weather.current.temperatureDewPoint.fahrenheit` |
| [!UICONTROL 降水最終時間インチ] | 周期時間液体沈殿量。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（インチ） | `weather.current.precip1Hour.inches` |
| [!UICONTROL 降水最終時間ミリ] | 周期時間液体沈殿量。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（ミリ） | `weather.current.precip1Hour.millimeters` |
| [!DNL Precipitation Last 24 Hours Inches] | 24 時間の液体沈殿量を圧延しています。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（インチ） | `weather.current.precip24Hour.inches` |
| [!UICONTROL 過去 24 時間の降水ミリメートル] | 24 時間の液体沈殿量を圧延しています。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（ミリ） | `weather.current.precip24Hour.millimeters` |
| [!UICONTROL 過去 6 時間の降水インチ] | 6 時間の液体沈殿量をローリング。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（インチ） | `weather.current.precip6Hour.inches` |
| [!UICONTROL 過去 6 時間の降水ミリメートル] | 6 時間の液体沈殿量をローリング。 提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 降水量（ミリ） | `weather.current.precip6Hour.millimeters` |
| [!UICONTROL 圧力変化] | ミリバールで過去 3 時間の圧力変化。 | `weather.current.pressureChange` |
| [!UICONTROL 圧力平均海面レベル] | 平均海面圧（ミリバール）。  つまり海面での平均気圧です<br>範囲 — ミリバール精度から1/10 MB まで。 | `weather.current.pressureMeanSeaLevel` |
| [!UICONTROL 相対湿度] | 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.current.relativeHumidity` |
| [!UICONTROL 雪（1 時間）センチ] | 1 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 センチメートル単位の降雪 | `weather.current.snow1Hour.centimeters` |
| [!UICONTROL 雪（過去 1 時間）インチ] | 1 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 インチ単位の降雪 | `weather.current.snow1Hour.inches` |
| [!UICONTROL 24 時間雪（センチ）] | 24 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 センチメートル単位の降雪 | `weather.current.snow24Hour.centimeters` |
| 雪 24 時間インチ | 24 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 インチ単位の降雪 | `weather.current.snow24Hour.inches` |
| [!UICONTROL 6 時間の雪（センチメートル）] | 1 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 センチメートル単位の降雪 | `weather.current.snow6Hour.centimeters` |
| [!UICONTROL 過去 6 時間の雪] | 1 時間の降雪量。  提示される金額は、リクエスト時間（現在は）を通じて周期的な時間です。 インチ単位の降雪 | `weather.current.snow6Hour.inches` |
| [!UICONTROL 終了時間] | 日没時刻 (UTC)。 | `weather.current.sunsetTime` |
| [!UICONTROL 摂氏温度] | 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.current.temperature.celsius` |
| [!UICONTROL 華氏温度] | 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.current.temperature.fahrenheit` |
| [!UICONTROL 摂氏 24 時間の温度変化] | 24 時間前のレポートと比較した温度の変化。 摂氏温度 | `weather.current.temperatureChange24Hour.celsius` |
| [!UICONTROL 華氏 24 時間の温度変化] | 24 時間前のレポートと比較した温度の変化。 温度（華氏） | `weather.current.temperatureChange24Hour.fahrenheit` |
| [!UICONTROL 気温は摂氏に近い] | 見かけの温度。 風の冷やしや熱指数の影響により、人の肌に露出した空気の温度が「感じる」様子を表します。<br>温度が 65 ° F 以上の場合、Feels Like 値は計算されたヒート指数を表します。  温度が 65 ° F 以下の場合、Feels Like の値は計算された風の冷を表します。<br>-140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.current.temperatureFeelsLike.celsius` |
| [!UICONTROL 温度は華氏の感じ] | 見かけの温度。 風の冷やしや熱指数の影響により、人の肌に露出した空気の温度が「感じる」様子を表します。<br>温度が 65 ° F 以上の場合、Feels Like 値は計算されたヒート指数を表します。  温度が 65 ° F 以下の場合、Feels Like の値は計算された風の冷を表します。<br>-140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.current.temperatureFeelsLike.fahrenheit` |
| [!UICONTROL 摂氏 7 時以降の最大温度] | 現地時間の午前 7 時からの最大温度。 摂氏温度 | `weather.current.temperatureMaxSince7Am.celsius` |
| [!UICONTROL 華氏 7 時からの最大温度] | 現地時間の午前 7 時からの最大温度。 温度（華氏） | `weather.current.temperatureMaxSince7Am.fahrenheit` |
| [!UICONTROL 最近の 24 時間の摂氏温度] | 過去 24 時間の最低温度。  24 時間の期間は、リクエスト時間（現在は）を参照しています。  摂氏温度 | `weather.current.temperatureMin24Hour.celsius` |
| [!UICONTROL 最近の 24 時間（華氏）の温度] | 過去 24 時間の最低温度。  24 時間の期間は、リクエスト時間（現在は）を参照しています。  温度（華氏） | `weather.current.temperatureMin24Hour.fahrenheit` |
| [!UICONTROL 風向き] | 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.current.windDirection` |
| [!UICONTROL 風速キロメートル/時] | このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.current.windGust.kilometersPerHour` |
| [!UICONTROL 風速マイル/時] | このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.current.windGust.`milesPerHour |
| [!UICONTROL 風速キロメートル/時] | 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.current.windSpeed.kilometersPerHour` |
| [!UICONTROL 風速マイル/時] | 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.current.windSpeed.milesPerHour` |

## 予測された天気 {#forecast}

特定の時点での、場所に基づく、ユーザーの予測天気。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!DNL Day 1 Forecast Calendar Day Temperature Max Celsius] | 1 日の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day01Forecast.calendarDayTemperatureMax.celsius` |
| [!DNL Day 1 Forecast Calendar Day Temperature Max Fahrenheit] | 1 日の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day01Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!DNL Day 1 Forecast Calendar Day Temperature Min Celsius] | 1 日の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day01Forecast.calendarDayTemperatureMin.celsius` |
| [!DNL Day 1 Forecast Calendar Day Temperature Min Fahrenheit] | 1 日の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day01Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!DNL Day 1 Forecast Day Cloud Cover Miles] | 1 日の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day01Forecast.day.cloudCover.inches` |
| [!DNL Day 1 Forecast Day Cloud Cover Kilometers] | 1 日の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day01Forecast.day.cloudCover.kilometers` |
| [!DNL Day 1 Forecast Day Precipitation Chance] | 1 日の天気予報。 昼間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day01Forecast.day.precipChance` |
| [!DNL Day 1 Forecast Day Precipitation Type] | 1 日の天気予報。 昼間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day01Forecast.day.precipType` |
| [!DNL Day 1 Forecast Day QPF Inches] | 1 日の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day01Forecast.day.qpf.inches` |
| [!DNL Day 1 Forecast Day QPF Millimeters] | 1 日の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day01Forecast.day.qpf.millimeters` |
| [!DNL Day 1 Forecast Day QPF Snow Centimeters] | 1 日の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day01Forecast.day.qpfSnow.centimeters` |
| [!DNL Day 1 Forecast Day QPF Snow Inches] | 1 日の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day01Forecast.day.qpfSnow.inches` |
| [!DNL Day 1 Forecast Day Relative Humidity] | 1 日の天気予報。 昼間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day01Forecast.day.relativeHumidity` |
| [!DNL Day 1 Forecast Day Snow Range] | 1 日の天気予報。 昼間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day01Forecast.day.snowRange` |
| [!DNL Day 1 Forecast Day Temperature Celsius] | 1 日の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day01Forecast.day.temperature.celsius` |
| [!DNL Day 1 Forecast Day Temperature Fahrenheit] | 1 日の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day01Forecast.day.temperature.fahrenheit` |
| [!DNL Day 1 Forecast Day Temperature Heat Index Celsius] | 1 日の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day01Forecast.day.temperatureHeatIndex.celsius` |
| [!DNL Day 1 Forecast Day Temperature Heat Index Fahrenheit] | 1 日の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day01Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!DNL Day 1 Forecast Day Temperature Wind Chill Celsius] | 1 日の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day01Forecast.day.temperatureWindChill.celsius` |
| [!DNL Day 1 Forecast Day Temperature Wind Chill Fahrenheit] | 1 日の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day01Forecast.day.temperatureWindChill.fahrenheit` |
| [!DNL Day 1 Forecast Day Thunder Index] | 1 日の天気予報。 昼間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day01Forecast.day.thunderIndex` |
| [!DNL Day 1 Forecast Day UV Index] | 1 日の天気予報。 昼間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day01Forecast.day.uvIndex` |
| [!DNL Day 1 Forecast Day Wind Direction] | 1 日の天気予報。 昼間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day01Forecast.day.windDirection` |
| [!DNL Day 1 Forecast Day Wind Gust Kilometers per Hour] | 1 日の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day01Forecast.day.windGust.kilometersPerHour` |
| [!DNL Day 1 Forecast Day Wind Gust Miles per Hour] | 1 日の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day01Forecast.day.windGust.milesPerHour` |
| [!DNL Day 1 Forecast Day Wind Speed Kilometers per Hour] | 1 日の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day01Forecast.day.windSpeed.kilometersPerHour` |
| [!DNL Day 1 Forecast Day Wind Speed Miles per Hour] | 1 日の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day01Forecast.day.windSpeed.milesPerHour ` |
| [!DNL Day 1 Forecast Night Cloud Cover Miles] | 1 日の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day01Forecast.night.cloudCover.inches` |
| [!DNL Day 1 Forecast Night Cloud Cover Kilometers] | 1 日の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day01Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 1 日目：夜の降水の可能性の予測] | 1 日の天気予報。 夜間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day01Forecast.night.precipChance` |
| [!DNL Day 1 Forecast Night Precipitation Type] | 1 日の天気予報。 夜間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day01Forecast.night.precipType` |
| [!DNL Day 1 Forecast Night QPF Inches] | 1 日の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day01Forecast.night.qpf.inches` |
| [!DNL Day 1 Forecast Night QPF Millimeters] | 1 日の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day01Forecast.night.qpf.millimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Centimeters] | 1 日の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day01Forecast.night.qpfSnow.centimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Inches] | 1 日の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day01Forecast.night.qpfSnow.inches` |
| [!DNL Day 1 Forecast Night Relative Humidity] | 1 日の天気予報。 夜間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day01Forecast.night.relativeHumidity` |
| [!DNL Day 1 Forecast Night Snow Range] | 1 日の天気予報。 夜間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day01Forecast.night.snowRange` |
| [!DNL Day 1 Forecast Night Temperature Celsius] | 1 日の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day01Forecast.night.temperature.celsius` |
| [!DNL Day 1 Forecast Night Temperature Fahrenheit] | 1 日の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day01Forecast.night.temperature.fahrenheit` |
| [!DNL  Day 1 Forecast Night Temperature Heat Index Celsius] | 1 日の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day01Forecast.night.temperatureHeatIndex.celsius` |
| [!DNL Day 1 Forecast Night Temperature Heat Index Fahrenheit] | 1 日の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day01Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!DNL Day 1 Forecast Night Temperature Wind Chill Celsius] | 1 日の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day01Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 1 日目予報夜の気温風冷華氏] | 1 日の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day01Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 1 日目予測夜雷指数] | 1 日の天気予報。 夜間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day01Forecast.night.thunderIndex` |
| [!UICONTROL 1 日目予測夜 UV 指数] | 1 日の天気予報。 夜間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day01Forecast.night.uvIndex` |
| [!UICONTROL 1 日目の予測夜風方向] | 1 日の天気予報。 夜間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day01Forecast.night.windDirection` |
| [!UICONTROL 1 日目予測夜風突風キロメートル/時] | 1 日の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day01Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 1 日目予測夜風突風マイル/時] | 1 日の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day01Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 1 日目予報夜風速キロメートル毎時] | 1 日の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day01Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 1 日目予測夜風速マイル/時] | 1 日の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day01Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 1 予測 QPF インチ] | 1 日の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day01Forecast.qpf.inches` |
| [!UICONTROL 日 1 予測 QPF ミリメートル] | 1 日の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day01Forecast.qpf.millimeters` |
| [!UICONTROL 1 日目予報 QPF 雪センチメートル] | 1 日の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day01Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 1 予測 QPF 雪インチ] | 1 日の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day01Forecast.qpfSnow.inches` |
| [!UICONTROL 日 2 予測カレンダ日温度最大摂氏] | 2 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day02Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日 2 予測カレンダ日温度最大華氏] | 2 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day02Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日 2 予測カレンダ日温度最小摂氏] | 2 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day02Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日 2 予測カレンダ日温度最小華氏] | 2 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day02Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 2 予測 Day Cloud のカバーマイル] | 2 日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day02Forecast.day.cloudCover.inches` |
| [!UICONTROL 2 日目予報日雲カバーキロメートル] | 2 日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day02Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 第 2 日予測日降水機会] | 2 日間の天気予報。 昼間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day02Forecast.day.precipChance` |
| [!UICONTROL 日 2 予測日の降水タイプ] | 2 日間の天気予報。 昼間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day02Forecast.day.precipType` |
| [!UICONTROL Day 2 予測日 QPF インチ] | 2 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day02Forecast.day.qpf.inches` |
| [!UICONTROL 日付 2 予測日付 QPF ミリメートル] | 2 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day02Forecast.day.qpf.millimeters` |
| [!UICONTROL 2 日目予報日 QPF 雪センチメートル] | 2 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day02Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 2 予測日 QPF 雪インチ] | 2 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day02Forecast.day.qpfSnow.inches` |
| [!UICONTROL 2 日目の予測日の相対湿度] | 2 日間の天気予報。 昼間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day02Forecast.day.relativeHumidity` |
| [!UICONTROL Day 2 予測日の雪範囲] | 2 日間の天気予報。 昼間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.day.snowRange` |
| [!UICONTROL 日 2 予測日温度摂氏] | 2 日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day02Forecast.day.temperature.celsius` |
| [!UICONTROL 日 2 予測日温度華氏] | 2 日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day02Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 日 2 予測日温熱指数摂氏] | 2 日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day02Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 日 2 予測日温度熱指数華氏] | 2 日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day02Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 日 2 予報日温度風冷摂氏] | 2 日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day02Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 日 2 予報日温度風冷華氏] | 2 日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day02Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 2 日目予測日雷指数] | 2 日間の天気予報。 昼間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day02Forecast.day.thunderIndex` |
| [!UICONTROL Day 2 予測日 UV 指数] | 2 日間の天気予報。 昼間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day02Forecast.day.uvIndex` |
| [!UICONTROL Day 2 予測日風の方向] | 2 日間の天気予報。 昼間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day02Forecast.day.windDirection` |
| [!UICONTROL 2 日目予報日風突風キロメートル/時] | 2 日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day02Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 2 日目予測日風突風マイル/時] | 2 日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day02Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 2 日目予報日風速キロメートル/時] | 2 日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day02Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 2 日目予測日風速マイル/時] | 2 日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day02Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 2 日目予測夜雲カバーマイル] | 2 日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day02Forecast.night.cloudCover.inches` |
| [!UICONTROL 2 日目予報ナイトクラウドカバーキロメートル] | 2 日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day02Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 2 日目：夜の降水の機会の予測] | 2 日間の天気予報。 夜間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day02Forecast.night.precipChance` |
| [!UICONTROL 第 2 日目予報夜間降水タイプ] | 2 日間の天気予報。 夜間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day02Forecast.night.precipType` |
| [!UICONTROL Day 2 予測夜 QPF インチ] | 2 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day02Forecast.night.qpf.inches` |
| [!UICONTROL 日 2 予報夜 QPF ミリメートル] | 2 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day02Forecast.night.qpf.millimeters` |
| [!UICONTROL 2 日目予報夜 QPF 雪センチメートル] | 2 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day02Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL Day 2 予測夜 QPF 雪インチ] | 2 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day02Forecast.night.qpfSnow.inches` |
| [!UICONTROL 2 日目の予報夜間相対湿度] | 2 日間の天気予報。 夜間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day02Forecast.night.relativeHumidity` |
| [!UICONTROL Day 2 予測夜雪範囲] | 2 日間の天気予報。 夜間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.night.snowRange` |
| [!UICONTROL 2 日目予測夜間気温摂氏] | 2 日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day02Forecast.night.temperature.celsius` |
| [!UICONTROL 2 日目予測夜温度華氏] | 2 日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day02Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 2 日目予測夜間気温熱指数摂氏] | 2 日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day02Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 2 日目予測夜間温度熱指数華氏] | 2 日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day02Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 2 日目予報夜の気温風の冷気摂氏] | 2 日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day02Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 2 日目予報夜の気温風冷華氏] | 2 日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day02Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 2 日目予測夜雷指数] | 2 日間の天気予報。 夜間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day02Forecast.night.thunderIndex` |
| [!UICONTROL 2 日目予測夜 UV 指数] | 2 日間の天気予報。 夜間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day02Forecast.night.uvIndex` |
| [!UICONTROL Day 2 予測夜風方向] | 2 日間の天気予報。 夜間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day02Forecast.night.windDirection` |
| [!UICONTROL 2 日目予報夜風突風キロメートル/時] | 2 日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day02Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 2 日目予測夜風突風マイル/時] | 2 日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day02Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 2 日目予報夜風速キロメートル/時] | 2 日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day02Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 2 日目予測夜風速マイル/時] | 2 日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day02Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 2 予測 QPF インチ] | 2 日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day02Forecast.qpf.inches` |
| [!UICONTROL 日 2 予測 QPF ミリメートル] | 2 日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day02Forecast.qpf.millimeters` |
| [!UICONTROL 2 日目予報 QPF 雪センチメートル] | 2 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day02Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 2 予測 QPF 雪インチ] | 2 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day02Forecast.qpfSnow.inches` |
| [!UICONTROL 日 3 予測カレンダ日温度最大摂氏] | 3 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day03Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日 3 予測カレンダ日温度最大華氏] | 3 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day03Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日 3 予測カレンダ日温度最小摂氏] | 3 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day03Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日 3 予測カレンダ日温度最小華氏] | 3 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day03Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 3 日目予測日クラウドカバーマイル] | 3 日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day03Forecast.day.cloudCover.inches` |
| [!UICONTROL 3 日目予報日雲カバーキロメートル] | 3 日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day03Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 3 日目予測日の降水機会] | 3 日間の天気予報。 昼間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day03Forecast.day.precipChance` |
| [!UICONTROL 日付 3 予測日の降水タイプ] | 3 日間の天気予報。 昼間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day03Forecast.day.precipType` |
| [!UICONTROL Day 3 予測日 QPF インチ] | 3 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day03Forecast.day.qpf.inches` |
| [!UICONTROL 日付 3 予測日付 QPF ミリメートル] | 3 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day03Forecast.day.qpf.millimeters` |
| [!UICONTROL 3 日目予報日 QPF 雪センチメートル] | 3 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day03Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 3 予測日 QPF 雪インチ] | 3 日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day03Forecast.day.qpfSnow.inches` |
| [!UICONTROL 3 日目の予測日の相対湿度] | 3 日間の天気予報。 昼間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day03Forecast.day.relativeHumidity` |
| [!UICONTROL Day 3 予測日の雪範囲] | 3 日間の天気予報。 昼間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.day.snowRange` |
| [!UICONTROL 日 3 予測日温度摂氏] | 3 日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day03Forecast.day.temperature.celsius` |
| [!UICONTROL 日 3 予測日温度華氏] | 3 日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day03Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 3 日目予測日温度熱指数摂氏] | 3 日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day03Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 3 日目予測日温度熱指数華氏] | 3 日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day03Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 日 3 予報日温度風冷摂氏] | 3 日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day03Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 3 日目予測日温度風冷華氏] | 3 日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day03Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 3 日目予測日雷指数] | 3 日間の天気予報。 昼間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day03Forecast.day.thunderIndex` |
| [!UICONTROL 3 日目の予測日の UV 指数] | 3 日間の天気予報。 昼間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day03Forecast.day.uvIndex` |
| [!UICONTROL Day 3 予測日風の方向] | 3 日間の天気予報。 昼間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day03Forecast.day.windDirection` |
| [!UICONTROL 3 日目予測日風突風キロメートル/時] | 3 日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day03Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 3 日目予測日風突風マイル/時] | 3 日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day03Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 3 日目予報日風速キロメートル/時] | 3 日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day03Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 3 日目予測日風速マイル/時] | 3 日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day03Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 3 日目予測夜雲カバーマイル] | 3 日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day03Forecast.night.cloudCover.inches` |
| [!UICONTROL 3 日目予報ナイトクラウドカバーキロメートル] | 3 日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day03Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 3 日目予測夜間降水の機会] | 3 日間の天気予報。 夜間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day03Forecast.night.precipChance` |
| [!UICONTROL 第 3 日目予報夜間降水量タイプ] | 3 日間の天気予報。 夜間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day03Forecast.night.precipType` |
| [!UICONTROL Day 3 予測夜 QPF インチ] | 3 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day03Forecast.night.qpf.inches` |
| [!UICONTROL 3 日目予測夜 QPF ミリメートル] | 3 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day03Forecast.night.qpf.millimeters` |
| [!UICONTROL 3 日目予報夜 QPF 雪センチメートル] | 3 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day03Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL 3 日目予報夜 QPF 雪インチ] | 3 日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day03Forecast.night.qpfSnow.inches` |
| [!UICONTROL 3 日目の予報夜間相対湿度] | 3 日間の天気予報。 夜間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day03Forecast.night.relativeHumidity` |
| [!UICONTROL 3 日目予報夜雪範囲] | 3 日間の天気予報。 夜間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.night.snowRange` |
| [!UICONTROL 3 日目予測夜間気温摂氏] | 3 日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day03Forecast.night.temperature.celsius` |
| [!UICONTROL 3 日目予測夜温度華氏] | 3 日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day03Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 3 日目予測夜間気温熱指数摂氏] | 3 日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day03Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 3 日目予測夜温度熱指数華氏] | 3 日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day03Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 3 日目予測夜温度風冷摂氏] | 3 日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day03Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 3 日目予報夜の気温風冷華氏] | 3 日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day03Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 3 日目予測夜雷指数] | 3 日間の天気予報。 夜間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day03Forecast.night.thunderIndex` |
| [!UICONTROL 3 日目予測夜 UV 指数] | 3 日間の天気予報。 夜間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day03Forecast.night.uvIndex` |
| [!UICONTROL 3 日目予測夜風方向] | 3 日間の天気予報。 夜間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day03Forecast.night.windDirection` |
| [!UICONTROL 3 日目予測夜風突風キロメートル/時] | 3 日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day03Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 3 日目予測夜風突風マイル/時] | 3 日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day03Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 3 日目予報夜風速キロメートル毎時] | 3 日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day03Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 3 日目予測夜風速マイル/時] | 3 日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day03Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL 3 日目予測 QPF インチ] | 3 日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day03Forecast.qpf.inches` |
| [!UICONTROL 3 日目予測 QPF ミリメートル] | 3 日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day03Forecast.qpf.millimeters` |
| [!UICONTROL 3 日目予報 QPF 雪センチメートル] | 3 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day03Forecast.qpfSnow.centimeters` |
| [!UICONTROL 3 日目予測 QPF 雪インチ] | 3 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day03Forecast.qpfSnow.inches` |
| [!UICONTROL 日付 5 予測カレンダ日温度最大摂氏] | 五日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day05Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日付 5 予測カレンダ日温度最大華氏] | 五日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day05Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日付 5 予測カレンダ日温度最小摂氏] | 五日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day05Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日付 5 予測カレンダ日温度最小華氏] | 五日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day05Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 5 予測 Day Cloud カバーマイル] | 五日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day05Forecast.day.cloudCover.inches` |
| [!UICONTROL 5 日目予報日雲カバーキロメートル] | 五日間の天気予報。 昼間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day05Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 第 5 日予測日の降水機会] | 五日間の天気予報。 昼間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day05Forecast.day.precipChance` |
| [!UICONTROL 日付 5 予測日の降水タイプ] | 五日間の天気予報。 昼間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day05Forecast.day.precipType` |
| [!UICONTROL Day 5 予測日 QPF インチ] | 五日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day05Forecast.day.qpf.inches` |
| [!UICONTROL 日付 5 予測日 QPF ミリメートル] | 五日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day05Forecast.day.qpf.millimeters` |
| [!UICONTROL 5 日目予報日 QPF 雪センチメートル] | 五日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day05Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 5 予測日 QPF 雪インチ] | 五日間の天気予報。 昼間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day05Forecast.day.qpfSnow.inches` |
| [!UICONTROL 5 日目の予測日の相対湿度] | 五日間の天気予報。 昼間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day05Forecast.day.relativeHumidity` |
| [!UICONTROL Day 5 予測日の雪範囲] | 五日間の天気予報。 昼間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.day.snowRange` |
| [!UICONTROL 日 5 予測日温度摂氏] | 五日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day05Forecast.day.temperature.celsius` |
| [!UICONTROL 日付 5 予測日温度華氏] | 五日間の天気予報。 昼間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day05Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 5 日目予測日温度熱指数摂氏] | 五日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day05Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 日 5 予測日温度熱指数華氏] | 五日間の天気予報。 昼間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day05Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 日 5 予報日温度風冷摂氏] | 五日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day05Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 日 5 予報日温度風冷華氏] | 五日間の天気予報。 昼間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day05Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 5 日目予測日雷指数] | 五日間の天気予報。 昼間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day05Forecast.day.thunderIndex` |
| [!UICONTROL Day 5 予測日の UV 指数] | 五日間の天気予報。 昼間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day05Forecast.day.uvIndex` |
| [!UICONTROL Day 5 予測日風の方向] | 五日間の天気予報。 昼間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day05Forecast.day.windDirection` |
| [!UICONTROL 5 日目予測日風突風キロメートル/時] | 五日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day05Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 5 日目予測日風突風マイル/時] | 五日間の天気予報。 昼間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day05Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 5 日目予報日風速キロメートル/時] | 五日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day05Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 5 日目予測日風速マイル/時] | 五日間の天気予報。 昼間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day05Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 5 日目予測夜雲カバーマイル] | 五日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day05Forecast.night.cloudCover.inches` |
| [!UICONTROL 5 日目予報ナイトクラウドカバーキロメートル] | 五日間の天気予報。 夜間の天気情報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day05Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 5 日目予測夜間降水の機会] | 五日間の天気予報。 夜間の天気情報。 降水の確率（パーセント）。 | `weather.forecast.day05Forecast.night.precipChance` |
| [!UICONTROL 5 日目予報夜間降水タイプ] | 五日間の天気予報。 夜間の天気情報。 降る可能性のある降水の形式（雨、雪、そりなど）。 | `weather.forecast.day05Forecast.night.precipType` |
| [!UICONTROL Day 5 予測夜 QPF インチ] | 五日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day05Forecast.night.qpf.inches` |
| [!UICONTROL 日 5 予報夜 QPF ミリメートル] | 五日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day05Forecast.night.qpf.millimeters` |
| [!UICONTROL 5 日目予報夜 QPF 雪センチメートル] | 五日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day05Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL Day 5 予測夜 QPF 雪インチ] | 五日間の天気予報。 夜間の天気情報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day05Forecast.night.qpfSnow.inches` |
| [!UICONTROL 5 日目の予報夜間相対湿度] | 五日間の天気予報。 夜間の天気情報。 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な蒸気の量との比を表します。 相対湿度は常にパーセンテージで表されます。<br>範囲 — 0 ～ 100。 | `weather.forecast.day05Forecast.night.relativeHumidity` |
| [!UICONTROL 5 日目予報夜雪範囲] | 五日間の天気予報。 夜間の天気情報。 潜在的な降雪のバケット（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.night.snowRange` |
| [!UICONTROL 5 日目予測夜間気温摂氏] | 五日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 摂氏温度 | `weather.forecast.day05Forecast.night.temperature.celsius` |
| [!UICONTROL 5 日目予測夜温度華氏] | 五日間の天気予報。 夜間の天気情報。 定義された測定単位での温度。 -140 ～ 140 の範囲で指定します。 温度（華氏） | `weather.forecast.day05Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 5 日目予測夜間気温熱指数摂氏] | 五日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 摂氏温度 | `weather.forecast.day05Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 5 日目予測夜間温度熱指数華氏] | 五日間の天気予報。 夜間の天気情報。 温度と湿度に基づいて人に触れる感じの温度。 温度（華氏） | `weather.forecast.day05Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 5 日目予測夜温度風冷摂氏] | 五日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 摂氏温度 | `weather.forecast.day05Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 5 日目予報夜の気温風冷華氏] | 五日間の天気予報。 夜間の天気情報。 温度と風速に基づいて人に触れるように感じる温度。 温度（華氏） | `weather.forecast.day05Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 5 日目予測夜雷指数] | 五日間の天気予報。 夜間の天気情報。 ある地域に影響を与える雷雨の確率の指標。 0 ( 雷なし 5 （激しい雷雨の高リスク）。 | `weather.forecast.day05Forecast.night.thunderIndex` |
| [!UICONTROL 5 日目予測夜 UV 指数] | 五日間の天気予報。 夜間の天気情報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day05Forecast.night.uvIndex` |
| [!UICONTROL Day 5 予測夜風方向] | 五日間の天気予報。 夜間の天気情報。 風が吹く磁気風の方向は、度単位で表されます。 磁気の方向は 0 度から 359 度まで異なります。0°は北を表し、90°は東を表し、180°は南を表し、270°は西を表します。<br>範囲 — 0&lt;=wind_dire_deg&lt;=350、10 度間隔。 | `weather.forecast.day05Forecast.night.windDirection` |
| [!UICONTROL 5 日目予測夜風突風キロメートル/時] | 五日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速キロメートルでの風速 | `weather.forecast.day05Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 5 日目予測夜風突風マイル/時] | 五日間の天気予報。 夜間の天気情報。 このデータフィールドには、平均風速の急激な変化と一時的な変化に関する情報が含まれます。 このレポートは、常に観測期間中に記録された最大風速を示します。 [ 風速 ] が表示されている場合は、必須の表示フィールドです。 突風の速度は、時速マイルまたはキロメートルで表すことができます。 時速マイルでの風速 | `weather.forecast.day05Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 5 日目予報夜風速キロメートル毎時] | 五日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day05Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 5 日目予測夜風速マイル/時] | 五日間の天気予報。 夜間の天気情報。 風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 現在の状況で報告される風の情報は、10 分の平均値である持続風速と呼ばれる。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day05Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 5 予測 QPF インチ] | 五日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（インチ） | `weather.forecast.day05Forecast.qpf.inches` |
| [!UICONTROL 日付 5 予測 QPF ミリメートル] | 五日間の天気予報。 12 時間または 24 時間の間に測定可能な沈殿物（液体または液体の等価物）を予測した。 ミリメートル単位で測定します。 降水量（ミリ） | `weather.forecast.day05Forecast.qpf.millimeters` |
| [!UICONTROL 5 日目予報 QPF 雪センチメートル] | 五日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day05Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 5 予測 QPF 雪インチ] | 五日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day05Forecast.qpfSnow.inches` |
| [!UICONTROL 日付 7 予測カレンダ日温度最大摂氏] | 7 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day07Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日付 7 予測カレンダ日温度最大華氏] | 7 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day07Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日付 7 予測カレンダ日温度最小摂氏] | 7 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day07Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日付 7 予測カレンダ日温度最小華氏] | 7 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day07Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 7 予測クラウドカバーマイル] | 7 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day07Forecast.cloudCover.inches` |
| [!UICONTROL 7 日目予報雲カバーキロメートル] | 7 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day07Forecast.cloudCover.kilometers` |
| [!UICONTROL Day 7 予測 QPF インチ] | 7 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（インチ） | `weather.forecast.day07Forecast.qpf.inches` |
| [!UICONTROL 7 日目予測 QPF ミリメートル] | 7 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（ミリ） | `weather.forecast.day07Forecast.qpf.millimeters` |
| [!UICONTROL 7 日目予報 QPF 雪センチメートル] | 7 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day07Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 7 予測 QPF 雪インチ] | 7 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day07Forecast.qpfSnow.inches` |
| [!UICONTROL 7 日目予測 UV 指数] | 7 日間の天気予報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day07Forecast.uvIndex` |
| [!UICONTROL Day 7 予測風向] | 7 日間の天気予報。 磁気表記の平均風向。 | `weather.forecast.day07Forecast.windDirection` |
| [!UICONTROL 7 日目予報風速キロメートル/時] | 7 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day07Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 7 日目の予測風速マイル/時] | 7 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day07Forecast.windSpeed.milesPerHour` |
| [!UICONTROL 日 10 予測カレンダ日温度最大摂氏] | 10 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day10Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日付 10 予測カレンダ日温度最大華氏] | 10 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day10Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日 10 予測カレンダ日温度最小摂氏] | 10 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day10Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日付 10 予測カレンダ日温度最小華氏] | 10 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day10Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 10 日目の予測クラウドカバーマイル] | 10 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day10Forecast.cloudCover.inches` |
| [!UICONTROL 10 日目予測雲カバーキロメートル] | 10 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day10Forecast.cloudCover.kilometers` |
| [!UICONTROL 10 日目予測 QPF インチ] | 10 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（インチ） | `weather.forecast.day10Forecast.qpf.inches` |
| [!UICONTROL 日付 10 予測 QPF ミリメートル] | 10 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（ミリ） | `weather.forecast.day10Forecast.qpf.millimeters` |
| [!UICONTROL 10 日目予報 QPF 雪センチメートル] | 10 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day10Forecast.qpfSnow.centimeters` |
| [!UICONTROL 10 日目予測 QPF 雪インチ] | 10 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day10Forecast.qpfSnow.inches` |
| [!UICONTROL 10 日目の予測 UV インデックス] | 10 日間の天気予報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day10Forecast.uvIndex` |
| [!UICONTROL 10 日目予測風の方向] | 10 日間の天気予報。 磁気表記の平均風向。 | `weather.forecast.day10Forecast.windDirection` |
| [!UICONTROL 10 日目予測風速キロメートル/時] | 10 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day10Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 10 日目予測風速マイル/時] | 10 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day10Forecast.windSpeed.milesPerHour` |
| [!UICONTROL 日 14 予測カレンダ日温度最大摂氏] | 14 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 摂氏温度 | `weather.forecast.day14Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 日 14 予測カレンダ日温度最大華氏] | 14 日間の天気予報。 指定した日の 1 日の午前 0 時から午前 0 時までの最大温度。 温度（華氏） | `weather.forecast.day14Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 日 14 予測カレンダ日温度最小摂氏] | 14 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 摂氏温度 | `weather.forecast.day14Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 日付 14 予測カレンダ日温度最小華氏] | 14 日間の天気予報。 指定した日の 1 日の最低気温（午前 0 時～午前 0 時）。 温度（華氏） | `weather.forecast.day14Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 14 日目予測クラウドカバーマイル] | 14 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（マイル単位）。 | `weather.forecast.day14Forecast.cloudCover.inches` |
| [!UICONTROL 14 日目予報雲カバーキロメートル] | 14 日間の天気予報。 日中の平均雲表示は、割合で表されます。 距離（キロ）。 | `weather.forecast.day14Forecast.cloudCover.kilometers` |
| 14 日目予測 QPF インチ | 14 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（インチ） | `weather.forecast.day14Forecast.qpf.inches` |
| [!UICONTROL 日付 14 予測 QPF ミリメートル] | 14 日間の天気予報。 24 時間の間に測定可能な沈殿物（液体または液体相当）を予測した。 降水量（ミリ） | `weather.forecast.day14Forecast.qpf.millimeters` |
| [!UICONTROL 14 日目予報 QPF 雪センチメートル] | 14 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 センチメートル単位の降雪 | `weather.forecast.day14Forecast.qpfSnow.centimeters` |
| [!UICONTROL 14 日目予測 QPF 雪インチ] | 14 日間の天気予報。 12 時間または 24 時間の予測期間中の雪としての測定可能な降水量を予測した。 センチ単位で測定します。 インチ単位の降雪 | `weather.forecast.day14Forecast.qpfSnow.inches` |
| [!UICONTROL 14 日目予測 UV 指数] | 14 日間の天気予報。 12 時間の予測期間の最大 UV インデックス。 | `weather.forecast.day14Forecast.uvIndex` |
| 14 日目の予測風の方向 | 14 日間の天気予報。 磁気表記の平均風向。 | `weather.forecast.day14Forecast.windDirection` |
| [!UICONTROL 14 日目予報風速キロメートル/時] | 14 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速キロメートルでの風速 | `weather.forecast.day14Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 14 日目予測風速マイル/時] | 14 日間の天気予報。 12 時間の予想期間における最大持続風速の予測。<br>風はベクトルとして扱われ、したがって、風は方向と大きさ（速度）を持たなければなりません。 この予報は、10 分間の平均値を示している。 風速の突然または短いバリエーションは、「風の吹き出し」と呼ばれ、別のデータフィールドで報告されます。 風の向きは、常に「風が吹く場所」と表現され、北から南へ北の風が吹くことを意味します。 北の風で北に向かえば、風はあなたの顔に向かいます。 南に向かい、北風があなたの背中にあります。 時速マイルでの風速 | `weather.forecast.day14Forecast.windSpeed.milesPerHour` |

## Triggers {#triggers}

トリガーは、様々な入力に基づいて意味的な気象条件を定義します。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL 製品トリガー] | 特定のカテゴリの製品の売り上げを促進するのに適した条件を示します。  これらは整数 ID にマッピングされます。 完全なリストは、IBMから取得できます。 | `weather.productTriggers` |
| [!UICONTROL 相対トリガー] | 人間の知覚に基づく相対的な条件。 暑いとか寒いとか。 これらは整数 ID にマッピングされます。 完全なリストは、IBMから取得できます。 | `weather.relativeTriggers` |
| [!UICONTROL 厳しい天候トリガー] | ハリケーンや過剰な雨など、様々な厳しい気象条件を示します。 | `weather.severeTriggers` |