---
keywords: Experience Platform;ホーム;人気のあるトピック;XDM;XDM システム;XDM 個人プロファイル;XDM エクスペリエンスイベント;XDM エクスペリエンスイベント;エクスペリエンスイベント;エクスペリエンスイベント;フィールドグループ;フィールドグループ;フィールドグループ;フィールドグループ;エクスペリエンスイベント;XDM エクスペリエンスイベント;XDM エクスペリエンスイベント;エクスペリエンスイベント;エクスペリエンスイベント;XDM エクスペリエンスイベント;エクスペリエンスデータモデル;エクスペリエンスデータモデル;エクスペリエンスデータモデル;データモデル;データモデル;スキーマレジストリ;スキーマレジストリ;スキーマライブラリ;スキーマライブラリ;スキーマ;レコードデータ;時系列;時系列
solution: Experience Platform
title: XDM システムの概要
description: 標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。
exl-id: 294d5f02-850f-47ea-9333-8b94a0bb291e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '2135'
ht-degree: 67%

---

# XDM システムの概要

標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。あらゆるアプリケーションとExperience Platform サービスの通信に使用できる共通の構造と定義を提供します。 XDM 標準に準拠することで、すべての顧客体験データを共通の表現に組み込み、より迅速かつ統合的な方法でインサイトを得ることができます。顧客のアクションから貴重なインサイトを得たり、セグメントを使用して顧客のオーディエンスを定義したり、パーソナライゼーションを目的として顧客属性を表すことができます。

XDM は、Experience Platform が提供する Adobe Experience Cloud が、適切なタイミングに、適切なチャネル経由で、適切な相手へと適切なメッセージを届けることを可能にする、基本的なフレームワークです。Experience Platformの基礎となる XDM システムは、Experience Data Model スキーマをExperience Platform サービスで操作できるようにします。

Experience Platformにおける XDM システムの役割について説明します。

## XDM スキーマ {#xdm-schemas}

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データをExperience Platformに取り込む前に、データの構造を記述し、各フィールドに含めることができるデータのタイプに制約を適用するためのスキーマを作成する必要があります。 スキーマは、基本クラスと 0 個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[ スキーマ構成の基本 ](schema/composition.md) を参照してください。

### 標準 XDM コンポーネント {#Standard-xdm-components}

XDM は、標準的なフィールドグループとデータタイプの堅牢なコレクションを提供します。これらは、様々な業界にわたる共通の概念およびユースケースを把握することを目的としています。Experience Platform を使用すると、これらのコンポを業界別にフィルタリングでき、特定のビジネスニーズをサポートする最適なスキーマを迅速かつ確実に構築できます。

Experience Platform UI でスキーマを構築する場合、リストされたフィールドグループが人気度指標と共に表示されます。この指標は、他のExperience Platform ユーザーがスキーマでそのフィールドグループを使用する頻度によって決定されます。 数値が大きくなるほど、フィールドグループの人気度が高くなります。デフォルトでは、最も人気のあるものから順に結果が表示されるので、業界のデータモデリングのトレンドを常に把握できます。

![ フィールドグループを追加 [!UICONTROL &#x200B; ダイアログの「人気度」列 &#x200B;] す。](./images/overview/popularity.png)

### [!DNL Schema Library] {#schema-library}

Experience Platform は、Experience Platform **[!DNL Schema Library]** にある、スキーマ関連のすべてのリソースを表示および管理できるユーザーインターフェイスと RESTful API を提供します。[!DNL Schema Library] には、アドビが提供する標準 XDM コンポーネントに加え、Experience Platform パートナーや、お客様が使用するアプリケーションのベンダーからのリソースが含まれています。

また、Experience Platform UI の [!DNL Schema Registry API] または [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースを使用して、組織に固有の新しいスキーマとリソースを作成および管理できます。

Experience Platformのスキーマの管理方法と操作方法について詳しくは、次のドキュメントを参照してください。

* [XDM UI ガイド](./ui/overview.md)
* [Schema Registry API ガイド](./api/overview.md)

## XDM システムでのデータの動作 {#data-behaviors}

>[!CONTEXTUALHELP]
>id="platform_schemas_behavior"
>title="データの動作"
>abstract="Experience Platform での使用を意図したデータは、レコード、時系列およびアドホックの 3 つの動作タイプに分けられます。レコードスキーマは、サブジェクトの属性に関する情報を提供し、時系列スキーマは、アクションが実行された時点でのシステムのスナップショットをキャプチャします。アドホックスキーマは、単一のデータセットでのみ使用するための名前空間であるフィールドをキャプチャします。Experience Platform でのデータの動作について詳しくは、ドキュメントを参照してください。"

Experience Platform 用のデータは、次の 3 つの動作タイプに分類されます。

* **レコード**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **時系列**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。
* **アドホック**：単一のデータセットでのみ使用するために名前空間が設定されているフィールドをキャプチャします。アドホックスキーマは、CSV ファイルの取り込みや特定の種類のソース接続の作成など、Experience Platform の様々なデータ取り込みワークフローで使用されます。

すべての XDM スキーマは、レコードまたは時系列として分類できるデータを記述します。スキーマのデータ動作は、スキーマのクラスによって定義され、スキーマの作成時に割り当てられます。XDM クラスは、特定のデータ動作を表すためにスキーマが格納する必要のある最小のプロパティ数を記述します。

[!DNL Schema Registry] 内で独自のクラスを定義できますが、レコードと時系列のデータには、それぞれ標準の **[!UICONTROL XDM 個人プロファイル]** と **[!UICONTROL XDM エクスペリエンスイベント]** を使用することをお勧めします。 これらのクラスの詳細については、以下で説明します。

>[!NOTE]
>
>アドホック動作に基づく標準クラスはありません。アドホックスキーマは、それを使用するExperience Platform プロセスによって自動的に生成されますが、[Schema Registry API を使用して手動で作成する ](./tutorials/ad-hoc.md) こともできます。

### [!UICONTROL XDM 個人プロファイル] {#xdm-individual-profile}

[!UICONTROL XDM 個人プロファイル &#x200B;] は、識別された主体や部分識別された主体の両方の属性を 1 つの表現で示す、レコードベースのクラスです。 識別の高いプロファイルは、個人的なコミュニケーションやターゲットを絞ったエンゲージメントに使用される場合があります。 識別されたプロファイルには、名前、性別、生年月日、場所、電話番号やメールアドレスなどの連絡先情報など、詳細な個人情報が含まれる場合があります。

識別されにくいプロファイルは、ブラウザーの cookie のように、匿名の行動シグナルのみで構成される場合があります。この場合、少ないプロファイルデータを使用して、匿名プロファイルの興味や好みを照合し、格納する情報ベースを構築します。これらの識別子は、時間が経過すると、主体が通知、購読、購入などに新規登録するため、より詳細になる場合があります。これによってプロファイル属性が増加すると、最終的に対象項目が特定され、ターゲットを絞ったエンゲージメントの度合いが高くなる可能性があります。

プロファイルが増え続けるに従って、ある人物の個人情報、識別情報、連絡先の詳細、コミュニケーションの環境設定を含む堅牢なリポジトリとなります。

クラスが提供するフィールドの構造およびユースケースについて詳しくは、[[!UICONTROL XDM 個人プロファイル]リファレンスガイド](./classes/individual-profile.md)を参照してください。

### [!UICONTROL XDM ExperienceEvent] {#xdm-experience-event}

XDM ExperienceEvent は、イベント（または一連のイベント）が発生したときのシステムの状態（主体が関与する時点や ID など）を取り込むために使用される、時系列ベースのクラスです。エクスペリエンスイベントは、その時点で起こったことを集約や解釈なしに表現した、変更できない事実の記録です。特定の時間枠内で発生した変更を分析し、複数の時間枠を比較してトレンドを追跡するために使用できるので、時間ドメイン分析には非常に重要です。

エクスペリエンスイベントは、明示的または暗黙的に指定できます。明示的なイベントは、ジャーニーのある時点で起きた人間のアクションを直接観察できます。暗黙的なイベントとは、直接的な人間のアクションなしで発生するけれど、個人に関わるイベントです。暗黙的なイベントの例としては、電子メールニュースレターのスケジュールされた送信や、特定のしきい値に到達するバッテリーの電圧などがあります。

すべてのイベントがすべてのデータソースをまたいで簡単に分類できるとは限りませんが、類似したイベントを、できる限り似たタイプのイベントと調和させるとは非常に便利です。

![ エクスペリエンスイベントを使用して経時的に可視化したカスタマージャーニーのインフォグラフィック。](images/overview/experience-event-journey.png)

クラスが提供するフィールドの構造およびユースケースについて詳しくは、[[!UICONTROL XDM エクスペリエンスイベント]リファレンスガイド](./classes/experienceevent.md)を参照してください。

## XDM スキーマと Experience Platform サービス {#schemas-and-platform-services}

Experience Platformは、スキーマに依存しません。つまり、XDM 標準に準拠するすべてのスキーマがExperience Platform サービスで使用できます。 様々なExperience Platform サービスによるスキーマの使用方法について、以下で詳しく説明します。

### カタログサービス、データ取り込み、データレイク {#ingestion-catalog-and-storage}

カタログサービスは、Experience Platform のアセットとその関連するスキーマの記録システムです。カタログは、実際のデータファイルやディレクトリを含まず、これらのファイルやディレクトリのメタデータや説明を保持します。

カタログデータは、接触チャネルやファイルフォーマットに関係なく、Experience Platformで管理されるすべてのデータを含む、非常にきめ細かいデータストアであるデータレイクに保存されます。

Experience Platform へのデータ取り込みを開始するには、カタログサービスを使用してデータセットを作成します。データセットは、取り込むデータの構造を記述した XDM スキーマを参照します。スキーマを使用せずにデータセットを作成した場合、Experience Platform は、取り込んだデータフィールドのタイプと内容を調べることで、「観察されたスキーマ」を派生します。その後、データセットはカタログサービスで追跡され、ベースとなるスキーマおよび観測されたスキーマと共にデータレイクに保存されます。

詳しくは、[ カタログサービスの概要 ](../catalog/home.md) を参照してください。 Adobe Experience Platformのデータ取り込みについて詳しくは、[ データ取り込みの概要 ](../ingestion/home.md) を参照してください。

### クエリサービス {#query-service}

標準の SQL を使用してExperience Platform データに対してクエリを実行すると、Adobe Experience Platform クエリサービスでの様々なユースケースをサポートできます。

スキーマを構成し、そのスキーマを参照するデータセットを作成すると、データが取り込まれ、データレイクに保存されます。 その後、クエリサービスを使用してデータレイク内のデータセットを結合し、クエリ結果を新しいデータセットとして取得して、レポート、機械学習、リアルタイム顧客プロファイルへの取り込みで使用できます。

サービスについて詳しくは、[クエリサービスの概要](../query-service/home.md)を参照してください。

### リアルタイム顧客プロファイル {#real-time-customer-profile}

リアルタイム顧客プロファイルは、ターゲットを絞ったパーソナライズされたエクスペリエンスを管理するための一元的な顧客プロファイルを提供します。各プロファイルには、すべてのシステムにわたって集計されたデータが含まれており、プロファイルの主体に関連するイベントの、タイムスタンプの付いた実用的なアカウントが含まれています。 これらのイベントは、Experience Platformで使用するシステムのいずれかで発生した可能性があります。

リアルタイム顧客プロファイルは、[!UICONTROL XDM 個人プロファイル &#x200B;] および [!UICONTROL XDM エクスペリエンスイベント &#x200B;] クラスに基づくスキーマ形式のデータを使用し、そのデータに基づいてクエリに応答します。

システムは、各顧客プロファイルのインスタンスを維持し、データを結合して個人の「信頼できる唯一の情報源」を形成します。この統合データは、「結合スキーマ」と呼ばれるもの（「結合ビュー」と呼ばれることもあります）を使用して表現されます。結合スキーマは、同じクラスを実装するすべてのスキーマのフィールドを単一のスキーマに集約します。 UI または API を使用してスキーマを構成する際には、スキーマをリアルタイム顧客プロファイルで使用できるようにしたり、タグ付けして結合に含めたりできます。その後、タグ付きスキーマは、プロファイルにフィードされるスキーマ定義に追加されます。

[!UICONTROL XDM 個人プロファイル &#x200B;] および [!UICONTROL XDM ExperienceEvent] のデータがデータレイクに取り込まれると、リアルタイム顧客プロファイルは、その使用が有効になっているすべてのデータを取り込みます。 取り込まれるインタラクションや詳細が多いほど、個人プロファイルは正確になります。

[!UICONTROL XDM 個人プロファイル]データは、あらゆるチャネルやアドビ製品統合をまたいで、情報を提供したり、アクションを強化したりするのに役立ちます。豊富な行動履歴とインタラクションデータを組み合わせることで、このデータを機械学習に活用できます。リアルタイム顧客プロファイル API は、サードパーティソリューション、CRM および独自ソリューションの機能を強化するためにも使用できます。

詳しくは、[リアルタイム顧客プロファイルの概要](../profile/home.md)を参照してください。

### データサイエンスワークスペース {#data-science-workspace}

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。 このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

Adobe Experience Platform Data Science Workspace は、機械学習と人工知能を使用して、Experience Platform 内に保存されたデータからインサイトを得ます。データサイエンティストは、Data Science Workspaceを使用することで、顧客とそのアクティビティに関する [!UICONTROL XDM 個人プロファイル &#x200B;] および [!UICONTROL XDM ExperienceEvent] データに基づいてレシピを作成できます。 これらのレシピは、購入傾向や、個人が評価して使用する可能性が高い推奨オファーなどの予測を容易にします。

Data Science Workspace を使用すれば、データサイエンティストは、機械学習を活用したインテリジェントサービス API を簡単に作成できます。これらのサービスは、Adobe Target や Adobe Analytics Cloud などの他のアドビソリューションと連携して、パーソナライズされ、ターゲットを絞ったデジタルエクスペリエンスを自動化します。

Experience Platform データを使用してインサイトを強化する方法について詳しくは、[Data Science Workspace の概要](../data-science-workspace/home.md)を参照してください。

## 次の手順とその他のリソース

これで、Experience Platform 全体でのスキーマの役割についての理解を深めることができ、独自のスキーマの作成を開始する準備が整いました。

Experience Platform で使用するスキーマを構成するためのデザインの原則とベストプラクティスを学ぶには、まず[スキーマ構成の基本](schema/composition.md)をお読みください。スキーマの作成手順については、[API を使用した](tutorials/create-schema-api.md)または[ユーザーインターフェイスを使用した](tutorials/create-schema-ui.md)スキーマの作成に関するチュートリアルを参照してください。

Experience Platform の [!DNL XDM System] の理解を深めるために、次のビデオを視聴してください。

>[!VIDEO](https://video.tv.adobe.com/v/38507?quality=12&learn=on&captions=jpn)
