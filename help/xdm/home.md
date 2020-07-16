---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: エクスペリエンスデータモデル(XDM)システム
topic: overview
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---


# XDMシステムの概要

標準化と相互運用性は、Adobe Experience Platformの背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、アドビが推進する機能で、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義するための取り組みです。

XDMは、デジタルエクスペリエンスのパワーを向上させるために設計された、公開された仕様です。 サービスとの通信に使用するアプリケーションの共通の構造と定義を提供し [!DNL Platform] ます。 XDM標準を守ることで、すべての顧客体験データを共通の表現に組み込むことができ、より迅速で統合的な方法でインサイトを提供できます。 顧客の行動から貴重なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションの目的で顧客属性を表したりできます。

XDMは、Adobe Experience Cloudの基本的なフレームワークです。Adobe Experience Cloudの基本的な機能 [!DNL Experience Platform]を活用し、適切なチャネルを適切な人に、まさに適切なタイミングで配信できます。 構築され [!DNL Experience Platform] た方法論、 **XDM System**、は、サー [!DNL Experience Data Model] ビスで使用する [!DNL Platform] スキーマを運用します。

このドキュメントでは、内のXDMシステムの役割の概要を説明 [!DNL Experience Platform]します。

## XDMスキーマ

[!DNL Experience Platform] では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。 システム間で一貫したデータを定義することで、意味を保持しやすくなり、データから価値を得ることができます。

データをに取り込む前に [!DNL Platform]、スキーマを構成して、データの構造を記述し、各フィールドに含めることができるデータの種類に制限を設ける必要があります。 スキーマは、基本クラスと0個以上のミックスインで構成されます。

デザインの原則やベストプラクティスなど、スキーマ構成モデルについて詳しくは、スキーマ構成の [基本を参照してください](schema/composition.md)。

### [!DNL Schema Registry] および [!DNL Schema Library]

に **[!DNL Schema Registry]** は、Adobe Experience Platform内のすべてのスキーマ関連リソースを表示および管理できるユーザーインターフェイスおよびRESTful APIが用意されて **[!DNL Schema Library]**&#x200B;います。 には、アドビが提供する業界標準のリソース、およびアプリケーションを使用する [!DNL Schema Library][!DNL Experience Platform] パートナーやベンダーのリソースが含まれます。 また、スキーマレジストリのUIとAPIを使用して、組織に固有の新しいスキーマやリソースを作成および管理することもできます。

に用意されている主な操作の詳細なガイドについては、『 [!DNL Schema Registry]スキーマレジストリ開発者ガイド [](api/getting-started.md)』を参照してください。

## XDMシステムでのデータ動作 {#data-behaviors}

での使用を意図したデータは、2つの動作タイプ [!DNL Experience Platform] に分類されます。

* **Record data**: 件名の属性に関する情報を提供します。 件名は、組織または個人にすることができます。
* **時系列データ**: レコードの件名によって直接または間接的にアクションが実行された時点のシステムのスナップショットを提供します。

すべてのXDMスキーマは、レコードまたは時系列として分類できるデータを記述します。 スキーマのデータ動作は、スキーマの **クラス**(最初に作成されたときにスキーマに割り当てられる)によって定義されます。 XDMクラスは、特定のデータの動作を表すためにスキーマが持つ必要のある最小のプロパティ数を記述します。

内で独自のクラスを定義できますが、レコードと時系列のデータには、それぞれ優先クラス [!DNL Schema Registry]と **[!DNL XDM Individual Profile]****[!DNL XDM ExperienceEvent]** 時系列のデータを使用することをお勧めします。 これらのクラスの詳細については、以下で説明します。

### [!DNL XDM Individual Profile]

[!DNL XDM Individual Profile] は、特定された対象と部分的に識別された対象の両方の属性の単数形式の表現を形成する、レコードベースのクラスです。 高度に識別されたプロファイルは、個人との通信やターゲットとなる関与に使用され、名前、性別、生年月日、場所、電話番号や電子メールアドレスなどの連絡先情報など、詳細な個人情報を含めることができます。

識別されないプロファイルは、ブラウザーのcookieのような匿名の行動シグナルでのみ構成される場合があります。 この場合、疎プロファイルデータを使用して、匿名プロファイルの興味や好みを照合して保存する情報ベースを構築する。 これらの識別子は、サブジェクトが通知、購読、購入などにサインアップすると、時間の経過とともにより詳細になる場合があります。 プロファイル属性の増加により、最終的には対象が特定され、ターゲットを絞った関与の度合いが高くなる可能性があります。

消費者プロファイルは増え続け、個人の個人情報、識別情報、連絡先の詳細、コミュニケーションの好みの強力なリポジトリとなります。

### [!DNL XDM ExperienceEvent]

XDM ExperienceEventは、イベント(または一連のイベント)が発生したときのシステムの状態（ポイントインタイムや関与する件名のIDなど）を取り込むために使用される時系列ベースのクラスです。 エクスペリエンスイベントは、発生した事実の記録なので、統合も解釈もせずに起こったことを表し、不変です。 特定の時間枠内に発生した変更を分析し、複数の時間枠を比較してトレンドを追跡するために使用できるので、時間ドメイン分析にとって重要です。

エクスペリエンスイベントは、明示的にも暗黙的にも指定できます。 明示的なイベントは、旅のある時点で起きる人間の行動を直接観察できます。 暗黙的なイベントとは、人間が直接行動を起こさずに育てられたが、個人に関係があるイベントです。 暗黙的なイベントの例としては、電子メールニュースレターのスケジュール送信や、特定のしきい値に達したバッテリの電圧などがあります。

すべてのイベントがすべてのデータソースにわたって簡単に分類できるわけではありませんが、類似したイベントを類似したタイプに調和させて処理できるようにすると非常に役立ちます。

![ExperienceEventユーザーの遍歴](images/overview/experience-event-journey.png)

## XDMスキーマと [!DNL Experience Platform] サービス

[!DNL Experience Platform] はスキーマにとらわれない方式です。つまり、XDM標準に準拠するスキーマは、 [!DNL Platform] サービスで使用できます。 様々な [!DNL Platform] サービスでスキーマを使用する方法を、以下で詳しく説明します。

### [!DNL Catalog Service], [!DNL Data Ingestion] &amp; [!DNL Data Lake]

[!DNL Catalog Service] は、 [!DNL Experience Platform] アセットとその関連スキーマの記録システムです。 [!DNL Catalog] は、実際のデータを含むファイルやディレクトリではなく、これらのファイルやディレクトリのメタデータや説明を保持します。

[!DNL Catalog] データは、接触チャネル [!DNL Data Lake]やファイル形式に関係なく、によって管理されるすべてのデータを含む、非常に詳細なデータストア [!DNL Platform]に保存されます。

データのへの取り込みを開始す [!DNL Experience Platform]るには、を使用してデータセットを作成し [!DNL Catalog Service]ます。 データセットは、取り込むデータの構造を記述したXDMスキーマを参照します。 スキーマなしでデータセットを作成した場合、は、取り込んだデータフィールドの種類と内容を調べることで、「観測されたスキーマ」を取得します。 [!DNL Experience Platform] データセットは、スキーマ [!DNL Catalog][!DNL Data Lake] と共に、で追跡および保存され、その基となる監視スキーマーに保存されます。

詳しくは、 [!DNL Catalog]Catalog Serviceの概要を参照してください [](../catalog/home.md)。 Adobe Experience Platformデータ取り込みについて詳しくは、 [データ取り込みの概要を参照してください](../ingestion/home.md)。

### [!DNL Query Service]

Adobe Experience Platform [!DNL Query Service] を使用すると、標準のSQLからクエリへの [!DNL Experience Platform] データを使用して、様々な使用例をサポートできます。

スキーマを構成し、そのスキーマを参照するデータセットを作成した後、データをに取り込んで保存し [!DNL Data Lake]ます。 を使用 [!DNL Query Service]すると、で任意のデータセットを結合し [!DNL Data Lake] て、レポート、機械学習、またはに取り込むための新しいデータセットとしてクエリ結果を取り込むことができ [!DNL Real-time Customer Profile]ます。

詳しくは、 [!DNL Query Service]クエリサービスの概要を参照して [ください](../query-service/home.md)。

### [!DNL Real-time Customer Profile]

リアルタイムの顧客プロファイルは、ターゲットを絞り込み、パーソナライズしたエクスペリエンス管理のための一元的な顧客プロファイルを提供します。 各プロファイルには、すべてのシステムにわたって集計されたデータと、お客様が使用するシステムのいずれかで発生した個人に関する、実用的なイベントのタイムスタンプ付きのアカウントが含まれ [!DNL Experience Platform]ます。

[!DNL Real-time Customer Profile] は、 [!DNL XDM Individual Profile] またはクラスに基づいてスキーマ形式のデータを使用し [!DNL XDM ExperienceEvent] 、そのデータに基づいてクエリに応答します。 [!DNL Profile] は、他のクラスに基づくスキーマの使用をサポートしていません。

[!DNL Profile] 各顧客プロファイルのインスタンスを維持し、データを結合して個人の「単一の真実の源泉」を形成します。 この統合データは、「和集合表示」と呼ばれるデータを使用して表されます。 和集合表示は、同じクラスを実装するすべてのスキーマのフィールドを1つのスキーマに集計します。  UIまたはAPIを使用してスキーマを構成する場合、スキーマのでの使用を有効にし、和集合表示に含めるためにタグを付けるこ [!DNL Real-time Customer Profile] とができます。 次に、タグ付けされたスキーマが、フィード先のスキーマ定義に参加 [!DNL Profile]します。

と [!DNL XDM Individual Profile] データが取り込まれ、管理されると [!DNL XDM ExperienceEvent] きに、データの使用 [!DNL Catalog][!DNL Real-time Customer Profile] が可能なデータの取り込みを開始します。 取り込まれるインタラクションや詳細情報が多いほど、個々のプロファイルがより強力になります。

[!DNL XDM Individual Profile] データは、チャネルやアドビのソリューション統合全体にわたってアクションを伝え、強化するのに役立ちます。行動やインタラクションデータの豊富な履歴と組み合わせると、このデータを電源機械学習に使用します。 また、この [!DNL Real-time Customer Profile] APIは、サードパーティのソリューション、CRM、独自仕様のソリューションの機能を強化するためにも使用できます。

詳しくは、 [リアルタイム顧客プロファイルの概要](../profile/home.md) （英語）を参照してください。

### [!DNL Data Science Workspace]

Adobe Experience Platform [!DNL Data Science Workspace] は、機械学習と人工知能を使用して、に保存されたデータからの洞察を得 [!DNL Experience Platform]ます。 [!DNL Data Science Workspace] データ科学者は、XDM Individualに基づくレシピ [!DNL Profile] と、顧客とそのアクティビティに関する [!DNL XDM ExperienceEvent] データを作成でき、購入傾向や推奨オファーなど、個人が認識し、使用する可能性が高い予測を容易に行うことができます。

データ科学者 [!DNL Data Science Workspace]は、機械学習を利用したインテリジェントなサービスAPIを簡単に作成できます。 これらのサービスは、Adobe TargetやAdobeAnalyticsクラウドなど、他のアドビソリューションと連携して、パーソナライズされたターゲットを絞ったデジタルエクスペリエンスを自動化します。

データを使用してインサイトを強化する方法について詳しくは、 [!DNL Experience Platform] Data Science Workspaceの概要 [](../data-science-workspace/home.md)を参照してください。

### [!DNL Decisioning Service]

[!DNL Decisioning Service] は、 [!DNL Platform]統合されたアプリケーションでパーソナライズされたオファー判定を設定する機能を提供します。 オファーには、商品のレコメンデーション、Webエクスペリエンスのコンテンツコンポーネント、会話スクリプト、実行するアクションがあります。

[!DNL Decisioning Service] データ [!DNL Real-time Customer Profile] を活用するため、またはクラスを実装するスキーマに基づくデータセットとのみ互換性があり [!DNL XDM Individual Profile] ま [!DNL XDM ExperienceEvent] す。

See the [Decisioning Service overview](../decisioning-service/home.md) for more information.

## 次の手順とその他のリソース

スキーマの役割を全体的によりよく理解でき [!DNL Experience Platform]たので、開始が自分で作成する準備が整いました。 学習内容の補足を続けるには、推奨ドキュメントを読み、次のビデオをご覧ください。

使用するスキーマを構成する際のデザインの原則とベストプラクティスを学ぶに [!DNL Experience Platform]は、まずスキーマ構成の [基本を読みます](schema/composition.md)。 スキーマの作成手順については、API [を使用したスキーマの作成またはユーザインターフェイスの](tutorials/create-schema-api.md) 使用に関するチュートリアルを参照してください [](tutorials/create-schema-ui.md)。

での理解を深めるには、次のビデオ [!DNL XDM System] をご覧く [!DNL Experience Platform]ださい。

>[!VIDEO](https://video.tv.adobe.com/v/27105?quality=12&learn=on)

