---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: デバイスデータタイプ
topic-legacy: overview
description: このドキュメントでは、Device XDMデータタイプの概要を説明します。
exl-id: 049a2ca1-6bc3-4b9c-832a-77102e8a0ed2
translation-type: tm+mt
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 18%

---

#  Devicedata type

 デバイスは、識別されたデバイスを記述する標準XDMデータ型です。デバイスは、セッション間で追跡可能なアプリケーションまたはブラウザーインスタンスで、通常はcookieによって追跡されます。

<img src="../images/data-types/device.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `colorDepth` | 整数 | ディスプレイが表す色の数。 |
| `manufacturer` | 文字列 | デバイスのデザインと作成を所有する組織の名前。 |
| `model` | 文字列 | デバイスのモデルの名前。 これは、デバイスの共通名、人間が読み取り可能な名前、またはマーケティング名です。 例えば、「iPhone 6S」は携帯電話の特定のモデルです。 |
| `modelNumber` | 文字列 | このデバイスの製造元によって割り当てられた一意のモデル番号です。 モデル番号は、バージョンではなく、特定のモデル設定を識別する一意の ID です。 |
| `screenHeight` | 整数 | デバイスのアクティブなディスプレイの、デフォルトの向きにおける垂直方向のピクセル数。 |
| `screenOrientation` | 文字列 | 現在の画面の向き。 指定できる値は`portrait`と`landscape`です。 |
| `screenWidth` | 文字列 | デバイスのアクティブなディスプレイの、デフォルトの向きにおける水平方向のピクセル数。 |
| `type` | 文字列 | 追跡するデバイスのタイプ。 次の値を指定できます。 <ul><li>`mobile`</li><li>`tablet`</li><li>`desktop`</li><li>`ereader`</li><li>`gaming`</li><li>`television`</li><li>`settop`</li><li>`mediaplayer`</li><li>`computers`</li><li>`tv screens`</li></ul> |
| `typeID` | 文字列 | デバイスの識別子。これは、DeviceAtlasまたは使用中のハードウェアを識別する別のサービスからの識別子である可能性があります。 |
| `typeIDService` | 文字列 | デバイスタイプを識別するために使用されるサービスの名前空間。使用できる値の詳細については、[付録](#typeIDService)を参照してください。 |

フィールドグループの詳細については、public XDM repositoryを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/device.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/device.schema.json)

## 付録

次の節では、[!UICONTROL デバイス]のデータ型に関する追加情報を説明します。

## typeIDService {#typeIDService}に指定できる値

次の表に、`typeIDService`に使用できる値とその関連する意味を示します。

| 値 | 説明 |
| --- | --- |
| `https://ns.adobe.com/xdm/external/deviceatlas` | デバイスはDeviceAtlasを使用して識別されました。 |
| `https://ns.adobe.com/xdm/external/adobecampaign` | デバイスはAdobe Campaignを使用して識別されました。 |
