---
title: 気象データフィールドのマッピング
description: The Weather Channel との統合の一環として利用できる気象データフィールドの参照ページです。
exl-id: bc0f158b-f9d0-424a-aa21-953e8380473f
source-git-commit: e2122008fcae1016db03d6b5f56e4fa25520f9d0
workflow-type: ht
source-wordcount: '12238'
ht-degree: 100%

---

# 気象データフィールドのマッピング

アドビでは、[!DNL [The Weather Company]](https://www.ibm.com/weather) と提携して、データストリームを通じて収集されたデータに米国の気象のコンテキストを追加しました。このデータは、Experience Platform での分析、ターゲティングおよびセグメント化の作成に使用できます。

このページには、オーディエンスデータの強化に使用できるすべてのフィールドが一覧表示されています。

これらのフィールドは、フィールドグループに合わせて 3 つの異なるグループに分類されています。

* [**[!UICONTROL 現在の天気]**](#current-weather)：所在地に基づく、ユーザーの現在の気象状況。これには、現在の気温、降水量、雲量などが含まれます。
* [**[!UICONTROL 予測された天気]**](#forecast)：予測には、ユーザーの所在地の 1 日、2 日、3 日、5 日、7 日および 10 日間の予測が含まれます。
* [**[!UICONTROL トリガー]**](#triggers)：トリガーとは、異なる意味の気象状況にマッピングされる特定の組み合わせのことです。天候トリガーは 3 種類あります。

   * **[!UICONTROL 相対トリガー]**：意味的に重要な気象状況（寒波や雨天など）。これらの定義は、気候によって異なる場合があります。
   * **[!UICONTROL 製品トリガー]**：様々な種類の製品の購入につながる可能性のある状況。例：気温が低くなる天気予報が出ると、レインコートが売れる可能性が高くなることがあります。
   * **[!UICONTROL 荒天トリガー]**：荒天の警告（冬の嵐や台風の警告など）。

## 現在の天気 {#current-weather}

所在地に基づく、ユーザーの現在の気象状況。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL 気温露点（摂氏）] | 大気が一定の圧力で冷却されて飽和状態に達する温度。露点は、大気の湿度の間接的な尺度でもあります。露点が気温を超えることはありません。露点と気温が等しい場合は、通常、雲または霧が発生します。気温と露点の値が近くなるほど、相対湿度が高くなります。範囲：-80～100（°F）または -62～37（°C）。°（摂氏）単位の気温 | `weather.current.temperatureDewPoint.celsius` |
| [!UICONTROL 気温露点（華氏）] | 大気が一定の圧力で冷却されて飽和状態に達する温度。露点は、大気の湿度の間接的な尺度でもあります。露点が気温を超えることはありません。露点と気温が等しい場合は、通常、雲または霧が発生します。気温と露点の値が近くなるほど、相対湿度が高くなります。範囲：-80～100（°F）または -62～37（°C）。°（華氏）単位の気温 | `weather.current.temperatureDewPoint.fahrenheit` |
| [!UICONTROL 過去 1 時間の降水量（インチ）] | 直前 1 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip1Hour.inches` |
| [!UICONTROL 過去 1 時間の降水量（ミリメートル）] | 直前 1 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip1Hour.millimeters` |
| [!DNL Precipitation Last 24 Hours Inches] | 直前 24 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip24Hour.inches` |
| [!UICONTROL 過去 24 時間の降水量（ミリメートル）] | 直前 24 時間の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip24Hour.millimeters` |
| [!UICONTROL 過去 6 時間の降水量（インチ）] | 6 時間周期の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。インチ単位の降水量 | `weather.current.precip6Hour.inches` |
| [!UICONTROL 過去 6 時間の降水量（ミリメートル）] | 6 時間周期の降雨量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。ミリメートル単位の降水量 | `weather.current.precip6Hour.millimeters` |
| [!UICONTROL 気圧変化] | 過去 3 時間の気圧変化（ミリバール）。 | `weather.current.pressureChange` |
| [!UICONTROL 気圧平均海面レベル] | 平均海面気圧（ミリバール）。つまり海面での平均気圧です。<br>範囲：ミリバールの精度は 0.1 mb です。 | `weather.current.pressureMeanSeaLevel` |
| [!UICONTROL 相対湿度] | 空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.current.relativeHumidity` |
| [!UICONTROL 過去 1 時間の降雪量（センチメートル）] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow1Hour.centimeters` |
| [!UICONTROL 過去 1 時間の降雪量（インチ）] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow1Hour.inches` |
| [!UICONTROL 24 時間の降雪量（センチメートル）] | 24 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow24Hour.centimeters` |
| 24 時間の降雪量（インチ） | 24 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow24Hour.inches` |
| [!UICONTROL 過去 6 時間の降雪量（センチメートル）] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（センチメートル） | `weather.current.snow6Hour.centimeters` |
| [!UICONTROL 過去 6 時間の降雪量（インチ）] | 1 時間の降雪量。表示される量は、リクエスト時点（現在）から一定時間前までのものです。降雪量（インチ） | `weather.current.snow6Hour.inches` |
| [!UICONTROL 日没時刻] | 日没時刻（UTC）。 | `weather.current.sunsetTime` |
| [!UICONTROL 摂氏温度] | 定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.current.temperature.celsius` |
| [!UICONTROL 華氏温度] | 定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.current.temperature.fahrenheit` |
| [!UICONTROL 24 時間の気温変化（摂氏）] | 24 時間前のレポートと比較した気温の変化。°（摂氏）単位の気温 | `weather.current.temperatureChange24Hour.celsius` |
| [!UICONTROL 24 時間の気温変化（華氏）] | 24 時間前のレポートと比較した気温の変化。°（華氏）単位の気温 | `weather.current.temperatureChange24Hour.fahrenheit` |
| [!UICONTROL 体感温度（摂氏）] | 体感温度。風冷指数や暑さ指数の複合的な影響により、露出した人間の肌にとって、気温が「どのように感じられるか」を表したものでます。<br>気温が 65 ° F 以上の場合、体感値は計算された暑さ指数で表します。気温が 65 ° F 未満の場合、体感値は計算された風の冷たさで表します。<br>範囲：-140～140。°（摂氏）単位の気温 | `weather.current.temperatureFeelsLike.celsius` |
| [!UICONTROL 体感温度（華氏）] | 体感温度。風冷指数や暑さ指数の複合的な影響により、露出した人間の肌にとって、気温が「どのように感じられるか」を表したものでます。<br>気温が 65 ° F 以上の場合、体感値は計算された暑さ指数で表します。気温が 65 ° F 未満の場合、体感値は計算された風の冷たさで表します。<br>範囲：-140～140。°（華氏）単位の気温 | `weather.current.temperatureFeelsLike.fahrenheit` |
| [!UICONTROL 午前 7 時以降最高気温（摂氏）] | 現地時間で午前 7 時以降の最高気温。°（摂氏）単位の気温 | `weather.current.temperatureMaxSince7Am.celsius` |
| [!UICONTROL 午前 7 時以降の最高気温（華氏）] | 現地時間で午前 7 時以降の最高気温。°（華氏）単位の気温 | `weather.current.temperatureMaxSince7Am.fahrenheit` |
| [!UICONTROL 過去 24 時間最低気温（摂氏）] | 過去 24 時間の最低気温。24 時間とは、リクエスト時刻（現在）を基準とした期間です。°（摂氏）単位の気温 | `weather.current.temperatureMin24Hour.celsius` |
| [!UICONTROL 過去 24 時間最低気温（華氏）] | 過去 24 時間の最低気温。24 時間とは、リクエスト時刻（現在）を基準とした期間です。°（華氏）単位の気温 | `weather.current.temperatureMin24Hour.fahrenheit` |
| [!UICONTROL 風の向き] | 風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0 ≦ wind_dire_deg ≦ 350（10°間隔）。 | `weather.current.windDirection` |
| [!UICONTROL 突風速度（キロメートル毎時）] | このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.current.windGust.kilometersPerHour` |
| [!UICONTROL 突風速度（マイル毎時）] | このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.current.windGust.`milesPerHour |
| [!UICONTROL 風速（キロメートル毎時）] | 風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.current.windSpeed.kilometersPerHour` |
| [!UICONTROL 風速（マイル毎時）] | 風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.current.windSpeed.milesPerHour` |

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
| [!DNL Day 1 Forecast Day Wind Speed Miles per Hour] | 1 日の天気予報昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day01Forecast.day.windSpeed.milesPerHour ` |
| [!DNL Day 1 Forecast Night Cloud Cover Miles] | 1 日の天気予報夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day01Forecast.night.cloudCover.inches` |
| [!DNL Day 1 Forecast Night Cloud Cover Kilometers] | 1 日の天気予報夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day01Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 1 日目の予測夜間降水確率] | 1 日の天気予報夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day01Forecast.night.precipChance` |
| [!DNL Day 1 Forecast Night Precipitation Type] | 1 日の天気予報夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day01Forecast.night.precipType` |
| [!DNL Day 1 Forecast Night QPF Inches] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day01Forecast.night.qpf.inches` |
| [!DNL Day 1 Forecast Night QPF Millimeters] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day01Forecast.night.qpf.millimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Centimeters] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day01Forecast.night.qpfSnow.centimeters` |
| [!DNL Day 1 Forecast Night QPF Snow Inches] | 1 日の天気予報夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day01Forecast.night.qpfSnow.inches` |
| [!DNL Day 1 Forecast Night Relative Humidity] | 1 日の天気予報夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0 ～ 100。 | `weather.forecast.day01Forecast.night.relativeHumidity` |
| [!DNL Day 1 Forecast Night Snow Range] | 1 日の天気予報夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day01Forecast.night.snowRange` |
| [!DNL Day 1 Forecast Night Temperature Celsius] | 1 日の天気予報夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperature.celsius` |
| [!DNL Day 1 Forecast Night Temperature Fahrenheit] | 1 日の天気予報夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperature.fahrenheit` |
| [!DNL  Day 1 Forecast Night Temperature Heat Index Celsius] | 1 日の天気予報夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureHeatIndex.celsius` |
| [!DNL Day 1 Forecast Night Temperature Heat Index Fahrenheit] | 1 日の天気予報夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!DNL Day 1 Forecast Night Temperature Wind Chill Celsius] | 1 日の天気予報夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 1 日目予測夜間気温風冷指数（華氏）] | 1 日の天気予報夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day01Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 1 日目予測夜間雷インデックス] | 1 日の天気予報夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day01Forecast.night.thunderIndex` |
| [!UICONTROL 1 日目予測夜間 UV インデックス] | 1 日の天気予報夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day01Forecast.night.uvIndex` |
| [!UICONTROL 1 日目予測夜間風向] | 1 日の天気予報夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day01Forecast.night.windDirection` |
| [!UICONTROL 1 日目予測夜間突風（キロメートル毎時）] | 1 日の天気予報夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 1 日目予測夜間突風（マイル毎時）] | 1 日の天気予報夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day01Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 1 日目予測夜間風速（キロメートル毎時）] | 1 日の天気予報夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day01Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 1 日目予測夜間風速（マイル毎時）] | 1 日の天気予報夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day01Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL 1 日目予測 QPF（インチ）] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day01Forecast.qpf.inches` |
| [!UICONTROL 1 日目予測 QPF（ミリメートル）] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day01Forecast.qpf.millimeters` |
| [!UICONTROL 1 日目予測 QPF 降雪量（センチメートル）] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day01Forecast.qpfSnow.centimeters` |
| [!UICONTROL 1 日目予測 QPF 降雪量（インチ）] | 1 日の天気予報12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day01Forecast.qpfSnow.inches` |
| [!UICONTROL 2 日目予測暦日最高気温（摂氏）] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 2 日目予測暦日最高気温（華氏）] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 2 日目予測暦日最低気温（摂氏）] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 2 日目予測暦日最低気温（華氏）] | 2 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 2 日目予測昼間雲量（マイル）] | 2 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day02Forecast.day.cloudCover.inches` |
| [!UICONTROL 2 日目予測昼間雲量（キロメートル）] | 2 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day02Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 2 日目予測昼間降水確率] | 2 日間の天気予報。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day02Forecast.day.precipChance` |
| [!UICONTROL 2 日目予測昼間降水種別] | 2 日間の天気予報。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day02Forecast.day.precipType` |
| [!UICONTROL 2 日目予測昼間 QPF（インチ）] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.day.qpf.inches` |
| [!UICONTROL 2 日目予測昼間 QPF（ミリメートル）] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.day.qpf.millimeters` |
| [!UICONTROL 2 日目予測昼間 QPF 降雪量（センチメートル）] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL 2 日目予測昼間 QPF 降雪（インチ）] | 2 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.day.qpfSnow.inches` |
| [!UICONTROL 2 日目予測昼間相対湿度] | 2 日間の天気予報。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day02Forecast.day.relativeHumidity` |
| [!UICONTROL 2 日目予測昼間降雪範囲] | 2 日間の天気予報。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.day.snowRange` |
| [!UICONTROL 2 日目予測昼間気温（摂氏）] | 2 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperature.celsius` |
| [!UICONTROL 2 日目予測昼間気温（華氏）] | 2 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 2 日目予測昼間気温暑さ指数（摂氏）] | 2 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 2 日目予測昼間気温暑さ指数（華氏）] | 2 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 2 日目予測昼間気温風冷指数（摂氏）] | 2 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 2 日目予測昼間気温風冷指数（華氏）] | 2 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 2 日目予測昼間雷インデックス] | 2 日間の天気予報。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day02Forecast.day.thunderIndex` |
| [!UICONTROL 2 日目予測昼間 UV インデックス] | 2 日間の天気予報。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day02Forecast.day.uvIndex` |
| [!UICONTROL 2 日目予測昼間風向] | 2 日間の天気予報。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day02Forecast.day.windDirection` |
| [!UICONTROL 2 日目予測昼間突風（キロメートル毎時）] | 2 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 2 日目予測昼間突風（マイル毎時）] | 2 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day02Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 2 日目予測昼間風速（キロメートル毎時）] | 2 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 2 日目予測昼間風速（マイル毎時）] | 2 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day02Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 2 日目予測夜間雲量（マイル）] | 2 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day02Forecast.night.cloudCover.inches` |
| [!UICONTROL 2 日目予測夜間雲量（キロメートル）] | 2 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day02Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 2 日目予測夜間降水可能性] | 2 日間の天気予報。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day02Forecast.night.precipChance` |
| [!UICONTROL 2 日目予測夜間降水種別] | 2 日間の天気予報。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day02Forecast.night.precipType` |
| [!UICONTROL 2 日目予測夜間 QPF（インチ）] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.night.qpf.inches` |
| [!UICONTROL 2 日目予測夜間 QPF（ミリメートル）] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.night.qpf.millimeters` |
| [!UICONTROL 2 日目予測夜間 QPF 降雪（センチメートル）] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL 2 日目予測夜間 QPF 降雪（インチ）] | 2 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.night.qpfSnow.inches` |
| [!UICONTROL 2 日目予測夜間相対湿度] | 2 日間の天気予報。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day02Forecast.night.relativeHumidity` |
| [!UICONTROL 2 日目予測夜間降雪範囲] | 2 日間の天気予報。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day02Forecast.night.snowRange` |
| [!UICONTROL 2 日目予測夜間気温（摂氏）] | 2 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperature.celsius` |
| [!UICONTROL 2 日目予測夜間気温（華氏）] | 2 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 2 日目予測夜間気温暑さ指数（摂氏）] | 2 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 2 日目予測夜間気温暑さ指数（華氏）] | 2 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 2 日目予測夜間気温風速冷却（摂氏）] | 2 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 2 日目予測夜間気温風速冷却（華氏）] | 2 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day02Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 2 日目予測夜間発雷指数] | 2 日間の天気予報。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day02Forecast.night.thunderIndex` |
| [!UICONTROL 2 日目予測夜間 UV 指数] | 2 日間の天気予報。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day02Forecast.night.uvIndex` |
| [!UICONTROL 2 日目予測夜間風向] | 2 日間の天気予報。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0 ≦ wind_dire_deg ≦ 350（10°間隔）。 | `weather.forecast.day02Forecast.night.windDirection` |
| [!UICONTROL 2 日目予測夜間突風（キロメートル毎時）] | 2 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 2 日目予測夜間突風（マイル毎時）] | 2 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day02Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 2 日目予測夜間風速（キロメートル毎時）] | 2 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day02Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 2 日目予測夜間風速（マイル毎時）] | 2 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day02Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL 2 日目予測 QPF（インチ）] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day02Forecast.qpf.inches` |
| [!UICONTROL 2 日目予測 QPF（ミリメートル）] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day02Forecast.qpf.millimeters` |
| [!UICONTROL 2 日目予測 QPF 降雪（センチメートル）] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day02Forecast.qpfSnow.centimeters` |
| [!UICONTROL 2 日目予測 QPF 降雪（インチ）] | 2 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day02Forecast.qpfSnow.inches` |
| [!UICONTROL 3 日目予測暦日最高気温（摂氏）] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 3 日目予測暦日最高気温（華氏）] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 3 日目予測暦日最低気温（摂氏）] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 3 日目予測暦日最低気温（華氏）] | 3 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 3 日目予測昼間雲量（マイル）] | 3 日間の天候予測。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day03Forecast.day.cloudCover.inches` |
| [!UICONTROL 3 日目予測昼間雲量（キロメートル）] | 3 日間の天候予測。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day03Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 3 日目予測昼間降水可能性] | 3 日間の天候予測。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day03Forecast.day.precipChance` |
| [!UICONTROL 3 日目予測昼間降水種別] | 3 日間の天候予測。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day03Forecast.day.precipType` |
| [!UICONTROL 3 日目予測昼間 QPF（インチ）] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.day.qpf.inches` |
| [!UICONTROL 3 日目予測昼間 QPF（ミリメートル）] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.day.qpf.millimeters` |
| [!UICONTROL 3 日目予測昼間 QPF 降雪（センチメートル）] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL 3 日目予測昼間 QPF 降雪量（インチ）] | 3 日間の天候予測。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.day.qpfSnow.inches` |
| [!UICONTROL 3 日目予測昼間相対湿度] | 3 日間の天候予測。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day03Forecast.day.relativeHumidity` |
| [!UICONTROL 3 日目予測昼間降雪範囲] | 3 日間の天候予測。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.day.snowRange` |
| [!UICONTROL 3 日目予測昼間気温（摂氏）] | 3 日間の天候予測。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperature.celsius` |
| [!UICONTROL 3 日目予測昼間気温（華氏）] | 3 日間の天候予測。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 3 日目予測昼間気温暑さ指数（摂氏）] | 3 日間の天候予測。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 3 日目予測昼間気温暑さ指数（華氏）] | 3 日間の天候予測。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 3 日目予測昼間気温風冷指数（摂氏）] | 3 日間の天候予測。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 3 日目予測昼間気温風冷指数（華氏）] | 3 日間の天候予測。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 3 日目予測昼間雷インデックス] | 3 日間の天候予測。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day03Forecast.day.thunderIndex` |
| [!UICONTROL 3 日目予測昼間 UV インデックス] | 3 日間の天候予測。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day03Forecast.day.uvIndex` |
| [!UICONTROL 3 日目予測昼間風向] | 3 日間の天候予測。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day03Forecast.day.windDirection` |
| [!UICONTROL 3 日目予測昼間突風（キロメートル毎時）] | 3 日間の天候予測。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 3 日目予測昼間突風（マイル毎時）] | 3 日間の天候予測。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day03Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 3 日目予測昼間風速（キロメートル毎時）] | 3 日間の天候予測。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 3 日目予測昼間風速（マイル毎時）] | 3 日間の天候予測。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day03Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 3 日目予測夜間雲量（マイル）] | 3 日間の天候予測。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day03Forecast.night.cloudCover.inches` |
| [!UICONTROL 3 日目予測夜間雲量（キロメートル）] | 3 日間の天候予測。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day03Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 3 日目予測夜間降水確率] | 3 日間の天候予測。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day03Forecast.night.precipChance` |
| [!UICONTROL 3 日目予測夜間降水種別] | 3 日間の天候予測。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day03Forecast.night.precipType` |
| [!UICONTROL 3 日目予測夜間 QPF（インチ）] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.night.qpf.inches` |
| [!UICONTROL 3 日目予測夜間 QPF（ミリメートル）] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.night.qpf.millimeters` |
| [!UICONTROL 3 日目予測夜間 QPF 降雪量（センチメートル）] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL 3 日目予測夜間 QPF 降雪量（インチ）] | 3 日間の天候予測。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.night.qpfSnow.inches` |
| [!UICONTROL 3 日目予測夜間相対湿度] | 3 日間の天候予測。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day03Forecast.night.relativeHumidity` |
| [!UICONTROL 3 日目予測夜間降雪範囲] | 3 日間の天候予測。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day03Forecast.night.snowRange` |
| [!UICONTROL 3 日目予測夜間気温（摂氏）] | 3 日間の天候予測。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperature.celsius` |
| [!UICONTROL 3 日目予測夜間気温（華氏）] | 3 日間の天候予測。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 3 日目予測夜間気温暑さ指数（摂氏）] | 3 日間の天候予測。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 3 日目予測夜間気温暑さ指数（華氏）] | 3 日間の天候予測。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 3 日目予測夜間気温風冷指数（摂氏）] | 3 日間の天候予測。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 3 日目予測夜間気温風冷指数（華氏）] | 3 日間の天候予測。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day03Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 3 日目予測夜間雷インデックス] | 3 日間の天候予測。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day03Forecast.night.thunderIndex` |
| [!UICONTROL 3 日目予測夜間 UV インデックス] | 3 日間の天候予測。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day03Forecast.night.uvIndex` |
| [!UICONTROL 3 日目予測夜間風向] | 3 日間の天候予測。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day03Forecast.night.windDirection` |
| [!UICONTROL 3 日目予測夜間突風（キロメートル毎時）] | 3 日間の天候予測。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 3 日目予測夜間突風（マイル毎時）] | 3 日間の天候予測。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day03Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 3 日目予測夜間風速（キロメートル毎時）] | 3 日間の天候予測。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day03Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 3 日目予測夜間風速（マイル毎時）] | 3 日間の天候予測。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day03Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL 3 日目予測 QPF（インチ）] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day03Forecast.qpf.inches` |
| [!UICONTROL 3 日目予測 QPF（ミリメートル）] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day03Forecast.qpf.millimeters` |
| [!UICONTROL 3 日目予測 QPF 降雪量（センチメートル）] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day03Forecast.qpfSnow.centimeters` |
| [!UICONTROL 3 日目予測 QPF 降雪量（インチ）] | 3 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day03Forecast.qpfSnow.inches` |
| [!UICONTROL 5 日目予測カレンダー日最高気温（摂氏）] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 5 日目予測カレンダー日最高気温（華氏）] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 5 日目予測カレンダ日最低気温（摂氏）] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 5 日目予測カレンダ日最低気温（華氏）] | 5 日間の天気予報。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 5 日目予測昼間雲量（マイル）] | 5 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day05Forecast.day.cloudCover.inches` |
| [!UICONTROL 5 日目予測昼間雲量（キロメートル）] | 5 日間の天気予報。昼間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day05Forecast.day.cloudCover.kilometers` |
| [!UICONTROL 5 日目予測昼間降水確率] | 5 日間の天気予報。昼間の気象情報。降水確率（パーセント）。 | `weather.forecast.day05Forecast.day.precipChance` |
| [!UICONTROL 5 日目予測昼間降水種別] | 5 日間の天気予報。昼間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day05Forecast.day.precipType` |
| [!UICONTROL 5 日目予測昼間 QPF（インチ）] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.day.qpf.inches` |
| [!UICONTROL 5 日目予測昼間 QPF（ミリメートル）] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.day.qpf.millimeters` |
| [!UICONTROL 5 日目予測昼間 QPF 降雪量（センチメートル）] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.day.qpfSnow.centimeters` |
| [!UICONTROL 5 日目予測昼間 QPF 降雪量（インチ）] | 5 日間の天気予報。昼間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.day.qpfSnow.inches` |
| [!UICONTROL 5 日目予測昼間相対湿度] | 5 日間の天気予報。昼間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day05Forecast.day.relativeHumidity` |
| [!UICONTROL 5 日目予測昼間降雪範囲] | 5 日間の天気予報。昼間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.day.snowRange` |
| [!UICONTROL 5 日目予測昼間気温（摂氏）] | 5 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperature.celsius` |
| [!UICONTROL 5 日目予測昼間気温（華氏）] | 5 日間の天気予報。昼間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperature.fahrenheit` |
| [!UICONTROL 5 日目予測昼間気温暑さ指数（摂氏）] | 5 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureHeatIndex.celsius` |
| [!UICONTROL 5 日目予測昼間気温暑さ指数（華氏）] | 5 日間の天気予報。昼間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 5 日目予測昼間気温風冷指数（摂氏）] | 5 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureWindChill.celsius` |
| [!UICONTROL 5 日目予測昼間気温冷風（華氏）] | 5 日間の天気予報。昼間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.day.temperatureWindChill.fahrenheit` |
| [!UICONTROL 5 日目予測昼間雷インデックス] | 5 日間の天気予報。昼間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day05Forecast.day.thunderIndex` |
| [!UICONTROL 5 日目予測昼間 UV インデックス] | 5 日間の天気予報。昼間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day05Forecast.day.uvIndex` |
| [!UICONTROL 5 日目予測昼間風向] | 5 日間の天気予報。昼間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day05Forecast.day.windDirection` |
| [!UICONTROL 5 日目予測昼間突風（キロメートル毎時）] | 5 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.day.windGust.kilometersPerHour` |
| [!UICONTROL 5 日目予測昼間突風（マイル毎時）] | 5 日間の天気予報。昼間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day05Forecast.day.windGust.milesPerHour` |
| [!UICONTROL 5 日目予測昼間風速（キロメートル毎時）] | 5 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.day.windSpeed.kilometersPerHour` |
| [!UICONTROL 5 日目予測昼間風速（マイル毎時）] | 5 日間の天気予報。昼間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day05Forecast.day.windSpeed.milesPerHour` |
| [!UICONTROL 5 日目予測夜間雲量（マイル）] | 5 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day05Forecast.night.cloudCover.inches` |
| [!UICONTROL 5 日目予測夜間雲量（キロメートル）] | 5 日間の天気予報。夜間の気象情報。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day05Forecast.night.cloudCover.kilometers` |
| [!UICONTROL 5 日目予測夜間降水確率] | 5 日間の天気予報。夜間の気象情報。降水確率（パーセント）。 | `weather.forecast.day05Forecast.night.precipChance` |
| [!UICONTROL 5 日目予測夜間降水種別] | 5 日間の天気予報。夜間の気象情報。可能性のある降水形態（雨、雪、みぞれなど）。 | `weather.forecast.day05Forecast.night.precipType` |
| [!UICONTROL 5 日目予測夜間 QPF（インチ）] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.night.qpf.inches` |
| [!UICONTROL 5 日目予測夜間 QPF（ミリメートル）] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.night.qpf.millimeters` |
| [!UICONTROL 5 日目予測夜間 QPF 降雪量（センチメートル）] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.night.qpfSnow.centimeters` |
| [!UICONTROL 5 日目予測夜間 QPF 降雪量（インチ）] | 5 日間の天気予報。夜間の気象情報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.night.qpfSnow.inches` |
| [!UICONTROL 5 日目予測夜間相対湿度] | 5 日間の天気予報。夜間の気象情報。空気の相対湿度。空気の中の水蒸気の量と、一定の温度で空気を飽和させるのに必要な水蒸気の量との比で定義されます。相対湿度は常にパーセンテージで表されます。<br>範囲：0～100。 | `weather.forecast.day05Forecast.night.relativeHumidity` |
| [!UICONTROL 5 日目予測夜間降雪範囲] | 5 日間の天気予報。夜間の気象情報。可能性のある降雪量（1～3 インチ、3～6 インチなど）。 | `weather.forecast.day05Forecast.night.snowRange` |
| [!UICONTROL 5 日目予測夜間気温（摂氏）] | 5 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperature.celsius` |
| [!UICONTROL 5 日目予測夜間気温（華氏）] | 5 日間の天気予報。夜間の気象情報。定義された測定単位での温度。範囲：-140～140。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperature.fahrenheit` |
| [!UICONTROL 5 日目予測夜間気温暑さ指数（摂氏）] | 5 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureHeatIndex.celsius` |
| [!UICONTROL 5 日目予測夜間温度暑さ指数（華氏）] | 5 日間の天気予報。夜間の気象情報。気温と湿度に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureHeatIndex.fahrenheit` |
| [!UICONTROL 5 日目予測夜間気温風冷指数（摂氏）] | 5 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（摂氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureWindChill.celsius` |
| [!UICONTROL 5 日目予測夜間気温風冷指数（華氏）] | 5 日間の天気予報。夜間の気象情報。気温と風速に基づいて人が体感する気温。°（華氏）単位の気温 | `weather.forecast.day05Forecast.night.temperatureWindChill.fahrenheit` |
| [!UICONTROL 5 日目予測夜間雷インデックス] | 5 日間の天気予報。夜間の気象情報。ある地域に影響を与える雷雨の確率のインデックス。0（雷なし）～ 5（激しい雷雨の高いリスク）。 | `weather.forecast.day05Forecast.night.thunderIndex` |
| [!UICONTROL 5 日目予測夜間 UV インデックス] | 5 日間の天気予報。夜間の気象情報。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day05Forecast.night.uvIndex` |
| [!UICONTROL 5 日目予測夜間風向] | 5 日間の天気予報。夜間の気象情報。風が吹いてくる方角の磁気方位は度単位で表されます。磁気方位は 0°から 359°の範囲で変化します。特に、0°は北、90°は東、180°は南、270°は西を表します。<br>範囲：0&lt;=wind_dire_deg&lt;=350 （10 度間隔）。 | `weather.forecast.day05Forecast.night.windDirection` |
| [!UICONTROL 5 日目予測夜間突風（キロメートル毎時）] | 5 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.night.windGust.kilometersPerHour` |
| [!UICONTROL 5 日目予測夜間突風（マイル毎時）] | 5 日間の天気予報。夜間の気象情報。このデータフィールドには、平均風速の突然の一時的な変化に関する情報が含まれています。このレポートは、常に観測期間中に記録された最大の突風速度を示します。「風速」が表示される場合は、必須の表示フィールドになります。突風の速度は、マイル毎時またはキロメートル毎時で表すことができます。マイル毎時単位の風速 | `weather.forecast.day05Forecast.night.windGust.milesPerHour` |
| [!UICONTROL 5 日目予測夜間風速（キロメートル毎時）] | 5 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day05Forecast.night.windSpeed.kilometersPerHour` |
| [!UICONTROL 5 日目予測夜間風速（マイル毎時）] | 5 日間の天気予報。夜間の気象情報。風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。現在の状況で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day05Forecast.night.windSpeed.milesPerHour` |
| [!UICONTROL 5 日目予測 QPF（インチ）] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。インチ単位の降水量 | `weather.forecast.day05Forecast.qpf.inches` |
| [!UICONTROL 5 日目予測 QPF（ミリメートル）] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位で測定されます。ミリメートル単位の降水量 | `weather.forecast.day05Forecast.qpf.millimeters` |
| [!UICONTROL 5 日目予測 QPF 降雪（センチメートル）] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day05Forecast.qpfSnow.centimeters` |
| [!UICONTROL 5 日目予測 QPF 降雪（インチ）] | 5 日間の天気予報。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day05Forecast.qpfSnow.inches` |
| [!UICONTROL 7 日目予測暦日最高気温（摂氏）] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 7 日目予測暦日最高気温（華氏）] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 7 日目予測暦日最低気温（摂氏）] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 7 日目予測暦日最低気温（華氏）] | 7 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day07Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 7 日目予測雲量（マイル）] | 7 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day07Forecast.cloudCover.inches` |
| [!UICONTROL 7 日目予測雲量（キロメートル）] | 7 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day07Forecast.cloudCover.kilometers` |
| [!UICONTROL 7 日目予測 QPF（インチ）] | 7 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day07Forecast.qpf.inches` |
| [!UICONTROL 7 日目予測 QPF（ミリメートル）] | 7 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day07Forecast.qpf.millimeters` |
| [!UICONTROL 7 日目予測 QPF 降雪（センチメートル）] | 7 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day07Forecast.qpfSnow.centimeters` |
| [!UICONTROL 7 日目予測 QPF 降雪（インチ）] | 7 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day07Forecast.qpfSnow.inches` |
| [!UICONTROL 7 日目予測 UV 指数] | 7 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day07Forecast.uvIndex` |
| [!UICONTROL 7 日目予測風向] | 7 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day07Forecast.windDirection` |
| [!UICONTROL 7 日目予測風速（キロメートル毎時）] | 7 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day07Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 7 日目予測風速（マイル毎時）] | 7 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day07Forecast.windSpeed.milesPerHour` |
| [!UICONTROL 10 日目予測暦日最高気温（摂氏）] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 10 日目予測暦日最高気温（華氏）] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 10 日目予測暦日最低気温（摂氏）] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 10 日目予測暦日最低気温（華氏）] | 10 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day10Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 10 日目予測雲量（マイル）] | 10 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day10Forecast.cloudCover.inches` |
| [!UICONTROL 10 日目予測雲量（キロメートル）] | 10 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day10Forecast.cloudCover.kilometers` |
| [!UICONTROL 10 日目予測 QPF（インチ）] | 10 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day10Forecast.qpf.inches` |
| [!UICONTROL 10 日目予測 QPF（ミリメートル）] | 10 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day10Forecast.qpf.millimeters` |
| [!UICONTROL 10 日目予測 QPF 降雪（センチメートル）] | 10 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day10Forecast.qpfSnow.centimeters` |
| [!UICONTROL 10 日目予測 QPF 降雪（インチ）] | 10 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day10Forecast.qpfSnow.inches` |
| [!UICONTROL 10 日目予測 UV 指数] | 10 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day10Forecast.uvIndex` |
| [!UICONTROL 10 日目予測風向] | 10 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day10Forecast.windDirection` |
| [!UICONTROL 10 日目予測風速（キロメートル毎時）] | 10 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day10Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 10 日目予測風速（マイル毎時）] | 10 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day10Forecast.windSpeed.milesPerHour` |
| [!UICONTROL 14 日目予測暦日最高気温（摂氏）] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（摂氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMax.celsius` |
| [!UICONTROL 14 日目予測暦日最高気温（華氏）] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最高気温。°（華氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMax.fahrenheit` |
| [!UICONTROL 14 日目予測暦日最低気温（摂氏）] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（摂氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMin.celsius` |
| [!UICONTROL 14 日目予測暦日最低気温（華氏）] | 14 日間の天候予測。指定日の 1 日（午前零時から翌日の午前零時まで）の最低気温。°（華氏）単位の気温 | `weather.forecast.day14Forecast.calendarDayTemperatureMin.fahrenheit` |
| [!UICONTROL 14 日目予測雲量（マイル）] | 14 日間の天候予測。パーセントで表された昼間平均雲量。マイル単位の距離。 | `weather.forecast.day14Forecast.cloudCover.inches` |
| [!UICONTROL 14 日目予測雲量（キロメートル）] | 14 日間の天候予測。パーセントで表された昼間平均雲量。キロメートル単位の距離。 | `weather.forecast.day14Forecast.cloudCover.kilometers` |
| 14 日目予測 QPF（インチ） | 14 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。インチ単位の降水量 | `weather.forecast.day14Forecast.qpf.inches` |
| [!UICONTROL 14 日目予測 QPF（ミリメートル）] | 14 日間の天候予測。24 時間の間に予測される測定可能な降水量（液体または液体と同等のもの）。ミリメートル単位の降水量 | `weather.forecast.day14Forecast.qpf.millimeters` |
| [!UICONTROL 14 日目予測 QPF 降雪（センチメートル）] | 14 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（センチメートル） | `weather.forecast.day14Forecast.qpfSnow.centimeters` |
| [!UICONTROL 14 日目予測 QPF 降雪（インチ）] | 14 日間の天候予測。12 時間または 24 時間の間に予測される測定可能な降雪量。センチメートル単位で測定されます。降雪量（インチ） | `weather.forecast.day14Forecast.qpfSnow.inches` |
| [!UICONTROL 14 日目予測 UV 指数] | 14 日間の天候予測。予測期間 12 時間の最大 UV インデックス。 | `weather.forecast.day14Forecast.uvIndex` |
| 14 日目予測風向 | 14 日間の天候予測。磁気方位で表された平均風向。 | `weather.forecast.day14Forecast.windDirection` |
| [!UICONTROL 14 日目予測風速（キロメートル毎時）] | 14 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。キロメートル毎時単位の風速 | `weather.forecast.day14Forecast.windSpeed.kilometersPerHour` |
| [!UICONTROL 14 日目予測風速（マイル毎時）] | 14 日間の天候予測。12 時間の予測期間における最大持続風速の予測。<br>風はベクトルとして扱われるので、風には方向と大きさ（速度）が必要です。予測で報告される風の情報は、持続風速と呼ばれる 10 分間平均値に相当します。風速の突然または短時間の変化は「突風」と呼ばれ、別個のデータフィールドで報告されます。風向は、常に「どちらから風が吹いてくるか」で表されます。つまり、北風は北から南へ吹く風ということです。北風のときに北を向くと、風は顔に当たります。同様に、北風のときに南を向くと、風は背中に当たります。マイル毎時単位の風速 | `weather.forecast.day14Forecast.windSpeed.milesPerHour` |

## トリガー {#triggers}

トリガーは、様々な入力に基づいて意味的な気象状況を定義します。

| フィールド | 説明 | XDM パス |
| --- | ---- | --- |
| [!UICONTROL 製品トリガー] | 特定の製品カテゴリの売り上げを促進するのに適した条件を示します。これらは整数の ID にマッピングされます。完全なリストは IBM から取得できます。 | `weather.productTriggers` |
| [!UICONTROL 相対トリガー] | 人間の感覚に基づく相対的な条件です。暑いや寒いなど。これらは整数の ID にマッピングされます。完全なリストは IBM から取得できます。 | `weather.relativeTriggers` |
| [!UICONTROL 荒天トリガー] | 台風や豪雨など、様々な厳しい気象状況を示します。 | `weather.severeTriggers` |
