---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: デバイスデータタイプ
topic-legacy: overview
description: このドキュメントでは、デバイス XDM データタイプの概要を説明します。
exl-id: 049a2ca1-6bc3-4b9c-832a-77102e8a0ed2
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 18%

---

#  Devicedata type

 デバイスは、識別されたデバイスを表す標準の XDM データ型です。デバイスは、セッションをまたいで（通常は cookie で）追跡可能なアプリケーションまたはブラウザーインスタンスです。

<img src="../images/data-types/device.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `colorDepth` | 整数 | ディスプレイが表す色の数。 |
| `manufacturer` | 文字列 | デバイスのデザインと作成を所有する組織の名前。 |
| `model` | 文字列 | デバイスのモデルの名前。 これは、デバイスの一般的な、人間が読める、またはマーケティング名です。 例えば、「iPhone 6S」は携帯電話の特定のモデルです。 |
| `modelNumber` | 文字列 | このデバイスの製造元によって割り当てられた一意のモデル番号。 モデル番号は、バージョンではなく、特定のモデル設定を識別する一意の ID です。 |
| `screenHeight` | 整数 | デバイスのアクティブなディスプレイのデフォルトの方向の垂直ピクセル数。 |
| `screenOrientation` | 文字列 | 現在の画面の向き。 指定できる値は `portrait` と `landscape` です。 |
| `screenWidth` | 文字列 | デバイスのアクティブなディスプレイのデフォルトの方向の水平ピクセル数。 |
| `type` | 文字列 | 追跡するデバイスのタイプ。 指定できる値は次のとおりです。 <ul><li>`mobile`</li><li>`tablet`</li><li>`desktop`</li><li>`ereader`</li><li>`gaming`</li><li>`television`</li><li>`settop`</li><li>`mediaplayer`</li><li>`computers`</li><li>`tv screens`</li></ul> |
| `typeID` | 文字列 | デバイスの識別子。これは、使用中のハードウェアを識別する DeviceAtlas または別のサービスからの識別子である可能性があります。 |
| `typeIDService` | 文字列 | デバイスタイプを識別するために使用されるサービスの名前空間。受け入れられる値の詳細については、[ 付録 ](#typeIDService) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/device.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/device.schema.json)

## 付録

次の節では、[!UICONTROL Device] データ型に関する追加情報を示します。

## typeIDService に指定できる値 {#typeIDService}

次の表に、`typeIDService` の受け入れ可能な値とそれに関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `https://ns.adobe.com/xdm/external/deviceatlas` | デバイスは DeviceAtlas を使用して識別されました。 |
| `https://ns.adobe.com/xdm/external/adobecampaign` | デバイスは、Adobe Campaignを使用して識別されています。 |
