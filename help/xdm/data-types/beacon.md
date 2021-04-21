---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；ビーコン；インタラクションの詳細；データ型；データ型；
solution: Experience Platform
title: ビーコンのデータタイプ
topic-legacy: overview
description: このドキュメントでは、XDM Individualプロファイルクラスの概要を説明します。
exl-id: a3767c8d-a009-49b4-81a4-b084b6e5101a
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 6%

---

#  Beacondata型

 ビーコンは、標準的なXDMデータ型で、モバイルデバイスが範囲内にあるときにID情報をモバイルアプリケーションに通信するワイヤレスデバイスを表します。

<img src="../images/data-types/beacon.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconMajor` | Double | メジャー値は、1 ～ 65,535の間のグループと符号なし整数値を識別し、区別します。 |
| `beaconMinor` | 重複 | マイナー値は、1 ～ 65,535の間の個々の整数値と符号なし整数値を識別し、区別します。 |
| `proximity` | 文字列 | ビーコンからの推定距離。使用できる値と定義については、[付録](#proximity)を参照してください。 |
| `proximityUUID` | 文字列 | 近接性UUID(Universally Unique Identifier)は、ネットワーク内のビーコンと、管理外のネットワーク内の他のすべてのビーコンを区別するために使用される特別な識別子です。 近接UUIDはビーコンに設定され、組織のビーコンを識別するために範囲内のモバイルデバイスに送信されます。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/beacon-interaction-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/beacon-interaction-details.schema.json)

## 付録

次の節では、[!UICONTROL ビーコン]データタイプに関する追加情報を示します。

## 近接性に指定できる値{#proximity}

次の表に、`proximity`に使用できる値とその関連する意味を示します。

| 値 | 説明 |
| --- | --- |
| `immediate` | 数センチ以内に。 |
| `near` | 10メートルも離れていない。 |
| `far` | 10メートル以上離れています。 |
| `unknown` | 信号が弱いためと思われるが、距離は確かめられなかった。 |
