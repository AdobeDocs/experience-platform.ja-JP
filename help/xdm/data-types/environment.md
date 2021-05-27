---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；環境；データ型；データ型；
solution: Experience Platform
title: 環境データタイプ
topic-legacy: overview
description: このドキュメントでは、環境XDMデータタイプの概要を説明します。
exl-id: ec806ee5-ed65-4148-9dbe-e297d9e8cd73
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 15%

---

#  環境データタイプ

 環境は、観察されたイベントの周囲の環境を記述する標準のXDMデータ型で、特にネットワークやソフトウェアのバージョンなどの一時的な情報を詳しく説明します。

>[!IMPORTANT]
>
>すべての値は、Adobeがライセンスを取得し、[DeviceAtlas](https://deviceatlas.com)データベースに合わせる必要があります。

<img src="../images/data-types/environment.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_dc` | オブジェクト | 1つのフィールド`language`を含むオブジェクト。ユーザーがデータを表示する際の言語、地理的または文化的な好みを表す環境の言語を示します。 言語は、[IETF RFC 3066](https://www.ietf.org/rfc/rfc3066.txt)で定義されている言語コードで指定されます。 |
| `browserDetails` | [ブラウザーの詳細](./browser-details.md) | ブラウザー名、バージョン、JavaScriptバージョン、ユーザーエージェント文字列、受け入れ可能な言語など、環境のブラウザー固有の詳細について説明します。 |
| `ISP` | 文字列 | ユーザーのインターネットサービスプロバイダーの名前。 |
| `carrier` | 文字列 | 通信サービスを販売し、ユーザに提供するモバイルネットワークキャリアまたはMNO（ワイヤレスサービスプロバイダ、ワイヤレスキャリア、携帯電話会社、モバイルネットワークキャリアとも呼ばれます）の名前。 |
| `colorDepth` | 整数 | 1ピクセルの各カラーコンポーネントに使用されるビット数。 |
| `connectionType` | 文字列 | インターネット接続の種類。 指定できる値は次のとおりです。 <ul><li>`dialup`</li><li>`isdn`</li><li>`bisdn`</li><li>`dsl`</li><li>`cable`</li><li>`wireless_wifi`</li><li>`mobile`</li><li>`mobile_edge`</li><li>`mobile_2g`</li><li>`mobile_3g`</li><li>`mobile_lte`</li><li>`t1`</li><li>`t3`</li><li>`oc3`</li><li>`lan`</li><li>`modem`</li></ul> |
| `domain` | 文字列 | ユーザーのISPのドメイン。 |
| `ipV4` | 文字列 | 通信にインターネットプロトコル（32ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `ipV6` | 文字列 | 通信にインターネットプロトコル（128ビット）を使用するコンピュータネットワークに参加するデバイスに割り当てられる数値ラベル。 |
| `operatingSystem` | 文字列 | 観察されたときに使用されたオペレーティングシステムの名前。属性には、`10.5.3`などのバージョン情報を含めず、代わりに`Ultimate`や`Professional`などの「編集」指定を含めます。 |
| `operatingSystemVendor` | 文字列 | 観察されたときに使用されたオペレーティングシステムベンダーの名前。 |
| `operatingSystemVersion` | 文字列 | 観察されたときに使用されたオペレーティングシステムの完全なバージョン識別子。バージョンは通常数値で構成されますが、ベンダーが定義した形式の場合もあります。 |
| `type` | 文字列 | アプリケーション環境のタイプ。 指定できる値については、[付録](#type)を参照してください。 |
| `viewportHeight` | 整数 | エクスペリエンスが表示されたウィンドウの縦方向のサイズ（ピクセル単位）。 Webビューイベントの場合、これはブラウザーの表示の高さです。 |
| `viewPortWidth` | 整数 | エクスペリエンスが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Webビューイベントの場合、これはブラウザーの表示域の幅です。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/environment.schema.json)

## 付録

次の節では、[!UICONTROL Device]データ型に関する追加情報を示します。

## 型に指定できる値 {#type}

次の表に、`type`で使用できる値と、それに関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `browser` | Browser |
| `application` | アプリケーション |
| `iot` | 物のインターネット |
| `external` | 外部システム |
| `widget` | アプリケーション拡張 |
