---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；コマース；データ型；データ型；
solution: Experience Platform
title: コマースデータタイプ
description: このドキュメントでは、Commerce Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: c9cc569b-1a91-4a6e-8bfd-7f8ec07d01d4
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 12%

---

# [!UICONTROL コマース] データタイプ

[!UICONTROL コマース] は、購入および販売アクティビティに関連するレコードを記述する標準 Experience Data Model(XDM) データタイプです。

<img src="../images/data-types/commerce.PNG" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `order` | [[!UICONTROL Order]](./order.md) | 1 つ以上の製品の注文を表します。 |
| `cartAbandons` | [[!UICONTROL 測定]](./measure.md) | 製品リストが、ユーザーによってアクセスまたは購入できなくなったと識別されたタイミングを説明するために使用されます。 |
| `checkouts` | [[!UICONTROL 測定]](./measure.md) | 製品リストのチェックアウトプロセス中のアクション。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数の手順がある場合、イベント時間情報と参照先のページまたはエクスペリエンスを使用して、手順と、順番に表される個々のイベントを識別します。 |
| `inStorePurchase` | [[!UICONTROL 測定]](./measure.md) | Analytics での使用に関連する値を店舗での購入と関連付けます。 |
| `productListAdds` | [[!UICONTROL 測定]](./measure.md) | 商品リストへの商品の追加（買い物かごに追加される商品など）。 |
| `productListOpens` | [[!UICONTROL 測定]](./measure.md) | 新しい製品リストの初期化（作成中の買い物かごなど）。 |
| `productListRemovals` | [[!UICONTROL 測定]](./measure.md) | 製品リストからの製品エントリの削除（買い物かごからの製品の削除など）。 |
| `productListReopens` | [[!UICONTROL 測定]](./measure.md) | 以前に破棄された製品リスト。ユーザーによって再アクティブ化されました。 |
| `productListViews` | [[!UICONTROL 測定]](./measure.md) | 製品リストの表示がいつおこなわれたかを表します。 |
| `productViews` | [[!UICONTROL 測定]](./measure.md) | 個々の製品の表示がいつ発生したかを表示します。 |
| `purchases` | [[!UICONTROL 測定]](./measure.md) | 注文が許可されたタイミングを追跡するために使用します。 コマースコンバージョンで必要なアクションは、購入イベントのみです。 購入イベントでは、製品リストが参照されている必要があります。 |
| `saveForLaters` | [[!UICONTROL 測定]](./measure.md) | 製品リストは、ウィッシュリストなど今後の使用のために保存されます。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json)
