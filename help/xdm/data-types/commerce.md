---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；コマース；データ型；データ型；
solution: Experience Platform
title: コマースデータタイプ
topic-legacy: overview
description: このドキュメントでは、Commerce Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: c9cc569b-1a91-4a6e-8bfd-7f8ec07d01d4
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 5%

---

#  Commercedata type

 Commerceis は、購入および販売活動に関連するレコードを記述する標準の Experience Data Model(XDM) データ型です。

<img src="../images/data-types/commerce.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `order` | [[!UICONTROL Order]](./order.md) | 1 つ以上の製品の発注を示します。 |
| `cartAbandons` | [[!UICONTROL 測定]](./measure.md) | 製品リストが、いつユーザーによってアクセスできなくなったか、または購入できなくなったかを説明するために使用されます。 |
| `checkouts` | [[!UICONTROL 測定]](./measure.md) | 製品リストのチェックアウトプロセス中のアクション。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、イベント時間情報と参照先のページまたはエクスペリエンスを使用して、順番に表された手順と個々のイベントを識別します。 |
| `inStorePurchase` | [[!UICONTROL 測定]](./measure.md) | Analytics での使用に関連する値を記述します。 |
| `productListAdds` | [[!UICONTROL 測定]](./measure.md) | 製品リストへの製品の追加（買い物かごに追加される製品など）。 |
| `productListOpens` | [[!UICONTROL 測定]](./measure.md) | 買い物かごの作成など、新しい製品リストの初期化。 |
| `productListRemovals` | [[!UICONTROL 測定]](./measure.md) | 製品リストからの製品エントリの削除（買い物かごからの製品の削除など）。 |
| `productListReopens` | [[!UICONTROL 測定]](./measure.md) | 以前に破棄された製品リストで、ユーザーによって再アクティブ化されたもの。 |
| `productListViews` | [[!UICONTROL 測定]](./measure.md) | 製品リストの表示または表示がいつおこなわれたかを示します。 |
| `productViews` | [[!UICONTROL 測定]](./measure.md) | 個々の製品の表示がいつおこなわれたかを説明します。 |
| `purchases` | [[!UICONTROL 測定]](./measure.md) | 注文がいつ受け入れられたかを追跡するために使用します。 コマースコンバージョンで必要なアクションは購入イベントのみです。 購入イベントでは、製品リストが参照されている必要があります。 |
| `saveForLaters` | [[!UICONTROL 測定]](./measure.md) | 製品リストは、ウィッシュリストなど、今後の使用のために保存されます。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json)
