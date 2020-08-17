---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルの概要
topic: guide
translation-type: tm+mt
source-git-commit: fa439ebb9d02d4a08c8ed92b18f2db819d089174
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 44%

---


# [!DNL Real-time Customer Profile] 概要

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。With [!DNL Real-time Customer Profile], you can see a holistic view of each individual customer that combines data from multiple channels, including online, offline, CRM, and third party data. [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。 この概要は、の役割と使用方法を理解するのに役立ち [!DNL Real-time Customer Profile] ま [!DNL Experience Platform]す。

## Understanding [!DNL Real-time Customer Profile]

[!DNL Real-time Customer Profile] は、様々な企業データアセットからのデータを結合し、個々の顧客プロファイルや関連する時系列イベントの形式でそのデータへのアクセスを提供する、汎用の参照エンティティストアです。 この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。

### [!DNL Profile] データストア

Although [!DNL Real-time Customer Profile] processes ingested data and uses Adobe Experience Platform [!DNL Identity Service] to merge related data through identity mapping, it maintains its own data in the [!DNL Profile] store. つまり、 [!DNL Profile] 店舗は、 [!DNL Catalog] データ([!DNL Data Lake])および [!DNL Identity Service] データ（アイデンティティグラフ）とは別のものです。

### [!DNL Profile] および [!DNL Platform] サービス

The relationship between [!DNL Real-time Customer Profile] and other services within [!DNL Experience Platform] is highlighted in the following diagram:

![プロファイルと他の Experience Platform サービスとの関係。](images/profile-overview/profile-in-platform.png)

### プロファイルとレコードデータ

プロファイルとは、主題、組織、または個人を表すもので、レコードデータとも呼ばれます。例えば、製品のプロファイルには SKU と説明が含まれ、人のプロファイルには名、姓、電子メールアドレスなどの情報が含まれる場合があります。Using [!DNL Experience Platform], you can customize profiles to use types of data relevant to your business. The standard [!DNL Experience Data Model] (XDM) [!DNL Individual Profile] class is the preferred class upon which to build a schema when describing customer record data, and supplies the data integral to many interactions between Platform services. For more information on working with schemas in [!DNL Experience Platform], please begin by reading the [XDM System overview](../xdm/home.md).

### 時系列イベント

時系列データは、主体が直接または間接的にアクションを実行した時点のシステムのスナップショットと、イベント自体を詳細に記述したデータを提供します。時系列データは、標準のスキーマクラス XDM ExperienceEvent で表され、イベント（買い物かごへの追加、リンククリック、ビデオ視聴など）を示すことができます。時系列データは、セグメント化ルールの基にするために使用でき、また、イベントはプロファイルのコンテキストで個別にアクセスできます。

### ID

どの企業も、個人的に感じる方法で顧客とコミュニケーションをとりたいと思っています。ただし、関連するデジタルエクスペリエンスを顧客に提供する際の課題の 1 つは、多くの場合、タブレット、携帯電話、ノートパソコンなどの様々なデジタルチャネルに広がっている、切断されたデータを結び付ける方法を理解することです。[!DNL Identity Service] 複数の顧客のIDをリンクし、各チャネルのIDグラフを作成することで、顧客の全体像をまとめ、顧客のIDをより深く理解できます。 詳しくは、「[ID サービスの概要](../identity-service/home.md)」を参照してください。

### セグメント化

Adobe Experience Platform [!DNL Segmentation Service] produces the audiences needed to power experiences for your individual customers. オーディエンスセグメントを作成すると、そのセグメントの ID が、すべての資格を持つプロファイルのセグメントメンバーのリストに追加されます。Segment rules are built and applied to [!DNL Real-time Customer Profile] data using RESTful APIs and the Segment Builder user interface. セグメント化の詳細については、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

### プロファイルフラグメントと和集合スキーマ {#profile-fragments-and-union-schemas}

One of the key features of [!DNL Real-time Customer Profile] is the ability to unify multi-channel data. When [!DNL Real-time Customer Profile] is used to access an entity, it can supply you with a merged view of all profile fragments for that entity across datasets, referred to as the union view and made possible through what is known as a union schema. [!DNL Real-time Customer Profile] データは、エンティティやプロファイルがIDでアクセスされたり、セグメントとしてエクスポートされたりすると、ソース間でマージされます。 APIを使用したプロファイルおよび和集合表示へのアクセスについて詳しくは、 [!DNL Real-time Customer Profile] Entitiesエンドポイントガイドを参照してください [](api/entities.md)。

### 結合ポリシー

When bringing data together from multiple sources and combining it in order to see a complete view of each of your individual customers, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view. RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。APIを使用した結合ポリシーの操作について詳しくは、 [!DNL Real-time Customer Profile][結合ポリシーエンドポイントガイドを参照してください](api/merge-policies.md)。 To work with merge policies using the [!DNL Experience Platform] UI, refer to the [merge policies user guide](ui/merge-policies.md).

### （アルファ）計算済み属性の設定

>[!IMPORTANT]
>このドキュメントで概要を説明する計算済み属性機能はアルファです。ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントの値を集計できます。各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式（「ルール」）が含まれます。これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。計算済み属性の詳細、および [!DNL Real-time Customer Profile] APIを使用して計算済み属性を操作する手順については、 [計算済み属性エンドポイントガイドを参照してください](api/computed-attributes.md)。 このガイドは、計算された属性がAdobe Experience Platform内で果たす役割をより深く理解するのに役立ちます。また、基本的なCRUD操作を実行するためのサンプルAPI呼び出しが含まれています。

## リアルタイムコンポーネント

This section introduces the components that allow [!DNL Real-time Customer Profile] to update and monitor record and time series data in real-time.

### ストリーミングの取得とストリーミングのセグメント化

リアルタイム入力は、ストリーミング取得と呼ばれるプロセスを通じて可能になります。As profile and time series data is ingested, [!DNL Real-time Customer Profile] automatically decides to include or exclude that data from segments through an ongoing process called streaming segmentation, before merging it with existing data and updating the union view. その結果、顧客がブランドとやり取りする際に、瞬時に計算をおこない、顧客に対して強化された個別的なエクスペリエンスを提供する意思決定をおこなうことができます。取得される間、データが正しく取得され、データセットの基になるスキーマに適合していることを確認する検証も行われます。取得中の検証の詳細については、まず「[データ取得の質の概要](../ingestion/quality/overview.md)」を読んでください。

### Edge Projectionの設定と宛先

複数のチャネルにわたって顧客の体験をリアルタイムで調整し、一貫性を保ち、パーソナライズするためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。Adobe Experience Platform を使用すると、エッジと呼ばれるものを使用して、データへのこのリアルタイムアクセスを可能にします。エッジとは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバーです。たとえば、Adobe Target や Adobe Campaign などの Adobe アプリケーションは、エッジを使用して、パーソナライズされた顧客体験をリアルタイムで提供します。データは投影によってエッジにルーティングされ、データの送信先となるエッジを定義する投影先と、エッジ上で利用可能にする特定の情報を定義する投影設定があります。詳しくは、 [!DNL Real-time Customer Profile] APIを使用した投影の操作を開始する方法については、『 [エッジ投影エンドポイントガイド](api/edge-projections.md)』を参照してください。

## ～へ追加のデータ [!DNL Real-time Customer Profile]

[!DNL Platform] を設定して、にレコードと時系列のデータを送信できます。これにより、リアルタイムストリーミングの取り込みとバッチ取り込みがサポートされ [!DNL Profile]ます。 詳しくは、[リアルタイム顧客プロファイルへのデータの追加](tutorials/add-profile-data.md)方法を概要するチュートリアルを参照してください。

>[!NOTE]
>
>Adobeソリューションで収集されたデータ( [!DNL Analytics Cloud]、、、 [!DNL Marketing Cloud]およびなど)は、に流れ込み、 [!DNL Advertising Cloud]に取り込まれ [!DNL Experience Platform][!DNL Profile]ます。

### [!DNL Profile] 摂取指標

観察性インサイトを使用すると、Adobe Experience Platform で主要指標を公開できます。In addition to [!DNL Platform] usage statistics and performance indicators for various [!DNL Platform] functionalities, there are specific [!DNL Profile]-related metrics that allow you to gain insight into incoming request rates, successful ingestion rates, ingested record sizes, and more. To learn more, begin by reading the [Observability Insights overview](../observability/home.md), and for a complete list of [!DNL Profile] metrics, see the documentation on [available metrics](../observability/metrics.md).

## [!DNL Data governance] および [!DNL Privacy]

[!DNL Data governance] は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。

As it relates to accessing data, data governance plays a key role within [!DNL Experience Platform] at various levels:
* データ使用のラベル付け
* データアクセスポリシー
* マーケティングアクションのデータのアクセス制御

[!DNL Data governance] は、いくつかのポイントで管理されます。 These include deciding what data is ingested into [!DNL Platform] and what data is accessible after ingestion for a given marketing action. 詳しくは、まず「[データガバナンスの概要](../data-governance/home.md)」を参照してください。

### オプトアウトおよびデータのプライバシー要求の処理

[!DNL Experience Platform] 顧客は、内部のデータの使用状況およびストレージに関するオプトアウト要求を送信でき [!DNL Real-time Customer Profile]ます。 オプトアウトリクエストの処理方法について詳しくは、[オプトアウトリクエストの処理](../segmentation/honoring-opt-outs.md)に関するドキュメントを参照してください。

## [!DNL Profile] ガイドライン

[!DNL Experience Platform] には、効果的に使用するために従うべき一連のガイドラインがあり [!DNL Profile]ます。

| セクション | 境界 |
| ------- | -------- |
| [!DNL Profile] 和集合スキーマ | 最大20個の **データセットが**[!DNL Profile] 和集合スキーマに貢献できます。 |
| 複数エンティティの関係 | 最大 **5** 個のマルチエンティティ関係を作成できます。 |
| 複数エンティティの関連付けのJSONの深さ | JSONの最大の深さは **4です**。 |
| 時系列データ | 非人口エンティティでは、時系列データは **使用できま** せん [!DNL Profile] 。 |
| 非人口スキーマの関係 | 非人物スキーマの関係は **許可されません** 。 |
| プロファイル断片 | プロファイルフラグメントの推奨最大サイズは10kB **です**。<br><br> プロファイルフラグメントの絶対最大サイズは **1MB**&#x200B;です。 |
| 非個人エンティティ | 1人の非個人エンティティの最大合計サイズは200MB **です**。 |
| 非人間エンティティごとのデータセット | 個人以外のエンティティに関連付けることができる **データセットは** 、最大1個です。 |

<!--
| Section | Boundary | Enforcement |
| ------- | -------- | ----------- |
| Profile union schema | A maximum of **20** datasets can contribute to the Profile union schema. | A message stating you've reached the maximum number of datasets appears. You must either disable or clean up other obsolete datasets in order to create a new dataset. |
| Multi-entity relationships | A maximum of **5** multi-entity relationship can be created. | A message stating all available mappings have been used appears when the fifth relationship is mapped. An error message letting you know you have exceeded the number of available mappings appears when attempting to map a sixth relationship. | 
| JSON depth for multi-entity association | The maximum JSON depth is **4**. | When trying to use the relationship selector with a field that is more than four levels deep, an error message appears, stating it is ineligible for multi-entity association. |
| Time series data | Time-series data is **not** permitted in Profile for non-people entities. | A message stating that this data cannot be enabled for Profile because it is of an unsupported type appears. |
| Non-people schema relationships | Non-people schema relationships are **not** permitted. | Relationships between two non-people schemas cannot be created. The relationships checkbox will be disabled. |
| Profile fragment | The recommended maximum size of a profile fragment is **10kB**.<br><br> The absolute maximum size of a profile fragment is **1MB**. | If you upload a fragment that is larger than 10kB, a warning appears, stating that performance may be degraded since the fragment exceeds the recommended maximum working size.<br><br> If you upload a fragment that is larger than 1MB, ingestion will fail, and an alert letting you know that records have failed will be sent. |
| Non-person entity | The maximum total size for a single non-person entity is **200MB**. | If you load an object as a non-person entity that is larger than 200MB, an alert will appear, stating that the entity has exceeded the maximum allowable size and will not be useable for segmentation. |
| Datasets per non-person entity | A maximum of **1** dataset can be associated to a non-person entity. | If you try to create a second dataset that is associated to the same non-person entity, an error appears, stating that only one dataset can be active per non-person entity. |

--->

>[!NOTE]
>
>
>非人物エンティティは、に含ま **れないXDMクラスを参照**[!DNL Profile]します。

## 次の手順とその他のリソース

To learn more about [!DNL Real-time Customer Profile], please continue reading the documentation and supplement your learning by watching the video below or exploring other [Experience Platform video tutorials](https://docs.adobe.com/content/help/en/platform-learn/tutorials/overview.html).

>[!WARNING]
>次のビデオに表示される [!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、 [リアルタイム顧客プロファイルユーザーガイド](ui/user-guide.md) (Real-time Customer User Guide)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27251?quality=12)