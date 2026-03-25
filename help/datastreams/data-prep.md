---
title: データ収集のためのデータ準備
description: Adobe Experience Platform Web および Mobile SDK のデータストリームを設定する際に、エクスペリエンスデータモデル（XDM）イベントスキーマにデータをマッピングする方法について説明します。
exl-id: 87a70d56-1093-445c-97a5-b8fa72a28ad0
source-git-commit: bdcea238740661b453032bbab3ec7e414efd63e3
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 41%

---

# データ収集のためのデータ準備

Data Prepは、[Experience Data Model （XDM） &#x200B;](../xdm/home.md)との間でデータをマッピング、変換、検証するために使用できるAdobe Experience Platform サービスです。 Experience Platform対応の[&#x200B; データストリーム &#x200B;](./overview.md)を設定する場合、データ準備機能を使用して、ソースデータをExperience Platform Edge Networkに送信する際にXDMにマッピングできます。

web ページから送信されるすべてのデータは、XDMとしてExperience Platformに格納される必要があります。 ページ上のデータレイヤーからExperience Platformが受け入れるXDMにデータを変換するには、次の3つの方法があります。

1. Web ページ自体のXDMにデータレイヤーを再フォーマットします。
2. タグのネイティブデータ要素機能を使用して、web ページの既存のデータレイヤー形式をXDMに再フォーマットします。
3. Data Prep for Data Collectionを使用して、web ページの既存のデータレイヤーフォーマットをEdge Networkを介してXDMに再フォーマットします。

このガイドでは、3つ目のオプションについて説明します。

## データ収集にデータ準備を使用する場合 {#when-to-use-data-prep}

データ収集用のデータ準備が有効な使用例は2つあります。

1. このweb サイトには、適切に形成され、管理され、管理されたデータレイヤーがあり、JavaScriptを使用してページ上のXDMに変換するのではなく、Edge Networkに直接送信することが好まれます（タグデータ要素を使用するか、JavaScriptを手動で操作します）。
2. タグ以外のタグ付けシステムがサイトにデプロイされます。

## WebSDK経由で既存のデータレイヤーをEdge Networkに送信する {#send-datalayer-via-websdk}

既存のデータレイヤーは、[`data`](/help/collection/js/commands/sendevent/data.md) コマンド内の`sendEvent` オブジェクトを使用して送信する必要があります。

タグを使用している場合は、**[!UICONTROL Data]** アクションタイプの[**[!UICONTROL Send Event]**](/help/tags/extensions/client/web-sdk/actions/send-event.md) フィールドを使用する必要があります。

このガイドの残りの部分では、データレイヤーをWeb SDKによって送信された後にXDM標準にマッピングする方法について説明します。

>[!NOTE]
>
>計算フィールドの変換機能を含む、すべてのデータ準備機能に関する包括的なガイダンスについては、次のドキュメントを参照してください。
>
>* [データ準備の概要](../data-prep/home.md)
>* [データ準備のマッピング機能](../data-prep/functions.md)
>* [Data Prep でのデータ形式の取り扱い](../data-prep/data-handling.md)

このガイドでは、UI 内のデータのマッピング方法を説明します。手順に従って、データストリームの作成から[基本設定手順](./overview.md#create)までのプロセスを開始します。

データ収集のためのデータ準備プロセスの簡単なデモについては、次のビデオを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/345566?captions=jpn&quality=12&enable10seconds=on&speedcontrol=on)

## [!UICONTROL Select data] {#select-data}

データストリームの基本設定を完了した後で「**[!UICONTROL Save and Add Mapping]**」を選択すると、**[!UICONTROL Select data]** ステップが表示されます。 ここから、Experience Platformに送信する予定のデータの構造を表すサンプル JSON オブジェクトを指定する必要があります。

データレイヤーから直接プロパティを取得するには、JSON オブジェクが単一のルートプロパティ `data` を持つ必要があります。次に、`data` オブジェクトのサブプロパティは、キャプチャするデータレイヤープロパティにマッピングする方法で構築する必要があります。 次のセクションを選択すると、`data` ルートを持つ適切にフォーマットされた JSON オブジェクトの例が表示されます。

+++`data` ルートを含むJSON ファイルのサンプル

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

+++`xdm` ルートを含むJSON ファイルのサンプル

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

オブジェクトをファイルとしてアップロードするオプションを選択するか、提供されたテキストボックスに生のオブジェクトを代わりに貼り付けることができます。JSON が有効な場合、右側のパネルにプレビュースキーマが表示されます。続行するには、**[!UICONTROL Next]**&#x200B;を選択してください。

![受信データのJSON サンプル。](assets/data-prep/select-data.png)

>[!NOTE]
>
>任意のページで使用できるすべてのデータレイヤー要素を表すサンプル JSON オブジェクトを使用します。 例えば、あらゆるページでショッピングカートのデータレイヤー要素が使用されているわけではありません。 ただし、このサンプル JSON オブジェクトには、ショッピングカートのデータレイヤー要素を含める必要があります。

## [!UICONTROL Mapping]

**[!UICONTROL Mapping]** ステップが表示され、ソースデータのフィールドをExperience Platformのターゲットイベントスキーマのフィールドにマッピングできます。 ここから、2 つの方法でマッピングを設定できます。

* 手動プロセスを使用して、このデータストリームの[&#x200B; マッピングルール &#x200B;](#create-mapping)を作成します。
* 既存のデータストリームから[マッピングルールを読み込みます](#import-mapping)。

>[!IMPORTANT]
>
>データ準備マッピングは`identityMap`個のXDM ペイロードを上書きし、Real-Time CDP オーディエンスに対するプロファイルマッチングにさらに影響を与える可能性があります。

### マッピングルールの作成 {#create-mapping}

マッピングルールを作成するには、**[!UICONTROL Add new mapping]**&#x200B;を選択します。

![新しいマッピングを追加しています。](assets/data-prep/add-new-mapping.png)

ソースアイコン（![ソースアイコン](/help/images/icons/source.png)）を選択して、表示されるダイアログで、提供されたキャンバスにマッピングするソースフィールドを選択します。フィールドを選択したら、**[!UICONTROL Select]** ボタンを使用して続行します。

![&#x200B; ソーススキーマでマッピングするフィールドを選択しています。](assets/data-prep/source-mapping.png)

次に、スキーマアイコン（![スキーマアイコン](/help/images/icons/schema.png)）を選択して、ターゲットイベントスキーマ用の同様のダイアログを開きます。データをマッピングするフィールドを選択してから、**[!UICONTROL Select]**&#x200B;を確認してください。

![&#x200B; ターゲットスキーマでマッピングするフィールドを選択しています。](assets/data-prep/target-mapping.png)

マッピングページが再表示され、完成したフィールドマッピングが表示されます。**[!UICONTROL Mapping progress]** セクションが更新され、正常にマッピングされたフィールドの合計数が反映されます。

![進行状況が反映され、フィールドが正常にマッピングされました。](assets/data-prep/field-mapped.png)

>[!TIP]
>
>オブジェクトの配列（ソースフィールド）を異なるオブジェクトの配列（ターゲットフィールド）にマッピングする場合は、次に示すように、ソースフィールドとターゲットフィールドのパスの配列名の後に `[*]` を追加します。
>
>![配列オブジェクトのマッピング。](assets/data-prep/array-object-mapping.png)

### 既存のマッピングルールを読み込む {#import-mapping}

以前にデータストリームを作成したことがある場合は、設定したマッピングルールを新しいデータストリームに再利用できます。

>[!WARNING]
>
>別のデータストリームからマッピングルールを読み込むと、読み込み前に追加したフィールドマッピングが上書きされます。

開始するには、**[!UICONTROL Import Mapping]**&#x200B;を選択します。

![&#x200B; マッピングの読み込みボタンが選択されています。](assets/data-prep/import-mapping-button.png)

表示されるダイアログで、マッピングルールを読み込むデータストリームを選択します。データストリームを選択したら、**[!UICONTROL Preview]**&#x200B;を選択します。

![既存のデータストリームを選択しています。](assets/data-prep/select-mapping-rules.png)

>[!NOTE]
>
>データストリームは、同じ[サンドボックス](../sandboxes/home.md)内でのみ読み込むことができます。つまり、あるサンドボックスから別のサンドボックスにデータストリームを読み込むことはできません。

次の画面に、選択したデータストリームの保存されたマッピングルールのプレビューを示します。表示されるマッピングが期待どおりであることを確認し、**[!UICONTROL Import]**&#x200B;を選択してマッピングを確認し、新しいデータストリームに追加します。

![読み込むマッピングルール。](assets/data-prep/import-mapping-rules.png)

>[!NOTE]
>
>読み込まれたマッピングルールのソースフィールドが、[以前に提供した](#select-data)サンプル JSON データに含まれていない場合、それらのフィールドのマッピングは読み込みに含まれません。

### マッピングの完了

前述の手順を続行して、残りのフィールドをターゲットスキーマにマッピングします。利用可能なすべてのソースフィールドをマッピングする必要はありませんが、必要に応じて設定されたターゲットスキーマのフィールドは、この手順を完了するためにマッピングする必要があります。 **[!UICONTROL Required fields]** カウンターは、現在の設定でまだマッピングされていない必須フィールドの数を示します。

必須フィールド数が0に達し、マッピングに満足したら、**[!UICONTROL Save]**&#x200B;を選択して変更を確定します。

![マッピング完了](assets/data-prep/mapping-complete.png)

## 次の手順

このガイドでは、UI でデータストリームを設定する際の、データの XDM へのマッピング方法について説明しました。一般的なデータストリームのチュートリアルを行っていた場合は、[データストリームの詳細の表示](./overview.md)に関する手順に戻ることができます。
