---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；環境；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 環境データタイプ
description: 環境 XDM データタイプについて説明します。
exl-id: ec806ee5-ed65-4148-9dbe-e297d9e8cd73
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 19%

---

# [!UICONTROL &#x200B; 環境 &#x200B;] データタイプ

[!UICONTROL &#x200B; 環境 &#x200B;] は、観測されたイベントの周辺環境を表す標準 XDM データタイプで、特に、ネットワークやソフトウェアバージョンなどの一時的な情報の詳細を示します。

>[!IMPORTANT]
>
>すべての値は、Adobeでライセンスされている [DeviceAtlas](https://deviceatlas.com) データベースに合わせる必要があります。

![](../images/data-types/environment.png){width=400}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc` | オブジェクト | データ表示に関するユーザーの言語的、地理的、文化的な好みを表す環境の言語を示す、単一のフィールド `language` を含むオブジェクト。 言語は、[IETF RFC 3066](https://www.ietf.org/rfc/rfc3066.txt) で定義されている言語コードで指定されます。 |
| `browserDetails` | [ ブラウザーの詳細 ](./browser-details.md) | ブラウザー名、バージョン、JavaScriptのバージョン、ユーザーエージェント文字列、受け入れる言語など、ブラウザー固有の環境の詳細を表します。 |
| `ISP` | 文字列 | ユーザーのインターネットサービスプロバイダーの名前。 |
| `carrier` | 文字列 | ユーザーに通信サービスを販売および提供するモバイルネットワーク通信事業者または MNO （ワイヤレスサービスプロバイダー、ワイヤレス通信事業者、携帯電話会社またはモバイルネットワーク通信事業者とも呼ばれます）の名前。 |
| `colorDepth` | 整数 | 単一のピクセルの各色成分に使用されるビット数。 |
| `connectionType` | 文字列 | インターネット接続タイプ。 使用できる値は次のとおりです。 <ul><li>`dialup`</li><li>`isdn`</li><li>`bisdn`</li><li>`dsl`</li><li>`cable`</li><li>`wireless_wifi`</li><li>`mobile`</li><li>`mobile_edge`</li><li>`mobile_2g`</li><li>`mobile_3g`</li><li>`mobile_lte`</li><li>`t1`</li><li>`t3`</li><li>`oc3`</li><li>`lan`</li><li>`modem`</li></ul> |
| `domain` | 文字列 | ユーザーの ISP のドメイン。 |
| `ipV4` | 文字列 | 通信用インターネットプロトコル （32 ビット）を使用するコンピューターネットワークに参加しているデバイスに割り当てられた数値ラベル。 |
| `ipV6` | 文字列 | 通信用インターネットプロトコルを使用するコンピューターネットワークに参加しているデバイスに割り当てられた数値ラベル （128 ビット）。 |
| `operatingSystem` | 文字列 | 観察されたときに使用されたオペレーティングシステムの名前。属性には、`10.5.3` などのバージョン情報を含めず、`Ultimate` や `Professional` などの「エディション」指定を含める必要があります。 |
| `operatingSystemVendor` | 文字列 | 観察されたときに使用されたオペレーティングシステムベンダーの名前。 |
| `operatingSystemVersion` | 文字列 | 観察されたときに使用されたオペレーティングシステムの完全なバージョン識別子。バージョンは通常、数値で構成されますが、ベンダーが定義した形式の場合もあります。 |
| `type` | 文字列 | アプリケーション環境のタイプ。 使用可能な値については、[ 付録 ](#type) を参照してください。 |
| `viewportHeight` | 整数 | エクスペリエンスが表示されたウィンドウ内の垂直方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、これはブラウザービューポートの高さです。 |
| `viewPortWidth` | 整数 | エクスペリエンスが表示されたウィンドウ内の水平方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、これはブラウザービューポートの幅です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.schema.json)

## 付録

次のセクションでは、[!UICONTROL &#x200B; デバイス &#x200B;] データタイプに関する追加情報を示します。

## タイプの許容値 {#type}

次の表に、`type` の許容値と関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `browser` | ブラウザー |
| `application` | アプリケーション |
| `iot` | モノのインターネット |
| `external` | 外部システム |
| `widget` | アプリケーション拡張機能 |
