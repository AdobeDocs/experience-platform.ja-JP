---
title: Advertising詳細スキーマフィールドグループ
description: Advertisingの詳細スキーマフィールドグループについて説明します。
exl-id: 25de09bd-eedd-489c-9cd5-8acd0c52ddbe
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 29%

---

# [!UICONTROL Advertisingの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL Advertisingの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `advertising` オブジェクトを提供します。これは、広告インプレッション数、クリックスルー数およびアトリビューションに関連する情報をキャプチャします。

![&#x200B; フィールドグループ構造 &#x200B;](../../images/field-groups/advertising-details/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adAssetReference` | オブジェクト | 広告に関するアセット情報をキャプチャします。 このオブジェクトの構造について詳しくは、次の [&#x200B; サブセクション &#x200B;](#adAssetReference) を参照してください。 |
| `adAssetViewDetails` | オブジェクト | 広告再生のビューの詳細をキャプチャします。 このオブジェクトの構造について詳しくは、次の [&#x200B; サブセクション &#x200B;](#adAssetViewDetails) を参照してください。 |
| `adViewability` | オブジェクト | エンドユーザーに表示されるインプレッション数（プレーヤーボリューム、ライブラリのバージョン、ウィンドウのステータス、広告ビューポートのサイズなど）をキャプチャします。 このオブジェクトの構造について詳しくは、次の [&#x200B; サブセクション &#x200B;](#adViewability) を参照してください。 |
| `clicks` | [[!UICONTROL 測定]](../../data-types/measure.md) | 広告でのクリックアクションの数。 |
| `completes` | [[!UICONTROL 測定]](../../data-types/measure.md) | タイムドメディアアセットが最後まで視聴された回数。 このことは、エンドユーザーが先にスキップした可能性があるので、必ずしもビデオ全体を視聴したことを意味するものではありません。 |
| `conversions` | [[!UICONTROL 測定]](../../data-types/measure.md) | パフォーマンス評価のために事前定義済みのアクション（またはアクション）でイベントがトリガーされた回数。 |
| `federated` | [[!UICONTROL 測定]](../../data-types/measure.md) | データフェデレーション (お客様間のデータ共有など) を使用してエクスペリエンスイベントが作成されたかどうかを示します。 |
| `firstQuartiles` | [[!UICONTROL 測定]](../../data-types/measure.md) | 通常の速度で、デュレーションの 25% を再生したデジタルビデオ広告の回数。 |
| `impressions` | [[!UICONTROL 測定]](../../data-types/measure.md) | 表示される可能性のあるエンドユーザーに送信された広告インプレッションの数。 |
| `midpoints` | [[!UICONTROL 測定]](../../data-types/measure.md) | 通常の速度で、デュレーションの 50% を再生したデジタルビデオ広告の回数。 |
| `starts` | [[!UICONTROL 測定]](../../data-types/measure.md) | デジタルビデオ広告の再生が開始された回数。 |
| `thirdQuartiles` | [[!UICONTROL 測定]](../../data-types/measure.md) | 通常の速度で、デュレーションの 75% を再生したデジタルビデオ広告の回数。 |
| `timePlayed` | [[!UICONTROL 測定]](../../data-types/measure.md) | 特定のタイムドメディアアセットでエンドユーザーが費やした時間。 |
| `downloadedPlayback` | ブール値 | `true` に設定した場合、ダウンロードされた広告セッションの再生に起因してヒットが生成されることを示します。 |

{style="table-layout:auto"}

## `adAssetReference` {#adAssetReference}

`adAssetReference` オブジェクトは、広告に関するアセット情報をキャプチャします。

![adAssetReference 構造 &#x200B;](../../images/field-groups/advertising-details/adAssetReference.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc.title` | 文字列 | 人間が読み取れる、わかりやすい広告アセットの名前。 |
| `_xmpDM.duration` | 整数 | アセットの長さまたはデュレーション （秒）。 |
| `_id` | 文字列 | [&#x200B; 広告 ID 標準 &#x200B;](https://datatracker.ietf.org/doc/html/rfc8107) に従った広告アセットの一意の ID。 |
| `advertiser` | 文字列 | 広告で商品が取り上げられる会社またはブランド。 |
| `campaign` | 文字列 | 広告キャンペーンの ID。 |
| `creativeID` | 文字列 | 広告クリエイティブの ID。 |
| `creativeURL` | 文字列 | 広告クリエイティブの URL。 |
| `placementID` | 文字列 | 広告のプレースメント ID。 |
| `siteID` | 文字列 | 広告サイトの ID。 |

{style="table-layout:auto"}

## `adAssetViewDetails` {#adAssetViewDetails}

`adAssetViewDetails` オブジェクトは、広告再生のためのビューの詳細をキャプチャします。

![adAssetViewDetails 構造 &#x200B;](../../images/field-groups/advertising-details/adAssetViewDetails.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `adBreak` | [[!UICONTROL &#x200B; 広告ブレーク &#x200B;]](../../data-types/ad-break.md) | タイムド広告がタイムドメディアにどのように挿入されるかを表します。 |
| `index` | 整数 | 親広告ブレーク内の広告のインデックス。 例えば、最初の広告にはインデックス `0` があり、2 番目の広告にはインデックス `1` があります。 |
| `playerName` | 文字列 | 広告のレンダリングを担当するプレイヤーの名前。 |

{style="table-layout:auto"}

## `adViewability` {#adViewability}

`adViewability` オブジェクトは、エンドユーザーに表示されるインプレッション数（プレーヤーボリューム、ライブラリのバージョン、ウィンドウのステータス、広告ビューポートのサイズなど）をキャプチャします。

![adViewability 構造 &#x200B;](../../images/field-groups/advertising-details/adViewability.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `implementationDetails` | [[!UICONTROL &#x200B; 実装の詳細 &#x200B;]](../../data-types/implementation-details.md) | 視認性指標を測定するために実装されたライブラリの名前とバージョン。 |
| `measuredAdNotVisible` | [[!UICONTROL 測定]](../../data-types/measure.md) | インプレッション時にビューアビリティライブラリで測定されたとおりに、広告が表示されないことを示します。 |
| `measuredMuted` | [[!UICONTROL 測定]](../../data-types/measure.md) | インプレッション時にビューアビリティライブラリで測定されたとおりに、広告がミュートされていることを示します。 |
| `unmeasurableIframe` | [[!UICONTROL 測定]](../../data-types/measure.md) | インプレッション時にビューアビリティライブラリで測定されたとおりに、広告が非アクティブウィンドウに表示されることを示します。 |
| `unmeasurableOther` | [[!UICONTROL 測定]](../../data-types/measure.md) | 広告が iframe 内に表示されているので、ビューアビリティライブラリが測定を適切に実行できないことを示します。 |
| `viewabilityEligibleImpressions` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリを備えたエンドユーザーに対する広告の (1 つまたは複数の) インプレッション。 |
| `viewabilityCompletes` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリによって完了時にビューアブルと判断されたエンドユーザーに対する広告の (1 つまたは複数の) 完了。 |
| `viewableFirstQuartiles` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリによって再生の第 1 四分位数時にビューアブルと判断されたエンドユーザーに対する広告の (1 つまたは複数の) 第 1 四分位数。 |
| `viewableImpressions` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリによって 2 秒間再生された後にビューアブルと判断されたエンドユーザーに対する広告のインプレッション。 |
| `viewableMidpoints` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリによって再生の中間点にビューアブルと判断されたエンドユーザーに対する広告の（1 つまたは複数の）中間点。 |
| `viewableThirdQuartiles` | [[!UICONTROL 測定]](../../data-types/measure.md) | ビューアビリティライブラリによって再生の第 3 四分位数時にビューアブルと判断されたエンドユーザーに対する広告の (1 つまたは複数の) 第 3 四分位数。 |
| `activeWindow` | ブール値 | 広告がユーザーのデバイスのアクティブウィンドウに表示されたかどうかを示します。 |
| `adHeight` | 整数 | 実行時に測定される、プレイヤーの垂直方向のピクセル数。プレイヤーに追加のコントロールまたはサムネールがある場合は、広告のサイズよりも大きくなる可能性があります。 |
| `adUnitDepth` | 整数 | パブリッシャーは、コンテナ（iFrame）内に広告ユニットを埋め込んで、広告のアクセスをページのコードのみに制限することができます。 この値は、広告ユニットが表示されるコンテナの数を表します。 |
| `adWidth` | 整数 | 実行時に測定される、プレイヤーの水平方向のピクセル数。プレイヤーに追加のコントロールまたはサムネールがある場合は、広告のサイズよりも大きくなる可能性があります。 |
| `measurementEligible` | ブール値 | 広告が視認性測定の資格を有したかどうか。ユニットにサポートされているクリエイティブフォーマットとタグタイプがある場合、広告は適格です。 |
| `percentViewable` | 整数 | 測定時にビューアブルと判断された広告のピクセルの割合。 |
| `playerVolume` | 整数 | 実行時に測定されたプレイヤーのボリュームの割合。`0` はミュートで、`100` は最大ボリュームです。 |
| `viewable` | ブール値 | 広告が実行時に表示可能だったかどうかを示します。 ディスプレイ広告は、広告の少なくとも 50% が 1 秒間表示される場合に表示可能と見なされます。 ビデオ広告は、ビデオの再生中に広告の 50% 以上が 2 秒以上連続して表示された場合、表示可能と見なされます。 |
| `viewportHeight` | 整数 | 実行時に測定された、エクスペリエンスが表示されたウィンドウ内の垂直方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、この値は、ブラウザービューポートの高さを示します。 |
| `viewportWidth` | 整数 | 実行時に測定された、エクスペリエンスが表示されたウィンドウ内の水平方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、この値は、ブラウザービューポートの幅を示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-advertising.schema.json) を参照してください。
