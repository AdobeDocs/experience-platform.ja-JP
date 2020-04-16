---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ターゲットマッピング場
topic: overview
translation-type: tm+mt
source-git-commit: 9f0200af0310eafbcc1851b089cfc254cb34af8f

---


# ターゲットマッピング場

Adobe Experience Platformを使用すると、アドビのターゲットデータをターゲットソースコネクタで取り込みます。 コネクタを使用する場合、ターゲットフィールドのすべてのデータを、XDM ExperienceEventクラスに関連付けられた [Experience Data Model](../../../xdm/home.md) (XDM)フィールドにマップする必要があります。

次の表に、エクスペリエンスイベントスキーマ(*XDM ExperienceEventフィールド*)のフィールドと、マッピングする必要がある対応するターゲットフィールド(*ターゲットリクエストフィールド*)の概要を示します。 一部のマッピングに関する追加のメモも提供されます。

>[!NOTE] 左右にスクロールして、テーブルの全内容を表示してください。

| XDM ExperienceEventフィールド | ターゲット要求フィールド | メモ |
| ------------------------- | -------------------- | ----- |
| **`id`** | 一意の要求識別子 |
| **`dataSource`** |  | すべてのクライアントに対して「1」に設定されます。 |
| `dataSource._id` | 要求で渡されない、システム生成値。 | このデータソースの一意のID。 これは、データソースを作成した個人またはシステムによって提供されます。 |
| `dataSource.code` | 要求で渡されない、システム生成値。 | 完全な@idへのショートカット。 少なくとも1つのコードまたは@idを使用できます。 このコードは、データソース統合コードと呼ばれる場合があります。 |
| `dataSource.tags` | 要求で渡されない、システム生成値。 | タグは、特定のデータソースで表されるエイリアスが、それらのエイリアスを使用するアプリケーションでどのように解釈されるかを示します。<br><br>例：<br><ul><li>`isAVID`:Analyticsの訪問者IDを表すデータソース。</li><li>`isCRSKey`:CRSでキーとして使用するエイリアスを表すデータソース。</li></ul>タグは、データソースの作成時に設定されますが、特定のデータソースを参照する際に、パイプラインメッセージにも含まれます。 |
| **`timestamp`** | イベントタイムスタンプ |
| **`channel`** | `context.channel` | 機能は表示配信 オプションは「web」と「mobile」で、「web」がデフォルトです。 |
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
| `environment.carrier` | 要求のIPアドレスに基づいて解決された携帯電話会社の名前。 |
| `environment.ipV4` | `mboxRequest.ipAddress` （V4形式の場合） |
| `environment.ipV6` | `mboxRequest.ipAddress` （V6形式の場合） |
| **`experience`** |
| `experience.target.clientCode` | `mboxRequest.client` |
| `experience.target.mboxName` | `mboxRequest.mboxName` |
| `experience.target.mboxVersion` | `mboxRequest.mboxVersion` |
| `experience.target.sessionId` | `mboxRequest.sessionId` |
| `experience.target.environmentID` | ターゲットの顧客定義環境（開発、QA、製品など）の内部マッピング。 |
| `experience.target.supplementalDataID` | Analyticsの識別子を使用してターゲットイベントをステッチするために使用されるイベント |
| `experience.target.pageDetails.pageId` | `mboxRequest.pageId` |
| `experience.target.pageDetails.pageScore` | `mboxRequest.mboxPageValue` |
| `experience.target.activities` | リストが適格とするアクティビティの訪問者（配列） |
| `experience.target.activities[i].activityID` | 訪問者が資格を持つ任意のアクティビティのID |
| `experience.target.activities[i].version` | 訪問者が認める任意のアクティビティのバージョン |
| `experience.target.activities[i].activityEvents` | ユーザーがこのアクティビティでヒットしたイベントの詳細が含まれます。 |
| **`device`** |
| `device.typeIDService` | `XDMDevice.Device.TypeIDService.typeIDService_deviceatlas` |
| `device.type` | 次の（またはNULL）のプ `deviceAtlas` ロパティの1つ： <ul><li>`type_mobile`</li><li>`type_tablet`</li><li>`type_desktop`</li><li>`type_ereader`</li><li>`type_television`</li><li>`type_settop`</li><li>`type_mediaplayer`</li></ul> |
| `device.typeID` | （空の文字列） |
| `device.manufacturer` | `deviceAtlas.manufacturer` |
| `device.model` | `deviceAtlas.model` |
| `device.modelNumber` | （空の文字列） |
| `device.screenHeight` | `deviceAtlas.displayHeight` |
| `device.screenWidth` | `deviceAtlas.displayWidth` |
| `device.colorDepth` | `deviceAtlas.displayColorDepth` |
| **`placeContext`** |
| `placeContext.geo.id` | ランダムUUID（必須） |
| `placeContext.geo.city` | 要求のIPアドレスに基づいて解決される都市名。 |
| `placeContext.geo.countryCode` | 要求のIPアドレスに基づいて解決される国コード。 |
| `placeContext.geo.dmaId` | リクエストのIPアドレスに基づいて解決された、指定市場地域コード。 |
| `placeContext.geo.postalCode` | 要求のIPアドレスに基づいて解決される郵便番号。 |
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
