---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;環境；データ型；データ型；
solution: Experience Platform
title: 環境データタイプ
topic-legacy: overview
description: このドキュメントでは、環境XDMデータ型の概要を説明します。
exl-id: ec806ee5-ed65-4148-9dbe-e297d9e8cd73
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 14%

---

#  環境データ型

 Environmentは、監視イベントの周囲の環境を記述する標準的なXDMデータ型です。特に、ネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を示します。

>[!IMPORTANT]
>
>すべての値は、Adobeがライセンスを取得した[DeviceAtlas](https://deviceatlas.com)データベースと一致する必要があります。

<img src="../images/data-types/environment.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc` | オブジェクト | 1つのフィールド`language`を含むオブジェクト。データ表示に関するユーザーの言語、地理的、文化的な好みを表す環境の言語を示します。 言語は、[IETF RFC 3066](https://www.ietf.org/rfc/rfc3066.txt)で定義されている言語コードで指定されます。 |
| `browserDetails` | [ブラウザーの詳細](./browser-details.md) | ブラウザー名、バージョン、JavaScriptバージョン、ユーザーエージェント文字列、受け入れ環境など、ブラウザー固有の言語の詳細について説明します。 |
| `ISP` | 文字列 | ユーザーのインターネットサービスプロバイダーの名前。 |
| `carrier` | 文字列 | ユーザーに通信サービスを販売し、配信するモバイルサービスプロバイダーキャリアまたはMNO(ワイヤレスネットワーク、ワイヤレスキャリア、セルラー会社、モバイルネットワークキャリアとも呼ばれます)の名前。 |
| `colorDepth` | 整数 | 1つのピクセルの各カラーコンポーネントに使用されるビット数。 |
| `connectionType` | 文字列 | インターネット接続の種類です。 次の値を指定できます。 <ul><li>`dialup`</li><li>`isdn`</li><li>`bisdn`</li><li>`dsl`</li><li>`cable`</li><li>`wireless_wifi`</li><li>`mobile`</li><li>`mobile_edge`</li><li>`mobile_2g`</li><li>`mobile_3g`</li><li>`mobile_lte`</li><li>`t1`</li><li>`t3`</li><li>`oc3`</li><li>`lan`</li><li>`modem`</li></ul> |
| `domain` | 文字列 | ユーザーのISPのドメイン。 |
| `ipV4` | 文字列 | 通信にインターネットプロトコル（32ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `ipV6` | 文字列 | 通信にインターネットプロトコル（128ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `operatingSystem` | 文字列 | 観察されたときに使用されたオペレーティングシステムの名前。属性には`10.5.3`などのバージョン情報を含めず、代わりに`Ultimate`や`Professional`などの「エディション」指定を含めます。 |
| `operatingSystemVendor` | 文字列 | 観察されたときに使用されたオペレーティングシステムベンダーの名前。 |
| `operatingSystemVersion` | 文字列 | 観察されたときに使用されたオペレーティングシステムの完全なバージョン識別子。バージョンは通常数値で構成されますが、ベンダー定義の形式になる場合があります。 |
| `type` | 文字列 | アプリケーション環境のタイプ。 使用できる値については、[付録](#type)を参照してください。 |
| `viewportHeight` | 整数 | エクスペリエンスが表示されたウィンドウの縦方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの高さです。 |
| `viewPortWidth` | 整数 | エクスペリエンスが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの幅です。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.schema.json)

## 付録

次の節では、[!UICONTROL デバイス]のデータ型に関する追加情報を説明します。

## {#type}型に指定できる値

次の表に、`type`に使用できる値とその関連する意味を示します。

| 値 | 説明 |
| --- | --- |
| `browser` | Browser |
| `application` | アプリケーション |
| `iot` | インターネット |
| `external` | 外部システム |
| `widget` | アプリケーション拡張 |
