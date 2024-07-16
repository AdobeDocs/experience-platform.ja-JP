---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；コマース；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: Commerce データタイプ
description: Commerce Experience Data Model （XDM）データタイプについて説明します。
exl-id: c9cc569b-1a91-4a6e-8bfd-7f8ec07d01d4
source-git-commit: f70ca0d8ab0e92cc0e1007021c0778361701dc84
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 11%

---

# [!UICONTROL Commerce] データ型

[!UICONTROL Commerce] は、売買に関連するレコードを記述した標準のエクスペリエンスデータモデル（XDM）データタイプです。

![[!UICONTROL Commerce] データタイプの図。](../images/data-types/commerce.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|------------------------------------------|-----------------------|------------------------------------|----------------------------------------------------------------------------------------------------------|
| [!UICONTROL Order] | `order` | [[!UICONTROL Order]](./order.md) | 1 つ以上の商品に対して行われた注文を表します。 |
| [!UICONTROL  プロモーション ID] | `promotionID` | [!UICONTROL  文字列 ] | 注文のプロモーション識別子（存在する場合）。 |
| [!UICONTROL  買い物かごの放棄 ] | `cartAbandons` | [[!UICONTROL 測定]](./measure.md) | 製品リストがユーザーによってアクセスまたは購入できなくなったと識別された場合を表します。 |
| [!UICONTROL  チェックアウト ] | `checkouts` | [[!UICONTROL 測定]](./measure.md) | 商品リストのチェックアウトプロセス中のアクション。 チェックアウトプロセスに複数のステップがある場合、複数のチェックアウトイベントが存在する可能性があります。 複数のステップがある場合、イベント時間情報と参照されるページまたはエクスペリエンスを使用して、ステップと個々のイベントを順番に識別します。 |
| [!UICONTROL  商品リスト （買い物かご）追加数 ] | `productListAdds` | [[!UICONTROL 測定]](./measure.md) | 商品リストへの商品の追加（買い物かごへの追加など）。 |
| [!UICONTROL  商品リスト（買い物かご）が開きました ] | `productListOpens` | [[!UICONTROL 測定]](./measure.md) | 買い物かごが作成されるなど、新しい商品リストの初期化。 |
| [!UICONTROL  商品リスト （買い物かご）削除数 ] | `productListRemovals` | [[!UICONTROL 測定]](./measure.md) | 商品リストからの商品の削除（買い物かごから削除される商品など）。 |
| [!UICONTROL  商品リスト （買い物かご）再開数 ] | `productListReopens` | [[!UICONTROL 測定]](./measure.md) | 以前に放棄された、ユーザーによって再アクティブ化された製品リスト。 |
| [!UICONTROL  商品リスト（買い物かご）表示数 ] | `productListViews` | [[!UICONTROL 測定]](./measure.md) | 商品リストの 1 つまたは複数の表示が発生したタイミングを表します。商品リストの 1 つまたは複数の表示が発生しました。 |
| [!UICONTROL  製品表示 ] | `productViews` | [[!UICONTROL 測定]](./measure.md) | 個々の商品の 1 つまたは複数の表示が発生したタイミングを表します。 |
| [!UICONTROL 購入] | `purchases` | [[!UICONTROL 測定]](./measure.md) | 注文が受理された日時をトラッキングするために使用されます。 購入イベントは、コマースコンバージョンで唯一必要なアクションです。 購入イベントでは商品リストが参照されている必要があります。 |
| [!UICONTROL  後で使用するために保存 ] | `saveForLaters` | [[!UICONTROL 測定]](./measure.md) | 後で使用するために商品リストを保存するタイミングを表します（ウィッシュリストなど）。 |
| [!UICONTROL  店舗での購入 ] | `inStorePurchase` | [[!UICONTROL 測定]](./measure.md) | 「inStore」購入を示します。 この情報は、分析用に保存されます。 |
| [!UICONTROL  買い物かご ] | `cart` | [[!UICONTROL  買い物かご ]](./cart.md) | 1 つ以上の商品を含む買い物かごのプロパティ。 |
| [!UICONTROL  送料 ] | `shipping` | [[!UICONTROL  送料 ]](./shipping.md) | 1 つ以上の商品に関する配送の詳細。 |
| [!UICONTROL 課金] | `billing` | [[!UICONTROL  請求 ]](#billing) | 1 つ以上の支払いに対する請求の詳細。 |
| [!UICONTROL  即時購入 ] | `instantPurchase` | [[!UICONTROL 測定]](./measure.md) | 商品が即座に購入され、買い物かごやチェックアウトをスキップした可能性を表します。 |
| [!UICONTROL  購買依頼リストのオープン ] | `requisitionListOpens` | [[!UICONTROL 測定]](./measure.md) | 新規購買依頼リストの初期化を示します。 |
| [!UICONTROL  購買依頼リスト削除 ] | `requisitionListDeletes` | [[!UICONTROL 測定]](./measure.md) | 購買依頼リストの削除を示します。 |
| [!UICONTROL  購買依頼リスト追加数 ] | `requisitionListAdds` | [[!UICONTROL 測定]](./measure.md) | 要求リストに製品を追加することを示します。 |
| [!UICONTROL  購買依頼リストの削除 ] | `requisitionListRemovals` | [[!UICONTROL 測定]](./measure.md) | 購買依頼製品リストからの製品の削除を示します。 |
| [!UICONTROL  購買依頼表 ] | `requisitionList` | [[!UICONTROL requisitionlist]](./requisition-list.md) | 顧客が作成した要求リストのプロパティ。 |
| [!UICONTROL スコープ] | `commerceScope` | [[!UICONTROL commercescope]](./commerce-scope.md) | イベントが発生した場所（ストア表示、ストア、web サイトなど）のコマース範囲識別子。 |

{style="table-layout:auto"}

## [!UICONTROL  請求 ] データタイプ {#billing}

[!UICONTROL  請求 ] は、請求の詳細に関する情報を含む、標準の Experience Data Model （XDM）データタイプです。 特に、請求先住所に焦点を当てています。

![ 請求データタイプの図。](../images/data-types/billing.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|-------------------------------|-----------------|-----------------|--------------------------|
| [!UICONTROL  請求先住所 ] | `address` | [[!UICONTROL  郵送先住所 ]](./postal-address.md) | 請求先住所。 |

{style="table-layout:auto"}

[!UICONTROL Commerce] のデータタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json)
