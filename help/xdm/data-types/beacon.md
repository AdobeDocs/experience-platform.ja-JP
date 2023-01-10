---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ビーコン；インタラクションの詳細；データ型；データ型；
solution: Experience Platform
title: ビーコンのデータタイプ
description: このドキュメントでは、XDM 個人プロファイルクラスの概要を説明します。
exl-id: a3767c8d-a009-49b4-81a4-b084b6e5101a
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 11%

---

# [!UICONTROL ビーコン] データタイプ

[!UICONTROL ビーコン] は、モバイルデバイスが範囲内に入るたびにモバイルアプリケーションに ID 情報を通信するワイヤレスデバイスを表す標準 XDM データ型です。

<img src="../images/data-types/beacon.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconMajor` | Double | メジャー値は、グループを識別および区別する、1 ～ 65,535 の符号なし整数値です。 |
| `beaconMinor` | Double | マイナー値は、個々の値を識別および区別し、1 ～ 65,535 の符号なし整数値です。 |
| `proximity` | 文字列 | ビーコンからの推定距離。詳しくは、 [付録](#proximity) を参照してください。 |
| `proximityUUID` | 文字列 | 近接 UUID(Universally Unique Identifier) は、ネットワーク内のビーコンを制御外のネットワーク内の他のすべてのビーコンと区別するために使用される特別なタイプの識別子です。 近接 UUID は、組織のビーコンを識別するために、範囲内のモバイルデバイスに送信されるビーコンに設定されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.schema.json)

## 付録

次の節では、 [!UICONTROL ビーコン] データタイプ。

## 近接性に使用できる値 {#proximity}

次の表に、 `proximity` そしてそれに関連する意味も

| 値 | 説明 |
| --- | --- |
| `immediate` | 数センチ以内で。 |
| `near` | 10 メートル未満です。 |
| `far` | 10 メートル以上離れています。 |
| `unknown` | この距離は、弱い信号が原因である可能性が高く、確認できなかった。 |
