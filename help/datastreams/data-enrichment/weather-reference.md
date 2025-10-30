---
title: 気象データフィールドのマッピング
description: The Weather Channel との統合の一環として利用できる気象データフィールドの参照ページです。
exl-id: bc0f158b-f9d0-424a-aa21-953e8380473f
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '11147'
ht-degree: 99%

---

# 気象データフィールドのマッピング

アドビでは、[!DNL [The Weather Company]](https://www.ibm.com/weather) と提携して、データストリームを通じて収集されたデータに米国の気象のコンテキストを追加しました。このデータは、Experience Platform での分析、ターゲティングおよびセグメント化の作成に使用できます。

このページには、オーディエンスデータの強化に使用できるすべてのフィールドが一覧表示されています。

これらのフィールドは、フィールドグループに合わせて 3 つの異なるグループに分類されています。

* [**[!UICONTROL Current Weather]**](#current-weather)：位置に基づく、ユーザーの現在の気象条件。 これには、現在の気温、降水量、雲量などが含まれます。
* [**[!UICONTROL Forecasted Weather]**](#forecast)：予測には、ユーザー事業所の 1、2、3、5、7 および 10 日の予測が含まれます。
* [**[!UICONTROL Triggers]**](#triggers):トリガーは、様々な意味論的気象条件にマッピングする特定の組み合わせです。 天候トリガーは 3 種類あります。

   * **[!UICONTROL Relative Triggers]**：寒さや雨などの意味論的に意味のある条件。 これらの定義は、気候によって異なる場合があります。
   * **[!UICONTROL Product Triggers]**：様々な種類の商品の購入につながる条件。 例：気温が低くなる天気予報が出ると、レインコートが購入される可能性が高くなることがあります。
   * **[!UICONTROL Severe Weather Triggers]**：冬の暴風雨やハリケーンの警報などの厳しい気象警報。

## 現在の天気 {#current-weather}

所在地に基づく、ユーザーの現在の気象状況。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL Temperature Dew Point Celsius] | 大気が一定の圧力で冷却されて飽和状態に達する温度。露点は、大気の湿度の間接的な尺度でもあります。露点が気温を超えることはありません。露点と気温が等しい場合は、通常、雲または霧が発生します。気温と露点の値が近くなるほど、相対湿度が高くなります。範囲：-80～100（°F）または -62～37（°C）。°（摂氏）単位の気温 | `weather.current.temperatureDewPoint.celsius` |
| [!UICONTROL Temperature Dew Point Fahrenheit] | 大気が一定の圧力で冷却されて飽和状態に達する温度。露点は、大気の湿度の間接的な尺度でもあります。露点が気温を超えることはありません。露点と気温が等しい場合は、通常、雲または霧が発生します。気温と露点の値が近くなるほど、相対湿度が高くなります。範囲：-80～100（°F）または -62～37（°C）。°（華氏）単位の気温 | `weather.current.temperatureDewPoint.fahrenheit` |
| [!UICONTROL Precipitation Last Hour Inches] | 直前 1 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip1Hour.inches` |
| [!UICONTROL Precipitation Last Hour Millimeters] | 直前 1 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip1Hour.millimeters` |
| [!DNL Precipitation Last 24 Hours Inches] | 直前 24 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip24Hour.inches` |
| [!UICONTROL Precipitation Last 24 Hours Millimeters] | 直前 24 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip24Hour.millimeters` |
| [!UICONTROL Precipitation Last 6 Hours Inches] | 6 時間周期の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip6Hour.inches` |
| [!UICONTROL Precipitation Last 6 Hours Millimeters] | 6 時間周期の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip6Hour.millimeters` |
| [!UICONTROL Pressure Change] | 過去 3 時間の気圧変化（ミリバール）。 | `weather.current.pressureChange` |
| [!UICONTROL Pressure Mean Sea Level] | 平均海面気圧（ミリバール）。つまり海面での平均気圧です。<br>範囲：ミリバールの精度は 0.1 mb です。 | `weather.current.pressureMeanSeaLevel` |
| [!UICONTROL Relative Humidity] | 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.current.relativeHumidity` |
| [!UICONTROL Snow Last Hour Centimeters] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow1Hour.centimeters` |
| [!UICONTROL Snow Last Hour Inches] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow1Hour.inches` |
| [!UICONTROL Snow 24 Hour Centimeters] | 24 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow24Hour.centimeters` |
| 24 時間の降雪量（インチ） | 24 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow24Hour.inches` |
| [!UICONTROL Snow Last 6 Hours Centimeters] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow6Hour.centimeters` |
| [!UICONTROL Snow Last 6 Hours Inches] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow6Hour.inches` |
| [!UICONTROL Sunset Time] | 日没時刻（UTC）。 | `weather.current.sunsetTime` |
| [!UICONTROL Temperature Celsius] | 定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.current.temperature.celsius` |
| [!UICONTROL Temperature Fahrenheit] | 定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.current.temperature.fahrenheit` |
| [!UICONTROL Temperature Change 24 hour Celsius] | 24 時間前のレポートと比較した気温の変化。°（摂氏）単位の気温 | `weather.current.temperatureChange24Hour.celsius` |
| [!UICONTROL Temperature Change 24 hour Fahrenheit] | 24 時間前のレポートと比較した気温の変化。°（華氏）単位の気温 | `weather.current.temperatureChange24Hour.fahrenheit` |
| [!UICONTROL Temperature Feels Like Celsius] | 体感温度。風冷指数や暑さ指数の複合的な影響により、露出した人間の肌にとって、気温が「どのように感じられるか」を表したものでます。<br>気温が 65 ° F 以上の場合、体感値は計算された暑さ指数で表します。気温が 65 ° F 未満の場合、体感値は計算された風の冷たさで表します。<br>範囲：-140～140。°（摂氏）単位の気温 | `weather.current.temperatureFeelsLike.celsius` |
| [!UICONTROL Temperature Feels Like Fahrenheit] | 体感温度。風冷指数や暑さ指数の複合的な影響により、露出した人間の肌にとって、気温が「どのように感じられるか」を表したものでます。<br>気温が 65 ° F 以上の場合、体感値は計算された暑さ指数で表します。気温が 65 ° F 未満の場合、体感値は計算された風の冷たさで表します。<br>範囲：-140～140。°（華氏）単位の気温 | `weather.current.temperatureFeelsLike.fahrenheit` |
| [!UICONTROL Temperature Max Since 7 AM Celsius] | 現地時間で午前 7 時以降の最高気温。°（摂氏）単位の気温 | `weather.current.temperatureMaxSince7Am.celsius` |
| [!UICONTROL Temperature Max Since 7 AM Fahrenheit] | 現地時間で午前 7 時以降の最高気温。°（華氏）単位の気温 | `weather.current.temperatureMaxSince7Am.fahrenheit` |
| [!UICONTROL Temperature Min Last 24 Hours Celsius] | 過去 24 時間の最低気温。24 時間とは、リクエスト時刻（現在）を基準とした期間です。°（摂氏）単位の気温 | `weather.current.temperatureMin24Hour.celsius` |
| [!UICONTROL Temperature Min Last 24 Hours Fahrenheit] | 過去 24 時間の最低気温。24 時間とは、リクエスト時刻（現在）を基準とした期間です。°（華氏）単位の気温 | `weather.current.temperatureMin24Hour.fahrenheit` |
| [!UICONTROL Wind Direction] | 風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.current.windDirection` |
| [!UICONTROL Wind Gust Kilometers per Hour] | このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.current.windGust.kilometersPerHour` |
| [!UICONTROL Wind Gust Miles per Hour] | このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.current.windGust.`milesPerHour |
| [!UICONTROL Wind Speed Kilometers per Hour] | 風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.current.windSpeed.kilometersPerHour` |
| [!UICONTROL Wind Speed Miles per Hour] | 風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.current.windSpeed.milesPerHour` |

## 予測された天気 {#forecast}

特定の時点でユーザーの所在地に基づいて予測された天候。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!DNL Day 1 Forecast Calendar Day Temperature Max Celsius] | 1 日の天気予報指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.calendarDayTemperatureMax.celsius` |
| [!DNL Day 1 Forecast Calendar Day Temperature Max Fahrenheit] | 1 日の天気予報指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!DNL Day 1 Forecast Calendar Day Temperature Min Celsius] | 1 日の天気予報指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.calendarDayTemperatureMin.celsius` |
| [!DNL Day 1 Forecast Calendar Day Temperature Min Fahrenheit] | 1 日の天気予報指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!DNL Day 1 Forecast Day Cloud Cover Miles] | 1 日の天気予報昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day01Forecast.day.cloudCover.inches` |
| [!DNL Day 1 Forecast Day Cloud Cover Kilometers] | 1 日の天気予報昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day01Forecast.day.cloudCover.kilometers` |
| [!DNL Day 1 Forecast Day Precipitation Chance] | 1 日の天気予報昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day01Forecast.day.precipChance` |
| [!DNL Day 1 Forecast Day Precipitation Type] | 1 日の天気予報昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day01Forecast.day.precipType` |
| [!DNL Day 1 Forecast Day QPF Inches] | 1 日の天気予報昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day01Forecast.day.qpf.inches` |
| [!DNL Day 1 Forecast Day QPF Millimeters] | 1 日の天気予報昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day01Forecast.day.qpf.millimeters` |
| [!DNL Day 1 Forecast Day QPF Snow Centimeters] | 1 日の天気予報昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day01Forecast.day.qpfSnow.centimeters` |
| [!DNL Day 1 Forecast Day QPF Snow Inches] | 1 日の天気予報昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day01Forecast.day.qpfSnow.inches` |
| [!DNL Day 1 Forecast Day Relative Humidity] | 1 日の天気予報昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day01Forecast.day.relativeHumidity` |
| [!DNL Day 1 Forecast Day Snow Range] | 1 日の天気予報昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day01Forecast.day.snowRange` |
| [!DNL Day 1 Forecast Day Temperature Celsius] | 1 日の天気予報昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.day.temperature.celsius` |
| [!DNL Day 1 Forecast Day Temperature Fahrenheit] | 1 日の天気予報昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day01Forecast.day.temperature.fahrenheit` |
| [!DNL Day 1 Forecast Day Temperature Heat Index Celsius] | 1 日の天気予報昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.day.temperatureHeatIndex.celsius` |
| [!DNL Day 1 Forecast Day Temperature Heat Index Fahrenheit] | 1 日の天気予報昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!DNL Day 1 Forecast Day Temperature Wind Chill Celsius] | 1 日の天気予報昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.day.temperatureWindChill.celsius` |
| [!DNL Day 1 Forecast Day Temperature Wind Chill Fahrenheit] | 1 日の天気予報昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.day.temperatureWindChill.fahrenheit` |
| [!DNL Day 1 Forecast Day Thunder Index] | 1 日の天気予報昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day01Forecast.day.thunderIndex` |
| [!DNL Day 1 Forecast Day UV Index] | 1 日の天気予報昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day01Forecast.day.uvIndex` |
| [!DNL Day 1 Forecast Day Wind Direction] | 1 日の天気予報昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day01Forecast.day.windDirection` |
| [!DNL Day 1 Forecast Day Wind Gust Kilometers per Hour] | 1 日の天気予報昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.day.windGust.kilometersPerHour` |
| [!DNL Day 1 Forecast Day Wind Gust Miles per Hour] | 1 日の天気予報昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day01Forecast.day.windGust.milesPerHour` |
| [!DNL Day 1 Forecast Day Wind Speed Kilometers per Hour] | 1 日の天気予報昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.day.windSpeed.kilometersPerHour` |
| [!DNL Day 1 Forecast Day Wind Speed Miles per Hour] | 1 日の天気予報昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day01Forecast.day.windSpeed.milesPerHour` |
| [!DNL Day 1 Forecast Night Cloud Cover Miles] | 1 日の天気予報夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day01Forecast.night.cloudCover.inches` |
| [!DNL Day 1 Forecast Night Cloud Cover Kilometers] | 1 日の天気予報夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day01Forecast.night.cloudCover.kilometers` |
| [!UICONTROL Day 1 Forecast Night Precipitation Chance] | 1 日の天気予報夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day01Forecast.night.precipChance` |
| [!DNL Day 1 Forecast Night Precipitation Type] | 1 日の天気予報夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day01Forecast.night.precipType` |
| [!DNL Day 1 Forecast Night QPF Inches] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day01Forecast.night.qpf.inches` |
| [!DNL Day 1 Forecast Night QPF Millimeters] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day01Forecast.night.qpf.millimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Centimeters] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day01Forecast.night.qpfSnow.centimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Inches] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day01Forecast.night.qpfSnow.inches` |
| [!DNL Day 1 Forecast Night Relative Humidity] | 1 日の天気予報夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day01Forecast.night.relativeHumidity` |
| [!DNL Day 1 Forecast Night Snow Range] | 1 日の天気予報夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day01Forecast.night.snowRange` |
| [!DNL Day 1 Forecast Night Temperature Celsius] | 1 日の天気予報夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperature.celsius` |
| [!DNL Day 1 Forecast Night Temperature Fahrenheit] | 1 日の天気予報夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperature.fahrenheit` |
| [!DNL  Day 1 Forecast Night Temperature Heat Index Celsius] | 1 日の天気予報夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureHeatIndex.celsius` |
| [!DNL Day 1 Forecast Night Temperature Heat Index Fahrenheit] | 1 日の天気予報夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!DNL Day 1 Forecast Night Temperature Wind Chill Celsius] | 1 日の天気予報夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL Day 1 Forecast Night Temperature Wind Chill Fahrenheit] | 1 日の天気予報夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 1 Forecast Night Thunder Index] | 1 日の天気予報夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day01Forecast.night.thunderIndex` |
| [!UICONTROL Day 1 Forecast Night UV Index] | 1 日の天気予報夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day01Forecast.night.uvIndex` |
| [!UICONTROL Day 1 Forecast Night Wind Direction] | 1 日の天気予報夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day01Forecast.night.windDirection` |
| [!UICONTROL Day 1 Forecast Night Wind Gust Kilometers per Hour] | 1 日の天気予報夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL Day 1 Forecast Night Wind Gust Miles per Hour] | 1 日の天気予報夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day01Forecast.night.windGust.milesPerHour` |
| [!UICONTROL Day 1 Forecast Night Wind Speed Kilometers per Hour] | 1 日の天気予報夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 1 Forecast Night Wind Speed Miles per Hour] | 1 日の天気予報夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day01Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 1 Forecast QPF Inches] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day01Forecast.qpf.inches` |
| [!UICONTROL Day 1 Forecast QPF Millimeters] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day01Forecast.qpf.millimeters` |
| [!UICONTROL Day 1 Forecast QPF Snow Centimeters] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day01Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 1 Forecast QPF Snow Inches] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day01Forecast.qpfSnow.inches` |
| [!UICONTROL Day 2 Forecast Calendar Day Temperature Max Celsius] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 2 Forecast Calendar Day Temperature Max Fahrenheit] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 2 Forecast Calendar Day Temperature Min Celsius] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 2 Forecast Calendar Day Temperature Min Fahrenheit] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 2 Forecast Day Cloud Cover Miles] | 2 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day02Forecast.day.cloudCover.inches` |
| [!UICONTROL Day 2 Forecast Day Cloud Cover Kilometers] | 2 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day02Forecast.day.cloudCover.kilometers` |
| [!UICONTROL Day 2 Forecast Day Precipitation Chance] | 2 日間の天気予報。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day02Forecast.day.precipChance` |
| [!UICONTROL Day 2 Forecast Day Precipitation Type] | 2 日間の天気予報。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day02Forecast.day.precipType` |
| [!UICONTROL Day 2 Forecast Day QPF Inches] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.day.qpf.inches` |
| [!UICONTROL Day 2 Forecast Day QPF Millimeters] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.day.qpf.millimeters` |
| [!UICONTROL Day 2 Forecast Day QPF Snow Centimeters] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 2 Forecast Day QPF Snow Inches] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.day.qpfSnow.inches` |
| [!UICONTROL Day 2 Forecast Day Relative Humidity] | 2 日間の天気予報。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day02Forecast.day.relativeHumidity` |
| [!UICONTROL Day 2 Forecast Day Snow Range] | 2 日間の天気予報。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.day.snowRange` |
| [!UICONTROL Day 2 Forecast Day Temperature Celsius] | 2 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperature.celsius` |
| [!UICONTROL Day 2 Forecast Day Temperature Fahrenheit] | 2 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperature.fahrenheit` |
| [!UICONTROL Day 2 Forecast Day Temperature Heat Index Celsius] | 2 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 2 Forecast Day Temperature Heat Index Fahrenheit] | 2 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 2 Forecast Day Temperature Wind Chill Celsius] | 2 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL Day 2 Forecast Day Temperature Wind Chill Fahrenheit] | 2 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 2 Forecast Day Thunder Index] | 2 日間の天気予報。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day02Forecast.day.thunderIndex` |
| [!UICONTROL Day 2 Forecast Day UV Index] | 2 日間の天気予報。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day02Forecast.day.uvIndex` |
| [!UICONTROL Day 2 Forecast Day Wind Direction] | 2 日間の天気予報。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day02Forecast.day.windDirection` |
| [!UICONTROL Day 2 Forecast Day Wind Gust Kilometers per Hour] | 2 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL Day 2 Forecast Day Wind Gust Miles per Hour] | 2 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day02Forecast.day.windGust.milesPerHour` |
| [!UICONTROL Day 2 Forecast Day Wind Speed Kilometers per Hour] | 2 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 2 Forecast Day Wind Speed Miles per Hour] | 2 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day02Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL Day 2 Forecast Night Cloud Cover Miles] | 2 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day02Forecast.night.cloudCover.inches` |
| [!UICONTROL Day 2 Forecast Night Cloud Cover Kilometers] | 2 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day02Forecast.night.cloudCover.kilometers` |
| [!UICONTROL Day 2 Forecast Night Precipitation Chance] | 2 日間の天気予報。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day02Forecast.night.precipChance` |
| [!UICONTROL Day 2 Forecast Night Precipitation Type] | 2 日間の天気予報。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day02Forecast.night.precipType` |
| [!UICONTROL Day 2 Forecast Night QPF Inches] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.night.qpf.inches` |
| [!UICONTROL Day 2 Forecast Night QPF Millimeters] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.night.qpf.millimeters` |
| [!UICONTROL Day 2 Forecast Night QPF Snow Centimeters] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL Day 2 Forecast Night QPF Snow Inches] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.night.qpfSnow.inches` |
| [!UICONTROL Day 2 Forecast Night Relative Humidity] | 2 日間の天気予報。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day02Forecast.night.relativeHumidity` |
| [!UICONTROL Day 2 Forecast Night Snow Range] | 2 日間の天気予報。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.night.snowRange` |
| [!UICONTROL Day 2 Forecast Night Temperature Celsius] | 2 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperature.celsius` |
| [!UICONTROL Day 2 Forecast Night Temperature Fahrenheit] | 2 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperature.fahrenheit` |
| [!UICONTROL Day 2 Forecast Night Temperature Heat Index Celsius] | 2 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 2 Forecast Night Temperature Heat Index Fahrenheit] | 2 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 2 Forecast Night Temperature Wind Chill Celsius] | 2 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL Day 2 Forecast Night Temperature Wind Chill Fahrenheit] | 2 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 2 Forecast Night Thunder Index] | 2 日間の天気予報。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day02Forecast.night.thunderIndex` |
| [!UICONTROL Day 2 Forecast Night UV Index] | 2 日間の天気予報。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day02Forecast.night.uvIndex` |
| [!UICONTROL Day 2 Forecast Night Wind Direction] | 2 日間の天気予報。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day02Forecast.night.windDirection` |
| [!UICONTROL Day 2 Forecast Night Wind Gust Kilometers per Hour] | 2 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL Day 2 Forecast Night Wind Gust Miles per Hour] | 2 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day02Forecast.night.windGust.milesPerHour` |
| [!UICONTROL Day 2 Forecast Night Wind Speed Kilometers per Hour] | 2 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 2 Forecast Night Wind Speed Miles per Hour] | 2 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day02Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 2 Forecast QPF Inches] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.qpf.inches` |
| [!UICONTROL Day 2 Forecast QPF Millimeters] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.qpf.millimeters` |
| [!UICONTROL Day 2 Forecast QPF Snow Centimeters] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 2 Forecast QPF Snow Inches] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.qpfSnow.inches` |
| [!UICONTROL Day 3 Forecast Calendar Day Temperature Max Celsius] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 3 Forecast Calendar Day Temperature Max Fahrenheit] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 3 Forecast Calendar Day Temperature Min Celsius] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 3 Forecast Calendar Day Temperature Min Fahrenheit] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 3 Forecast Day Cloud Cover Miles] | 3 日間の天候予測。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day03Forecast.day.cloudCover.inches` |
| [!UICONTROL Day 3 Forecast Day Cloud Cover Kilometers] | 3 日間の天候予測。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day03Forecast.day.cloudCover.kilometers` |
| [!UICONTROL Day 3 Forecast Day Precipitation Chance] | 3 日間の天候予測。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day03Forecast.day.precipChance` |
| [!UICONTROL Day 3 Forecast Day Precipitation Type] | 3 日間の天候予測。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day03Forecast.day.precipType` |
| [!UICONTROL Day 3 Forecast Day QPF Inches] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.day.qpf.inches` |
| [!UICONTROL Day 3 Forecast Day QPF Millimeters] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.day.qpf.millimeters` |
| [!UICONTROL Day 3 Forecast Day QPF Snow Centimeters] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 3 Forecast Day QPF Snow Inches] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.day.qpfSnow.inches` |
| [!UICONTROL Day 3 Forecast Day Relative Humidity] | 3 日間の天候予測。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day03Forecast.day.relativeHumidity` |
| [!UICONTROL Day 3 Forecast Day Snow Range] | 3 日間の天候予測。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.day.snowRange` |
| [!UICONTROL Day 3 Forecast Day Temperature Celsius] | 3 日間の天候予測。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperature.celsius` |
| [!UICONTROL Day 3 Forecast Day Temperature Fahrenheit] | 3 日間の天候予測。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperature.fahrenheit` |
| [!UICONTROL Day 3 Forecast Day Temperature Heat Index Celsius] | 3 日間の天候予測。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 3 Forecast Day Temperature Heat Index Fahrenheit] | 3 日間の天候予測。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 3 Forecast Day Temperature Wind Chill Celsius] | 3 日間の天候予測。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL Day 3 Forecast Day Temperature Wind Chill Fahrenheit] | 3 日間の天候予測。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 3 Forecast Day Thunder Index] | 3 日間の天候予測。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day03Forecast.day.thunderIndex` |
| [!UICONTROL Day 3 Forecast Day UV Index] | 3 日間の天候予測。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day03Forecast.day.uvIndex` |
| [!UICONTROL Day 3 Forecast Day Wind Direction] | 3 日間の天候予測。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day03Forecast.day.windDirection` |
| [!UICONTROL Day 3 Forecast Day Wind Gust Kilometers per Hour] | 3 日間の天候予測。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL Day 3 Forecast Day Wind Gust Miles per Hour] | 3 日間の天候予測。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day03Forecast.day.windGust.milesPerHour` |
| [!UICONTROL Day 3 Forecast Day Wind Speed Kilometers per Hour] | 3 日間の天候予測。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 3 Forecast Day Wind Speed Miles per Hour] | 3 日間の天候予測。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day03Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL Day 3 Forecast Night Cloud Cover Miles] | 3 日間の天候予測。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day03Forecast.night.cloudCover.inches` |
| [!UICONTROL Day 3 Forecast Night Cloud Cover Kilometers] | 3 日間の天候予測。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day03Forecast.night.cloudCover.kilometers` |
| [!UICONTROL Day 3 Forecast Night Precipitation Chance] | 3 日間の天候予測。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day03Forecast.night.precipChance` |
| [!UICONTROL Day 3 Forecast Night Precipitation Type] | 3 日間の天候予測。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day03Forecast.night.precipType` |
| [!UICONTROL Day 3 Forecast Night QPF Inches] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.night.qpf.inches` |
| [!UICONTROL Day 3 Forecast Night QPF Millimeters] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.night.qpf.millimeters` |
| [!UICONTROL Day 3 Forecast Night QPF Snow Centimeters] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL Day 3 Forecast Night QPF Snow Inches] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.night.qpfSnow.inches` |
| [!UICONTROL Day 3 Forecast Night Relative Humidity] | 3 日間の天候予測。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day03Forecast.night.relativeHumidity` |
| [!UICONTROL Day 3 Forecast Night Snow Range] | 3 日間の天候予測。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.night.snowRange` |
| [!UICONTROL Day 3 Forecast Night Temperature Celsius] | 3 日間の天候予測。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperature.celsius` |
| [!UICONTROL Day 3 Forecast Night Temperature Fahrenheit] | 3 日間の天候予測。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperature.fahrenheit` |
| [!UICONTROL Day 3 Forecast Night Temperature Heat Index Celsius] | 3 日間の天候予測。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 3 Forecast Night Temperature Heat Index Fahrenheit] | 3 日間の天候予測。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 3 Forecast Night Temperature Wind Chill Celsius] | 3 日間の天候予測。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL Day 3 Forecast Night Temperature Wind Chill Fahrenheit] | 3 日間の天候予測。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 3 Forecast Night Thunder Index] | 3 日間の天候予測。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day03Forecast.night.thunderIndex` |
| [!UICONTROL Day 3 Forecast Night UV Index] | 3 日間の天候予測。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day03Forecast.night.uvIndex` |
| [!UICONTROL Day 3 Forecast Night Wind Direction] | 3 日間の天候予測。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day03Forecast.night.windDirection` |
| [!UICONTROL Day 3 Forecast Night Wind Gust Kilometers per Hour] | 3 日間の天候予測。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL Day 3 Forecast Night Wind Gust Miles per Hour] | 3 日間の天候予測。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day03Forecast.night.windGust.milesPerHour` |
| [!UICONTROL Day 3 Forecast Night Wind Speed Kilometers per Hour] | 3 日間の天候予測。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 3 Forecast Night Wind Speed Miles per Hour] | 3 日間の天候予測。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day03Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 3 Forecast QPF Inches] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.qpf.inches` |
| [!UICONTROL Day 3 Forecast QPF Millimeters] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.qpf.millimeters` |
| [!UICONTROL Day 3 Forecast QPF Snow Centimeters] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 3 Forecast QPF Snow Inches] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.qpfSnow.inches` |
| [!UICONTROL Day 5 Forecast Calendar Day Temperature Max Celsius] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 5 Forecast Calendar Day Temperature Max Fahrenheit] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 5 Forecast Calendar Day Temperature Min Celsius] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 5 Forecast Calendar Day Temperature Min Fahrenheit] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 5 Forecast Day Cloud Cover Miles] | 5 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day05Forecast.day.cloudCover.inches` |
| [!UICONTROL Day 5 Forecast Day Cloud Cover Kilometers] | 5 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day05Forecast.day.cloudCover.kilometers` |
| [!UICONTROL Day 5 Forecast Day Precipitation Chance] | 5 日間の天気予報。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day05Forecast.day.precipChance` |
| [!UICONTROL Day 5 Forecast Day Precipitation Type] | 5 日間の天気予報。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day05Forecast.day.precipType` |
| [!UICONTROL Day 5 Forecast Day QPF Inches] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.day.qpf.inches` |
| [!UICONTROL Day 5 Forecast Day QPF Millimeters] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.day.qpf.millimeters` |
| [!UICONTROL Day 5 Forecast Day QPF Snow Centimeters] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL Day 5 Forecast Day QPF Snow Inches] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.day.qpfSnow.inches` |
| [!UICONTROL Day 5 Forecast Day Relative Humidity] | 5 日間の天気予報。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day05Forecast.day.relativeHumidity` |
| [!UICONTROL Day 5 Forecast Day Snow Range] | 5 日間の天気予報。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.day.snowRange` |
| [!UICONTROL Day 5 Forecast Day Temperature Celsius] | 5 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperature.celsius` |
| [!UICONTROL Day 5 Forecast Day Temperature Fahrenheit] | 5 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperature.fahrenheit` |
| [!UICONTROL Day 5 Forecast Day Temperature Heat Index Celsius] | 5 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 5 Forecast Day Temperature Heat Index Fahrenheit] | 5 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 5 Forecast Day Temperature Wind Chill Celsius] | 5 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL Day 5 Forecast Day Temperature Wind Chill Fahrenheit] | 5 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 5 Forecast Day Thunder Index] | 5 日間の天気予報。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day05Forecast.day.thunderIndex` |
| [!UICONTROL Day 5 Forecast Day UV Index] | 5 日間の天気予報。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day05Forecast.day.uvIndex` |
| [!UICONTROL Day 5 Forecast Day Wind Direction] | 5 日間の天気予報。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day05Forecast.day.windDirection` |
| [!UICONTROL Day 5 Forecast Day Wind Gust Kilometers per Hour] | 5 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL Day 5 Forecast Day Wind Gust Miles per Hour] | 5 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day05Forecast.day.windGust.milesPerHour` |
| [!UICONTROL Day 5 Forecast Day Wind Speed Kilometers per Hour] | 5 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 5 Forecast Day Wind Speed Miles per Hour] | 5 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day05Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL Day 5 Forecast Night Cloud Cover Miles] | 5 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day05Forecast.night.cloudCover.inches` |
| [!UICONTROL Day 5 Forecast Night Cloud Cover Kilometers] | 5 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day05Forecast.night.cloudCover.kilometers` |
| [!UICONTROL Day 5 Forecast Night Precipitation Chance] | 5 日間の天気予報。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day05Forecast.night.precipChance` |
| [!UICONTROL Day 5 Forecast Night Precipitation Type] | 5 日間の天気予報。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day05Forecast.night.precipType` |
| [!UICONTROL Day 5 Forecast Night QPF Inches] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.night.qpf.inches` |
| [!UICONTROL Day 5 Forecast Night QPF Millimeters] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.night.qpf.millimeters` |
| [!UICONTROL Day 5 Forecast Night QPF Snow Centimeters] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL Day 5 Forecast Night QPF Snow Inches] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.night.qpfSnow.inches` |
| [!UICONTROL Day 5 Forecast Night Relative Humidity] | 5 日間の天気予報。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day05Forecast.night.relativeHumidity` |
| [!UICONTROL Day 5 Forecast Night Snow Range] | 5 日間の天気予報。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.night.snowRange` |
| [!UICONTROL Day 5 Forecast Night Temperature Celsius] | 5 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperature.celsius` |
| [!UICONTROL Day 5 Forecast Night Temperature Fahrenheit] | 5 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperature.fahrenheit` |
| [!UICONTROL Day 5 Forecast Night Temperature Heat Index Celsius] | 5 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL Day 5 Forecast Night Temperature Heat Index Fahrenheit] | 5 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL Day 5 Forecast Night Temperature Wind Chill Celsius] | 5 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL Day 5 Forecast Night Temperature Wind Chill Fahrenheit] | 5 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL Day 5 Forecast Night Thunder Index] | 5 日間の天気予報。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day05Forecast.night.thunderIndex` |
| [!UICONTROL Day 5 Forecast Night UV Index] | 5 日間の天気予報。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day05Forecast.night.uvIndex` |
| [!UICONTROL Day 5 Forecast Night Wind Direction] | 5 日間の天気予報。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day05Forecast.night.windDirection` |
| [!UICONTROL Day 5 Forecast Night Wind Gust Kilometers per Hour] | 5 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL Day 5 Forecast Night Wind Gust Miles per Hour] | 5 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day05Forecast.night.windGust.milesPerHour` |
| [!UICONTROL Day 5 Forecast Night Wind Speed Kilometers per Hour] | 5 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 5 Forecast Night Wind Speed Miles per Hour] | 5 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day05Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL Day 5 Forecast QPF Inches] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.qpf.inches` |
| [!UICONTROL Day 5 Forecast QPF Millimeters] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.qpf.millimeters` |
| [!UICONTROL Day 5 Forecast QPF Snow Centimeters] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 5 Forecast QPF Snow Inches] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.qpfSnow.inches` |
| [!UICONTROL Day 7 Forecast Calendar Day Temperature Max Celsius] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 7 Forecast Calendar Day Temperature Max Fahrenheit] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 7 Forecast Calendar Day Temperature Min Celsius] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 7 Forecast Calendar Day Temperature Min Fahrenheit] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 7 Forecast Cloud Cover Miles] | 7 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day07Forecast.cloudCover.inches` |
| [!UICONTROL Day 7 Forecast Cloud Cover Kilometers] | 7 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day07Forecast.cloudCover.kilometers` |
| [!UICONTROL Day 7 Forecast QPF Inches] | 7 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day07Forecast.qpf.inches` |
| [!UICONTROL Day 7 Forecast QPF Millimeters] | 7 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day07Forecast.qpf.millimeters` |
| [!UICONTROL Day 7 Forecast QPF Snow Centimeters] | 7 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day07Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 7 Forecast QPF Snow Inches] | 7 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day07Forecast.qpfSnow.inches` |
| [!UICONTROL Day 7 Forecast UV Index] | 7 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day07Forecast.uvIndex` |
| [!UICONTROL Day 7 Forecast Wind Direction] | 7 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day07Forecast.windDirection` |
| [!UICONTROL Day 7 Forecast Wind Speed Kilometers per Hour] | 7 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day07Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 7 Forecast Wind Speed Miles per Hour] | 7 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day07Forecast.windSpeed.milesPerHour` |
| [!UICONTROL Day 10 Forecast Calendar Day Temperature Max Celsius] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 10 Forecast Calendar Day Temperature Max Fahrenheit] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 10 Forecast Calendar Day Temperature Min Celsius] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 10 Forecast Calendar Day Temperature Min Fahrenheit] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 10 Forecast Cloud Cover Miles] | 10 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day10Forecast.cloudCover.inches` |
| [!UICONTROL Day 10 Forecast Cloud Cover Kilometers] | 10 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day10Forecast.cloudCover.kilometers` |
| [!UICONTROL Day 10 Forecast QPF Inches] | 10 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day10Forecast.qpf.inches` |
| [!UICONTROL Day 10 Forecast QPF Millimeters] | 10 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day10Forecast.qpf.millimeters` |
| [!UICONTROL Day 10 Forecast QPF Snow Centimeters] | 10 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day10Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 10 Forecast QPF Snow Inches] | 10 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day10Forecast.qpfSnow.inches` |
| [!UICONTROL Day 10 Forecast UV Index] | 10 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day10Forecast.uvIndex` |
| [!UICONTROL Day 10 Forecast Wind Direction] | 10 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day10Forecast.windDirection` |
| [!UICONTROL Day 10 Forecast Wind Speed Kilometers per Hour] | 10 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day10Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 10 Forecast Wind Speed Miles per Hour] | 10 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day10Forecast.windSpeed.milesPerHour` |
| [!UICONTROL Day 14 Forecast Calendar Day Temperature Max Celsius] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL Day 14 Forecast Calendar Day Temperature Max Fahrenheit] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL Day 14 Forecast Calendar Day Temperature Min Celsius] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL Day 14 Forecast Calendar Day Temperature Min Fahrenheit] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL Day 14 Forecast Cloud Cover Miles] | 14 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day14Forecast.cloudCover.inches` |
| [!UICONTROL Day 14 Forecast Cloud Cover Kilometers] | 14 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day14Forecast.cloudCover.kilometers` |
| 14 日目予測 QPF（インチ） | 14 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day14Forecast.qpf.inches` |
| [!UICONTROL Day 14 Forecast QPF Millimeters] | 14 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day14Forecast.qpf.millimeters` |
| [!UICONTROL Day 14 Forecast QPF Snow Centimeters] | 14 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day14Forecast.qpfSnow.centimeters` |
| [!UICONTROL Day 14 Forecast QPF Snow Inches] | 14 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day14Forecast.qpfSnow.inches` |
| [!UICONTROL Day 14 Forecast UV Index] | 14 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day14Forecast.uvIndex` |
| 14 日目予測風向 | 14 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day14Forecast.windDirection` |
| [!UICONTROL Day 14 Forecast Wind Speed Kilometers per Hour] | 14 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day14Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL Day 14 Forecast Wind Speed Miles per Hour] | 14 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day14Forecast.windSpeed.milesPerHour` |

## トリガー {#triggers}

トリガーは、様々な入力に基づいて意味的な気象状況を定義します。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL Product Triggers] | 特定の製品カテゴリの売り上げを促進するのに適した条件を示します。これらは整数の ID にマッピングされます。完全なリストは IBM から取得できます。 | `weather.productTriggers` |
| [!UICONTROL Relative Triggers] | 人間の感覚に基づく相対的な条件です。暑いや寒いなど。これらは整数の ID にマッピングされます。完全なリストは IBM から取得できます。 | `weather.relativeTriggers` |
| [!UICONTROL Severe Weather Triggers] | 台風や豪雨など、様々な厳しい気象状況を示します。 | `weather.severeTriggers` |
