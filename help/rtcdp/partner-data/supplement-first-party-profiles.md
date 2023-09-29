---
title: パートナー提供の属性を使用してファーストパーティプロファイルを補完
description: 信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善する方法を説明します。
source-git-commit: b4a18cdf434055be81dacbf19de4dd3e3f229d19
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 93%

---

# パートナー提供の属性を使用してファーストパーティプロファイルを補完

>[!AVAILABILITY]
>
>* この機能は、Real-Time CDP（アプリサービス）、Adobe Experience Platform Activation、リアルタイム CDP、Real-Time CDP Prime、Real-Time CDP Ultimate のライセンスを持つお客様が利用できます。 これらのパッケージについて詳しくは、[製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)を参照し、アドビ担当者にお問い合わせください。

信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。

![パートナー提供の属性を使用してプロファイルを強化し、ユースケースの高レベルの視覚的な概要を表示します。](/help/rtcdp/assets/partner-data/enrichment/enrichment-use-case-overview.png)

## 前提条件と計画 {#prerequisites-and-planning}

データパートナーからの属性を使用して独自のファーストパーティプロファイルを補完することを検討する場合は、データエンリッチメントループに関する次の詳細についてデータパートナーと話し合って対処する必要があります。

* データベンダーと共有するために、Real-Time CDP からオーディエンスリストを書き出す場所を検討します。この場所は、ファイルの書き出しをサポートする必要があります。
* 追加の属性を重ね合わせるために、データベンダーが前提とする識別子は何ですか？
* パートナー提供の属性を含むファイルを Real-Time CDP に取り込むにはどうすればよいですか？例えば、[Amazon S3](/help/sources/connectors/cloud-storage/s3.md) や [SFTP](/help/sources/connectors/cloud-storage/sftp.md) などのクラウドストレージソースコネクタを通じてファイルを取り込むことができます。
* パートナー提供の属性が Real-Time CDP に戻されて更新されるケイデンスはどれくらいですか？

>[!WARNING]
>
>Real-Time CDP に取り込まれたパートナー提供の追加属性は、*平均プロファイルリッチネス*&#x200B;に影響を与えます。プロファイルリッチネスについて詳しくは、[Real-Time Customer Data Platform の製品説明](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform.html)を参照してください。

## ビデオチュートリアル {#video-walkthrough}

以下のビデオチュートリアルでは、パートナーが指定した属性でファーストパーティオーディエンスを補足する方法について説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/3423075/?learn=on)

## ユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

![パートナー提供の属性を使用してプロファイルを強化し、ユースケースのおおまかな概要図を表示します。](/help/rtcdp/assets/partner-data/enrichment/enrichment-use-case-steps.png)

1. **顧客**&#x200B;は、**データパートナー**&#x200B;から属性のライセンスを取得します。
2. **顧客**&#x200B;は、**パートナー**&#x200B;提供の属性に対応するためにプロファイルデータとガバナンスモデルを拡張します。
3. **顧客**&#x200B;は、データパートナーと共に、強化するオーディエンスをオンボーディングします。一般に、これらのオーディエンスのメール、名前、住所などの個人を特定できる情報（PII）要素などの入力識別子を取得します。
4. **パートナー**&#x200B;は、照合できるプロファイルにライセンス済み属性を追加します。オプションで、[パートナー ID](/help/identity-service/namespaces.md) を含めて、パートナースコープの ID 名前空間に取り込むことができます。
5. **顧客**&#x200B;は、データパートナーからの属性を Real-Time CDP の顧客プロファイルに読み込みます。

## ユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要の各手順を完了するには、詳しいドキュメントへのリンクを含む以下の節を参照してください。

### パートナーからのライセンス属性 {#license-attributes-from-partner}

この手順については、[前提条件](#prerequisites-and-planning)で説明しています。アドビでは、ファーストパーティプロファイルを強化するために、信頼できるデータベンダーと適切な契約が締結されていることを前提としています。

### プロフィールデータとガバナンスモデルを拡張して、パートナー提供の属性に対応します。 {#extend-governance-model}

この時点で、Real-Time CDP のデータ管理フレームワークを拡張して、パートナー提供の属性に対応します。

**[!UICONTROL XDM 個人プロファイル]**&#x200B;クラスの新しいスキーマを作成するか、同じタイプの既存スキーマを拡張して、パートナー提供の属性を含めるようにするかを選択できます。アドビでは、データベンダーからの追加属性を最もよく表す、フィールドグループの新しいセットを使用して新しいスキーマを作成することを強くお勧めします。これにより、データスキーマが明確になり、相互に独立して発展できるようになります。

パートナー提供の属性をスキーマに含めるには、必要な属性を含む新しいフィールドグループを作成するか、アドビが提供する事前設定済みのフィールドグループの 1 つを使用します。

詳しくは、次のドキュメントページを参照してください。

* [スキーマ構成の基本](/help/xdm/schema/composition.md)
* [[!UICONTROL XDM 個人プロファイル]クラスの概要](/help/xdm/classes/individual-profile.md)
* [UI でのスキーマの作成と編集](/help/xdm/ui/resources/schemas.md)
* [UI でのスキーマフィールドグループの作成と編集](/help/xdm/ui/resources/field-groups.md)

<!--

Commenting out links for now
* [Create and edit schemas using the API](/help/xdm/api/schemas.md#create)
* [Update an existing schema to add field groups using the API](/help/xdm/api/schemas.md#patch)
* Link to new field group documentation page when it exists

-->

また、この手順では、パートナーが提供するサードパーティデータを含めるようにデータ管理戦略を拡張する際の、データガバナンスモデルの変化についても検討します。次のドキュメントリンクにある考慮事項を参照してください。

* （**近日公開**）サードパーティデータを別のデータセットに保存すると、削除や統合の取り消しが簡単になります。
* （**近日公開**）データハイジーンアドオンを購入したクライアントのデータセットに対して[データセットの有効期限](/help/hygiene/ui/dataset-expiration.md)機能を使用します。
* （**近日公開**）サードパーティデータを取り込む派生データセットを作成する場合は注意が必要です。一度混合すると、サードパーティデータを削除する唯一の解決策は、派生データセット全体を削除することになるからです。

>[!TIP]
>
>データベンダーからの個人ベースの識別子で顧客プロファイルを補完することを選択した場合は、**[[!UICONTROL パートナー ID]](/help/identity-service/namespaces.md)** タイプの新しい ID タイプを作成できます。
>
>パートナー ID について詳しくは、[ID タイプの節](/help/identity-service/namespaces.md)を参照してください。
>詳しくは、Experience Platform ユーザーインターフェイスの [ID フィールドの定義方法](/help/xdm/ui/fields/identity.md)を参照してください。

### 個人を特定できる情報（PII）またはハッシュ化された PII をキーオフした際に強化するオーディエンスを書き出します {#export-audiences}

パートナーに強化してもらうオーディエンスを書き出します。Amazon S3 や SFTP など、Real-Time CDP によって提供されるクラウドストレージの宛先を使用します。この手順を完了するには、次のドキュメントページを参照してください。

* [Amazon S3 の宛先](/help/destinations/catalog/cloud-storage/amazon-s3.md)ドキュメントページ
* [SFTP の宛先](/help/destinations/catalog/cloud-storage/sftp.md)ドキュメントページ
* [宛先への接続](/help/destinations/ui/connect-destination.md)方法
* [クラウドストレージの宛先へのデータの書き出し](/help/destinations/ui/activate-batch-profile-destinations.md)方法

### データパートナーは、照合できるプロファイルにライセンス済み属性を追加します。 {#partner-appends-attributes}

この手順では、データパートナーは、書き出されたオーディエンスにライセンス済み属性を追加します。通常、出力は、Real-Time CDP に取り込むことができるフラットファイルとして使用できます。詳しくは、[Real-Time CDP へのファイルの取り込み](/help/ingestion/tutorials/ingest-batch-data.md#upload-file)を参照してください。

### Real-Time CDP は、顧客プロファイルに強化された属性を追加します {#ingest-data}

次に、ソースコネクタを通じてパートナーからデータを取り込み、強化されたデータを Real-Time CDP に戻し、パートナーが提供するデータでプロファイルを補完する必要があります。

この目的で推奨されるソースコネクタを次に示します。

* [Amazon S3](/help/sources/connectors/cloud-storage/s3.md)
* [SFTP](/help/sources/connectors/cloud-storage/sftp.md)

## 制限事項とトラブルシューティング {#limitations-and-troubleshooting}

このページで説明するユースケースを参照する際は、次の制限事項に注意してください。

* パートナー ID の使用を選択した場合、これらの ID は [ID グラフの作成](/help/identity-service/ui/identity-graph-viewer.md)時に使用されません。

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* Real-Time CDP のサードパーティデータのサポートを使用して、[データパートナーの見込み客プロファイルでプロファイルベースを拡張し、新規顧客の獲得またはリーチのために見込み客との関わりを深めます](/help/rtcdp/partner-data/prospecting.md)。
* [パートナーの支援による認識を活用して、オンサイトエクスペリエンスをパーソナライズする](/help/rtcdp/partner-data/onsite-personalization.md) ユーザーがブランドを認証していない、または以前の履歴がない訪問中に発生した場合に発生していた問題を修正しました。
* [見込み客プロファイルと見込み客オーディエンスのアクティブ化の拡張](/help/destinations/ui/activate-prospect-audiences.md) をクリックして、宛先を選択します。