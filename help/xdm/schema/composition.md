---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；列挙；mixin；フィールドグループ；フィールドグループ；mixin；データ型；データ型；データ型；プライマリID；プライマリID;XDM個人プロファイル；列挙型；エクスペリエンスイベント；XDM ExperienceEvent;experienceEvent;experienceEvent;XDM ExperienceEvent；スキーマデザイン；クラス；クラス；クラス；クラス；データ型；データ型；データ型；データ型；スキーマ；IDマップ；IDマップ；IDマップ；スキーマデザイン；マップ；和集合スキーマ；和集合スキーマ
solution: Experience Platform
title: スキーマ構成の基本
topic-legacy: overview
description: このドキュメントでは、エクスペリエンスデータモデル（XDM）スキーマの概要と、Adobe Experience Platform で使用するスキーマを構成するための構成要素、原則およびベストプラクティスを紹介します。
exl-id: d449eb01-bc60-4f5e-8d6f-ab4617878f7e
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '3726'
ht-degree: 31%

---

# スキーマ合成の基本

このドキュメントでは、[!DNL Experience Data Model](XDM)スキーマの概要と、Adobe Experience Platformで使用するスキーマを構成するための構成要素、原則およびベストプラクティスを説明します。 XDMと[!DNL Platform]内での使用方法に関する一般的な情報については、「[XDMシステムの概要](../home.md)」を参照してください。

## スキーマ

スキーマは、データの構造と形式を表し、検証する一連のルールです。スキーマは、高いレベルで、実世界のオブジェクト（人など）の抽象的な定義を提供し、そのオブジェクトの各インスタンスに含めるデータ（名、姓、誕生日など）の概要を示します。

スキーマは、データの構造を説明するだけでなく、制約や期待をデータに適用し、システム間の移動時に検証できるようにします。これらの標準的な定義により、接触チャネルに関係なくデータを一貫して解釈でき、アプリケーション間での翻訳の必要性を排除できます。

[!DNL Experience Platform] スキーマを使用して、このセマンティックの正規化を維持します。Schemas are the standard way of describing data in [!DNL Experience Platform], allowing all data that conforms to schemas to be reused across an organization without conflicts, or even shared between multiple organizations.

XDMスキーマは、大量の複雑なデータを自己完結型の形式で保存するのに最適です。 XDMがこれをどのように実現するかについて詳しくは、このドキュメントの付録にある[埋め込みオブジェクト](#embedded)と[ビッグデータ](#big-data)の節を参照してください。

### [!DNL Experience Platform]のスキーマベースのワークフロー

標準化は、[!DNL Experience Platform]の背後にある重要な概念です。 アドビが推進する XDM は、顧客エクスペリエンスデータを標準化し、顧客エクスペリエンス管理の標準スキーマを定義する取り組みです。

The infrastructure on which [!DNL Experience Platform] is built, known as [!DNL XDM System], facilitates schema-based workflows and includes the [!DNL Schema Registry], [!DNL Schema Editor], schema metadata, and service consumption patterns. 詳しくは、「[XDM システムの概要](../home.md)」を参照してください。

[!DNL Experience Platform]でスキーマを活用する主なメリットはいくつかあります。 第1に、スキーマは、より優れたデータガバナンスとデータ最小化を可能にします。これは、プライバシー規制で特に重要です。 第2に、Adobeの標準コンポーネントを使用してスキーマを作成すると、カスタマイズを最小限に抑えながら、標準のインサイトとAI/MLサービスの使用が可能になります。 最後に、スキーマは、データ共有のインサイトと効率的なオーケストレーションのためのインフラストラクチャを提供します。

## スキーマの計画

スキーマを構築する最初の手順は、スキーマ内で捕捉しようとする概念、すなわち現実世界のオブジェクトを決定することです。説明しようとしている概念を特定したら、データのタイプ、潜在的な ID フィールド、将来のスキーマの発展について考え、スキーマの計画を始めることができます。

### Data behaviors in [!DNL Experience Platform]

[!DNL Experience Platform]での使用を意図したデータは、次の2つの動作タイプにグループ化されます。

* **レコードデータ**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **時系列データ**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

すべての XDM スキーマは、レコードまたは時系列として分類できるデータを記述します。スキーマのデータ動作は、スキーマのクラスによって定義され、スキーマの作成時に割り当てられます。XDM クラスについては、このドキュメントで後述します。

レコードと時系列の両方のスキーマには、ID のマップ（`xdm:identityMap`）が含まれます。このフィールドには、次の節で説明する「ID」とマークされたフィールドから作成された、主体の ID 表現が含まれます。

### [!UICONTROL ID] {#identity}

スキーマは、データを[!DNL Experience Platform]に取り込むために使用されます。 このデータは、複数のサービスで使用して、個々のエンティティの単一の統合表示を作成できます。したがって、スキーマについて考える際には、顧客IDと、データの送信元に関係なく、対象を識別するために使用できるフィールドについて考えることが重要です。

この処理を支援するために、スキーマ内のキーフィールドをIDとしてマークできます。 データ取り込み時に、これらのフィールドのデータがその個人の「[!UICONTROL IDグラフ]」に挿入されます。 その後、グラフデータは、各顧客の関連付けられた表示を提供するために、[[!DNL Real-time Customer Profile]](../../profile/home.md)および他の[!DNL Experience Platform]サービスによってアクセスされます。

Fields that are commonly marked as &quot;[!UICONTROL Identity]&quot; include: email address, phone number, [[!DNL Experience Cloud ID (ECID)]](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja), CRM ID, or other unique ID fields. また、「[!UICONTROL ID]」フィールドにも適切な場合があるので、組織固有の一意の識別子も考慮する必要があります。

スキーマ計画段階で顧客IDを考慮し、可能な限り堅牢なプロファイルを構築するためにデータを統合できるようにすることが重要です。 See the overview on [Adobe Experience Platform Identity Service](../../identity-service/home.md) to learn more about how identity information can help you deliver digital experiences to your customers.

IDデータをPlatformに送信する方法は2つあります。

1. [スキーマエディターのUI](../ui/fields/identity.md)または[スキーマレジストリAPI](../api/descriptors.md#create)を使用して、ID記述子を個々のフィールドに追加する
1. [`identityMap`フィールド](#identityMap)の使用

#### `identityMap` {#identityMap}

`identityMap` は、個人の様々なID値と、関連する名前空間を説明するマップタイプのフィールドです。このフィールドは、スキーマ自体の構造内でID値を定義する代わりに、スキーマのID情報を提供するために使用できます。

`identityMap`を使用する主な欠点は、IDがデータに埋め込まれ、結果として見えなくなることです。 生データを取り込む場合は、個々のIDフィールドを実際のスキーマ構造内で定義する必要があります。 `identityMap`を使用するスキーマも関係に参加できません。

However, identity maps can be particularly useful if you are bringing in data from sources that store identities together (such as [!DNL Airship] or Adobe Audience Manager), or when there are a variable number of identities for a schema. In addition, identity maps are required if you are using the [Adobe Experience Platform Mobile SDK](https://aep-sdks.gitbook.io/docs/).

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

上記の例で示すように、`identityMap`オブジェクトの各キーはID名前空間を表します。 各キーの値は、各名前空間のID値(`id`)を表すオブジェクトの配列です。 Adobeアプリケーションで認識される標準ID名前空間](../../identity-service/troubleshooting-guide.md#standard-namespaces)の[リストについては、 [!DNL Identity Service]のドキュメントを参照してください。

>[!NOTE]
>
>値がプライマリID(`primary`)であるかどうかを表すboolean値もID値ごとに指定できます。 プライマリIDは、[!DNL Real-time Customer Profile]で使用するスキーマに対してのみ設定する必要があります。 詳しくは、[和集合スキーマ](#union)の節を参照してください。

### スキーマ進化の原理 {#evolution}

デジタルエクスペリエンスの本質が進化し続けるにつれ、デジタルエクスペリエンスを表すスキーマが必要になります。したがって、適切に設計されたスキーマは、必要に応じて適応し、進化し、以前のバージョンのスキーマに破壊的な変更を加えることなく使用できます。

Since maintaining backwards compatibility is crucial for schema evolution, [!DNL Experience Platform] enforces a purely additive versioning principle. この原則により、スキーマのリビジョンは、非破壊的な更新と変更のみを生み出します。 つまり、**後方互換性を壊すような変更**&#x200B;はサポートされません。

>[!NOTE]
>
>If a schema has not yet been used to ingest data into [!DNL Experience Platform] and hasn&#39;t been enabled for use in Real-time Customer Profile, you may introduce a breaking change to that schema. ただし、[!DNL Platform]でスキーマを使用した後は、追加のバージョン管理ポリシーに従う必要があります。

次の表に、スキーマ、フィールドグループおよびデータタイプの編集時にサポートされる変更を示します。

| サポートされる変更 | 後方互換性のない変更（サポートなし） |
| --- | --- |
| <ul><li>リソースへの新しいフィールドの追加</li><li>必須フィールドのオプション化</li><li>リソースの表示名と説明の変更</li><li>スキーマのプロファイルへの参加の有効化</li></ul> | <ul><li>以前に定義したフィールドの削除</li><li>新しい必須フィールドの紹介</li><li>既存のフィールドの名前の変更または再定義</li><li>以前にサポートされていたフィールド値の削除または制限</li><li>Moving existing fields to a different location in the tree</li><li>スキーマの削除</li><li>プロファイルへのスキーマの参加の無効化</li></ul> |

### スキーマとデータの取得

[!DNL Experience Platform]にデータを取り込むには、まずデータセットを作成する必要があります。 データセットは、[[!DNL Catalog Service]](../../catalog/home.md)のデータ変換と追跡の構成要素で、通常、取り込んだデータを含むテーブルやファイルを表します。 すべてのデータセットは既存の XDM スキーマに基づいており、取得するデータの内容と構造の制約を提供します。詳しくは、「[Adobe Experience Platform でのデータ取得の概要](../../ingestion/home.md)」を参照してください。

## スキーマの構成要素

[!DNL Experience Platform] では、標準の構築要素を組み合わせてスキーマを作成する構成アプローチを使用します。このアプローチは、既存のコンポーネントの再利用性を促進し、業界全体の標準化を推進して、[!DNL Platform]のベンダーのスキーマとコンポーネントをサポートします。

スキーマは、次の式を使用して構成されます。

**Class + Schema Field Group&amp;ast; = XDM Schema**

&amp;ast；スキーマは、クラスと0個以上のスキーマフィールドグループで構成されます。 This means that you could compose a dataset schema without using field groups at all.

### クラス {#class}

クラスを割り当てることで、スキーマの構成が開始されます。クラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動面を定義します。これに加えて、クラスは、そのクラスに基づくすべてのスキーマに含める必要のある共通のプロパティの最小数を記述し、複数の互換性のあるデータセットを結合する方法を提供します。

A schema&#39;s class determines which field groups will be eligible for use in that schema. これについては、[次の節](#field-group)で詳しく説明します。

Adobe provides several standard (&quot;core&quot;) XDM classes. これらのクラスのうち、[!DNL XDM Individual Profile]と[!DNL XDM ExperienceEvent]の2つは、ほぼすべてのダウンストリームPlatformプロセスに必要です。 In addition these core classes, you can also create your own custom classes to describe more specific use cases for your organization. カスタムクラスは、一意の使用例を説明するAdobe定義のコアクラスがない場合に、組織によって定義されます。

次のスクリーンショットは、Platform UIでクラスがどのように表されるかを示しています。 表示されるサンプルスキーマにはフィールドグループが含まれていないので、表示されるすべてのフィールドは、スキーマのクラス([!UICONTROL XDM Individual Profile])によって提供されます。

![](../images/schema-composition/class.png)

使用可能な標準XDMクラスの最新のリストについては、[公式のXDMリポジトリ](https://github.com/adobe/xdm/tree/master/components/classes)を参照してください。 Alternatively, you can refer to the guide on [exploring XDM components](../ui/explore.md) if you prefer to view resources in the UI.

### [フィールド]領域 {#field-group}

フィールドグループは、個人の詳細、ホテルの環境設定、住所など、特定の機能を実装する1つ以上のフィールドを定義する再利用可能なコンポーネントです。 フィールドグループは、互換性のあるクラスを実装するスキーマの一部として含まれるように意図されています。

フィールドグループは、表すデータ（レコードまたは時系列）の動作に基づいて、互換性のあるクラスを定義します。 つまり、すべてのフィールドグループがすべてのクラスで使用できるわけではありません。

[!DNL Experience Platform] には、多くの標準Adobeフィールドグループが含まれますが、ベンダーはユーザーのフィールドグループを定義し、個々のユーザーは独自の概念のフィールドグループを定義できます。

例えば、「[!UICONTROL ロイヤルティメンバー]」スキーマの「[!UICONTROL 名]」や「[!UICONTROL 自宅住所]」などの詳細を取り込むには、これらの共通概念を定義する標準フィールドグループを使用できます。 ただし、一般的でない使用例に固有の概念（「[!UICONTROL ロイヤルティプログラムレベル]」など）には、多くの場合、定義済みのフィールドグループはありません。 この場合、この情報を取り込むには、独自のフィールドグループを定義する必要があります。

スキーマは「0個以上」のフィールドグループで構成されるので、フィールドグループをまったく使用せずに有効なスキーマを作成できます。

次のスクリーンショットは、Platform UIでフィールドグループがどのように表されるかを示しています。 この例では、1つのフィールドグループ（[!UICONTROL 人口統計的詳細]）がスキーマに追加され、スキーマの構造に対してフィールドのグループが提供されます。

![](../images/schema-composition/field-group.png)

使用可能な標準XDMフィールドグループの最新のリストについては、[公式のXDMリポジトリ](https://github.com/adobe/xdm/tree/master/components/fieldgroups)を参照してください。 Alternatively, you can refer to the guide on [exploring XDM components](../ui/explore.md) if you prefer to view resources in the UI.

### データタイプ {#data-type}

データタイプは、基本リテラルフィールドと同様に、クラスやスキーマの参照フィールド型として使用されます。主な違いは、データタイプが複数のサブフィールドを定義できる点です。Similar to a field group, a data type allows for the consistent use of a multi-field structure, but has more flexibility than a field group because a data type can be included anywhere in a schema by adding it as the &quot;data type&quot; of a field.

[!DNL Experience Platform] には、共通のデータ構造を記述するための標準パタ [!DNL Schema Registry] ーンの使用をサポートするために、の一部として多数の一般的なデータ型が用意されています。これについては、 [!DNL Schema Registry]のチュートリアルで詳しく説明しています。このチュートリアルでは、データタイプを定義する手順を実行すると、より明確になります。

次のスクリーンショットは、データタイプがPlatform UIでどのように表されるかを示しています。 [!UICONTROL Details]フィールドグループで指定されるフィールドの1つでは、「[!UICONTROL Person name]」データ型が使用されます。このデータ型は、フィールド名の横にパイプ文字(`|`)の後に続くテキストで示されます。 この特定のデータ型は、個々の人の名前に関連するいくつかのサブフィールドを提供します。これは、人の名前を取り込む必要がある他のフィールドで再利用できる構成体です。

![](../images/schema-composition/data-type.png)

使用可能な標準XDMデータタイプの最新のリストについては、[公式のXDMリポジトリ](https://github.com/adobe/xdm/tree/master/components/datatypes)を参照してください。 また、UIでリソースを表示したい場合は、 [XDMコンポーネントの詳細](../ui/explore.md)に関するガイドを参照することもできます。

### フィールド

フィールドは、スキーマの最も基本的な構成要素です。フィールドには、特定のデータタイプを定義することで含めることができるデータの種類に関する制約が含まれます。これらの基本的なデータタイプは単一のフィールドを定義しますが、前述の[データタイプ](#data-type)では複数のサブフィールドを定義し、様々なスキーマで同じ複数フィールド構造を再利用できます。So, in addition to defining a field&#39;s &quot;data type&quot; as one of the data types defined in the registry, [!DNL Experience Platform] supports basic scalar types such as:

* 文字列
* 整数
* Double
* Boolean
* 配列
* オブジェクト

>[!TIP]
>
>オブジェクトタイプフィールドに対するフリーフォームフィールドの使用に関する長所と短所については、[付録](#objects-v-freeform)を参照してください。

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

* [[!DNL Real-time Customer Profile]](../../profile/home.md)
* [[!DNL Identity Service]](../../identity-service/home.md)
* [[!DNL Segmentation]](../../segmentation/home.md)
* [[!DNL Query Service]](../../query-service/home.md)
* [[!DNL Data Science Workspace]](../../data-science-workspace/home.md)

ダウンストリームサービスで使用するスキーマを作成する前に、スキーマが意図するデータ操作のフィールド要件と制約をより深く理解するために、これらのサービスに関する適切なドキュメントを確認してください。

### XDM フィールド

XDMは、基本的なフィールドと独自のデータ型を定義する機能に加えて、[!DNL Experience Platform]サービスで暗黙的に認識されるフィールドとデータ型の標準セットを提供し、[!DNL Platform]コンポーネント間で使用する場合に一貫性を高めます。

These fields, such as &quot;First Name&quot; and &quot;Email Address&quot; contain added connotations beyond basic scalar field types, telling [!DNL Platform] that any fields sharing the same XDM data type will behave in the same way. この動作は、データの送信元や、データが使用されている[!DNL Platform]サービスに関係なく、一貫性を保つために信頼できます。

使用可能な XDM フィールドの完全なリストについては [XDM フィールドディクショナリ](field-dictionary.md)を参照してください。[!DNL Experience Platform]全体での一貫性と標準化をサポートするために、可能な限りXDMフィールドとデータ型を使用することをお勧めします。

## 合成の例

Schemas represent the format and structure of data that will be ingested into [!DNL Platform], and are built using a composition model. 前述のように、これらのスキーマは、クラスと、そのクラスと互換性のある0個以上のフィールドグループで構成されます。

例えば、小売店で行われた購入を記述するスキーマを、「[!UICONTROL 店舗トランザクション]」と呼ぶことができます。 The schema implements the [!DNL XDM ExperienceEvent] class combined with the standard [!UICONTROL Commerce] field group and a user-defined [!UICONTROL Product Info] field group.

Webサイトトラフィックを追跡する別のスキーマは、「[!UICONTROL Web訪問回数]」と呼ばれる場合があります。 また、[!DNL XDM ExperienceEvent]クラスも実装しますが、今回は標準の[!UICONTROL Web]フィールドグループを組み合わせます。

The diagram below shows these schemas and the fields contributed by each field group. また、このガイドで前述した「[!UICONTROL ロイヤルティメンバー]」スキーマを含む、[!DNL XDM Individual Profile]クラスに基づく2つのスキーマが含まれます。

![](../images/schema-composition/composition.png)

### 結合 {#union}

[!DNL Experience Platform]では特定の使用例のスキーマを作成できますが、特定のクラスタイプのスキーマの「和集合」を確認することもできます。 上の図は、XDM ExperienceEventクラスに基づく2つのスキーマと、[!DNL XDM Individual Profile]クラスに基づく2つのスキーマを示しています。 The union, shown below, aggregates the fields of all schemas that share the same class ([!DNL XDM ExperienceEvent] and [!DNL XDM Individual Profile], respectively).

![](../images/schema-composition/union.png)

スキーマを[!DNL Real-time Customer Profile]で使用できるようにすると、そのクラスタイプの和集合に含められます。 [!DNL Profile] は、顧客属性の堅牢で一元化されたプロファイルと、と統合されたシステム全体で顧客がおこなったすべてのイベントのタイムスタンプ付きの説明を提供しま [!DNL Platform]す。[!DNL Profile] は和集合表示を使用してこのデータを表し、各顧客の全体像を提供します。

[!DNL Profile]の使用について詳しくは、「 [リアルタイム顧客プロファイルの概要](../../profile/home.md) 」を参照してください。

## XDM スキーマへのデータファイルのマッピング

[!DNL Experience Platform]に取り込まれるすべてのデータファイルは、XDMスキーマの構造に準拠している必要があります。 XDM 階層（サンプルファイルを含む）に準拠するようにデータファイルをフォーマットする方法の詳細は、[ETL 変換のサンプル](../../etl/transformations.md)に関するドキュメントを参照してください。For general information about ingesting datafiles into [!DNL Experience Platform], see the [batch ingestion overview](../../ingestion/batch-ingestion/overview.md).

## 外部セグメントのスキーマ

外部システムからPlatformにセグメントを取り込む場合は、次のコンポーネントを使用してスキーマに取り込む必要があります。

* [[!UICONTROL セグメ] ント定義クラス](../classes/segment-definition.md):この標準クラスを使用して、外部セグメント定義のキー属性を取り込みます。
* [[!UICONTROL セグメントメンバーシ] ップの詳細フィールドグループ](../field-groups/profile/segmentation.md):顧客プロファイルを特定のセグメ [!UICONTROL ントに関連付け] るために、このフィールドグループをXDM個別プロファイルスキーマに追加します。

## 次の手順

これで、スキーマ構成の基本を理解したので、[!DNL Schema Registry]を使用してスキーマを調べ、構築する準備が整いました。

2つのコアXDMクラスと、よく使用される互換性のあるフィールドグループの構造を確認するには、次の参照ドキュメントを参照してください。

* [[!DNL XDM Individual Profile]](../classes/individual-profile.md)
* [[!DNL XDM ExperienceEvent]](../classes/experienceevent.md)

[!DNL Schema Registry]は、Adobe Experience Platform内の[!DNL Schema Library]にアクセスするために使用され、使用可能なすべてのライブラリリソースにアクセスできるユーザーインターフェイスとRESTful APIを提供します。 [!DNL Schema Library]には、Adobeが定義する業界リソース、[!DNL Experience Platform]パートナーが定義するベンダーリソース、組織のメンバーが構成するクラス、フィールドグループ、データ型、スキーマが含まれます。

UI を使用してスキーマの構成を開始するには、[スキーマエディターのチュートリアル](../tutorials/create-schema-ui.md)を参照しながら、このドキュメントで取り上げる「ロイヤルティメンバー」スキーマを作成します。

[!DNL Schema Registry] APIの使用を開始するには、まず『[スキーマレジストリAPI開発者ガイド](../api/getting-started.md)』を読んでください。 開発者ガイドを読んだ後、[スキーマレジストリ API を使用したスキーマの作成](../tutorials/create-schema-api.md)に関するチュートリアルで説明されている手順に従います。

## 付録

The following sections contain additional information regarding the principles of schema composition.

### リレーショナルテーブルと埋め込みオブジェクト {#embedded}

リレーショナルデータベースを使用する場合のベストプラクティスには、データの正規化、またはエンティティを取得してそれを個別のピースに分割し、複数のテーブルに表示することが含まれます。データ全体の読み取りやエンティティの更新を行うには、JOIN を使用して、多くの個々のテーブルに対して読み取りおよび書き込み操作をおこなう必要があります。

Through the use of embedded objects, XDM schemas can directly represent complex data and store it in self-contained documents with a hierarchical structure. この構造の主な利点の 1 つは、複数の非正規化テーブルへの高価な結合によってエンティティを再構築しなくても、データをクエリできる点です。There are no hard restrictions to how many levels your schema hierarchy can be.

### スキーマとビッグデータ {#big-data}

現代のデジタルシステムは、大量の行動信号（トランザクションデータ、Web ログ、物のインターネット、ディスプレイなど）を生成します。このビッグデータは、エクスペリエンスを最適化する非常に大きな機会を生み出しますが、データの規模と多様性が原因で、使用が困難です。データから値を得るには、一貫性と効率性を持って処理できるように、構造、形式および定義を標準化する必要があります。

スキーマは、複数のソースからのデータの統合、共通の構造や定義による標準化、複数のソリューション間での共有を可能にすることで、この問題を解決します。これにより、後続のプロセスとサービスは、データについて尋ねられるあらゆるタイプの質問に答えることができます。これは、データについて尋ねられるすべての質問が事前にわかっていて、データがそれらの期待に準拠するようにモデル化される従来のデータモデリングアプローチとは異なります。

### Objects versus free-form fields {#objects-v-freeform}

スキーマをデザインする際に、フリーフォームのフィールド上でオブジェクトを選択する際に考慮すべき重要な要因は次のとおりです。

| オブジェクト | 自由形式のフィールド |
| --- | --- |
| ネストが増加 | ネストが少ない、またはない |
| Creates logical field groupings | フィールドはアドホックな場所に配置されます |

{style=&quot;table-layout:auto&quot;}

#### オブジェクト

The pros and cons of using objects over free-form fields are listed below.

**長所**:

* オブジェクトは、特定のフィールドの論理的なグループを作成する場合に最も適しています。
* Objects organize the schema in a more structured manner.
* オブジェクトは、セグメントビルダーUIで適切なメニュー構造を間接的に作成するのに役立ちます。 スキーマ内のグループ化されたフィールドは、セグメントビルダーUIで提供されるフォルダー構造に直接反映されます。

**短所**:

* フィールドがネストされます。
* [Adobe Experience Platformクエリサービス](../../query-service/home.md)を使用する場合、オブジェクトにネストされたクエリフィールドには、長い参照文字列を指定する必要があります。

#### 自由形式のフィールド

オブジェクトに対するフリーフォームフィールドの使用に関する長所と短所を次に示します。

**長所**:

* フリーフォームフィールドは、スキーマのルートオブジェクト(`_tenantId`)の直下に作成され、表示が向上します。
* クエリサービスを使用すると、フリーフォームフィールドの参照文字列が短くなる傾向があります。

**短所**:

* スキーマ内の自由形式のフィールドの場所はアドホックです。つまり、スキーマエディター内では、アルファベット順に表示されます。 これにより、スキーマの構造が小さくなり、類似したフリーフォームフィールドが名前に応じて大きく区切られる可能性があります。
