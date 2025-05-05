---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；デバイス；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: デバイスのデータタイプ
description: デバイス XDM データタイプについて説明します。
exl-id: 049a2ca1-6bc3-4b9c-832a-77102e8a0ed2
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 19%

---

# [!UICONTROL &#x200B; デバイス &#x200B;] データタイプ

[!UICONTROL &#x200B; デバイス &#x200B;] は、識別されたデバイスを記述する標準の XDM データタイプです。 デバイスは、セッションをまたいで（通常は cookie で）トラッキング可能なアプリケーションまたはブラウザーインスタンスです。

![](../images/data-types/device.png){width=450}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `colorDepth` | 整数 | ディスプレイが表す色の数。 |
| `manufacturer` | 文字列 | デバイスのデザインと作成を所有する組織の名前。 |
| `model` | 文字列 | デバイスのモデルの名前。これは、デバイスの一般的な、人間が読み取れる、またはマーケティング上の名前です。 例えば、「iPhone 6S」は携帯電話の特定のモデルです。 |
| `modelNumber` | 文字列 | 製造元によってこのデバイスに割り当てられた一意のモデル番号指定。モデル番号はバージョンではなく、特定のモデル設定を識別する一意の識別子です。 |
| `screenHeight` | 整数 | デフォルトの向きにおける、デバイスのアクティブなディスプレイの垂直方向のピクセル数。 |
| `screenOrientation` | 文字列 | 現在の画面の向き。 許容される値は、`portrait` および `landscape` です。 |
| `screenWidth` | 文字列 | デフォルトの向きにおける、デバイスのアクティブなディスプレイの水平方向のピクセル数。 |
| `type` | 文字列 | トラッキングされるデバイスのタイプ。 使用できる値は次のとおりです。 <ul><li>`mobile`</li><li>`tablet`</li><li>`desktop`</li><li>`ereader`</li><li>`gaming`</li><li>`television`</li><li>`settop`</li><li>`mediaplayer`</li><li>`computers`</li><li>`tv screens`</li></ul> |
| `typeID` | 文字列 | デバイスの識別子。これは、使用されているハードウェアを識別する DeviceAtlas またはその他のサービスの識別子である可能性があります。 |
| `typeIDService` | 文字列 | デバイスタイプを識別するために使用されるサービスの名前空間。 使用可能な値について詳しくは、[ 付録 ](#typeIDService) を参照してください。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/device.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/device.schema.json)

## 付録

次のセクションでは、[!UICONTROL &#x200B; デバイス &#x200B;] データタイプに関する追加情報を示します。

## typeIDService の許容値 {#typeIDService}

次の表に、`typeIDService` の許容値と関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `https://ns.adobe.com/xdm/external/deviceatlas` | DeviceAtlas を使用してデバイスが識別されている。 |
| `https://ns.adobe.com/xdm/external/adobecampaign` | デバイスはAdobe Campaignを使用して識別されています。 |
