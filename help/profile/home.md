---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルの概要
topic: guide
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '1666'
ht-degree: 1%

---


# [!DNL Real-time Customer Profile]概要

Adobe Experience Platformを使用すると、顧客がブランドとどこで、いつやり取りしたかに関係なく、顧客に対して、調整され、一貫性のある関連性のあるエクスペリエンスを提供できます。 を使用 [!DNL Real-time Customer Profile]すると、オンライン、オフライン、CRM、サードパーティなどの複数のチャネルからのデータを組み合わせた個々の顧客の全体的な表示を確認できます。 [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。 この概要は、の役割と使用方法を理解するのに役立ち [!DNL Real-time Customer Profile] ま [!DNL Experience Platform]す。

## Understanding [!DNL Real-time Customer Profile]

[!DNL Real-time Customer Profile] は、様々な企業データアセットからのデータを結合し、個々の顧客プロファイルや関連する時系列イベントの形式でそのデータへのアクセスを提供する、汎用の参照エンティティストアです。 この機能により、マーケターは、複数のチャネルにわたるオーディエンスに対して、調整され、一貫性のある、関連性のあるエクスペリエンスを提供できます。

### [!DNL Profile] データストア

取り込まれたデータを [!DNL Real-time Customer Profile] 処理し、Adobe Experience Platform [!DNL Identity Service] を使用してIDマッピングを通じて関連データを結合しますが、独自のデータを [!DNL Profile] ストアに保持します。 つまり、 [!DNL Profile] 店舗は、 [!DNL Catalog] データ([!DNL Data Lake])および [!DNL Identity Service] データ（アイデンティティグラフ）とは別のものです。

### [!DNL Profile] および [!DNL Platform] サービス

次の図では、 [!DNL Real-time Customer Profile] との他のサービスとの関係 [!DNL Experience Platform] が強調表示されています。

![プロファイルと他のExperience Platformサービスとの関係。](images/profile-overview/profile-in-platform.png)

### プロファイルと記録データ

プロファイルとは、対象、組織、または個人を表したもので、レコードデータとも呼ばれます。 例えば、製品のプロファイルにはSKUと説明が含まれ、個人のプロファイルには名、姓、電子メールアドレスなどの情報が含まれます。 を使用 [!DNL Experience Platform]すると、ビジネスに関連するデータのタイプを使用するようにプロファイルをカスタマイズできます。 標準 [!DNL Experience Data Model] (XDM) [!DNL Individual Profile] クラスは、顧客レコードデータを記述する際にスキーマを構築し、Platformサービス間の多くのやり取りにデータ統合を提供するのに適したクラスです。 でスキーマを使用する方法の詳細は、まず [!DNL Experience Platform]XDMシステムの概要を読んでください [](../xdm/home.md)。

### 時系列イベント

時系列データは、対象者が直接または間接的にアクションを実行した時点のシステムのスナップショットと、イベント自体を詳細に示すデータを提供します。 標準のスキーマクラスXDM ExperienceEventによって表される時間系列データは、買い物かごに追加されるイベント、クリックされるリンク、ビデオ閲覧などのアイテムを表します。 時系列データは、セグメントルールの基にするために使用でき、プロファイルのコンテキストでイベントに個別にアクセスできます。

### ID

どの企業も、個人的な感じ方で顧客とコミュニケーションを取りたいと思っています。 ただし、関連するデジタルエクスペリエンスを顧客に提供する際の課題の1つは、切断されたデータを結び付ける方法を理解することです。この方法は、タブレット、携帯電話、ラップトップなどの様々なデジタルチャネルに多く見られます。 [!DNL Identity Service] 複数の顧客のIDをリンクし、各チャネルのIDグラフを作成することで、顧客の全体像をまとめ、顧客のIDをより深く理解できます。 詳しくは、「 [IDサービスの概要](../identity-service/home.md) 」を参照してください。

### セグメント化

Adobe Experience Platform [!DNL Segmentation Service] は、個々の顧客に対してパワーエクスペリエンスを提供するために必要なオーディエンスを生成します。 オーディエンスセグメントを作成すると、そのセグメントのIDが、すべての資格を持つプロファイルのセグメントメンバーシップのリストに追加されます。 セグメントルールは、RESTful APIとセグメントビルダーユーザーインターフェイスを使用して作成し、 [!DNL Real-time Customer Profile] データに適用します。 セグメント化の詳細については、 [Segmentation Serviceの概要を参照してください](../segmentation/home.md)。

### プロファイルフラグメントと和集合スキーマ {#profile-fragments-and-union-schemas}

の主な特徴の1つ [!DNL Real-time Customer Profile] は、複数チャネルのデータを統合する機能です。 を使用 [!DNL Real-time Customer Profile] してエンティティにアクセスすると、そのエンティティのすべてのプロファイルフラグメントのマージされた表示を和集合表示と呼ばれ、和集合スキーマと呼ばれる要素を介して提供できます。 [!DNL Real-time Customer Profile] データは、エンティティやプロファイルがIDでアクセスされたり、セグメントとしてエクスポートされたりすると、ソース間でマージされます。 APIを使用したプロファイルおよび和集合表示へのアクセスについて詳しくは、 [!DNL Real-time Customer Profile] Entitiesエンドポイントガイドを参照してください [](api/entities.md)。

### 結合ポリシー

複数のソースからデータを統合し、それを組み合わせて各顧客の完全な表示を確認する場合、マージポリシーは、データの優先順位付け方法とどのデータを組み合わせて統合表示を作成するかを決定するために使用されるルールです。 [!DNL Platform] RESTful APIまたはユーザーインターフェイスを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 APIを使用した結合ポリシーの操作について詳しくは、 [!DNL Real-time Customer Profile][結合ポリシーエンドポイントガイドを参照してください](api/merge-policies.md)。 UIを使用して結合ポリシーを操作するには、 [!DNL Experience Platform][merge policiesユーザーガイドを参照してください](ui/merge-policies.md)。

### (Alpha)計算済み属性の設定

>[!IMPORTANT]
>このドキュメントで概要を説明する計算済みの属性機能は、alphaです。 ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。 計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントに対して集計値を実行できます。 計算済みの各属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式(「rule」)が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーション開封回数などに関する質問に簡単に回答できます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。 計算済み属性の詳細、および [!DNL Real-time Customer Profile] APIを使用して計算済み属性を操作する手順については、 [計算済み属性エンドポイントガイドを参照してください](api/computed-attributes.md)。 このガイドは、計算された属性がAdobe Experience Platform内で果たす役割をより深く理解するのに役立ちます。また、基本的なCRUD操作を実行するためのサンプルAPI呼び出しが含まれています。

## リアルタイムコンポーネント

この節では、レコードと時系列データをリアルタイムで更新 [!DNL Real-time Customer Profile] および監視できるコンポーネントについて説明します。

### ストリーミング取り込みとストリーミングセグメント化

リアルタイム入力は、ストリーミング取り込みと呼ばれるプロセスによって可能になります。 プロファイルと時系列データが取り込まれると、は、既存のデータと結合して和集合表示を更新する前に、ストリーミングセグメント化と呼ばれる継続的なプロセスを通じて、そのデータをセグメントに含めるか、除外するかを自動的に決定します。 [!DNL Real-time Customer Profile] その結果、ブランドとのやり取りを通じて顧客に対して、計算を瞬時に実行し、強化された個別のエクスペリエンスを提供する意思決定を行うことができます。 データは取り込まれる間、適切に取り込まれ、データセットの基盤となるスキーマに従うかどうかの検証も行われます。 取り込み中に行われる検証の詳細については、まず [データ取り込みの質の概要を読んでください](../ingestion/quality/overview.md)。

### Edge Projectionの設定と宛先

複数のチャネルにわたって顧客に対して、調整、一貫性、パーソナライズされたエクスペリエンスをリアルタイムで提供するためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。 Adobe Experience Platformを使用すると、エッジと呼ばれるものを使用して、データにリアルタイムでアクセスできます。 エッジは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバです。 例えば、Adobe TargetやAdobe Campaignなどのアドビアプリケーションは、パーソナライズされた顧客体験をリアルタイムで提供するためにエッジを使用します。 データは投影によってエッジにルーティングされ、投影先はデータの送信先となるエッジを定義し、投影設定はエッジで利用可能にする特定の情報を定義します。 詳しくは、 [!DNL Real-time Customer Profile] APIを使用した投影の操作を開始する方法については、『 [エッジ投影エンドポイントガイド](api/edge-projections.md)』を参照してください。

## ～へ追加のデータ [!DNL Real-time Customer Profile]

[!DNL Platform] を設定して、にレコードと時系列のデータを送信できます。これにより、リアルタイムストリーミングの取り込みとバッチ取り込みがサポートされ [!DNL Profile]ます。 詳細については、チュートリアルの概要に記載されているチュートリアルの「リアルタイム顧客プロファイルにデータを [追加する方法](tutorials/add-profile-data.md)」を参照してください。

>[!N注意]
>アドビのソリューションで収集されたデータ( [!DNL Analytics Cloud]、、 [!DNL Marketing Cloud]およびを含む)は、に流れ込み、 [!DNL Advertising Cloud]に取り込まれ [!DNL Experience Platform][!DNL Profile]ます。

### [!DNL Profile] 摂取指標

観察性インサイトを使用すると、主要指標をAdobe Experience Platformに表示できます。 様々な機能の [!DNL Platform] 使用状況統計およびパフォーマンスインジケーターに加えて、特定の [!DNL Platform][!DNL Profile]関連指標があり、入力要求率、取り込み成功率、取り込まれたレコードのサイズなどを把握できます。 詳細については、「 [観察性インサイトの概要](../observability/home.md)」を読み、 [!DNL Profile] 指標の完全なリストについては、 [利用可能な指標に関するドキュメントを参照してください](../observability/metrics.md)。

## [!DNL Data governance] および [!DNL Privacy]

[!DNL Data governance] は、お客様のデータを管理し、データの使用に適用される規制、制限、ポリシーの遵守を確保するために使用される一連の戦略とテクノロジーです。

データへのアクセスに関しては、データガバナンスが様々なレベルで重要な役割 [!DNL Experience Platform] を果たします。
* データ使用のラベル付け
* データアクセスポリシー
* マーケティングアクションのデータに関するアクセス制御

[!DNL Data governance] は、いくつかのポイントで管理されます。 これには、取り込むデータや、取り込んだ後にアクセスできるデータ [!DNL Platform] を特定のマーケティングアクションのために決定することなどが含まれます。 詳細については、 [データ管理の概要を読んでください](../data-governance/home.md)。

### オプトアウトおよびデータのプライバシー要求の処理

[!DNL Experience Platform] 顧客は、内部のデータの使用状況およびストレージに関するオプトアウト要求を送信でき [!DNL Real-time Customer Profile]ます。 オプトアウト要求の処理方法について詳しくは、オプトアウト要求の [実行に関するドキュメントを参照してください](../segmentation/honoring-opt-outs.md)。

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
| 非人間エンティティごとのデータセット | 個人以外のエンティティに関連付けることができる **データセットは、最大** 1つです。 |

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

詳しくは、ドキュメントを読み続け [!DNL Real-time Customer Profile]て、以下のビデオを見て学習を補完するか、他の [Experience Platformビデオチュートリアルを参照してください](https://docs.adobe.com/content/help/en/platform-learn/tutorials/overview.html)。

>[!WARNING]
>次のビデオに表示される [!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、 [リアルタイム顧客プロファイルユーザーガイド](ui/user-guide.md) (Real-time Customer User Guide)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27251?quality=12)