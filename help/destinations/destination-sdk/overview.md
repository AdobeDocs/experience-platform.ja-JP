---
description: Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントまたはストレージの場所に配信するExperience Platformの宛先統合パターンを設定できる設定 API のセットです。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。
title: Adobe Experience Platform Destination SDK
exl-id: 7aca9f40-98c8-47c2-ba88-4308fc2b1798
source-git-commit: 94d46ceeef6eef507115c60aaa6820d4560e4d44
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 57%

---

# Adobe Experience Platform Destination SDK

## 概要 {#destinations-sdk}

Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントまたはストレージの場所に配信するExperience Platformの宛先統合パターンを設定できる設定 API スイートです。 設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。

Destination SDK ドキュメントでは、Adobe Experience Platform Destination SDK を使用して Adobe Experience Platform との製品化された宛先統合を設定、テスト、リリースし、増え続ける宛先カタログに宛先を含める手順を説明しています。Destination SDKを使用すると、必要に応じてカスタマイズされたデータを書き出す独自のカスタムプライベート宛先を作成することもできます。

![Experience PlatformUI のスクリーンショット、宛先カタログの表示](./assets/destinations-catalog-overview.png)

## 製品化されたカスタム統合 {#productized-custom-integrations}

>[!IMPORTANT]
>
> 非公開のカスタムの宛先を作成するこの機能は、次の場合にのみ使用できます。 [Real-time Customer Data Platform Ultimate](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) 顧客。

Destination SDK のパートナーは、製品化された宛先を [Experience Platform カタログ](/help/destinations/catalog/overview.md)に追加することで、次のメリットを得ることができます。
1. 事前設定済みのパラメーターを使用して顧客全体で統合設定を標準化し、顧客の設定エクスペリエンスを簡素化できます。
2. Experience Platform の宛先カタログでブランド化した宛先カードを紹介できるため、顧客の設定の簡素化や認知の向上につながります。
3. Adobe Experience Platform および Real-time Customer Data Platform との製品化された宛先統合として取り上げられます。

Experience Platformのお客様は、アクティベーションのニーズに最適な、独自の非公開カスタムの宛先を作成することもできます。

![宛先開発者がDestination SDKとやり取りする方法、および製品化された宛先とプライベートの宛先からリアルタイム CDP のお客様が得るメリットを示す概要図です。](./assets/destination-sdk-visual.png)

## サポートされる統合のタイプ {#supported-integration-types}

Adobe Experience Platform は、Destination SDK を通じて、REST API エンドポイントを持つ宛先とのリアルタイム統合をサポートしています。Experience Platform とのリアルタイム統合は、次のような機能をサポートします。
* メッセージの変換と集計
* プロファイルのバックフィル
* オーディエンス設定とデータ転送を初期化するための設定可能なメタデータ統合
* 設定可能な認証
* 宛先設定をテストして反復するためのテスト API および検証 API のスイート

Destination SDKを通じて、選択したストレージの場所にファイルを定期的に書き出す統合を設定することもできます。 Experience Platform とのリアルタイム統合は、次のような機能をサポートします。
* サポートされる複数の形式 (CSV、Parquet、JSON) でのファイルのエクスポート
* 設定可能なファイルフォーマットオプション。ダウンストリームの要件を満たすように書き出されるファイルの形式を構造化できます。

宛先側の技術要件については、[統合の前提条件](./integration-prerequisites.md)の記事を参照してください。

## Destination SDK へのアクセスの取得 {#get-access}

Destination SDKへのアクセスは、パートナーまたはExperience Platform、Real-Time CDPのお客様のステータスに応じて異なります。 詳しくは、次の表を参照してください。


| パートナーまたは顧客のタイプ | Destination SDK へのアクセス方法 |
---------|----------|
| 独立系ソフトウェアベンダー（ISV） | [Adobe Exchange プログラム](https://partners.adobe.com/exchangeprogram/experiencecloud.html)に参加し、Destination SDK にアクセスできるようプロビジョニングされた Experience Platform サンドボックスの取得をリクエストします。 |
| システムインテグレーター（SI） | [Adobe Solution Partner Program](https://solutionpartners.adobe.com/home.html) で Gold レベルまたは Platinum レベルであれば、プロビジョニングされた Experience Platform サンドボックスと Destination SDK へのアクセスが提供されます。 |
| Experience Platform顧客 [Real-Time CDP Ultimate パッケージ](https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform.html) | デフォルトでは、Experience PlatformサンドボックスとDestination SDKにアクセスでき、組織のプライベートな宛先を構築できます。 |

{style=&quot;table-layout:auto&quot;}

## プロセスの概要 {#process}

Experience Platform で宛先を設定するプロセスの概要を次に示します。

1. ISV または SI の場合は、前述の節に記載されているアクセス情報の取得を参照してください。[Adobe Experience Platform Activation](https://helpx.adobe.com/jp/legal/product-descriptions/adobe-experience-platform0.html)をご利用のお客様は、この手順を省略できます。
2. [Experience Platform サンドボックスのプロビジョニングをリクエスト](https://adobeexchangeec.zendesk.com/hc/en-us/articles/360037457812-Adobe-Experience-Platform-Sandbox-Accounts-Access-Adding-Users-and-Support)し、宛先オーサリング権限を有効にします。
3. 統合を構築します。 製品ドキュメントの手順に従って、を設定します。 [ストリーミング先](./configure-destination-instructions.md) または [ファイルベースの宛先](./configure-file-based-destination-instructions.md).
4. 統合をテストします。 製品ドキュメントの手順に従って、をテストします。 [ストリーミング先](./test-destination.md) または [ファイルベースの宛先](./file-based-destination-testing-overview.md).
5. ISV または SI が [製品化統合](./overview.md#productized-custom-integrations), [統合の送信](./submit-destination.md) Adobeのレビュー用（標準応答時間は 5 営業日）。
6. 製品化された統合を作成する ISV または SI の場合は、[セルフサービスのドキュメントプロセス](./docs-framework/documentation-instructions.md)を使用して、宛先の製品ドキュメントページを Experience League に作成します。
7. 製品化された統合の場合、Adobeによって承認されると、統合は [Experience Platformカタログ](/help/destinations/catalog/overview.md).
8. 統合を更新する場合は、同じ手順に従います。

## リファレンス {#reference}

アドビでは、次の Experience Platform ドキュメントを読んで理解を深めることをお勧めします。

* [Adobe Experience Platform 宛先の概要](https://experienceleague.adobe.com/docs/experience-platform/destinations/home.html?lang=ja)
* [XDM スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=ja)
* [ID 名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)
