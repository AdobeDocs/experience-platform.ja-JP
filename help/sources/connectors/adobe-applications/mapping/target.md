---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ターゲットマッピング場
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# ターゲットマッピングフィールド

Adobe Experience Platformを使用すると、Adobe Targetソースコネクタを介してTargetデータを取り込むことができます。 コネクタを使用する場合、Targetフィールドのすべてのデータを、XDM ExperienceEventクラスに関連付けられた [Experience Data Model(XDM)](../../../../xdm/home.md) フィールドにマップする必要があります。

次の表に、エクスペリエンスイベントスキーマ(*XDM ExperienceEventフィールド*)のフィールドと、マップする必要がある対応するTargetフィールド(*Targetリクエストフィールド*)の概要を示します。 一部のマッピングに関する追加の注意事項も表示されます。

>[!NOTE]
>
>左右にスクロールして、テーブルの全内容を表示してください。

| XDM ExperienceEventフィールド | Targetリクエストフィールド | メモ |
| ------------------------- | -------------------- | ----- |
| **`id`** | 一意の要求識別子 |
| **`dataSource`** |  | すべてのクライアントに対して「1」に設定されています。 |
| `dataSource._id` | 要求で渡すことができない、システム生成値。 | このデータソースの一意のID。 これは、データソースを作成した個人またはシステムが提供します。 |
| `dataSource.code` | 要求で渡すことができない、システム生成値。 | 完全な@idへのショートカット。 コードまたは@idの少なくとも1つを使用できます。 このコードは、データソース統合コードと呼ばれる場合もあります。 |
| `dataSource.tags` | 要求で渡すことができない、システム生成値。 | タグは、特定のデータソースで表されるエイリアスが、それらのエイリアスを使用するアプリケーションでどのように解釈されるかを示すために使用されます。<br><br>例：<br><ul><li>`isAVID`: Analytics訪問者IDを表すデータソース。</li><li>`isCRSKey`: CRSでキーとして使用する必要があるエイリアスを表すデータソース。</li></ul>タグは、データソースの作成時に設定されますが、特定のデータソースを参照する際にパイプラインメッセージにも含まれます。 |
| **`timestamp`** | イベントタイムスタンプ |
| **`channel`** | `context.channel` | 表示配信でのみ機能します。 オプションは「web」と「mobile」で、「web」がデフォルトです。 |
| **`endUserIds`** |
| `endUserIds.experience.tntId` | `tntId/mboxPC` |
| `endUserIds.experience.mcId` | `marketingCloudVisitorId` |
| **`environment`** |
| `environment.browserDetails.userAgent` | `mboxRequest.userAgent` |
| `environment.browserDetails.viewPortHeight` | `mboxRequest.browserHeight` |
| `environment.browserDetails.viewPortWidth` | `mboxRequest.browserWidth` |
| `environment.operatingSystem` | `deviceAtlas.osName` |
| `environment.operatingSystemVersion` | `deviceAtlas.osVersion` |
| `environment.viewportHeight` | `mboxRequest.screenHeight` |
| `environment.viewportWidth` | `mboxRequest.screenWidth` |
| `environment.colorDepth` | `mboxRequest.colorDepth` |
| `environment.carrier` | リクエストのIPアドレスに基づいて解決された携帯電話会社名。 |
| `environment.ipV4` | `mboxRequest.ipAddress` （V4形式の場合） |
| `environment.ipV6` | `mboxRequest.ipAddress` （V6形式の場合） |
| **`experience`** |
| `experience.target.clientCode` | `mboxRequest.client` |
| `experience.target.mboxName` | `mboxRequest.mboxName` |
| `experience.target.mboxVersion` | `mboxRequest.mboxVersion` |
| `experience.target.sessionId` | `mboxRequest.sessionId` |
| `experience.target.environmentID` | 顧客定義環境に対するTargetの内部マッピング（dev、qa、prodなど）。 |
| `experience.target.supplementalDataID` | AnalyticsイベントでTargetイベントをステッチするために使用される識別子 |
| `experience.target.pageDetails.pageId` | `mboxRequest.pageId` |
| `experience.target.pageDetails.pageScore` | `mboxRequest.mboxPageValue` |
| `experience.target.activities` | 訪問者が認定したアクティビティのリスト（配列） |
| `experience.target.activities[i].activityID` | 訪問者が資格を持つ任意のアクティビティのID |
| `experience.target.activities[i].version` | 訪問者が対象とする任意のアクティビティのバージョン |
| `experience.target.activities[i].activityEvents` | ユーザーがこのイベントでヒットしたアクティビティイベントの詳細が含まれます。 |
| **`device`** |
| `device.typeIDService` | `XDMDevice.Device.TypeIDService.typeIDService_deviceatlas` |
| `device.type` | 次のいずれかのプロパティ `deviceAtlas` （またはNULL）: <ul><li>`type_mobile`</li><li>`type_tablet`</li><li>`type_desktop`</li><li>`type_ereader`</li><li>`type_television`</li><li>`type_settop`</li><li>`type_mediaplayer`</li></ul> |
| `device.typeID` | （空の文字列） |
| `device.manufacturer` | `deviceAtlas.manufacturer` |
| `device.model` | `deviceAtlas.model` |
| `device.modelNumber` | （空の文字列） |
| `device.screenHeight` | `deviceAtlas.displayHeight` |
| `device.screenWidth` | `deviceAtlas.displayWidth` |
| `device.colorDepth` | `deviceAtlas.displayColorDepth` |
| **`placeContext`** |
| `placeContext.geo.id` | ランダムなUUID（必須） |
| `placeContext.geo.city` | 要求のIPアドレスに基づいて解決された都市名。 |
| `placeContext.geo.countryCode` | リクエストのIPアドレスに基づいて解決される国コード。 |
| `placeContext.geo.dmaId` | 指定された市場地域コードは、要求のIPアドレスに基づいて解決されます。 |
| `placeContext.geo.postalCode` | リクエストのIPアドレスに基づいて解決される郵便番号です。 |
| `placeContext.geo.stateProvince` | 要求のIPアドレスに基づいて解決された州または郡。 |
| `placeContext.localTime` | `mboxRequest.offsetTime` + `mboxRequest.currentServerTime` |
| **`commerce`** |  | 注文の詳細がリクエストに存在する場合にのみ設定します。 |
| `commerce.order.priceTotal` | `mboxRequest.orderTotal` |
| `commerce.order.purchaseOrderNumber` | `mboxRequest.orderId` |
| `commerce.order.purchaseID` | `mboxRequest.orderId` |
| **`web`** |
| `web.withWebPageDetails.url` | `mboxURL.context.address.url` |
| `web.webReferrer.url` | `mboxReferrer.context.address.url` |
| **`identityMap`** |
| `identityMap.TNTID` | `tntId.mboxPC` |
| `identityMap.ECID` | `marketingCloudVisitorId` |
