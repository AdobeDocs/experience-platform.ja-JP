---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データ型；データ型；
solution: Experience Platform
title: デバイスデータタイプ
description: このドキュメントでは、デバイス XDM データタイプの概要を説明します。
exl-id: 049a2ca1-6bc3-4b9c-832a-77102e8a0ed2
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 18%

---

# [!UICONTROL デバイス] データタイプ

[!UICONTROL デバイス] は、識別されたデバイスを表す標準的な XDM データ型です。 デバイスは、セッションをまたいで（通常は cookie で）追跡可能なアプリケーションまたはブラウザーインスタンスです。

<img src="../images/data-types/device.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `colorDepth` | 整数 | ディスプレイが表す色の数。 |
| `manufacturer` | 文字列 | デバイスのデザインと作成を所有する組織の名前。 |
| `model` | 文字列 | デバイスのモデルの名前。 これは、デバイスの一般的な、人間が読み取れる、またはマーケティング名です。 例えば、「iPhone 6S」は携帯電話の特定のモデルです。 |
| `modelNumber` | 文字列 | このデバイスの製造元によって割り当てられた一意のモデル番号。 モデル番号は、バージョンではなく、特定のモデル設定を識別する一意の ID です。 |
| `screenHeight` | 整数 | デフォルトの向きにおける、デバイスのアクティブなディスプレイの垂直方向のピクセル数。 |
| `screenOrientation` | 文字列 | 現在の画面の向き。 指定できる値は次のとおりです。 `portrait` および `landscape`. |
| `screenWidth` | 文字列 | デフォルトの向きにおける、デバイスのアクティブなディスプレイの水平方向のピクセル数。 |
| `type` | 文字列 | 追跡されるデバイスのタイプ。 指定できる値は次のとおりです。 <ul><li>`mobile`</li><li>`tablet`</li><li>`desktop`</li><li>`ereader`</li><li>`gaming`</li><li>`television`</li><li>`settop`</li><li>`mediaplayer`</li><li>`computers`</li><li>`tv screens`</li></ul> |
| `typeID` | 文字列 | デバイスの識別子。これは、使用されているハードウェアを識別する DeviceAtlas または他のサービスからの識別子である可能性があります。 |
| `typeIDService` | 文字列 | デバイスタイプを識別するために使用されるサービスの名前空間。詳しくは、 [付録](#typeIDService) を参照してください。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/device.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/device.schema.json)

## 付録

次の節では、 [!UICONTROL デバイス] データタイプ。

## typeIDService に指定できる値 {#typeIDService}

次の表に、 `typeIDService` そしてそれに関連する意味も

| 値 | 説明 |
| --- | --- |
| `https://ns.adobe.com/xdm/external/deviceatlas` | デバイスが DeviceAtlas を使用して識別されている。 |
| `https://ns.adobe.com/xdm/external/adobecampaign` | デバイスがAdobe Campaignを使用して識別されている。 |
