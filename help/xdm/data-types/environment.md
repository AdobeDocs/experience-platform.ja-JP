---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；環境；データ型；データ型；
solution: Experience Platform
title: 環境データタイプ
description: このドキュメントでは、環境 XDM データタイプの概要を説明します。
exl-id: ec806ee5-ed65-4148-9dbe-e297d9e8cd73
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 15%

---

# [!UICONTROL 環境] データタイプ

[!UICONTROL 環境] は、観測されたイベントの周囲の環境を記述する標準の XDM データ型です。特に、ネットワークバージョンやソフトウェアバージョンなどの一時的な情報の詳細が記述されます。

>[!IMPORTANT]
>
>すべての値は、 [DeviceAtlas](https://deviceatlas.com) データベース (Adobeがライセンス )

<img src="../images/data-types/environment.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc` | オブジェクト | 単一のフィールドを含むオブジェクト `language`：ユーザーがデータを表示する際における言語的、地理的または文化的な好みを表す環境の言語を示します。 言語は、 [IETF RFC 3066](https://www.ietf.org/rfc/rfc3066.txt). |
| `browserDetails` | [ブラウザーの詳細](./browser-details.md) | 環境のブラウザー固有の詳細（ブラウザー名、バージョン、JavaScript バージョン、ユーザーエージェント文字列、受け入れ可能な言語など）について説明します。 |
| `ISP` | 文字列 | ユーザーのインターネットサービスプロバイダーの名前。 |
| `carrier` | 文字列 | 通信サービスを販売し、ユーザに提供するモバイルネットワークキャリアまたは MNO（ワイヤレスサービスプロバイダ、ワイヤレスキャリア、携帯電話会社、またはモバイルネットワークキャリアとも呼ばれます）の名前。 |
| `colorDepth` | 整数 | 単一ピクセルの各色成分に使用されるビット数。 |
| `connectionType` | 文字列 | インターネット接続の種類。 指定できる値は次のとおりです。 <ul><li>`dialup`</li><li>`isdn`</li><li>`bisdn`</li><li>`dsl`</li><li>`cable`</li><li>`wireless_wifi`</li><li>`mobile`</li><li>`mobile_edge`</li><li>`mobile_2g`</li><li>`mobile_3g`</li><li>`mobile_lte`</li><li>`t1`</li><li>`t3`</li><li>`oc3`</li><li>`lan`</li><li>`modem`</li></ul> |
| `domain` | 文字列 | ユーザーの ISP のドメイン。 |
| `ipV4` | 文字列 | 通信にインターネットプロトコル（32 ビット）を使用するコンピューターネットワークに参加しているデバイスに割り当てられる数値ラベル。 |
| `ipV6` | 文字列 | 通信にインターネットプロトコル（128 ビット）を使用するコンピューターネットワークに参加しているデバイスに割り当てられる数値ラベル。 |
| `operatingSystem` | 文字列 | 観察されたときに使用されたオペレーティングシステムの名前。属性には、次のようなバージョン情報を含めることはできません。 `10.5.3`の代わりに、次のような「エディション」指定が含まれます。 `Ultimate` または `Professional`. |
| `operatingSystemVendor` | 文字列 | 観察されたときに使用されたオペレーティングシステムベンダーの名前。 |
| `operatingSystemVersion` | 文字列 | 観察されたときに使用されたオペレーティングシステムの完全なバージョン識別子。バージョンは通常数値で構成されますが、ベンダーが定義した形式の場合もあります。 |
| `type` | 文字列 | アプリケーション環境のタイプ。 詳しくは、 [付録](#type) を参照してください。 |
| `viewportHeight` | 整数 | エクスペリエンスが表示されたウィンドウ内の垂直方向のサイズ（ピクセル単位）。 Web ビューポートイベントの場合、これはブラウザーの表示域の高さです。 |
| `viewPortWidth` | 整数 | エクスペリエンスが表示されたウィンドウ内の水平方向のサイズ（ピクセル単位）。 Web ビューポートイベントの場合、これはブラウザーの表示域の幅です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.schema.json)

## 付録

次の節では、 [!UICONTROL デバイス] データタイプ。

## タイプに使用できる値 {#type}

次の表に、 `type` そしてそれに関連する意味も

| 値 | 説明 |
| --- | --- |
| `browser` | ブラウザー |
| `application` | アプリケーション |
| `iot` | 物のインターネット |
| `external` | 外部システム |
| `widget` | アプリケーション拡張 |
