---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ビーコン；インタラクションの詳細；データ型；データ型；
solution: Experience Platform
title: ビーコンのデータタイプ
topic-legacy: overview
description: このドキュメントでは、XDM Individual Profile クラスの概要を説明します。
exl-id: a3767c8d-a009-49b4-81a4-b084b6e5101a
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 7%

---

#  ビーコンデータ型

 ビーコンは、モバイルデバイスが範囲内に入ると、ID 情報をモバイルアプリケーションに通信するワイヤレスデバイスを記述する標準 XDM データ型です。

<img src="../images/data-types/beacon.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconMajor` | Double | メジャー値は、1 ～ 65,535 の間のグループ値と符号なし整数値を識別して区別します。 |
| `beaconMinor` | ダブル | 小さな値は、1 ～ 65,535 の個々の整数値と符号なしの整数値を識別して区別します。 |
| `proximity` | 文字列 | ビーコンからの推定距離。指定できる値と定義については、[ 付録 ](#proximity) を参照してください。 |
| `proximityUUID` | 文字列 | 近接 UUID(Universally Unique Identifier) は、ネットワーク内のビーコンを制御外のネットワーク内の他のすべてのビーコンと区別するために使用される特別なタイプの識別子です。 近接 UUID は、組織のビーコンを識別するために範囲内のモバイルデバイスに送信されるビーコンに設定されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.schema.json)

## 付録

次の節では、[!UICONTROL  ビーコン ] データタイプに関する追加情報を示します。

## 近接性に使用できる値 {#proximity}

次の表に、`proximity` の受け入れ可能な値とそれに関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `immediate` | 数センチ以内に。 |
| `near` | 10 メートル以内です。 |
| `far` | 10 メートル以上離れている。 |
| `unknown` | 距離は弱い信号のために確かめられなかった。 |
