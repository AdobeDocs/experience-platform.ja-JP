---
keywords: Experience Platform；ホーム；人気の高いトピック；XDM;XDMプロファイル;XDM個人イベント;XDM ExperienceEvent;XDM ExperienceEvent;XDM ExperienceEvent;ExperienceEvent;Field groups;Field group;Experienceイベント;XDMeniceEvent;event;XDM Experienceevent;Experience Data Model;Experience Data Model;Data Model;Data Model;スキーマレジストリ；スキーマレジストリ；イベントライブラリ；イベント；レコードデータ；時系列；時系列
solution: Experience Platform
title: XDMシステムの概要
topic-legacy: overview
description: 標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。アドビが推進するエクスペリエンスデータモデル（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。
exl-id: 294d5f02-850f-47ea-9333-8b94a0bb291e
translation-type: tm+mt
source-git-commit: b70e693b4ffeda561de4d4c8dd8fd1adeec489f7
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 44%

---

# XDM システムの概要

>[!NOTE]
>
>「mixin」という用語は、理解を促進するためにスキーマ「フィールドグループ」に更新されました。 フィールドグループは、ビジネスでの使用例をサポートする再利用可能なフィールドのセットです。 この変更は、スキーマレジストリAPI、Adobe Experience PlatformUI、およびすべてのプラットフォームドキュメントに反映されます。

標準化と相互運用性は、Adobe Experience Platform の背後にある重要な概念です。[!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。[!DNL Platform]サービスとの通信に使用するアプリケーションの共通の構造と定義を提供します。 XDM 標準に準拠することで、すべての顧客体験データを共通の表現に組み込み、より迅速かつ統合的な方法でインサイトを得ることができます。顧客の行動から貴重なインサイトを得たり、セグメントを使用して顧客のオーディエンスを定義したり、パーソナライゼーションを目的として顧客属性を表すことができます。

XDMは、[!DNL Experience Platform]の力を借りたAdobe Experience Cloudが、正しい人に正しいチャネルに、正しいメッセージを、まさに適切なタイミングで届けることを可能にする基礎的な枠組みです。 [!DNL Experience Platform]が構築される方法論XDM Systemは、[!DNL Platform]サービスが使用する[!DNL Experience Data Model]スキーマを操作します。

このドキュメントでは、[!DNL Experience Platform]内のXDMシステムの役割の概要を説明します。

## XDM スキーマ

[!DNL Experience Platform] では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。システムをまたいで一貫したデータを定義することで、意味を保有しやすくなり、データから価値を得ることができます。

データを[!DNL Platform]に取り込む前に、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類に制約を与える必要があります。 スキーマは、基本クラスと、0個以上のスキーマフィールドグループで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、[スキーマ構成の基本](schema/composition.md)を参照してください。

### [!DNL Schema Registry] および [!DNL Schema Library]

**[!DNL Schema Registry]**&#x200B;は、Adobe Experience Platform **[!DNL Schema Library]**&#x200B;内のすべてのスキーマ関連リソースを表示し、管理するためのユーザーインターフェイスとRESTful APIを提供します。 [!DNL Schema Library]には、Adobeが提供する業界標準のリソースと、使用するアプリケーションの[!DNL Experience Platform]パートナーやベンダーのリソースが含まれています。 また、スキーマレジストリの UI と API を使用して、組織に固有の新しいスキーマやリソースを作成および管理することもできます。

[!DNL Schema Registry]で入手できる主な操作の詳細なガイドについては、[スキーマレジストリ開発者ガイド](api/getting-started.md)を参照してください。

## XDM システムでのデータの動作 {#data-behaviors}

[!DNL Experience Platform]での使用を意図したデータは、次の2つの動作タイプに分類されます。

* **レコードデータ**：主体の属性に関する情報を提供します。主体は、組織または個人にすることができます。
* **時系列データ**：レコードの主体によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

すべての XDM スキーマは、レコードまたは時系列として分類できるデータを記述します。スキーマのデータ動作は、スキーマのクラスによって定義され、スキーマの作成時に割り当てられます。XDM クラスは、特定のデータ動作を表すためにスキーマが格納する必要のある最小のプロパティ数を記述します。

独自のクラスを[!DNL Schema Registry]内に定義できますが、レコードと時系列のデータにはそれぞれ優先クラス&#x200B;**[!DNL XDM Individual Profile]**&#x200B;と&#x200B;**[!DNL XDM ExperienceEvent]**&#x200B;を使用することをお勧めします。 これらのクラスの詳細については、以下で説明します。

### [!DNL XDM Individual Profile] {#xdm-individual-profile}

[!DNL XDM Individual Profile] は、特定された対象と部分的に識別された対象の両方の属性の単数形式の表現を形成する、レコードベースのクラスです。高度に識別されるプロファイルには、個人的なコミュニケーションやターゲットを絞ったエンゲージメントに使用したり、詳細な個人情報（名前、性別、生年月日、場所など）および連絡先情報（電話番号や電子メールアドレスなど）を含めたりできます。

識別されにくいプロファイルは、ブラウザーの cookie のように、匿名の行動シグナルのみで構成される場合があります。この場合、少ないプロファイルデータを使用して、匿名プロファイルの興味や好みを照合し、格納する情報ベースを構築します。これらの識別子は、時間が経過すると、主体が通知、購読、購入などに新規登録するため、より詳細になる場合があります。これによってプロファイル属性が増加すると、最終的に件名が特定され、ターゲットを絞ったエンゲージメントの度合いが高くなる可能性があります。

消費者プロファイルが増え続けるにつれ、ある人物の個人情報、識別情報、連絡先の詳細、コミュニケーションの環境設定を含む堅牢なリポジトリーとなります。

### [!DNL XDM ExperienceEvent] {#xdm-experience-event}

XDM ExperienceEvent は、イベント（または一連のイベント）が発生したときのシステムの状態（サブジェクトが関与する時点や ID など）を取り込むために使用される、時系列ベースのクラスです。エクスペリエンスイベントは、何が発生したかに関する事実の記録なので、不変であり、何が発生したかを集計や解釈を加えずに表します。特定の時間枠内で発生した変更を分析し、複数の時間枠を比較してトレンドを追跡するために使用できるので、時間ドメイン分析には非常に重要です。

エクスペリエンスイベントは、明示的または暗黙的に指定できます。明示的なイベントは、ジャーニーのある時点で起きた人間の行動を直接観察できます。暗黙的なイベントとは、直接的な人間のアクションなしで発生するけれど、個人に関わるイベントです。暗黙的なイベントの例としては、電子メールニュースレターのスケジュールされた送信や、特定のしきい値に到達するバッテリーの電圧などがあります。

すべてのイベントがすべてのデータソースをまたいで簡単に分類できるとは限りませんが、類似したイベントを、できる限り似たタイプのイベントと調和させるとは非常に便利です。

![ExperienceEvent カスタマージャーニー](images/overview/experience-event-journey.png)

## XDMスキーマと[!DNL Experience Platform]サービス

[!DNL Experience Platform] はスキーマにとらわれず、XDM標準に準拠するスキーマはすべて [!DNL Platform] サービスで使用できます。異なる[!DNL Platform]サービスがスキーマを使用する方法について、以下で詳しく説明します。

### [!DNL Catalog Service],  [!DNL Data Ingestion] &amp;  [!DNL Data Lake]

[!DNL Catalog Service] は、 [!DNL Experience Platform] アセットとその関連スキーマの記録システムです。[!DNL Catalog] は、実際のデータを含むファイルやディレクトリではなく、これらのファイルやディレクトリのメタデータや説明を保持します。

[!DNL Catalog] データは、接触チャネル [!DNL Data Lake]やファイル形式に関係なく、によって管理されるすべてのデータを含む、非常に詳細なデータストア [!DNL Platform]に保存されます。

[!DNL Experience Platform]へのデータの取り込みを開始するには、[!DNL Catalog Service]を使用してデータセットを作成します。 データセットは、取り込むデータの構造を記述した XDM スキーマを参照します。スキーマなしでデータセットを作成した場合、[!DNL Experience Platform]は、取り込んだデータフィールドの種類と内容を調べ、「観測されたスキーマ」を引き出します。 次に、データセットは[!DNL Catalog]で追跡され、スキーマとその基となる監視スキーマーの横の[!DNL Data Lake]に保存されます。

[!DNL Catalog]について詳しくは、[カタログサービスの概要](../catalog/home.md)を参照してください。 Adobe Experience Platform のデータ取り込みについて詳しくは、[データ取り込みの概要](../ingestion/home.md)を参照してください。

### [!DNL Query Service]

Adobe Experience Platform[!DNL Query Service]では、標準のSQLからクエリ[!DNL Experience Platform]データを使用して、様々な使用例をサポートできます。

スキーマを構成し、そのスキーマを参照するデータセットを作成した後、データを[!DNL Data Lake]に取り込んで保存します。 [!DNL Query Service]を使用すると、[!DNL Data Lake]内の任意のデータセットに参加し、クエリ結果を新しいデータセットとして取り込み、レポート、機械学習、または[!DNL Real-time Customer Profile]への取り込みに使用できます。

[!DNL Query Service]の詳細については、[クエリサービスの紹介](../query-service/home.md)を参照してください。

### [!DNL Real-time Customer Profile]

リアルタイムの顧客プロファイルは、ターゲットを絞り、パーソナライズされたエクスペリエンス管理のための一元的な顧客プロファイルを提供します。各プロファイルには、すべてのシステムにわたって集計されたデータと、[!DNL Experience Platform]で使用するシステムのいずれかで行われた個人のイベントに関する、対応可能なタイムスタンプ付きのアカウントが含まれます。

[!DNL Real-time Customer Profile] は、 [!DNL XDM Individual Profile] OR [!DNL XDM ExperienceEvent] クラスに基づいてスキーマ形式のデータを使用し、そのデータに基づいてクエリに応答します。[!DNL Profile] は、他のクラスに基づくスキーマの使用をサポートしていません。

[!DNL Profile] 各顧客プロファイルのインスタンスを維持し、データを結合して個人の「単一の真実の源泉」を形成します。この統合データは、「和集合ビュー」と呼ばれるものを使用して表されます。和集合ビューは、同じクラスを実装するすべてのスキーマのフィールドを、1 つのスキーマに集計します。UIまたはAPIを使用してスキーマを構成する場合、スキーマの[!DNL Real-time Customer Profile]での使用を有効にし、和集合表示に含めるタグを付けることができます。 次に、タグ付きスキーマは[!DNL Profile]に送られるスキーマ定義に参加します。

[!DNL XDM Individual Profile]と[!DNL XDM ExperienceEvent]のデータは[!DNL Catalog]に取り込まれ、管理されるので、[!DNL Real-time Customer Profile]をトリガーして、使用可能になったデータの取り込みを開始します。 取り込まれるインタラクションや詳細が多いほど、個人プロファイルは強力になります。

[!DNL XDM Individual Profile] データは、チャネルやAdobeソリューションの統合全体にわたってアクションを伝え、強化します。行動やインタラクションデータの豊富な履歴と組み合わせると、このデータを電源機械学習に使用します。[!DNL Real-time Customer Profile] APIは、サードパーティのソリューション、CRM、および独自仕様のソリューションの機能を強化するためにも使用できます。

詳しくは、[リアルタイム顧客プロファイルの概要](../profile/home.md)を参照してください。

### [!DNL Data Science Workspace]

Adobe Experience Platform[!DNL Data Science Workspace]は、機械学習と人工知能を使って[!DNL Experience Platform]に保存されたデータから洞察を得ています。 [!DNL Data Science Workspace] データ科学者は、XDM Individualに基づくレシピ [!DNL Profile] と、顧客とそのアクティビティに関する [!DNL XDM ExperienceEvent] データを作成でき、購入傾向や推奨オファーなど、個人が認識し、使用する可能性が高い予測を容易にできます。

[!DNL Data Science Workspace]を使うと、データ科学者は機械学習を利用したインテリジェントなサービスAPIを簡単に作成できます。 これらのサービスは、Adobe Target や Adobe Analytics Cloud などの他のアドビソリューションと連携して、パーソナライズされ、ターゲットを絞ったデジタルエクスペリエンスを自動化します。

[!DNL Experience Platform]データを使用してインサイトを強化する方法の詳細については、[データサイエンスワークスペースの概要](../data-science-workspace/home.md)を参照してください。

## 次の手順とその他のリソース

これで、[!DNL Experience Platform]を通じてスキーマの役割をより深く理解できたので、自分で作成する開始を作る準備が整いました。 学習内容の補足を続けるには、推奨ドキュメントを読み、次のビデオをご覧ください。

[!DNL Experience Platform]で使用するスキーマを構成する際の設計原則とベストプラクティスを学ぶには、まずスキーマ組成の[基本を読みます](schema/composition.md)。 スキーマの作成手順については、[API を使用した](tutorials/create-schema-api.md)または[ユーザーインターフェイスを使用した](tutorials/create-schema-ui.md)スキーマの作成に関するチュートリアルを参照してください。

[!DNL Experience Platform]の[!DNL XDM System]に対する理解を深めるには、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/27105?quality=12&learn=on)
