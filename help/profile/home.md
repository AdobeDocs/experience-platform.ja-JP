---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API;unified profile;Unified Profile;unified;Profile;rtcp;XDM graphs
title: リアルタイム顧客プロファイルの概要
topic: guide
description: リアルタイム顧客プロファイルは、様々な企業データアセットのデータを結合し、個々の顧客プロファイルおよび関連する時系列イベントの形でそのデータにアクセスできる汎用参照エンティティストアです。この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。
translation-type: tm+mt
source-git-commit: 47c65ef5bdd083c2e57254189bb4a1f1d9c23ccc
workflow-type: tm+mt
source-wordcount: '1820'
ht-degree: 41%

---


# [!DNL Real-time Customer Profile]概要

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。With [!DNL Real-time Customer Profile], you can see a holistic view of each individual customer that combines data from multiple channels, including online, offline, CRM, and third party data. [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。 この概要は、の役割と使用方法を理解するのに役立ち [!DNL Real-time Customer Profile] ま [!DNL Experience Platform]す。

## [!DNL Profile] experience platformで

次の図では、リアルタイム顧客プロファイルと Experience Platform 内の他のサービスとの関係が強調表示されています。

![Adobe Experience Platformサービス。](images/profile-overview/profile-in-platform.png)

### プロファイルデータストア

Although [!DNL Real-time Customer Profile] processes ingested data and uses Adobe Experience Platform [!DNL Identity Service] to merge related data through identity mapping, it maintains its own data in the [!DNL Profile] store. つまり、 [!DNL Profile] 店舗は、 [!DNL Catalog] データ([!DNL Data Lake])および [!DNL Identity Service] データ（アイデンティティグラフ）とは別のものです。

### プロファイルガードレール

Experience Platformは、リアルタイム顧客プロファイルがサポートできない [Experience Data Model](../xdm/home.md) (XDM)スキーマの作成を回避するのに役立つ一連のガードレールを提供しています。 これには、パフォーマンスの低下を引き起こすソフトリミットや、エラーやシステムの破損を引き起こすハードリミットが含まれます。 ガイドラインのリストや使用例など、詳細については、 [プロファイル用ガードレールのドキュメントをお読みください](guardrails.md) 。

## プロファイルについて

[!DNL Real-time Customer Profile] 様々なエンタープライズシステムのデータをマージし、関連する時系列イベントを使用して、顧客プロファイルの形でそのデータへのアクセスを提供します。 この機能を使用すると、マーケターは、複数のチャネルにわたって、オーディエンスとの調整された一貫した関連性のあるエクスペリエンスを促進できます。以下の節では、プラットフォーム内でプロファイルを効果的に構築し、維持するために理解しておく必要がある主要な概念の一部を取り上げます。

### プロファイルフラグメントと結合されたプロファイル

個々の顧客プロファイルは、複数のプロファイルフラグメントで構成され、それらが結合されてその顧客の単一の表示が形成されます。 例えば、顧客が複数のチャネルをまたがって自社のブランドとやり取りを行う場合、1人の顧客に関連する複数のプロファイルフラグメントが複数のデータセットに表示されます。 これらのフラグメントがPlatformに取り込まれると、それらのフラグメントが結合され、そのお客様用の単一のプロファイルが作成されます。

複数のソースからのデータが競合する場合(例えば、1人のフラグメントリストが「独身」、他のリストが「既婚」である場合)、 [](#merge-policies) マージポリシーによって、個々のプロファイルに優先順位を付け、含める情報が決まります。 したがって、各プロファイルは複数のフラグメントで構成されるので、Platform内のプロファイルフラグメントの合計数は、結合されたプロファイルの合計数よりも常に多くなる可能性が高くなります。

### レコードデータ

プロファイルとは、多数の属性（レコードデータとも呼ばれます）で構成される件名、組織、または個人を表したものです。 例えば、製品のプロファイルには SKU と説明が含まれ、人のプロファイルには名、姓、電子メールアドレスなどの情報が含まれる場合があります。Using [!DNL Experience Platform], you can customize profiles to use specific data relevant to your business. The standard [!DNL Experience Data Model] (XDM) class, [!DNL XDM Individual Profile], is the preferred class upon which to build a schema when describing customer record data, and supplies the data integral to many interactions between Platform services. For more information on working with schemas in [!DNL Experience Platform], please begin by reading the [XDM System overview](../xdm/home.md).

### 時系列イベント

時系列データは、主体が直接または間接的にアクションを実行した時点のシステムのスナップショットと、イベント自体を詳細に記述したデータを提供します。時系列データは、標準のスキーマクラス XDM ExperienceEvent で表され、イベント（買い物かごへの追加、リンククリック、ビデオ視聴など）を示すことができます。時系列データは、セグメント化ルールの基にするために使用でき、また、イベントはプロファイルのコンテキストで個別にアクセスできます。

### ID

どの企業も、個人的に感じる方法で顧客とコミュニケーションをとりたいと思っています。ただし、関連するデジタルエクスペリエンスを顧客に提供する際の課題の 1 つは、多くの場合、タブレット、携帯電話、ノートパソコンなどの様々なデジタルチャネルに広がっている、切断されたデータを結び付ける方法を理解することです。[!DNL Identity Service] 複数の顧客のIDをリンクし、各チャネルのIDグラフを作成することで、顧客の全体像をまとめることができます。 詳しくは、「[ID サービスの概要](../identity-service/home.md)」を参照してください。

### 結合ポリシー

When bringing data fragments together from multiple sources and combining them in order to see a complete view of each of your individual customers, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be used to create the customer profile. 複数のデータセットのデータに矛盾がある場合、マージポリシーによって、そのデータの処理方法と使用する値が決定されます。 RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。APIを使用した結合ポリシーの操作について詳しくは、 [!DNL Real-time Customer Profile][結合ポリシーエンドポイントガイドを参照してください](api/merge-policies.md)。 To work with merge policies using the [!DNL Experience Platform] UI, refer to the [merge policies user guide](ui/merge-policies.md).

### 和集合スキーマ {#profile-fragments-and-union-schemas}

One of the key features of [!DNL Real-time Customer Profile] is the ability to unify multi-channel data. When [!DNL Real-time Customer Profile] is used to access an entity, it can supply you with a merged view of all profile fragments for that entity across datasets, referred to as the union view and made possible through what is known as a union schema. [!DNL Real-time Customer Profile] データは、エンティティやプロファイルがIDでアクセスされたり、セグメントとしてエクスポートされたりすると、ソース間でマージされます。 APIを使用したプロファイルおよび和集合表示へのアクセスについて詳しくは、 [!DNL Real-time Customer Profile] Entitiesエンドポイントガイドを参照して [ください](api/entities.md)。

### セグメント化

Adobe Experience Platform [!DNL Segmentation Service] produces the audiences needed to power experiences for your individual customers. オーディエンスセグメントを作成すると、そのセグメントの ID が、すべての資格を持つプロファイルのセグメントメンバーのリストに追加されます。Segment rules are built and applied to [!DNL Real-time Customer Profile] data using RESTful APIs and the Segment Builder user interface. セグメント化の詳細については、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

### （アルファ）計算済み属性の設定

>[!IMPORTANT]
>
>計算済みの属性機能はアルファベットで表されます。 ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントの値を集計できます。各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式（「ルール」）が含まれます。これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。計算済み属性の詳細、および [!DNL Real-time Customer Profile] APIを使用して計算済み属性を操作する手順については、 [計算済み属性エンドポイントガイドを参照してください](api/computed-attributes.md)。 このガイドは、計算された属性がAdobe Experience Platform内で果たす役割をより深く理解するのに役立ちます。また、基本的なCRUD操作を実行するためのサンプルAPI呼び出しが含まれています。

## リアルタイムコンポーネント

This section introduces the components that allow [!DNL Real-time Customer Profile] to update and monitor record and time series data in real-time.

### ストリーミングの取得とストリーミングのセグメント化

リアルタイム入力は、ストリーミング取得と呼ばれるプロセスを通じて可能になります。As profile and time series data is ingested, [!DNL Real-time Customer Profile] automatically decides to include or exclude that data from segments through an ongoing process called streaming segmentation, before merging it with existing data and updating the union view. その結果、顧客がブランドとやり取りする際に、瞬時に計算をおこない、顧客に対して強化された個別的なエクスペリエンスを提供する意思決定をおこなうことができます。取得される間、データが正しく取得され、データセットの基になるスキーマに適合していることを確認する検証も行われます。取得中の検証の詳細については、まず「[データ取得の質の概要](../ingestion/quality/overview.md)」を読んでください。

### Edge Projectionの設定と宛先

複数のチャネルにわたって顧客の体験をリアルタイムで調整し、一貫性を保ち、パーソナライズするためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。Adobe Experience Platform を使用すると、エッジと呼ばれるものを使用して、データへのこのリアルタイムアクセスを可能にします。エッジとは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバーです。たとえば、Adobe Target や Adobe Campaign などの Adobe アプリケーションは、エッジを使用して、パーソナライズされた顧客体験をリアルタイムで提供します。データは投影によってエッジにルーティングされ、データの送信先となるエッジを定義する投影先と、エッジ上で利用可能にする特定の情報を定義する投影設定があります。詳しくは、 [!DNL Real-time Customer Profile] APIを使用した投影の操作を開始する方法については、『 [エッジ投影エンドポイントガイド](api/edge-projections.md)』を参照してください。

## Ingest data into [!DNL Profile]

[!DNL Platform] は、にレコードと時系列のデータを送信するように設定でき [!DNL Profile]ます。これにより、リアルタイムストリーミングの取り込みとバッチ取り込みがサポートされます。 詳しくは、[リアルタイム顧客プロファイルへのデータの追加](tutorials/add-profile-data.md)方法を概要するチュートリアルを参照してください。

>[!NOTE]
>
>Adobeソリューションで収集されたデータ( [!DNL Analytics Cloud]、、、 [!DNL Marketing Cloud]およびなど)は、に流れ込み、 [!DNL Advertising Cloud]に取り込まれ [!DNL Experience Platform][!DNL Profile]ます。

### [!DNL Profile] 摂取指標

観察性インサイトを使用すると、Adobe Experience Platform で主要指標を公開できます。In addition to [!DNL Platform] usage statistics and performance indicators for various [!DNL Platform] functionalities, there are specific [!DNL Profile]-related metrics that allow you to gain insight into incoming request rates, successful ingestion rates, ingested record sizes, and more. To learn more, begin by reading the [Observability Insights API overview](../observability/api/overview.md), and for a complete list of [!DNL Profile] metrics, see the documentation on [available metrics](../observability/api/metrics.md#available-metrics).

## [!DNL Data governance] および [!DNL Privacy]

[!DNL Data governance] は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。

As it relates to accessing data, data governance plays a key role within [!DNL Experience Platform] at various levels:
* データ使用のラベル付け
* データアクセスポリシー
* マーケティングアクションのデータのアクセス制御

[!DNL Data governance] は、いくつかのポイントで管理されます。 These include deciding what data is ingested into [!DNL Platform] and what data is accessible after ingestion for a given marketing action. 詳しくは、まず「[データガバナンスの概要](../data-governance/home.md)」を参照してください。

### オプトアウトおよびデータのプライバシー要求の処理

[!DNL Experience Platform] 顧客は、内部のデータの使用状況およびストレージに関するオプトアウト要求を送信でき [!DNL Real-time Customer Profile]ます。 オプトアウトリクエストの処理方法について詳しくは、[オプトアウトリクエストの処理](../segmentation/honoring-opt-outs.md)に関するドキュメントを参照してください。

## 次の手順とその他のリソース

での作業について詳し [!DNL Real-time Customer Profile]くは、 [プロファイルUIガイド](ui/user-guide.md) または [API開発者ガイドを参照してください](api/overview.md)。

>[!WARNING]
>
>Experience Platformのユーザーインターフェイスは頻繁に更新され、このビデオの録画以降に変更された可能性があります。 最新のUIのスクリーンショットと機能については、 [リアルタイム顧客プロファイルユーザーガイド](ui/user-guide.md) (Real-time Customer User Guide)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27251?quality=12)