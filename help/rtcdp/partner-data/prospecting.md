---
title: サードパーティ cookie に依存せずに新規顧客を獲得
description: サードパーティ Cookie に依存せずに、見込み客のユースケースを通じて新規顧客をエンゲージおよび獲得する方法を説明します。
feature: Use Cases, Customer Acquisition
exl-id: b9e7b3af-2a13-4904-bd12-e3ed05a1988e
source-git-commit: e7c0551276d31d6809ace096c00e0dc2665090e6
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 74%

---

# サードパーティ cookie に依存せずに新規顧客を獲得

>[!AVAILABILITY]
>
>* この機能は、Real-Time CDP（アプリサービス）、Adobe Experience Platform アクティベーション、Real-Time CDP、Real-Time CDP Prime、Real-Time CDP Ultimateのライセンスを持つ顧客が使用できます。 これらのパッケージについて詳しくは、[製品の説明](https://helpx.adobe.com/jp/legal/product-descriptions.html)を参照し、アドビ担当者にお問い合わせください。

Real-Time CDP でサードパーティデータのサポートを使用して、データパートナーの見込み客プロファイルでプロファイルベースを拡張し、新規顧客を獲得またはリーチできるよう見込み客にエンゲージします。

![顧客プロスペクティングユースケースについての大まかで視覚的な概要。](/help/rtcdp/assets/partner-data/prospecting/prospecting-use-case-overview.png)

## このユースケースを検討する理由 {#why-this-use-case}

ブランドは、サードパーティ cookie への依存、限られた予算、透明性と広告費用対効果に対する需要の高まりに依存せずに、funnelで提供される顧客獲得の上位のユースケースを責任を持って実行するという困難な課題に同時に直面しています。

Adobe Real-Time Customer Data Platformを使用すると、企業は、Data Management Platform （DMP）でサポートされているユースケースを cookie のない代替手段に安全に移行し、セルフサービスのセグメント化、オーディエンスキュレーション、アクティベーションの完全な高度化と機能を 1 つのシステムに導入できます。 特許取得済みのデータガバナンスと同意のフレームワークを通じて、Adobeの責任あるデータ使用に揺るぎない焦点を当てることに妥協することなく、これらすべてを実現します。

例えば、見込み客を引き付けてユーザーまたは既知の顧客にするキャンペーンを実行する必要がある場合は、このユースケースで説明している手順に従ってください。

## 前提条件と計画 {#prerequisites-and-planning}

新規顧客への接触および獲得を検討する際は、計画プロセスで次の前提条件を考慮してください。

* パートナーが提供したプロファイルが Real-Time CDP に取り込まれて更新される頻度？
* ダウンストリームの宛先で必要な ID は何か
* 取り込まれた識別子がダウンストリームで使用可能であることを確認する
* 取り込んでいるパートナーデータは、個人識別情報（PII）、ハッシュ化された PII、パートナー ID など、広く受け入れられている永続的な識別子に関連付けられているか？
* パートナーの観点や、自社の法務、プライバシー、コンプライアンスチームの観点から考えて、どのようなデータ使用ポリシーに注意する必要があるか

## ユースケースの達成方法：概要 {#achieve-the-use-case-high-level}

Real-Time CDP を拡張して新規顧客のエンゲージメントや獲得を行う前に、必ず Real-Time CDP を使用してデータの最初の部分で堅牢な基盤を構築してください。このユースケースを実現するワークフローは、既知の顧客と関わるためのワークフローに似ています。

![顧客プロスペクティングユースケースについての大まかで視覚的な概要。](/help/rtcdp/assets/partner-data/prospecting/prospecting-use-case-steps.png)

1. **顧客**&#x200B;として、1 つ以上の&#x200B;**データパートナー**&#x200B;から見込み客プロファイルのライセンスを取得して、顧客獲得の初期段階を支援します。
2. **顧客**&#x200B;として、プロファイルデータとガバナンスモデルを拡張して、**パートナー**&#x200B;が提供する見込み客プロファイルのリストを取り込みます。
3. **顧客**&#x200B;として、見込み客プロファイルを Real-Time CDP に読み込み、ガバナンスポリシーを作成して責任を持って使用します。
4. **顧客**&#x200B;として、見込み客プロファイルのリストから焦点を絞ってオーディエンスを作成します。
5. **顧客**&#x200B;として、見込み客リストで利用可能な ID を受け入れる宛先に対して、見込み客のオーディエンスをアクティブ化します。
6. 必要に応じて、**データパートナー**&#x200B;と連携し、目的の有料メディアの宛先に対してオーディエンスをラストマイルでアクティブ化します。

## ビデオチュートリアル {#video-walkthrough}

見込み客オーディエンスにリーチしてエンゲージする方法のウォークスルーについては、以下のビデオチュートリアルをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3423071/?learn=on)

## ユースケースの達成方法：手順 {#step-by-step-instructions}

上記の概要の各手順を完了するには、詳しいドキュメントへのリンクを含む以下の節を参照してください。

### 使用する UI 機能と要素 {#ui-functionality-and-elements}

このユースケースを実装する手順を完了したら、次の Real-Time CDP 機能と UI 要素（使用順に一覧表示されています）を使用します。これらすべての領域に必要な属性ベースのアクセス制御権限があることを確認するか、必要な権限の付与をシステム管理者に依頼してください。

* [ID](/help/identity-service/features/namespaces.md)
* [スキーマ](/help/xdm/home.md)
* [データ使用ラベル](/help/data-governance/labels/overview.md)
* [データセット](/help/catalog/datasets/overview.md)
* [ソース](/help/sources/home.md)
* [見込み客プロファイル](/help/profile/ui/prospect-profile.md)
* [見込み客オーディエンス](/help/segmentation/types/prospect-audiences.md)
* [宛先](/help/destinations/home.md)

### パートナーからサードパーティのプロファイル詳細のライセンスを取得する {#license-profiles-from-partner}

この手順は[前提条件](#prerequisites-and-planning)で説明されています。アドビでは、データパートナーが提供する見込み客プロファイルを取り込むために、信頼できるデータベンダーと適切な契約が締結されていることを前提としています。

### プロファイルデータとガバナンスモデルの拡張により、パートナー提供の見込み客プロファイルに対応する {#extend-governance-model}

データパートナーから見込み客プロファイルを受け取る準備として、この新しいプロファイルタイプに対応できるように Real-Time CDP のデータ管理フレームワークを拡張する必要があります。

使用する ID、データ管理、ガバナンスの各コンポーネントは次のとおりです。

* パートナーが提供するプロファイルの新しい **[!UICONTROL Partner ID]** ID タイプ
* 新しい **[!UICONTROL XDM Individual Prospect Profile]** XDM クラス
* **（ドキュメントは近日公開予定）**&#x200B;パートナーデータサポートに合わせてカスタマイズされたフィールドグループ
* **（ドキュメントは近日公開予定）**&#x200B;パートナーから取得した属性に追加するサードパーティラベル

#### パートナー ID の ID 名前空間の作成 {#create-partner-id-namespace}

まず、パートナーから受け取るプロファイルの新しい ID タイプを作成します。これを行うには、「ID」セクションで、タイプ **[!UICONTROL Partner ID]** の新しい ID 名前空間を作成する必要があります。

![新しいパートナー ID の ID 名前空間を作成します。](/help/rtcdp/assets/partner-data/prospecting/create-partner-identity-namespace.png)

* パートナー ID 名前空間について詳しくは、[ID タイプの節](/help/identity-service/features/namespaces.md)を参照してください。
* 詳しくは、Experience Platform ユーザーインターフェイスの [ID フィールドの定義方法](/help/xdm/ui/fields/identity.md)を参照してください。

#### **[!UICONTROL XDM Individual Prospect Profile]** クラスを使用して新しいスキーマを作成します

次に、**[!UICONTROL Data Management]**/**[!UICONTROL Schemas]** で、新しいスキーマを作成し、**[!UICONTROL XDM Individual Prospect Profile]** クラスに割り当てます。

![XDM スキーマビルダーで XDM 個人見込み客プロファイルクラスを検索します。](/help/rtcdp/assets/partner-data/prospecting/xdm-individual-prospect-class.png)

XDM 個人見込み客プロファイルクラスの完全な情報については、[UI でのスキーマの作成と編集](/help/xdm/ui/resources/schemas.md)を参照してください（リンクは後日公開します）。

**[!UICONTROL XDM Individual Prospect Profile]** クラスは、以下に示すフィールドで事前設定されています。 パートナー提供の見込み客プロファイルの属性でスキーマを強化するには、必要な属性を含む新しいフィールドグループを作成してスキーマに追加するか、アドビが提供する事前設定済みのフィールドグループの 1 つを使用します。

![XDM 個人見込み客プロファイルクラスの事前設定済みフィールド。](/help/rtcdp/assets/partner-data/prospecting/preconfigured-fields-individual-prospect-class.png)

次に、スキーマのプライマリ ID として、先ほど作成したパートナー ID の ID を選択する必要があります。プロファイルレコードにはプライマリ識別子が含まれている必要があります。この手順は、見込み客データをプロファイルストアに読み込み、セグメント化やアクティベーションに使用できるようにするために必要です。

>[!AVAILABILITY]
>
>見込み客プロファイルは、自動的に、パートナー ID タイプの ID 名前空間のみを使用するように制限されます。これは仕様であり、この結果、ファーストパーティプロファイルと見込み客プロファイルがきれいに分離されます。

![以前に設定したパートナー ID を、スキーマのプライマリ ID として選択します。](/help/rtcdp/assets/partner-data/prospecting/select-partner-id-as-primary-identity.gif)

なお、スキーマはプロファイルに対してまだ有効になっていません。「プロファイル」ボタンを切り替えて、このスキーマをプロファイルサービスに含めるように設定します。リアルタイム顧客プロファイルでスキーマを使用できるようにする方法について詳しくは、[スキーマ作成チュートリアル](/help/xdm/tutorials/create-schema-ui.md#profile)を参照してください。

![プロファイルでスキーマを有効にする.](/help/rtcdp/assets/partner-data/prospecting/enable-schema-for-profile.png)

#### スキーマのすべてのフィールドに対するサードパーティデータガバナンスラベルの追加

スキーマを構成するすべてのフィールドにサードパーティデータガバナンスラベルを追加することを検討します。これは、サードパーティデータの責任ある使用を徹底し、データ漏洩のリスクを最小限に抑えるために重要です。[ サードパーティのデータガバナンスラベル ](../../data-governance/labels/reference.md#partner-ecosystem-labels) についての詳細情報

これを行うには、以下の手順に従います。

1. 作成したスキーマに移動して、「**[!UICONTROL Labels]**」タブを選択します。
2. 上部のチェックボックスボタンを使用してこのスキーマ内のすべてのフィールドを選択し、右側の鉛筆アイコンをクリックして、このスキーマにデータガバナンスラベルを適用します。
3. 左側のカテゴリから **[!UICONTROL Partner Ecosystem]** ラベルを選択します。
4. **[!UICONTROL Third Party]** というラベルを選択し、「**[!UICONTROL Save]**」を選択します。
5. スキーマのすべてのフィールドに、前の手順で選択したラベルが含まれるようになります。

>[!SUCCESS]
>
>これで、スキーマを使用する準備が整いました。次の手順に進んで、データパートナーから見込み客データを取り込むことができます。

また、この手順では、パートナーが提供するサードパーティデータを含めるようにデータ管理戦略を拡張する際の、データガバナンスモデルの変化についても検討します。次のドキュメントリンクにある考慮事項を参照してください。

* （**近日公開**）サードパーティデータを別のデータセットに保存すると、削除や統合の取り消しが簡単になります。
* （**近日公開**）データハイジーンアドオンを購入したクライアントのデータセットに対して[データセットの有効期限](/help/hygiene/ui/dataset-expiration.md)機能を使用します。
* （**近日公開**）サードパーティデータを取り込む派生データセットを作成する場合は注意が必要です。一度混合すると、サードパーティデータを削除する唯一の解決策は、派生データセット全体を削除することになるからです。

### 見込み客プロファイルのリストの読み込みと見込み客プロファイルビューの調査

見込み客プロファイルを管理するためのデータモデルを用意したら、次はデータを取り込みます。

#### データセットの作成とサンプルの見込み客データの読み込み

サンプルデータを読み込み、見込み客プロファイルに入力するには、データセットを作成し、データパートナーから受け取ったファイルをアップロードします。以下の手順を完了します。

1. **[!UICONTROL Data Management]**/**[!UICONTROL Datasets]** に移動し、「**[!UICONTROL Create dataset]**」を選択します。
2. 「スキーマからデータセットを作成」の選択
3. 前の手順で作成したスキーマを選択します。
4. データセットに名前を付け、必要に応じて説明を追加します。
5. **[!UICONTROL Finish]** を選択します。

![見込み客プロファイルのデータセットを作成する手順の録画。](/help/rtcdp/assets/partner-data/prospecting/create-dataset-for-prospect-profiles.gif)

なお、スキーマの作成手順と同様に、データセットをリアルタイム顧客プロファイルに含めることができるようにする必要があります。データセットをリアルタイム顧客プロファイルで使用できるようにする方法について詳しくは、[スキーマ作成チュートリアル](/help/xdm/tutorials/create-schema-ui.md#profile)を参照してください。

![プロファイルのデータセットを有効にします。](/help/rtcdp/assets/partner-data/prospecting/enable-dataset-for-profile.png)

パートナーから受け取ったファイルをデータセットに読み込むには、データセットを選択して、右側のパネルを下にスクロールして、「**[!UICONTROL Add data]**」を選択します。 ファイルをドラッグ&amp;ドロップするか、**[!UICONTROL Choose files]** を選択して、ファイルの場所に移動し、ファイルを選択します。

![データセットにファイルを追加します。](/help/rtcdp/assets/partner-data/prospecting/add-file-to-dataset.png)

データパートナーから Real-Time CDP にプロファイルのリストを読み込んだ後、「[読み込まれた見込み客プロファイルセクションを検査](#inspect-profiles)」に進み、見込み客のプロファイルが UI に設定されていることを確認します。

#### ソースコネクタを通じた見込み客データの取り込み

繰り返しのファイル読み込みを設定して、ソースコネクタを通じてパートナーからデータを取り込み、見込み客プロファイルのリストを Real-Time CDP に取り込むことができます。

この目的で推奨されるソースコネクタの一部を以下に示しますが、Real-Time CDP へのファイルベースの取り込み方法はいずれも、お客様の目的の働きをすることに注意してください。

* [Amazon S3](/help/sources/connectors/cloud-storage/s3.md)
* [SFTP](/help/sources/connectors/cloud-storage/sftp.md)

データパートナーから Real-Time CDP にプロファイルのリストを読み込んだ後、次のセクションに進み、見込み客のプロファイルが UI に設定されていることを確認します。

#### 読み込まれた見込み客プロファイルを検査 {#inspect-profiles}

見込み客プロファイルのリストを表示するには、左側のパネルで **[!UICONTROL Prospects]**/**[!UICONTROL Profiles]** に移動します。

Real-Time CDPに読み込んだばかりの見込み客プロファイルが見込み客プロファイル画面の **[!UICONTROL Browse]** ビューに表示されるまで、最大 2 時間かかる場合があります。 ページに「現在表示できる見込み客プロファイルがありません」というメッセージが表示された場合は、しばらくしてからもう一度試してください。少し待つと、見込み客プロファイルが **[!UICONTROL Browse]** ビューに表示され始めます。

>[!TIP]
>
>**[!UICONTROL Identity Namespace]** 列が存在することに注意してください。 複数のデータベンダーと連携している場合は、この列を使用して見込み客プロファイルの出所を推測します。

![Real-Time CDP に読み込まれた見込み客プロファイルのビュー。](/help/rtcdp/assets/partner-data/prospecting/prospect-profiles-view.png)

次に示すように、任意の見込み客プロファイルを選択し、さらに検査することもできます。

![見込み客プロファイルの検査方法のビュー。](/help/rtcdp/assets/partner-data/prospecting/inspect-prospect-profile.gif)

詳しくは、[ 見込み客プロファイル ](/help/profile/ui/prospect-profile.md) を参照してください。

### 見込み客オーディエンスを作成 {#create-prospect-audiences}

Real-Time CDP のセグメント化機能を使用して、見込み客プロファイルからオーディエンスを作成します。必要なセグメント化ルールを使用して、カスタマイズされたオーディエンスを作成します。

開始し、見込み客プロファイルで構成されるオーディエンスを作成するには、**[!UICONTROL Prospects]**/**[!UICONTROL Audiences]** に移動します。

![見込み客のオーディエンスの表示。](/help/rtcdp/assets/partner-data/prospecting/prospect-audiences.png)

見込み客のプロファイルのオーディエンス構築エクスペリエンスは、既知のファーストパーティ顧客からオーディエンスを構築するエクスペリエンスとは異なります。この表示は次の項目に制限されます。

* 先ほど作成した見込み客クラスの属性。
* バッチプロファイル評価のみ。
* 時系列イベントに基づくオーディエンスの作成をサポートしていません。

詳しくは、[ 見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md) を参照してください。

### 宛先への見込み客のプロファイルをアクティブ化 {#activate-to-destinations}

見込み客のオーディエンスを宛先に書き出して利用します。現在、特定のクラウドストレージの宛先のみが、見込み客プロファイルのアクティブ化をサポートしています。

![ 見込み客オーディエンスをサポートする宛先。](/help/destinations/assets/ui/activate-prospect-audiences/data-types-filter.png)

クラウドストレージの宛先に対する見込み客のアクティブ化については、[ 詳細情報 ](/help/destinations/ui/activate-prospect-audiences.md) を参照してください。

## パートナーデータサポートを通じて達成されるその他のユースケース {#other-use-cases}

Real-Time CDP のパートナーデータサポートを通じて達成されるその他のユースケースを調べます。

* [信頼できるデータパートナーからの属性でファーストパーティプロファイルを補完し、データ基盤を改善し、顧客ベースに関する新しいインサイトを得て、オーディエンスの最適化を改善します。](/help/rtcdp/partner-data/supplement-first-party-profiles.md)
* [ パートナー支援の訪問者認識を使用して、不明な訪問者に対するオンサイトエクスペリエンスをパーソナライズ ](/help/rtcdp/partner-data/onsite-personalization.md) ユーザーがブランドを認証したり、ブランドの過去の履歴を持ったりすることなく、訪問中に行えます。
* [ 見込み客プロファイルと見込み客オーディエンスのアクティベーションを拡張 ](/help/destinations/ui/activate-prospect-audiences.md) し、宛先を選択できるようになりました。
