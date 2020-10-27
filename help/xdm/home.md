---
keywords: Experience Platform;home;popular topics;XDM;XDM system;XDM individual profile;XDM ExperienceEvent;XDM Experience Event;experienceEvent;experience event;Mixins;mixins;mixin;Mixin;Experience event;XDM Experience Event;XDM ExperienceEvent;experienceEvent;experienceevent;XDM Experienceevenet;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema registry;Schema Registry;schema library;Schema Library;schema;record data;time series;time-series
solution: Experience Platform
title: エクスペリエンスデータモデル（XDM）システム
topic: overview
description: '標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。 '
translation-type: tm+mt
source-git-commit: 1c00456ee06c1fc09c8e4ce070c90255f51811e1
workflow-type: tm+mt
source-wordcount: '1586'
ht-degree: 50%

---


# XDM システムの概要

標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。[!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。It provides common structures and definitions for any application to use to communicate with [!DNL Platform] services. XDM 標準に準拠することで、すべての顧客体験データを共通の表現に組み込み、より迅速かつ統合的な方法でインサイトを得ることができます。顧客の行動から貴重なインサイトを得たり、セグメントを使用して顧客のオーディエンスを定義したり、パーソナライゼーションを目的として顧客属性を表すことができます。

XDM is the foundational framework that allows Adobe Experience Cloud, powered by [!DNL Experience Platform], to deliver the right message to the right person, on the right channel, at exactly the right moment. The methodology on which [!DNL Experience Platform] is built, XDM System, operationalizes [!DNL Experience Data Model] schemas for use by [!DNL Platform] services.

This document provides an overview of the role of XDM System within [!DNL Experience Platform].

## XDM スキーマ

[!DNL Experience Platform] では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

Before data can be ingested into [!DNL Platform], a schema must be composed to describe the data&#39;s structure and provide constraints to the type of data that can be contained within each field. スキーマは、基本クラスと 0 個以上の Mixin で構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](schema/composition.md)を参照してください。

### [!DNL Schema Registry] および [!DNL Schema Library]

The **[!DNL Schema Registry]** provides a user interface and RESTful API from which you can view and manage all schema-related resources in the Adobe Experience Platform **[!DNL Schema Library]**. The [!DNL Schema Library] contains industry standard resources made available to you by Adobe, as well as resources from [!DNL Experience Platform] partners and vendors whose applications you use. また、スキーマレジストリの UI と API を使用して、組織に固有の新しいスキーマやリソースを作成および管理することもできます。

For a comprehensive guide to the major operations available in the [!DNL Schema Registry], see the [Schema Registry developer guide](api/getting-started.md).

## XDM システムでのデータの動作 {#data-behaviors}

Data intended for use in [!DNL Experience Platform] is grouped into two behavior types:

* **レコードデータ**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **時系列データ**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

すべての XDM スキーマは、レコードまたは時系列として分類できるデータを記述します。スキーマのデータ動作は、スキーマのクラスによって定義され、スキーマの作成時に割り当てられます。XDM クラスは、特定のデータ動作を表すためにスキーマが格納する必要のある最小のプロパティ数を記述します。

Although you are able to define your own classes within the [!DNL Schema Registry], it is recommended that you use the preferred classes **[!DNL XDM Individual Profile]** and **[!DNL XDM ExperienceEvent]** for record and time series data, respectively. これらのクラスの詳細については、以下で説明します。

### [!DNL XDM Individual Profile] {#xdm-individual-profile}

[!DNL XDM Individual Profile] は、特定された対象と部分的に識別された対象の両方の属性の単数形式の表現を形成する、レコードベースのクラスです。 高度に識別されるプロファイルには、個人的なコミュニケーションやターゲットを絞ったエンゲージメントに使用したり、詳細な個人情報（名前、性別、生年月日、場所など）および連絡先情報（電話番号や電子メールアドレスなど）を含めたりできます。

識別されにくいプロファイルは、ブラウザーの cookie のように、匿名の行動シグナルのみで構成される場合があります。この場合、少ないプロファイルデータを使用して、匿名プロファイルの興味や好みを照合し、格納する情報ベースを構築します。これらの識別子は、時間が経過すると、主体が通知、購読、購入などに新規登録するため、より詳細になる場合があります。これによってプロファイル属性が増加すると、最終的に件名が特定され、ターゲットを絞ったエンゲージメントの度合いが高くなる可能性があります。

消費者プロファイルが増え続けるにつれ、ある人物の個人情報、識別情報、連絡先の詳細、コミュニケーションの環境設定を含む堅牢なリポジトリーとなります。

### [!DNL XDM ExperienceEvent] {#xdm-experience-event}

XDM ExperienceEvent は、イベント（または一連のイベント）が発生したときのシステムの状態（サブジェクトが関与する時点や ID など）を取り込むために使用される、時系列ベースのクラスです。エクスペリエンスイベントは、何が発生したかに関する事実の記録なので、不変であり、何が発生したかを集計や解釈を加えずに表します。特定の時間枠内で発生した変更を分析し、複数の時間枠を比較してトレンドを追跡するために使用できるので、時間ドメイン分析には非常に重要です。

エクスペリエンスイベントは、明示的または暗黙的に指定できます。明示的なイベントは、ジャーニーのある時点で起きた人間の行動を直接観察できます。暗黙的なイベントとは、直接的な人間のアクションなしで発生するけれど、個人に関わるイベントです。暗黙的なイベントの例としては、電子メールニュースレターのスケジュールされた送信や、特定のしきい値に到達するバッテリーの電圧などがあります。

すべてのイベントがすべてのデータソースをまたいで簡単に分類できるとは限りませんが、類似したイベントを、できる限り似たタイプのイベントと調和させるとは非常に便利です。

![ExperienceEvent カスタマージャーニー](images/overview/experience-event-journey.png)

## XDM schemas and [!DNL Experience Platform] services

[!DNL Experience Platform] はスキーマに依存しないので、XDM 標準に準拠するスキーマはすべて サービスで使用できます。[!DNL Platform]The ways in which different [!DNL Platform] services use schemas are outlined in more detail below.

### [!DNL Catalog Service], [!DNL Data Ingestion] &amp; [!DNL Data Lake]

[!DNL Catalog Service] は、 [!DNL Experience Platform] アセットとその関連スキーマの記録システムです。 [!DNL Catalog] は、実際のデータを含むファイルやディレクトリではなく、これらのファイルやディレクトリのメタデータや説明を保持します。

[!DNL Catalog] データは、接触チャネル [!DNL Data Lake]やファイル形式に関係なく、によって管理されるすべてのデータを含む、非常に詳細なデータストア [!DNL Platform]に保存されます。

To begin ingesting data into [!DNL Experience Platform], a dataset is created using [!DNL Catalog Service]. データセットは、取り込むデータの構造を記述した XDM スキーマを参照します。If a dataset is created without a schema, [!DNL Experience Platform] will derive an &quot;observed schema&quot; by inspecting the type and content of ingested data fields. Datasets are then tracked in [!DNL Catalog] and stored in the [!DNL Data Lake] alongside the schemas and observed schemas on which they are based.

For more information on [!DNL Catalog], see the [Catalog Service overview](../catalog/home.md). Adobe Experience Platform のデータ取り込みについて詳しくは、[データ取り込みの概要](../ingestion/home.md)を参照してください。

### [!DNL Query Service]

Adobe Experience Platform [!DNL Query Service] allows you to use standard SQL to query [!DNL Experience Platform] data to support many different use cases.

After a schema has been composed and a dataset has been created which references that schema, data is then ingested and stored in the [!DNL Data Lake]. Using [!DNL Query Service], you can join any datasets in the [!DNL Data Lake] and capture the query results as a new dataset for use in reporting, machine learning, or for ingestion into [!DNL Real-time Customer Profile].

To learn more about [!DNL Query Service], please see the [Query Service introduction](../query-service/home.md).

### [!DNL Real-time Customer Profile]

リアルタイムの顧客プロファイルは、ターゲットを絞り、パーソナライズされたエクスペリエンス管理のための一元的な顧客プロファイルを提供します。Each profile contains data that is aggregated across all systems, as well as actionable timestamped accounts of events involving the individual that have taken place in any of the systems you use with [!DNL Experience Platform].

[!DNL Real-time Customer Profile] は、 [!DNL XDM Individual Profile] またはクラスに基づいてスキーマ形式のデータを使用し [!DNL XDM ExperienceEvent] 、そのデータに基づいてクエリに応答します。 [!DNL Profile] は、他のクラスに基づくスキーマの使用をサポートしていません。

[!DNL Profile] 各顧客プロファイルのインスタンスを維持し、データを結合して個人の「単一の真実の源泉」を形成します。 この統合データは、「和集合ビュー」と呼ばれるものを使用して表されます。和集合ビューは、同じクラスを実装するすべてのスキーマのフィールドを、1 つのスキーマに集計します。When composing a schema using the UI or API, you can enable the schema for use with [!DNL Real-time Customer Profile] and tag it for inclusion in the union view. The tagged schema will then participate in the schema definition being fed to [!DNL Profile].

と [!DNL XDM Individual Profile] データが取り込まれ、管理されると [!DNL XDM ExperienceEvent] きに、データの使用 [!DNL Catalog][!DNL Real-time Customer Profile] が可能なデータの取り込みを開始します。 取り込まれるインタラクションや詳細が多いほど、個人プロファイルは強力になります。

[!DNL XDM Individual Profile] データは、チャネルやAdobeソリューションの統合全体にわたってアクションを伝え、強化します。行動やインタラクションデータの豊富な履歴と組み合わせると、このデータを電源機械学習に使用します。 The [!DNL Real-time Customer Profile] API can also be used to enrich the functionality of third-party solutions, CRMs, and proprietary solutions.

詳しくは、[リアルタイム顧客プロファイルの概要](../profile/home.md)を参照してください。

### [!DNL Data Science Workspace]

Adobe Experience Platform [!DNL Data Science Workspace] uses machine learning and artificial intelligence to gain insights from data stored within [!DNL Experience Platform]. [!DNL Data Science Workspace] データ科学者は、XDM Individualに基づくレシピ [!DNL Profile] と、顧客とそのアクティビティに関する [!DNL XDM ExperienceEvent] データを作成でき、購入傾向や推奨オファーなど、個人が認識し、使用する可能性が高い予測を容易に行うことができます。

With [!DNL Data Science Workspace], data scientists can easily create intelligent services APIs powered by machine learning. これらのサービスは、Adobe Target や Adobe Analytics Cloud などの他のアドビソリューションと連携して、パーソナライズされ、ターゲットを絞ったデジタルエクスペリエンスを自動化します。

For more information on using [!DNL Experience Platform] data to power insights, see the [Data Science Workspace overview](../data-science-workspace/home.md).

## 次の手順とその他のリソース

Now that you better understand the role of schemas throughout [!DNL Experience Platform], you are ready to start composing your own. 学習内容の補足を続けるには、推奨ドキュメントを読み、次のビデオをご覧ください。

To learn design principles and best practices for composing schemas to be used with [!DNL Experience Platform], begin by reading the [basics of schema composition](schema/composition.md). スキーマの作成手順については、[API を使用した](tutorials/create-schema-api.md)または[ユーザーインターフェイスを使用した](tutorials/create-schema-ui.md)スキーマの作成に関するチュートリアルを参照してください。

での理解を深めるには、次のビデオ [!DNL XDM System] をご覧く [!DNL Experience Platform]ださい。

>[!VIDEO](https://video.tv.adobe.com/v/27105?quality=12&learn=on)

