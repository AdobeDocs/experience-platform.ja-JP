---
title: データ収集におけるID
description: データ収集がWeb実装全体でECID、CORE ID、ファーストパーティ永続性、およびIDMapを使用する方法について説明します。
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 0%

---

# データ収集におけるID

Adobe Data Collectionは、ID シグナルを活用して再訪問者を認識し、エクスペリエンス全体のコンテクストを反映させます。 訪問者がサイトにアクセスすると、Edge NetworkはExperience Cloud ID （ECID）を生成し、ファーストパーティ Cookieに保持します。 このECIDは、Adobe Experience Cloudアプリケーションで使用される主要なデバイス識別子であり、分析、パーソナライゼーション、オーディエンスのアクティベーションの基盤となります。 実装では、[`getIdentity`](/help/collection/js/commands/getidentity.md) コマンドを使用して、クライアントサイドの訪問者のECIDにアクセスできます。 データストリームレベルでは、[Data Prep for Data Collection](/help/datastreams/data-prep.md)を使用して、ECIDを下流サービスに到達する前にカスタム XDM フィールドにマッピングできます。

ECIDは、ユーザーではなくデバイスを識別します。 アクティビティを既知のユーザーに関連付けるには、XDM [`identityMap`](./identity-map.md)を使用して、CRM IDやハッシュ化された電子メールなどの追加の識別子をECIDと共に送信できます。 Adobeでは、使用可能なユーザーの名前空間を[&#x200B; プライマリ ID](/help/tags/extensions/client/web-sdk/data-element-types.md#identity-map)として設定することをお勧めします。

デフォルトのECIDを超えて、データ収集では、実装に応じて追加のID シグナルをサポートします。

* **ファーストパーティデバイス ID （FPID）**：制御するインフラストラクチャで生成および管理するデバイス ID。 Edge Networkでは、FPIDを使用して[ECID](./fpid.md)のシードを設定します。これにより、ブラウザーの制限によってAdobeで管理するCookieの寿命が短くなると、所有プロパティに対するCookieの永続性が強化されます。
* **CORE ID**: サードパーティ Cookieが使用可能な場合にサードパーティ ID ワークフローに参加する、デマンドベースの個別の識別子。 CORE IDはECIDとは異なり、[`getIdentity`](/help/collection/js/commands/getidentity.md)を通じて取得できます。 ファーストパーティとサードパーティのID コンテキストの連携の詳細については、[統合ID サポート &#x200B;](./unified-identity-support.md)を参照してください。

Visitor APIからアップグレードする場合、または古いIDの動作を調整する場合は、[`idMigrationEnabled`](/help/collection/js/commands/configure/idmigrationenabled.md)を参照して、既存のAMCV Cookieを移行します。

## ファーストパーティおよびサードパーティの収集 {#first-party-and-third-party-collection}

Web SDKは、どのエンドポイントがデータ収集リクエストを受け取るかに関係なく、ID [cookie](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/cookies/web-sdk) （`kndctr_` Cookieなど）をドメイン上のファーストパーティ Cookieとして常に設定します。 コレクションエンドポイント（実装がデータを送信するドメイン）は、ブラウザーとネットワークポリシーがリクエスト自体をどのように処理するかに影響する別の選択肢です。

**ファーストパーティコレクション**&#x200B;は、Adobe Edge Networkを指すCNAMEを使用して、組織が制御するドメイン（`data.example.com`など）を通じてデータ収集リクエストをルーティングします。 リクエストはドメイン上に残るため、広告ブロッカーやブラウザーネットワークの制限によってブロックされる可能性が低くなります。 また、ファーストパーティデータの収集は、独自のサーバーインフラストラクチャから[&#x200B; ファーストパーティデバイス ID](./fpid.md)を設定するための前提条件です。これは、利用可能な最も耐久性のあるID戦略です。 Adobeでは、[Adobeが管理する証明書プログラム &#x200B;](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/adobe-managed-cert)を使用して、実装にファーストパーティコレクションを設定することをお勧めします。

**サードパーティコレクション**&#x200B;は、Adobeが所有する[`edgeDomain`](/help/collection/js/commands/configure/edgedomain.md) （`example.data.adobedc.net`など）に直接リクエストを送信します。 ID Cookieはまだドメインのファーストパーティとして設定されていますが、リクエスト自体はサードパーティのドメインに送信され、一部のブラウザーや広告ブロッカーによって制限される可能性があります。

## 適切なID パターンの選択 {#choose-your-path}

* **所有プロパティに対するIDの永続性を強化**: ブラウザーの制限によってCookieの寿命が短くなり、管理するサイトで分析とパーソナライゼーションを強化する必要がある場合は、[&#x200B; ファーストパーティデバイス ID](./fpid.md)を使用します。
* **アプリからモバイル webにIDを渡す**：訪問者がモバイルアプリで開始し、WebViewまたはモバイル web ページで続行する場合は、[&#x200B; モバイル web ID共有](./mobile-to-web.md)を使用します。
* **ドメイン全体でIDを継続的に保持**：訪問者が組織が所有するweb プロパティ間を移動し、一貫したレポートとパーソナライズが必要な場合は、[&#x200B; クロスドメイン共有](./cross-domain-sharing.md)を使用します。
* **ファーストパーティの永続性とサードパーティのアクティベーションを組み合わせる**: サポート対象のサードパーティのアクティベーションフローと一緒に耐久性のあるファーストパーティ IDが必要な場合は、[統合ID サポート &#x200B;](./unified-identity-support.md)を使用します。
* **個人レベルのIDを送信**: [`identityMap`](./identity-map.md)を使用して、CRM ID、ハッシュ化された電子メール、その他の個人レベルのIDをECIDと一緒に送信し、ダウンストリームサービスでアクティビティを既知の人物に結合できるようにします。
* **同意がID**&#x200B;にどのように影響するかを理解する：[同意とID](./consent.md)を読んで、Web SDKがECIDを生成し、Cookieを設定し、データを送信する際に`defaultConsent`と`setConsent`がどのように制御するかを学びます。

訪問者のインフレーション、ECIDの不整合、FPIDの問題など、IDの問題を診断する方法については、[IDのトラブルシューティング &#x200B;](./troubleshooting.md)を参照してください。
