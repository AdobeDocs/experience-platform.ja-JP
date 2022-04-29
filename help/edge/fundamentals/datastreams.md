---
title: データストリームの設定
description: クライアントサイドの Experience Platform SDK 統合を、アドビ製品およびサードパーティの宛先と接続します。
keywords: 設定；datastreams;datastreamId;edge;datastream id；環境設定；edgeConfigId;ID 同期有効；ID 同期コンテナ ID；サンドボックス；ストリーミングインレット；イベントデータセット；ターゲットコード；クライアントコード；Cookie 宛先；Analytics 設定ブロックスイート ID；データデータ収集の準備；データ準備；マッパー；XDM マッパー；エッジでのマッパー；
exl-id: 736c75cb-e290-474e-8c47-2a031f215a56
source-git-commit: 2275a32cfa9419d2ca407dd48a15f8d06354cd49
workflow-type: tm+mt
source-wordcount: '2081'
ht-degree: 4%

---

# データストリームの設定

datastream は、Adobe Experience Platform Web および Mobile SDK を実装する際のサーバー側の設定を表します。 また、 [設定コマンド](configuring-the-sdk.md) は、クライアントで処理する必要のあるものを制御します ( 例えば、 `edgeDomain`)、データストリームは、SDK のその他すべての設定を処理します。 リクエストがAdobe Experience Platform Edge Network に送信されると、 `edgeConfigId` は、データストリームを参照するために使用されます。 これにより、Web サイトでコードを変更しなくても、サーバー側の設定を更新できます。

このドキュメントでは、データ収集 UI でデータストリームを設定する手順を説明します。

>[!NOTE]
>
>UI でこの機能にアクセスするには、組織がこの機能のプロビジョニングを受ける必要があります。 次の事項を入力してください [フォーム](https://adobe.ly/websdkaccess) 必要なアクセスをリクエストします。 データストリームを管理するには、ユーザーアカウントを製品プロファイルに追加して、 [!DNL Adobe Experience Platform].

## 次にアクセス： [!UICONTROL データストリーム] workspace

データ収集 UI でデータストリームを作成および管理するには、次を選択します。 **[!UICONTROL データストリーム]** をクリックします。

![データ収集 UI の「データストリーム」タブ](../images/datastreams/datastreams-tab.png)

この [!UICONTROL データストリーム] 「 」タブには、既存のデータストリームのリストが表示されます。これには、わかりやすい名前、ID、最終変更日が含まれます。 データストリームの名前を選択 [詳細の表示とサービスの設定](#view-details).

「その他」のアイコン (**...**) をクリックして、その他のオプションを表示します。 選択 **[!UICONTROL 編集]** を更新するには、 [基本設定](#configure) データストリームの場合は、 **[!UICONTROL 削除]** データストリームを削除します。

![データストリームを編集または削除するオプションと既存のデータストリーム](../images/datastreams/edit-datastream.png)

## 新しいデータストリームの作成 {#create}

データストリームを作成するには、まず「 」を選択します。 **[!UICONTROL 新規データストリーム]**.

![新規データストリームを選択](../images/datastreams/new-datastream-button.png)

### [!UICONTROL 設定] {#configure}

設定手順から、データストリーム作成ワークフローが表示されます。 ここから、データストリームの名前と説明（オプション）を入力する必要があります。

このデータストリームをExperience Platformで使用するように設定し、Platform Web SDK を使用する場合は、 [イベントベースのエクスペリエンスデータモデル (XDM) スキーマ](../../xdm/classes/experienceevent.md) 取り込む予定のデータを表す。

![データストリームの基本設定](../images/datastreams/configure.png)

選択 **[!UICONTROL 詳細オプション]** をクリックすると、データストリームを設定する追加のコントロールが表示されます。

![詳細設定オプション](../images/datastreams/advanced-options.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL 地域] | ユーザーの IP アドレスに基づいて GPS 検索を行うかどうかを指定します。 デフォルト設定 **[!UICONTROL なし]** GPS 検索を無効にし、 **[!UICONTROL 市区町村]** を設定すると、小数点以下 2 桁までの GPS 座標が表示されます。 |
| [!UICONTROL ファーストパーティ ID Cookie] | この設定が有効な場合、 [ファーストパーティデバイス ID](../identity/first-party-device-ids.md)を使用します。<br><br>この設定を有効にする場合、ID の保存先となる Cookie の名前を指定する必要があります。 |
| [!UICONTROL サードパーティ ID の同期] | ID 同期をコンテナにグループ化して、ID 同期を異なる時間に実行できるようにします。 この設定を有効にすると、このデータストリームに対して実行する ID 同期のコンテナを指定できます。 |

この節の残りの部分では、選択した Platform イベントスキーマにデータをマッピングする手順に焦点を当てます。 Mobile SDK を使用している場合や、Platform 用にデータストリームを設定していない場合は、 **[!UICONTROL 保存]** ～に関する次の項に進む前に [データストリームへのサービスの追加](#add-services).

### データ収集のためのデータ準備 {#data-prep}

>[!IMPORTANT]
>
>データ収集用のデータ準備は、現在、Mobile SDK 実装ではサポートされていません。

データ準備は、Experience Data Model(XDM) との間でデータのマッピング、変換、検証をおこなえるExperience Platformサービスです。 Platform が有効なデータストリームを設定する場合、Data Prep 機能を使用して、Platform Edge ネットワークに送信する際にソースデータを XDM にマッピングできます。

>[!NOTE]
>
>計算フィールドの変換関数を含む、すべての Data Prep 機能に関する包括的なガイダンスについては、次のドキュメントを参照してください。
>
>* [データ準備の概要](../../data-prep/home.md)
>* [データ準備のマッピング機能](../../data-prep/functions.md)
>* [Data Prep でのデータ形式の取り扱い](../../data-prep/data-handling.md)


以下のサブセクションでは、データ収集 UI 内でデータをマッピングするための基本的な手順について説明します。 これらの手順の簡単なデモについては、次のビデオを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342120?quality=12&enable10seconds=on&speedcontrol=on)

#### [!UICONTROL データの選択]

選択 **[!UICONTROL マッピングの保存と追加]** 完了後 [基本設定手順](#configure)、および **[!UICONTROL データを選択]** 手順が表示されます。 ここから、Platform に送信する予定のデータの構造を表すサンプル JSON オブジェクトを提供する必要があります。

この JSON オブジェクトを構築して、取得するデータレイヤーのプロパティにマッピングできるようにする必要があります。 以下のセクションを選択して、適切に書式設定された JSON オブジェクトの例を表示します。

+++サンプル JSON ファイル

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

>[!IMPORTANT]
>
>JSON オブジェクトには 1 つのルートノードが必要です `data` 検証に合格するために。

このオプションを選択して、オブジェクトをファイルとしてアップロードするか、生のオブジェクトを指定されたテキストボックスに貼り付けることができます。 JSON が有効な場合は、右側のパネルにプレビュースキーマが表示されます。 「**[!UICONTROL 次へ]**」をクリックして続行します。

![予想される受信データの JSON サンプル](../images/datastreams/select-data.png)

#### [!UICONTROL マッピング]

この **[!UICONTROL マッピング]** 手順が表示され、ソースデータのフィールドを Platform のターゲットイベントスキーマのフィールドにマッピングできます。 利用を開始するには、「 **[!UICONTROL 新しいマッピングを追加]** 新しいマッピング行を作成します。

![新しいマッピングの追加](../images/datastreams/add-new-mapping.png)

ソースアイコン (![ソースアイコン](../images/datastreams/source-icon.png)) をクリックし、表示されるダイアログで、提供されたキャンバスにマッピングするソースフィールドを選択します。 フィールドを選択したら、 **[!UICONTROL 選択]** ボタンをクリックして続行します。

![ソーススキーマにマッピングするフィールドの選択](../images/datastreams/source-mapping.png)

次に、スキーマアイコン (![スキーマアイコン](../images/datastreams/schema-icon.png)) をクリックして、ターゲットイベントスキーマに類似したダイアログを開きます。 でを確認する前に、データのマッピング先のフィールドを選択します。 **[!UICONTROL 選択]**.

![ターゲットスキーマにマッピングするフィールドの選択](../images/datastreams/target-mapping.png)

マッピングページが再び表示され、完了したフィールドマッピングが表示されます。 この **[!UICONTROL マッピングの進行状況]** 「 」セクションが更新され、正常にマッピングされたフィールドの合計数が反映されます。

![フィールドが正常にマッピングされ、進行状況が反映されました](../images/datastreams/field-mapped.png)

>[!TIP]
>
>（ソースフィールド内の）オブジェクトの配列を（ターゲットフィールド内の）異なるオブジェクトの配列にマッピングする場合は、 `[*]` の後に追加します。
>
>![配列オブジェクトのマッピング](../images/datastreams/array-object-mapping.png)

上記の手順に従って、残りのフィールドをターゲットスキーマにマッピングします。 使用可能なすべてのソースフィールドをマッピングする必要はありませんが、この手順を完了するには、必要に応じて設定されたターゲットスキーマ内のフィールドをマッピングする必要があります。 この **[!UICONTROL 必須フィールド]** カウンターは、現在の設定でまだマッピングされていない必須フィールドの数を示します。

必須フィールドの数が 0 に達し、マッピングに問題がなければ、「 」を選択します。 **[!UICONTROL 保存]** 変更を確定します。

![マッピングが完了しました](../images/datastreams/mapping-complete.png)

## データストリームの詳細を表示 {#view-details}

新しいデータストリームを設定するか、表示する既存のデータストリームを選択すると、そのデータストリームの詳細ページが表示されます。 ID など、データストリームに関する詳細情報は、ここで確認できます。

![作成されたデータストリームの詳細ページ](../images/datastreams/view-details.png)

データストリームの詳細画面から、次の操作を実行できます。 [サービスの追加](#add-services) アクセス権のあるAdobe Experience Cloud製品の機能を有効にする。

## データストリームへのサービスの追加 {#add-services}

データストリームの詳細ページで、 **[!UICONTROL サービスを追加]** をクリックして、そのデータストリームで使用可能なサービスの追加を開始します。

![「サービスを追加」を選択して続行します](../images/datastreams/add-service.png)

次の画面では、ドロップダウンメニューを使用して、このデータストリーム用に設定するサービスを選択します。 このリストには、アクセス権のあるサービスのみが表示されます。

![リストからサービスを選択](../images/datastreams/service-selection.png)

目的のサービスを選択し、表示される設定オプションを入力して、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、サービスをデータストリームに追加します。 データストリームの詳細表示には、追加されたすべてのサービスが表示されます。

![データストリームに追加されたサービス](../images/datastreams/services-added.png)

以下のサブセクションでは、各サービスの設定オプションについて説明します。

>[!NOTE]
>
>各サービス設定には、 **[!UICONTROL 有効]** サービスが選択されたときに自動的にアクティブ化される切り替え。 このデータストリームに対して選択したサービスを無効にするには、 **[!UICONTROL 有効]** もう一度切り替えます。

### Adobe Analytics設定

このサービスは、データをAdobe Analyticsに送信するかどうかと方法を制御します。 詳しくは、 [Analytics にデータを送信する](../data-collection/adobe-analytics/analytics-overview.md).

![Adobe Analytics Settings Block](../images/datastreams/analytics-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL レポートスイート ID] | **（必須）** データの送信先の Analytics レポートスイートの ID。 この ID は、Adobe Analytics UI の下にあります。 [!UICONTROL 管理者] > [!UICONTROL レポートスイート]. 複数のレポートスイートが指定されている場合、データは各レポートスイートにコピーされます。 |

### Adobe Audience Manager設定

このサービスは、データをAdobe Audience Managerに送信するかどうかと方法を制御します。 データをAudience Managerに送信するために必要なのは、このセクションを有効にすることだけです。 その他の設定はオプションですが、推奨されます。

![AdobeAudience Manager 設定ブロック](../images/datastreams/audience-manager-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Cookie の宛先が有効になっています] | SDK が [cookie の宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-cookie-destination.html) から [!DNL Audience Manager]. |
| [!UICONTROL URL の宛先が有効になっています] | SDK が [URL の宛先](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/destinations/custom-destinations/create-url-destination.html) から [!DNL Audience Manager]. |

### Adobe Experience Platform 設定

>[!IMPORTANT]
>
>Platform のデータストリームを有効にする場合は、データ収集 UI の上部のリボンに表示されるように、現在使用している Platform サンドボックスを控えておきます。
>
>![選択したサンドボックス](../images/datastreams/platform-sandbox.png)
>
>サンドボックスは、Adobe Experience Platformの仮想パーティションで、データと実装を組織内の他のユーザーから分離できます。 データストリームを作成した後は、そのサンドボックスを変更できません。 Experience Platformでのサンドボックスの役割について詳しくは、 [サンドボックスドキュメント](../../sandboxes/home.md).

このサービスは、データをAdobe Experience Platformに送信するかどうかと方法を制御します。

![Adobe Experience Platform設定ブロック](../images/datastreams/platform-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL イベントデータセット] | **（必須）** 顧客イベントデータのストリーミング先の Platform データセットを選択します。 このスキーマでは [XDM ExperienceEvent クラス](../../xdm/classes/experienceevent.md). |
| [!UICONTROL プロファイルデータセット] | 顧客属性データの送信先の Platform データセットを選択します。 このスキーマでは [XDM Individual Profile クラス](../../xdm/classes/individual-profile.md). |
| [!UICONTROL Offer Decisioning] | Platform Web SDK 実装のOffer decisioningを有効にするには、このチェックボックスを選択します。 詳しくは、 [Platform Web SDK でのOffer decisioningの使用](../personalization/offer-decisioning/offer-decisioning-overview.md) を参照してください。 offer decisioning機能について詳しくは、 [Adobe Journey Optimizerドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/offer-decisioniong/get-started/starting-offer-decisioning.html?lang=ja). |
| [!UICONTROL エッジセグメント化] | 有効にするには、このチェックボックスを選択します [エッジセグメント化](../../segmentation/ui/edge-segmentation.md) このデータストリーム用。 SDK がエッジセグメント化対応データストリームを介してデータを送信すると、該当するプロファイルの更新済みのセグメントメンバーシップが応答に返されます。<br><br>このオプションは、 [!UICONTROL パーソナライズ機能の宛先] 対象 [次のページのパーソナライゼーションの使用例](../../destinations/ui/configure-personalization-destinations.md). |
| [!UICONTROL パーソナライズ機能の宛先] | を [!UICONTROL エッジセグメント化] 「 」チェックボックスにチェックを入れると、データストリームがAdobe Targetなどのパーソナライゼーションエンジンに接続できるようになります。 の具体的な手順については、宛先のドキュメントを参照してください。 [パーソナライゼーションの宛先の設定](../../destinations/ui/configure-personalization-destinations.md). |

### Adobe Target設定

このサービスは、データをAdobe Targetに送信するかどうかと方法を制御します。

![Adobe Target設定ブロック](../images/datastreams/target-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL プロパティトークン] | [!DNL Target] を使用すると、プロパティを使用して権限を制御できます。 プロパティについて詳しくは、 [enterprise 権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja) 内 [!DNL Target] ドキュメント。<br><br>プロパティトークンは、Adobe Target UI の下にあります。 [!UICONTROL 設定] > [!UICONTROL プロパティ]. |
| [!UICONTROL Target 環境 ID] | [Adobe Targetの環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) は、あらゆる開発段階を通じて実装を管理するのに役立ちます。 この設定は、このデータストリームで使用する環境を指定します。<br><br>ベストプラクティスは、これを各 `dev`, `stage`、および `prod` データストリーム環境を使用して、操作をシンプルにします。 ただし、既にAdobe Target環境を定義している場合は、それらを使用できます。 |
| [!UICONTROL Target サードパーティ ID 名前空間] | の ID 名前空間 `mbox3rdPartyId` このデータストリームにを使用します。 詳しくは、 [実装 `mbox3rdPartyId` Web SDK を使用](../personalization/adobe-target/using-mbox-3rdpartyid.md) を参照してください。 |

### [!UICONTROL イベント転送] 設定

このサービスは、データの送信先と送信方法を制御します [イベント転送](../../tags/ui/event-forwarding/overview.md).

![設定 UI の「イベント転送」セクション](../images/datastreams/event-forwarding-config.png)

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Launch プロパティ] | **（必須）** データの送信先のイベント転送プロパティ。 |
| [!UICONTROL Launch 環境] | **（必須）** データの送信先となる、選択したプロパティ内の環境。 |

>[!NOTE]
>
>次を選択できます。 **[!UICONTROL ID を手動で入力]** をクリックして、ドロップダウンメニューを使用する代わりにプロパティ名と環境名を入力します。

## 次の手順

このガイドでは、データ収集 UI でのデータストリームの設定方法について説明しました。 データストリームの設定後に Web SDK をインストールおよび設定する方法について詳しくは、 [データ収集 E2E ガイド](../../collection/e2e.md#install).
