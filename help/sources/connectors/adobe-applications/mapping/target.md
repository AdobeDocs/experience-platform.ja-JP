---
keywords: Experience Platform；ホーム；人気の高いトピック；ターゲットマッピング；ターゲットマッピング
solution: Experience Platform
title: Adobe Targetイベントデータの XDM へのマッピング
topic-legacy: overview
description: Adobe Experience Platformで使用するAdobe Targetイベントフィールドを Experience Data Model(XDM) スキーマにマッピングする方法について説明します。
exl-id: dab08ab6-6c1c-460a-bb52-8dcdb5709a34
source-git-commit: e33d59c4ac28f55ba6ae2fc073d02f8738159263
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 87%

---

# ターゲットマッピングフィールドマッピング

Adobe Experience Platform を使用すると、Adobe Target のデータを Target ソースコネクタを介して取り込めます。コネクタを使用する場合、Target フィールドのすべてのデータを、XDM ExperienceEvent クラスに関連付けられた「[エクスペリエンスデータモデル（XDM）](../../../../xdm/home.md)」フィールドにマッピングする必要があります。

次の表に、エクスペリエンスイベントスキーマのフィールド（*XDM ExperienceEvent フィールド*）と、マッピング先の対応する Target フィールド（*Target Request フィールド*）の概要を示します。一部のマッピングに関する追加のメモも示します。

>[!NOTE]
>
>テーブルのコンテンツをすべて表示するには、左右にスクロールしてください。

| XDM ExperienceEvent フィールド | Target Request フィールド | メモ |
| ------------------------- | -------------------- | ----- |
| **`id`** | 一意のリクエスト識別子 |
| **`dataSource`** |  | すべてのクライアントに対して「1」に設定されます。 |
| `dataSource._id` | リクエストで渡すことができないシステム生成値。 | このデータソースの一意の ID。この ID は、データソースを作成した個人またはシステムによって提供されます。 |
| `dataSource.code` | リクエストで渡すことができないシステム生成値。 | 完全な @id へのショートカット。少なくとも 1 つのコードまたは @id を使用できます。このコードは、データソース統合コードと呼ばれる場合があります。 |
| `dataSource.tags` | リクエストで渡すことができないシステム生成値。 | タグを使用して、特定のデータソースで表されるエイリアスが、それらのエイリアスを使用するアプリケーションでどのように解釈されるかを示します。<br><br>例：<br><ul><li>`isAVID`：Analytics の訪問者 ID を表すデータソース。</li><li>`isCRSKey`：CRS でキーとして使用する必要があるエイリアスを表すデータソース。</li></ul>タグはデータソースの作成時に設定されますが、特定のデータソースを参照する際に、パイプラインメッセージにも含まれます。 |
| **`timestamp`** | イベントタイムスタンプ |
| **`channel`** | `context.channel` | 表示配信のみで機能します。オプションは「web」と「mobile」で、「web」がデフォルトです。 |
| **`endUserIds`** |
| `endUserIds.experience.tntId` | `tntId/mboxPC` |
| `endUserIds.experience.mcId` | `marketingCloudVisitorId` | Experience CloudID(ECID) は MCID とも呼ばれ、名前空間で引き続き使用されます。 |
| **`environment`** |
| `environment.browserDetails.userAgent` | `mboxRequest.userAgent` |
| `environment.browserDetails.viewPortHeight` | `mboxRequest.browserHeight` |
| `environment.browserDetails.viewPortWidth` | `mboxRequest.browserWidth` |
| `environment.operatingSystem` | `deviceAtlas.osName` |
| `environment.operatingSystemVersion` | `deviceAtlas.osVersion` |
| `environment.viewportHeight` | `mboxRequest.screenHeight` |
| `environment.viewportWidth` | `mboxRequest.screenWidth` |
| `environment.colorDepth` | `mboxRequest.colorDepth` |
| `environment.carrier` | リクエストの IP アドレスに基づいて解決された携帯電話会社の名前。 |
| `environment.ipV4` | `mboxRequest.ipAddress` （V4 形式の場合） |
| `environment.ipV6` | `mboxRequest.ipAddress` （V6 形式の場合） |
| **`experience`** |
| `experience.target.clientCode` | `mboxRequest.client` |
| `experience.target.mboxName` | `mboxRequest.mboxName` |
| `experience.target.mboxVersion` | `mboxRequest.mboxVersion` |
| `experience.target.sessionId` | `mboxRequest.sessionId` |
| `experience.target.environmentID` | Target の顧客定義環境（開発、QA、製品など）の内部マッピング。 |
| `experience.target.supplementalDataID` | Analytics イベントで Target イベントをステッチするために使用される識別子。 |
| `experience.target.pageDetails.pageId` | `mboxRequest.pageId` |
| `experience.target.pageDetails.pageScore` | `mboxRequest.mboxPageValue` |
| `experience.target.activities` | 訪問者が利用できるアクティビティのリスト（配列）。 |
| `experience.target.activities[i].activityID` | 訪問者が利用できる任意のアクティビティの ID。 |
| `experience.target.activities[i].version` | 訪問者が利用できる任意のアクティビティのバージョン。 |
| `experience.target.activities[i].activityEvents` | このイベントでユーザーがヒットしたアクティビティイベントの詳細が含まれます。 |
| **`device`** |
| `device.typeIDService` | `XDMDevice.Device.TypeIDService.typeIDService_deviceatlas` |
| `device.type` | `deviceAtlas` の次のいずれかのプロパティ（または NULL）。 <ul><li>`type_mobile`</li><li>`type_tablet`</li><li>`type_desktop`</li><li>`type_ereader`</li><li>`type_television`</li><li>`type_settop`</li><li>`type_mediaplayer`</li></ul> |
| `device.typeID` | （空の文字列） |
| `device.manufacturer` | `deviceAtlas.manufacturer` |
| `device.model` | `deviceAtlas.model` |
| `device.modelNumber` | （空の文字列） |
| `device.screenHeight` | `deviceAtlas.displayHeight` |
| `device.screenWidth` | `deviceAtlas.displayWidth` |
| `device.colorDepth` | `deviceAtlas.displayColorDepth` |
| **`placeContext`** |
| `placeContext.geo.id` | ランダム UUID（必須） |
| `placeContext.geo.city` | リクエストの IP アドレスに基づいて解決された市区町村の名前。 |
| `placeContext.geo.countryCode` | リクエストの IP アドレスに基づいて解決された国コード。 |
| `placeContext.geo.dmaId` | リクエストの IP アドレスに基づいて解決された指定販売地域コード。 |
| `placeContext.geo.postalCode` | リクエストの IP アドレスに基づいて解決された郵便番号。 |
| `placeContext.geo.stateProvince` | リクエストの IP アドレスに基づいて解決された州または地域。 |
| `placeContext.localTime` | `mboxRequest.offsetTime` + `mboxRequest.currentServerTime` |
| **`commerce`** |  | リクエストに注文の詳細が存在する場合にのみ設定します。 |
| `commerce.order.priceTotal` | `mboxRequest.orderTotal` |
| `commerce.order.purchaseOrderNumber` | `mboxRequest.orderId` |
| `commerce.order.purchaseID` | `mboxRequest.orderId` |
| **`web`** |
| `web.withWebPageDetails.url` | `mboxURL.context.address.url` |
| `web.webReferrer.url` | `mboxReferrer.context.address.url` |
| **`identityMap`** |
| `identityMap.TNTID` | `tntId.mboxPC` |
| `identityMap.ECID` | `marketingCloudVisitorId` |

{style=&quot;table-layout:auto&quot;}
