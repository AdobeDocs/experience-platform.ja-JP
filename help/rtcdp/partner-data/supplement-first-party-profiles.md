---
title: （ベータ版）ファーストパーティプロファイルをパートナー提供の属性で補完する
description: ファーストパーティプロファイルを信頼できるデータパートナーの属性で補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善する方法を説明します。
hide: true
hidefromtoc: true
badgeBeta: label="Beta" type="informative" before-title="true"
source-git-commit: 486e1390dfa0602bef15d196d4a1a5befdc9ff23
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 0%

---

# ファーストパーティプロファイルをパートナー提供の属性で補完

>[!AVAILABILITY]
>
>* このベータ版機能は、Real-Time CDP（アプリサービス）、Adobe Experience Platform Activation、リアルタイム CDP、Real-Time CDP Prime、Real-Time CDP Ultimate のライセンスを持つお客様が利用できます。 これらのパッケージの詳細については、 [製品の説明](https://helpx.adobe.com/legal/product-descriptions.html) 詳しくは、Adobe担当者にお問い合わせください。

信頼できるデータパートナーの属性でファーストパーティプロファイルを補完し、データ基盤を強化して、顧客ベースに対する新しいインサイトを得て、より優れたオーディエンス最適化を実現します。

![パートナー提供の属性の使用例によるプロファイルのエンリッチメントの概要レベルの視覚的な概要。](/help/rtcdp/assets/partner-data/enrichment/enrichment-use-case-overview.png)

## 前提条件と計画 {#prerequisites-and-planning}

独自のファーストパーティプロファイルにデータパートナーの属性を追加する場合は、データパートナーとのデータエンリッチメントループに関して次の詳細について話し合い、対処する必要があります。

* オーディエンスリストがReal-Time CDPから書き出され、データベンダーと共有される場所について考えます。 この場所は、ファイルの書き出しをサポートする必要があります。
* 追加の属性を重ね合わせるためにデータベンダーが想定する識別子は何ですか。
* パートナーが提供する属性を含むファイルをリアルタイム CDP に取り込む方法を教えてください。 例えば、ファイルは、次のようなクラウドストレージソースコネクタを通じて取り込むことができます。 [Amazon S3](/help/sources/connectors/cloud-storage/s3.md) または [SFTP](/help/sources/connectors/cloud-storage/sftp.md).
* パートナー提供の属性がReal-Time CDPに取り込まれて更新されると予想されるケイデンスは何ですか？

>[!WARNING]
>
>Real-Time CDPに取り込まれる追加のパートナー提供属性は、 *平均的なプロファイル充実度*. 詳しくは、 [Real-time Customer Data Platform Product Description](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html) プロファイルの充実性に関する詳細

## このユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

![パートナー提供の属性の使用例によるプロファイルのエンリッチメントの概要レベルの視覚的な概要。](/help/rtcdp/assets/partner-data/enrichment/enrichment-use-case-steps.png)

1. As a **顧客**&#x200B;の場合は、 **データパートナー**.
2. As a **顧客**&#x200B;を使用する場合は、 **パートナー** — 指定された属性。
3. As a **顧客**&#x200B;を使用すると、データパートナーでエンリッチメントするオーディエンスをオンボーディングできます。 一般に、これらのオーディエンスは、電子メール、名前、住所などの個人を特定できる情報 (PII) 要素などの入力識別子でキー設定されます。
4. この **パートナー** 照合可能なプロファイルのライセンス済み属性をに追加します。 オプションで、 [パートナー ID](/help/identity-service/namespaces.md) は、パートナースコープ ID 名前空間に含め、取り込むことができます。
5. As a **顧客**&#x200B;を使用して、データパートナーからReal-Time CDPの顧客プロファイルに属性を読み込みます。

## このユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要の各手順を完了するには、以下の節に記載されている詳細ドキュメントへのリンクをお読みください。

### パートナーからのライセンス属性 {#license-attributes-from-partner}

この手順については、 [前提条件](#prerequisites-and-planning) およびAdobeは、信頼されたデータベンダーとの間で、ファーストパーティプロファイルを拡張するための適切な契約がおこなわれていることを前提としています。

### パートナーが提供する属性に対応するように、プロファイルデータとガバナンスモデルを拡張します。 {#extend-governance-model}

この時点で、パートナーが指定した属性に対応するように、Real-Time CDPでデータ管理フレームワークを拡張します。

新しいスキーマを作成するオプションは、 **[!UICONTROL XDM 個人プロファイル]** クラスを使用するか、同じタイプの既存のスキーマを拡張して、パートナー指定の属性を含めます。 Adobeでは、データベンダーの追加属性を最も適切に表す新しいフィールドグループセットを使用して、新しいスキーマを作成することを強くお勧めします。 これにより、データスキーマが明確になり、互いに独立して発展することができます。

Adobeにパートナー提供の属性を含めるには、必要な属性を持つ新しいフィールドグループを作成するか、スキーマが提供する事前設定済みのフィールドグループの 1 つを使用します。

詳しくは、以下のドキュメントページを参照してください。

* [スキーマ構成の基本](/help/xdm/schema/composition.md)
* [の概要 [!UICONTROL XDM 個人プロファイル] クラス](/help/xdm/classes/individual-profile.md)
* [UI でのスキーマの作成と編集](/help/xdm/ui/resources/schemas.md)
* [UI でのスキーマフィールドグループの作成と編集](/help/xdm/ui/resources/field-groups.md)

<!--

Commenting out links for now
* [Create and edit schemas using the API](/help/xdm/api/schemas.md#create)
* [Update an existing schema to add field groups using the API](/help/xdm/api/schemas.md#patch)
* Link to new field group documentation page when it exists

-->

また、この手順では、データ管理戦略を拡大して、パートナーから提供されるサードパーティのデータを含める際の、データガバナンスモデルの変化について考えます。 以下のドキュメントリンクにある考慮事項を参照してください。

* (**近日開始**) サードパーティデータを別のデータセットに保持して、統合の削除と取り消しを容易におこなえるようにします。
* (**近日開始**) [データセットの有効期限](/help/hygiene/ui/dataset-expiration.md) の機能は、データ衛生アドオンを購入したクライアントのデータセットに対して使用できます。
* (**近日開始**) サードパーティデータを取り込む派生データセットを作成する場合は注意が必要です。混在させた場合、サードパーティデータを削除する唯一の方法は、派生データセット全体を削除することです。

>[!TIP]
>
>データベンダーの個人ベースの識別子で顧客プロファイルを補完する場合は、タイプの新しい ID タイプを作成できます **[[!UICONTROL パートナー ID]](/help/identity-service/namespaces.md)**.
>
>詳しくは、 [id タイプセクション](/help/identity-service/namespaces.md).
>詳細 [ID フィールドの定義方法](/help/xdm/ui/fields/identity.md) 」をクリックします。

### 個人識別情報 (PII) またはハッシュ化された PII をキーオフしたときにエンリッチメントするオーディエンスをエクスポートします {#export-audiences}

パートナーにエンリッチメントするオーディエンスをエクスポートします。 Amazon S3 や SFTP など、リアルタイム CDP によって提供されるクラウドストレージの宛先を使用します。 この手順を完了するには、次のドキュメントページをお読みください。

* [Amazon S3 の宛先](/help/destinations/catalog/cloud-storage/amazon-s3.md) ドキュメントページ
* [SFTP の宛先](/help/destinations/catalog/cloud-storage/sftp.md) ドキュメントページ
* 方法 [宛先に接続](/help/destinations/ui/connect-destination.md)
* 方法 [クラウドストレージの宛先へのデータの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md)

### データパートナーは、照合可能なプロファイルのライセンス済み属性を追加します {#partner-appends-attributes}

この手順では、データパートナーが、エクスポートするオーディエンスのライセンス済み属性を追加します。 通常、出力は、Real-Time CDPに取り込み可能なフラットファイルとして使用できます。 詳細を表示 [Real-Time CDPへのファイルの取り込み](/help/ingestion/tutorials/ingest-batch-data.md#upload-file).

### Real-Time CDPは、顧客プロファイルにエンリッチメントされた属性を追加します {#ingest-data}

次に、ソースコネクタを通じてパートナーからデータを取り込み、エンリッチメントされたデータをReal-Time CDPに戻し、パートナー提供のデータでプロファイルを補完する必要があります。

この目的で推奨されるソースコネクタは次のとおりです。

* [Amazon S3](/help/sources/connectors/cloud-storage/s3.md)
* [SFTP](/help/sources/connectors/cloud-storage/sftp.md)

## 制限事項とトラブルシューティング {#limitations-and-troubleshooting}

このページで説明する使用例を参照する際は、次の制限事項に注意してください。

* パートナー ID を使用することを選択した場合、 [ID グラフ](/help/identity-service/ui/identity-graph-viewer.md).

## パートナーデータサポートを通じて達成されたその他の使用例 {#other-use-cases}

Real-Time CDPのパートナーデータサポートを通じて有効化されたその他の使用例を調べる：

* (**近日開始**) [!BADGE ベータ版]{type=Informative}**パートナーの支援認識を活用** 訪問中のオンサイトエクスペリエンスのパーソナライズ、および訪問後のオフサイトリターゲティング（ユーザーによる認証やブランドとの以前の履歴がない）。
* (**近日開始**) [!BADGE ベータ版]{type=Informative}**拡張されたアクティベーション** PII またはハッシュ化された PII を受け入れないエコシステムを公開するためのパートナー ID の使用。