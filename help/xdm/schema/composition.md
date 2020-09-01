---
keywords: Experience Platform;home;popular topics;schema;Schema;enum;mixin;Mixin;Mixins;mixins;data type;data types;Data types;Data type;primary identity;primary idenity;XDM individual profile;XDM fields;enum datatype;Experience event;XDM Experience Event;XDM ExperienceEvent;experienceEvent;experienceevent;XDM Experienceevenet;schema design;class;Class;classes;Classes;datatype;Datatype;data type;Data type;schemas;Schemas;identityMap;identity map;Identity map;
solution: Experience Platform
title: スキーマ合成の基本
topic: overview
description: このドキュメントでは、エクスペリエンスデータモデル（XDM）スキーマの概要と、Adobe Experience Platform で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します。
translation-type: tm+mt
source-git-commit: ed1f2fdac0f9c977d11c867327c084353c1bcd0f
workflow-type: tm+mt
source-wordcount: '2839'
ht-degree: 55%

---


# スキーマ合成の基本

This document provides an introduction to [!DNL Experience Data Model] (XDM) schemas and the building blocks, principles, and best practices for composing schemas to be used in Adobe Experience Platform. For general information on XDM and how it is used within [!DNL Platform], see the [XDM System overview](../home.md).

## スキーマ

スキーマは、データの構造と形式を表し、検証する一連のルールです。スキーマは、高いレベルで、実世界のオブジェクト（人など）の抽象的な定義を提供し、そのオブジェクトの各インスタンスに含めるデータ（名、姓、誕生日など）の概要を示します。

スキーマは、データの構造を説明するだけでなく、制約や期待をデータに適用し、システム間の移動時に検証できるようにします。これらの標準的な定義により、接触チャネルに関係なくデータを一貫して解釈でき、アプリケーション間での翻訳の必要性を排除できます。

[!DNL Experience Platform] は、スキーマの使用を通して、このセマンティックの正規化を使用して維持します。Schemas are the standard way of describing data in [!DNL Experience Platform], allowing all data that conforms to schemas to be reusable without conflicts across an organization and even to be sharable between multiple organizations.

### リレーショナルテーブルと埋め込みオブジェクト

リレーショナルデータベースを使用する場合のベストプラクティスには、データの正規化、またはエンティティを取得してそれを個別のピースに分割し、複数のテーブルに表示することが含まれます。データ全体の読み取りやエンティティの更新を行うには、JOIN を使用して、多くの個々のテーブルに対して読み取りおよび書き込み操作をおこなう必要があります。

XDM スキーマは、埋め込みオブジェクトを使用することで、複雑なデータを直接表現し、階層構造を持つ独立したドキュメントに格納できます。この構造の主な利点の 1 つは、複数の非正規化テーブルへの高価な結合によってエンティティを再構築しなくても、データをクエリできる点です。

### スキーマとビッグデータ

現代のデジタルシステムは、大量の行動信号（トランザクションデータ、Web ログ、物のインターネット、ディスプレイなど）を生成します。このビッグデータは、エクスペリエンスを最適化する非常に大きな機会を生み出しますが、データの規模と多様性が原因で、使用が困難です。データから値を得るには、一貫性と効率性を持って処理できるように、構造、形式および定義を標準化する必要があります。

スキーマは、複数のソースからのデータの統合、共通の構造や定義による標準化、複数のソリューション間での共有を可能にすることで、この問題を解決します。これにより、後続のプロセスとサービスは、データについて尋ねられるあらゆるタイプの質問に答えることができます。これは、データについて尋ねられるすべての質問が事前にわかっていて、データがそれらの期待に準拠するようにモデル化される従来のデータモデリングアプローチとは異なります。

### Schema-based workflows in [!DNL Experience Platform]

Standardization is a key concept behind [!DNL Experience Platform]. アドビが推進する XDM は、顧客エクスペリエンスデータを標準化し、顧客エクスペリエンス管理の標準スキーマを定義する取り組みです。

The infrastructure on which [!DNL Experience Platform] is built, known as [!DNL XDM System], facilitates schema-based workflows and includes the [!DNL Schema Registry], [!DNL Schema Editor], schema metadata, and service consumption patterns. 詳しくは、「[XDM システムの概要](../home.md)」を参照してください。

## スキーマの計画

スキーマを構築する最初の手順は、スキーマ内で捕捉しようとする概念、すなわち現実世界のオブジェクトを決定することです。説明しようとしている概念を特定したら、データのタイプ、潜在的な ID フィールド、将来のスキーマの発展について考え、スキーマの計画を始めることができます。

### Data behaviors in [!DNL Experience Platform]

Data intended for use in [!DNL Experience Platform] is grouped into two behavior types:

* **レコードデータ**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **時系列データ**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

すべての XDM スキーマは、レコードまたは時系列として分類できるデータを記述します。スキーマのデータ動作は、スキーマの&#x200B;**クラス**&#x200B;によって定義され、スキーマの作成時に割り当てられます。XDM クラスについては、このドキュメントで後述します。

レコードと時系列の両方のスキーマには、ID のマップ（`xdm:identityMap`）が含まれます。このフィールドには、次の節で説明する「ID」とマークされたフィールドから作成された、主体の ID 表現が含まれます。

### [!UICONTROL ID]

Schemas are used for ingesting data into [!DNL Experience Platform]. このデータは、複数のサービスで使用して、個々のエンティティの単一の統合表示を作成できます。したがって、顧客のIDについて考える際は、スキーマの場所に関係なく、どのフィールドを使用して対象を識別できるかを考慮する必要があります。

この処理を行うために、スキーマ内の主要フィールドをIDとしてマークすることができます。 Upon data ingestion, the data in those fields is inserted into the &quot;[!UICONTROL Identity Graph]&quot; for that individual. The graph data can then be accessed by [[!DNL Real-time Customer Profile]](../../profile/home.md) and other [!DNL Experience Platform] services to provide a stitched-together view of each individual customer.

Fields that are commonly marked as &quot;[!UICONTROL Identity]&quot; include: email address, phone number, [[!DNL Experience Cloud ID (ECID)]](https://docs.adobe.com/content/help/ja-JP/id-service/using/home.html), CRM ID, or other unique ID fields. You should also consider any unique identifiers specific to your organization, as they may be good &quot;[!UICONTROL Identity]&quot; fields as well.

スキーマ計画段階で顧客の ID を考慮し、可能な限り堅牢なプロファイルを構築するためにデータを統合できるようにすることが重要です。See the overview on [Adobe Experience Platform Identity Service](../../identity-service/home.md) to learn more about how identity information can help you deliver digital experiences to your customers.

#### xdm:identityMap {#identityMap}

`xdm:identityMap` は、個人の様々なID値と関連する名前空間を説明するmap-typeフィールドです。 このフィールドは、スキーマ自体の構造内でidentity値を定義する代わりに、スキーマの識別情報を提供するために使用できます。

単純なIDマップの例を次に示します。

```json
"identityMap": {
  "email": [
    {
      "id": "jsmith@example.com",
      "primary": false
    }
  ],
  "ECID": [
    {
      "id": "87098882279810196101440938110216748923",
      "primary": false
    },
    {
      "id": "55019962992006103186215643814973128178",
      "primary": false
    }
  ],
  "loyaltyId": [
    {
      "id": "2e33192000007456-0365c00000000000",
      "primary": true
    }
  ]
}
```

上の例が示すように、 `identityMap` オブジェクトの各キーはID名前空間を表します。 各キーの値はオブジェクトの配列で、各名前空間のID値(`id`)を表します。 Adobeアプリケーションで認識される標準ID名前空間の [!DNL Identity Service] リストについては、 [ドキュメントを参照してください](../../identity-service/troubleshooting-guide.md#standard-namespaces) 。

>[!NOTE]
>
>値がプライマリID(`primary`)であるかどうかを表すboolean値を、各ID値に対して指定することもできます。 プライマリIDは、で使用するスキーマに対してのみ設定する必要があり [!DNL Real-time Customer Profile]ます。 詳しくは、 [和集合スキーマの節を参照してください](#union) 。

### スキーマ進化の原理 {#evolution}

デジタルエクスペリエンスの本質が進化し続けるにつれ、デジタルエクスペリエンスを表すスキーマが必要になります。したがって、適切に設計されたスキーマは、必要に応じて適応し、進化し、以前のバージョンのスキーマに破壊的な変更を加えることなく使用できます。

Since maintaining backwards compatibility is crucial for schema evolution, [!DNL Experience Platform] enforces a purely additive versioning principle to ensure that any revisions to the schema only result in non-destructive updates and changes. つまり、**後方互換性を壊すような変更**&#x200B;はサポートされません。

| サポートされる変更 | 後方互換性のない変更（サポートなし） |
|------------------------------------|---------------------------------|
| <ul><li>既存スキーマへの新しいフィールドの追加</li><li>必須フィールドのオプション化</li></ul> | <ul><li>以前に定義したフィールドの削除</li><li>新しい必須フィールドの紹介</li><li>既存のフィールドの名前の変更または再定義</li><li>以前にサポートされていたフィールド値の削除または制限</li><li>ツリー内の別の場所への属性の移動</li></ul> |

>[!NOTE]
>
>If a schema has not yet been used to ingest data into [!DNL Experience Platform], you may introduce a breaking change to that schema. However, once the schema has been used in [!DNL Platform], it must adhere to the additive versioning policy.

### スキーマとデータの取得

In order to ingest data into [!DNL Experience Platform], a dataset must first be created. Datasets are the building blocks for data transformation and tracking for [[!DNL Catalog Service]](../../catalog/home.md), and generally represent tables or files that contain ingested data. すべてのデータセットは既存の XDM スキーマに基づいており、取得するデータの内容と構造の制約を提供します。詳しくは、「[Adobe Experience Platform でのデータ取得の概要](../../ingestion/home.md)」を参照してください。

## スキーマの構成要素

[!DNL Experience Platform] では、標準の構築要素を組み合わせてスキーマを作成する構成アプローチを使用します。This approach promotes the reusability of existing components and drives standardization across the industry to support vendor schemas and components in [!DNL Platform].

スキーマは、次の式を使用して構成されます。

**Class + Mixin&amp;ast; = XDM スキーマ**

&amp;ast;スキーマは、クラスと _0 個以上_&#x200B;の Mixin で構成されます。つまり、Mixin を使用せずにデータセットスキーマを作成できます。

### クラス

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。これに加えて、クラスは、そのクラスに基づくすべてのスキーマに含める必要のある共通のプロパティの最小数を記述し、複数の互換性のあるデータセットを結合する方法を提供します。

クラスは、どの Mixin をスキーマで使用できるかを決定します。これについては、後述の「[Mixin](#mixin)」の節で詳しく説明します。

There are standard classes provided with every integration of [!DNL Experience Platform], known as &quot;Industry&quot; classes. 業界クラスは、一般的に認められる業界標準で、幅広い使用例に適用されます。Examples of Industry classes include the [!DNL XDM Individual Profile] and [!DNL XDM ExperienceEvent] classes provided by Adobe.

[!DNL Experience Platform] また、「Vendor」クラスも使用できます。これは、パートナーによって定義され、内でそのベンダーサービスまたはアプリケーションを使用するすべての顧客が利用でき [!DNL Experience Platform][!DNL Platform]ます。

There are also classes used to describe more specific use cases for individual organizations within [!DNL Platform], called &quot;Customer&quot; classes. 顧客クラスは、一意の使用例を説明するために使用できる業界クラスまたはベンダークラスがない場合、組織によって定義されます。

For example, a schema representing members of a Loyalty program describes record data about an individual and therefore can be based on the [!DNL XDM Individual Profile] class, a standard Industry class defined by Adobe.

### Mixin {#mixin}

Mixin は、個人の詳細、ホテルの環境設定、住所などの特定の機能を実装する 1 つ以上のフィールドを定義する再利用可能なコンポーネントです。Mixin は、互換性のあるクラスを実装するスキーマの一部として含まれることを意図しています。

Mixin は、表すデータ（レコードまたは時系列）の動作に基づいて、互換性のあるクラスを定義します。つまり、すべての Mixin がすべてのクラスで使用できるわけではありません。

Mixins have the same scope and definition as classes: there are Industry mixins, Vendor mixins, and Customer mixins that are defined by individual organizations using [!DNL Platform]. [!DNL Experience Platform] には多くの標準的な業界 Mixin が含まれていますが、ベンダーはユーザーの Mixin を定義し、個々のユーザーは独自の概念の Mixin を定義できます。

For example, to capture details such as &quot;[!UICONTROL First Name]&quot; and &quot;[!UICONTROL Home Address]&quot; for your &quot;[!UICONTROL Loyalty Members]&quot; schema, you would be able to use standard mixins that define those common concepts. However, concepts that are specific to less-common use cases (such as &quot;[!UICONTROL Loyalty Program Level]&quot;) often do not have a pre-defined mixin. この場合、この情報を取り込むには、独自の Mixin を定義する必要があります。

スキーマは「0 個以上」の Mixin で構成されているので、Mixin を一切使用しないでも有効なスキーマを作成できます。

### データタイプ {#data-type}

データタイプは、基本リテラルフィールドと同様に、クラスやスキーマの参照フィールド型として使用されます。主な違いは、データタイプが複数のサブフィールドを定義できる点です。Mixin と同様に、データタイプはマルチフィールド構造の一貫した使用を可能にしますが、データタイプはフィールドの「データタイプ」として追加することでスキーマ内の任意の場所に含めることができるので、Mixin よりも柔軟性が高くなります。

[!DNL Experience Platform] には、一般的なデータ構造を記述するための標準パターンの使用をサポートするため [!DNL Schema Registry] の一部として、多くの共通データ型が用意されています。 This is explained in more detail in the [!DNL Schema Registry] tutorials, where it will become more clear as you walk through the steps to define data types.

### フィールド

フィールドは、スキーマの最も基本的な構成要素です。フィールドには、特定のデータタイプを定義することで含めることができるデータの種類に関する制約が含まれます。これらの基本的なデータタイプは単一のフィールドを定義しますが、前述の[データタイプ](#data-type)では複数のサブフィールドを定義し、様々なスキーマで同じ複数フィールド構造を再利用できます。So, in addition to defining a field&#39;s &quot;data type&quot; as one of the data types defined in the registry, [!DNL Experience Platform] supports basic scalar types such as:

* 文字列
* 整数
* 数値
* Boolean
* 配列
* オブジェクト

これらのスカラー型の有効な範囲は、特定のパターン、形式、最小値や最大値、事前定義値にさらに制限できます。これらの制約を使用すると、以下のような、より詳細なフィールドタイプを幅広く表すことができます。

* Enum
* Long
* Short
* Byte
* Date
* Date-time
* Map

>[!NOTE]
>
>「マップ」フィールドタイプでは、1 つのキーの複数の値を含む、キーと値のペアのデータを使用できます。マップは、システムレベルでのみ定義できます。つまり、業界またはベンダー定義のスキーマでマップが見つかる場合がありますが、定義したフィールドでは使用できません。フィールドの種類の定義について詳しくは、『[スキーマレジストリ API 開発者ガイド](../api/getting-started.md)』を参照してください。

ダウンストリームサービスやアプリケーションで使用される一部のデータ操作では、特定のフィールドタイプに制約が適用されます。影響を受けるサービスには、次のものが含まれます。

* [[!DNLリアルタイム顧客プロファイル]](../../profile/home.md)
* [[!DNL IDサービス]](../../identity-service/home.md)
* [[!DNLセグメント]](../../segmentation/home.md)
* [[!DNLクエリサービス]](../../query-service/home.md)
* [[!DNL Data Science Workspace]](../../data-science-workspace/home.md)

ダウンストリームサービスで使用するスキーマを作成する前に、スキーマが意図するデータ操作のフィールド要件と制約をより深く理解するために、これらのサービスに関する適切なドキュメントを確認してください。

### XDM フィールド

In addition to basic fields and the ability to define your own data types, XDM provides a standard set of fields and data types that are implicitly understood by [!DNL Experience Platform] services and provide greater consistency when used across [!DNL Platform] components.

These fields, such as &quot;First Name&quot; and &quot;Email Address&quot; contain added connotations beyond basic scalar field types, telling [!DNL Platform] that any fields sharing the same XDM data type will behave in the same way. This behavior can be trusted to be consistent regardless of where the data is coming from, or in which [!DNL Platform] service the data is being used.

使用可能な XDM フィールドの完全なリストについては [XDM フィールドディクショナリ](field-dictionary.md)を参照してください。It is recommended to use XDM fields and data types wherever possible to support consistency and standardization across [!DNL Experience Platform].

## 合成の例

Schemas represent the format and structure of data that will be ingested into [!DNL Platform], and are built using a composition model. 前述のとおり、これらのスキーマはクラスと、そのクラスと互換性のある 0 個以上の Mixin で構成されます。

For example, a schema describing purchases made at a retail store might be called &quot;[!UICONTROL Store Transactions]&quot;. The schema implements the [!DNL XDM ExperienceEvent] class combined with the standard [!UICONTROL Commerce] mixin and a user-defined [!UICONTROL Product Info] mixin.

Another schema which tracks website traffic might be called &quot;[!UICONTROL Web Visits]&quot;. It also implements the [!DNL XDM ExperienceEvent] class, but this time combines the standard [!UICONTROL Web] mixin.

次の図に、これらのスキーマと各 Mixin が寄与するフィールドを示します。It also contains two schemas based on the [!DNL XDM Individual Profile] class, including the &quot;[!UICONTROL Loyalty Members]&quot; schema mentioned previously in this guide.

![](../images/schema-composition/composition.png)

### 和集合 {#union}

While [!DNL Experience Platform] allows you to compose schemas for particular use cases, it also allows you to see a &quot;union&quot; of schemas for a specific class type. The previous diagram shows two schemas based on the XDM ExperienceEvent class and two schemas based on [!DNL XDM Individual Profile] class. The union, shown below, aggregates the fields of all schemas that share the same class ([!DNL XDM ExperienceEvent] and [!DNL XDM Individual Profile], respectively).

![](../images/schema-composition/union.png)

By enabling a schema for use with [!DNL Real-time Customer Profile], it will be included in the union for that class type. [!DNL Profile] 顧客属性の堅牢で一元的なプロファイルと、統合されたあらゆるシステムにおいて顧客が持つすべてのイベントのタイムスタンプのあるアカウントを提供 [!DNL Platform]します。 [!DNL Profile] 和集合表示を使用してこのデータを表し、個々の顧客の全体的な表示を提供します。

For more information on working with [!DNL Profile], see the [Real-time Customer Profile overview](../../profile/home.md).

## XDM スキーマへのデータファイルのマッピング

All datafiles that are ingested into [!DNL Experience Platform] must conform to the structure of an XDM schema. XDM 階層（サンプルファイルを含む）に準拠するようにデータファイルをフォーマットする方法の詳細は、[ETL 変換のサンプル](../../etl/transformations.md)に関するドキュメントを参照してください。For general information about ingesting datafiles into [!DNL Experience Platform], see the [batch ingestion overview](../../ingestion/batch-ingestion/overview.md).

## 次の手順

Now that you understand the basics of schema composition, you are ready to begin building schemas using the [!DNL Schema Registry].

The [!DNL Schema Registry] is used to access the [!DNL Schema Library] within Adobe Experience Platform, and provides a user interface and RESTful API from which all available library resources are accessible. The [!DNL Schema Library] contains Industry resources defined by Adobe, Vendor resources defined by [!DNL Experience Platform] partners, and classes, mixins, data types, and schemas that have been composed by members of your organization.

UI を使用してスキーマの構成を開始するには、[スキーマエディターのチュートリアル](../tutorials/create-schema-ui.md)を参照しながら、このドキュメントで取り上げる「ロイヤルティメンバー」スキーマを作成します。

To begin using the [!DNL Schema Registry] API, start by reading the [Schema Registry API developer guide](../api/getting-started.md). 開発者ガイドを読んだ後、[スキーマレジストリ API を使用したスキーマの作成](../tutorials/create-schema-api.md)に関するチュートリアルで説明されている手順に従います。