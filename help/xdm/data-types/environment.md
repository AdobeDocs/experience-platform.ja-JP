---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；環境；データ型；データ型；
solution: Experience Platform
title: 環境データタイプ
topic-legacy: overview
description: このドキュメントでは、環境 XDM データ型の概要を説明します。
exl-id: ec806ee5-ed65-4148-9dbe-e297d9e8cd73
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 15%

---

#  環境データタイプ

 環境は、観察されたイベントの周囲の環境を記述する標準の XDM データ型で、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を示します。

>[!IMPORTANT]
>
>すべての値は、Adobeがライセンスを取得した [DeviceAtlas](https://deviceatlas.com) データベースに合わせる必要があります。

<img src="../images/data-types/environment.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc` | オブジェクト | 1 つのフィールド `language` を含むオブジェクト。これは、ユーザーがデータを表示する際の言語、地理的または文化的な環境設定を表す環境の言語を示します。 言語は、[IETF RFC 3066](https://www.ietf.org/rfc/rfc3066.txt) で定義されている言語コードで指定されます。 |
| `browserDetails` | [ブラウザーの詳細](./browser-details.md) | ブラウザー名、バージョン、JavaScript バージョン、ユーザーエージェント文字列、受け入れ可能な言語など、環境のブラウザー固有の詳細について説明します。 |
| `ISP` | 文字列 | ユーザーのインターネットサービスプロバイダーの名前。 |
| `carrier` | 文字列 | 通信サービスを販売し、ユーザに提供するモバイルネットワークキャリアまたは MNO（ワイヤレスサービスプロバイダ、ワイヤレスキャリア、携帯電話会社、またはモバイルネットワークキャリアとも呼ばれます）の名前。 |
| `colorDepth` | 整数 | 1 つのピクセルの各色成分に使用されるビット数。 |
| `connectionType` | 文字列 | インターネット接続の種類。 指定できる値は次のとおりです。 <ul><li>`dialup`</li><li>`isdn`</li><li>`bisdn`</li><li>`dsl`</li><li>`cable`</li><li>`wireless_wifi`</li><li>`mobile`</li><li>`mobile_edge`</li><li>`mobile_2g`</li><li>`mobile_3g`</li><li>`mobile_lte`</li><li>`t1`</li><li>`t3`</li><li>`oc3`</li><li>`lan`</li><li>`modem`</li></ul> |
| `domain` | 文字列 | ユーザーの ISP のドメイン。 |
| `ipV4` | 文字列 | 通信にインターネットプロトコル（32 ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `ipV6` | 文字列 | 通信にインターネットプロトコル（128 ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `operatingSystem` | 文字列 | 観察されたときに使用されたオペレーティングシステムの名前。属性には、`10.5.3` などのバージョン情報を含めず、代わりに `Ultimate` や `Professional` などの「編集」指定を含めます。 |
| `operatingSystemVendor` | 文字列 | 観察されたときに使用されたオペレーティングシステムベンダーの名前。 |
| `operatingSystemVersion` | 文字列 | 観察されたときに使用されたオペレーティングシステムの完全なバージョン識別子。バージョンは通常数値で構成されますが、ベンダーが定義した形式の場合もあります。 |
| `type` | 文字列 | アプリケーション環境のタイプ。 指定できる値については、[ 付録 ](#type) を参照してください。 |
| `viewportHeight` | 整数 | エクスペリエンスが表示されたウィンドウの垂直方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、これはブラウザーの表示の高さです。 |
| `viewPortWidth` | 整数 | エクスペリエンスが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Web ビューイベントの場合、これはブラウザーの表示域の幅です。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.schema.json)

## 付録

次の節では、[!UICONTROL Device] データ型に関する追加情報を示します。

## 型に指定できる値 {#type}

次の表に、`type` の受け入れ可能な値とそれに関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `browser` | ブラウザー |
| `application` | アプリケーション |
| `iot` | 物のインターネット |
| `external` | 外部システム |
| `widget` | アプリケーション拡張 |
