---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；コマース；データ型；データ型；データ型；
solution: Experience Platform
title: コマースデータタイプ
topic-legacy: overview
description: このドキュメントでは、Commerce Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: c9cc569b-1a91-4a6e-8bfd-7f8ec07d01d4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 4%

---

# [!UICONTROL Commercedata] 型

[!UICONTROL Commercesは、] 標準のExperience Data Model(XDM)データ型で、購入および販売のアクティビティに関連する記録を記述します。

<img src="../images/data-types/commerce.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `order` | [[!UICONTROL Order]](./order.md) | 1つ以上の製品の発注を示します。 |
| `cartAbandons` | [[!UICONTROL 測定]](./measure.md) | 製品リストがユーザーによってアクセス不能または購入不能になったと識別された場合を説明するために使用します。 |
| `checkouts` | [[!UICONTROL 測定]](./measure.md) | 製品リストのチェックアウトプロセス中のアクション。 チェックアウトプロセスに複数のステップがある場合は、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、イベント時間の情報と参照先のページまたはエクスペリエンスを使用して、手順と各イベントが順番に示されます。 |
| `inStorePurchase` | [[!UICONTROL 測定]](./measure.md) | 解析用の店頭購入に関連付けられた値を示します。 |
| `productListAdds` | [[!UICONTROL 測定]](./measure.md) | 買い物かごに追加される商品など、商品を商品リストに追加すること。 |
| `productListOpens` | [[!UICONTROL 測定]](./measure.md) | 買い物かごの作成中など、新しい商品リストの初期化。 |
| `productListRemovals` | [[!UICONTROL 測定]](./measure.md) | 買い物かごから製品を削除するなど、製品リストから製品エントリを削除すること。 |
| `productListReopens` | [[!UICONTROL 測定]](./measure.md) | 以前に破棄された製品リストが、ユーザーによって再アクティブ化されました。 |
| `productListViews` | [[!UICONTROL 測定]](./measure.md) | 製品リストの表示または表示が発生したタイミングを説明します。 |
| `productViews` | [[!UICONTROL 測定]](./measure.md) | 個々の製品の表示や表示が発生したタイミングを説明します。 |
| `purchases` | [[!UICONTROL 測定]](./measure.md) | 注文がいつ受け入れられたかを追跡するために使用します。 コマースコンバージョンで必要なアクションは購入イベントのみです。 購入イベントでは、商品リストが参照されている必要があります。 |
| `saveForLaters` | [[!UICONTROL 測定]](./measure.md) | ウィッシュリストなどの将来的な使用のために、製品リストを保存します。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json)
