---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ビーコン；インタラクションの詳細；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: ビーコンデータタイプ
description: XDM Individual Profile クラスについて説明します。
exl-id: a3767c8d-a009-49b4-81a4-b084b6e5101a
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 17%

---

# [!UICONTROL &#x200B; ビーコン &#x200B;] データタイプ

[!UICONTROL &#x200B; ビーコン &#x200B;] は、モバイルデバイスが範囲内に入るとモバイルアプリケーションに ID 情報を通信するワイヤレスデバイスを記述する、標準の XDM データタイプです。

![](../images/data-types/beacon.png){width=450}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `beaconMajor` | Double | メジャー値は、グループを識別および区別する、1 ～ 65,535 の符号なし整数値です。 |
| `beaconMinor` | Double | マイナー値は、個人を識別および区別する、1 ～ 65,535 の符号なし整数値です。 |
| `proximity` | 文字列 | ビーコンからの推定距離。 使用できる値と定義については、[&#x200B; 付録 &#x200B;](#proximity) を参照してください。 |
| `proximityUUID` | 文字列 | 近接 UUID （ユニバーサル固有識別子）は、管理外のネットワーク内の他のすべてのビーコンからネットワーク内のビーコンを区別するために使用される、特別なタイプの識別子です。 近接 UUID は、ビーコンに設定され、組織のビーコンを識別するために、範囲内のモバイルデバイスに送信されます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/deprecated/beacon-interaction-details.schema.json)

## 付録

次の節では、[!UICONTROL &#x200B; ビーコン &#x200B;] データタイプに関する追加情報について説明します。

## 近接性の許容値 {#proximity}

次の表に、`proximity` の許容値と関連する意味の概要を示します。

| 値 | 説明 |
| --- | --- |
| `immediate` | 数センチ以内。 |
| `near` | 10 メートル未満です。 |
| `far` | 10 メートル以上の距離。 |
| `unknown` | 距離を確認できませんでした。信号が弱い可能性があります。 |
