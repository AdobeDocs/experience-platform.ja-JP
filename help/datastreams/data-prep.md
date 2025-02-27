---
title: データ収集のためのデータ準備
description: Adobe Experience Platform Web および Mobile SDK のデータストリームを設定する際に、エクスペリエンスデータモデル（XDM）イベントスキーマにデータをマッピングする方法について説明します。
exl-id: 87a70d56-1093-445c-97a5-b8fa72a28ad0
source-git-commit: e90bd5abe502a7638ae54fca5eb0f051a925a2d8
workflow-type: tm+mt
source-wordcount: '1199'
ht-degree: 59%

---

# データ収集のためのデータ準備

データ準備は、[エクスペリエンスデータモデル（XDM）](../xdm/home.md)との間でデータのマッピング、変換、検証を可能にする、Adobe Experience Platform サービスです。Platform 対応の[データストリーム](./overview.md)を設定する場合、データ準備機能を使用して、Platform Edge Network に送信する際にソースデータを XDM にマッピングできます。

Web ページから送信されるすべてのデータは、XDM としてExperience Platformに届く必要があります。 ページ上のデータレイヤーからExperience Platformが受け入れる XDM にデータを変換する方法は 3 つあります。

1. Web ページ自体で、データレイヤーを XDM に再フォーマットします。
2. タグネイティブデータ要素機能を使用して、web ページの既存のデータレイヤー形式を XDM に再フォーマットします。
3. データ収集のためのデータ準備を使用して、Edge Networkを通じて web ページの既存のデータレイヤー形式を XDM に再書式設定します。

このガイドでは、3 番目のオプションに焦点を当てています。

## データ収集のためにデータ準備を使用するタイミング {#when-to-use-data-prep}

データ収集のためのデータ準備が役立つユースケースは 2 つあります。

1. Web サイトには、適切に形成、管理、管理されたデータレイヤーがあり、JavaScript操作を使用してページ上で（タグデータ要素を使用して、または手動のEdge Network操作を使用して） XDM に変換する代わりに、JavaScriptに直接送信する環境設定があります。
2. タグ以外のタグ付けシステムがサイトにデプロイされている。

## WebSDK を使用したEdge Networkへの既存のデータレイヤーの送信 {#send-datalayer-via-websdk}

既存のデータレイヤーは、`sendEvent` コマンド内の [`data`](/help/web-sdk/commands/sendevent/data.md) オブジェクトを使用して送信する必要があります。

タグを使用する場合は、**[!UICONTROL Web SDK タグ拡張機能のドキュメント ](/help/tags/extensions/client/web-sdk/action-types.md) に記載されているように、**[!UICONTROL  イベントを送信 ]**アクションタイプの [ データ]** フィールドを使用する必要があります。

このガイドの残りの部分では、WebSDK によって送信されたデータレイヤーを XDM 標準にマッピングする方法について重点的に説明します。

>[!NOTE]
>
>計算フィールドの変換機能を含む、すべてのデータ準備機能に関する包括的なガイダンスについては、次のドキュメントを参照してください。
>
>* [データ準備の概要](../data-prep/home.md)
>* [データ準備のマッピング機能](../data-prep/functions.md)
>* [Data Prep でのデータ形式の取り扱い](../data-prep/data-handling.md)

このガイドでは、UI 内のデータのマッピング方法を説明します。手順に従って、データストリームの作成から[基本設定手順](./overview.md#create)までのプロセスを開始します。

データ収集のためのデータ準備プロセスの簡単なデモについては、次のビデオを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342120?quality=12&enable10seconds=on&speedcontrol=on)

## [!UICONTROL データの選択] {#select-data}

データストリームの基本設定が完了してから「**[!UICONTROL 保存してマッピングを追加]**」を選択すると、**[!UICONTROL データを選択]**&#x200B;手順が表示されます。ここから、Platform に送信する予定のデータの構造を表す、サンプル JSON オブジェクトを指定する必要があります。

データレイヤーから直接プロパティを取得するには、JSON オブジェクが単一のルートプロパティ `data` を持つ必要があります。`data` オブジェクトのサブプロパティは、取得したいデータレイヤープロパティにマッピングするように構築する必要があります。 次のセクションを選択すると、`data` ルートを持つ適切にフォーマットされた JSON オブジェクトの例が表示されます。

+++ `data` ルートを持つサンプル JSON ファイル

```json
{
  "data": {
    "eventMergeId": "cce1b53c-571f-4f36-b3c1-153d85be6602",
    "eventType": "view:load",
    "timestamp": "2021-09-30T14:50:09.604Z",
    "web": {
      "webPageDetails": {
        "siteSection": "Product section",
        "server": "example.com",
        "name": "product home",
        "URL": "https://www.example.com"
      },
      "webReferrer": {
        "URL": "https://www.adobe.com/index2.html",
        "type": "external"
      }
    },
    "commerce": {
      "purchase": 1,
      "order": {
        "orderID": "1234"
      }
    },
    "product": [
      {
        "productInfo": {
          "productID": "123"
        }
      },
      {
        "productInfo": {
          "productID": "1234"
        }
      }
    ],
    "reservation": {
      "id": "anc45123xlm",
      "name": "Embassy Suits",
      "SKU": "12345-L",
      "skuVariant": "12345-LG-R",
      "priceTotal": "112.99",
      "currencyCode": "USD",
      "adults": 2,
      "children": 3,
      "productAddMethod": "PDP",
      "_namespace": {
        "test": 1,
        "priceTotal": "112.99",
        "category": "Overnight Stay"
      },
      "freeCancellation": false,
      "cancellationFee": 20,
      "refundable": true
    }
  }
}
```

+++

XDM オブジェクトデータ要素からプロパティを取得するには、同じルールを JSON オブジェクトに適用しますが、ルートプロパティは、変わりに `xdm` としてキーにする必要があります。次のセクションを選択すると、`xdm` ルートを持つ適切にフォーマットされた JSON オブジェクトの例が表示されます。

+++ `xdm` ルートを持つサンプル JSON ファイル

```json
{
  "xdm": {
    "environment": {
      "type": "browser",
      "browserDetails": {
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_5) AppleWebkit/537.36 (KHTML, like Gecko) Chrome/49.0.2623.112 Safari/537.36",
        "javaScriptEnabled": true,
        "javaScriptVersion": "1.8.5",
        "cookiesEnabled": true,
        "viewportHeight": 900,
        "viewportWidth": 1680,
        "javaEnabled": true
      },
      "domain": "adobe.com",
      "colorDepth": 24,
      "viewportHeight": 1050,
      "viewportWidth": 1680
    },
    "device": {
      "screenHeight": 1050,
      "screenWidth": 1680
    }
  }
}
```

+++

オブジェクトをファイルとしてアップロードするオプションを選択するか、提供されたテキストボックスに生のオブジェクトを代わりに貼り付けることができます。JSON が有効な場合、右側のパネルにプレビュースキーマが表示されます。「**[!UICONTROL 次へ]**」をクリックして続行します。

![ 期待される受信データの JSON サンプル。](assets/data-prep/select-data.png)

>[!NOTE]
>
> 任意のページで使用できるすべてのデータレイヤー要素を表すサンプル JSON オブジェクトを使用します。 例えば、すべてのページが買い物かごデータレイヤー要素を使用しているわけではありません。 ただし、買い物かごのデータレイヤー要素は、このサンプル JSON オブジェクトに含める必要があります。

## [!UICONTROL マッピング]

**[!UICONTROL マッピング]**&#x200B;手順が表示され、ソースデータフィールドを Platform のターゲットイベントスキーマのフィールドにマッピングできます。ここから、2 つの方法でマッピングを設定できます。

* 手動のプロセスでこのデータストリームに対して [ マッピングルールを作成 ](#create-mapping) します。
* 既存のデータストリームから[マッピングルールを読み込みます](#import-mapping)。

>[!IMPORTANT]
>
>データ準備マッピングは XDM ペイロード `identityMap` 上書きします。これにより、Real-Time CDP オーディエンスに対するプロファイルのマッチングにさらに影響を与える可能性があります。

### マッピングルールの作成 {#create-mapping}

マッピングルールを作成するには、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。

![ 新しいマッピングの追加 ](assets/data-prep/add-new-mapping.png)

ソースアイコン（![ソースアイコン](/help/images/icons/source.png)）を選択して、表示されるダイアログで、提供されたキャンバスにマッピングするソースフィールドを選択します。フィールドを選択したら、「**[!UICONTROL 選択]**」ボタンを使用して続行します。

![ ソーススキーマのマッピングされるフィールドを選択。](assets/data-prep/source-mapping.png)

次に、スキーマアイコン（![スキーマアイコン](/help/images/icons/schema.png)）を選択して、ターゲットイベントスキーマ用の同様のダイアログを開きます。データをマッピングするフィールドを選択してから、「**[!UICONTROL 選択]**」で確定します。

![ ターゲットスキーマのマッピングされるフィールドを選択 ](assets/data-prep/target-mapping.png)

マッピングページが再表示され、完成したフィールドマッピングが表示されます。「**[!UICONTROL マッピングの進行状況]**」セクションが更新され、正常にマッピングされたフィールドの合計数が反映されます。

![ フィールドが正常にマッピングされ、進行状況が反映されました。](assets/data-prep/field-mapped.png)

>[!TIP]
>
>オブジェクトの配列（ソースフィールド）を異なるオブジェクトの配列（ターゲットフィールド）にマッピングする場合は、次に示すように、ソースフィールドとターゲットフィールドのパスの配列名の後に `[*]` を追加します。
>
>![ 配列オブジェクトのマッピング。](assets/data-prep/array-object-mapping.png)

### 既存のマッピングルールを読み込む {#import-mapping}

以前にデータストリームを作成したことがある場合、その設定されたマッピングルールを新しいデータストリームで再利用できます。

>[!WARNING]
>
>別のデータストリームからマッピングルールを読み込むと、読み込む前に追加したフィールドマッピングが上書きされます。

開始するには、「**[!UICONTROL マッピングをインポート]**」を選択します。

![ 「マッピングをインポート」ボタンを選択している ](assets/data-prep/import-mapping-button.png)

表示されるダイアログで、マッピングルールを読み込むデータストリームを選択します。データストリームを選択したら、「**[!UICONTROL プレビュー]**」を選択します。

![ 既存のデータストリームの選択 ](assets/data-prep/select-mapping-rules.png)

>[!NOTE]
>
>データストリームは、同じ[サンドボックス](../sandboxes/home.md)内でのみ読み込むことができます。つまり、あるサンドボックスから別のサンドボックスにデータストリームを読み込むことはできません。

次の画面に、選択したデータストリームの保存されたマッピングルールのプレビューを示します。表示されたマッピングが期待どおりのものであることを確認してから、「**[!UICONTROL インポート]**」を選択して確定すると、新しいデータストリームにマッピングが追加されます。

![ 読み込まれるマッピングルール。](assets/data-prep/import-mapping-rules.png)

>[!NOTE]
>
>読み込まれたマッピングルールのソースフィールドが、[以前に提供した](#select-data)サンプル JSON データに含まれていない場合、それらのフィールドのマッピングは読み込みに含まれません。

### マッピングの完了

前述の手順を続行して、残りのフィールドをターゲットスキーマにマッピングします。使用可能なすべてのソースフィールドをマッピングする必要はありませんが、ターゲットスキーマで必須として設定されているフィールドは、この手順を完了するためにマッピングする必要があります。 **[!UICONTROL 必須フィールド]**&#x200B;カウンターは、現在の設定でまだマッピングされていない必須フィールドの数を示します。

必須フィールドのカウントがゼロになって、マッピングに満足したら、「**[!UICONTROL 保存]**」を選択して変更を確定します。

![マッピング完了](assets/data-prep/mapping-complete.png)

## 次の手順

このガイドでは、UI でデータストリームを設定する際の、データの XDM へのマッピング方法について説明しました。一般的なデータストリームのチュートリアルを行っていた場合は、[データストリームの詳細の表示](./overview.md)に関する手順に戻ることができます。
