---
title: Oracle Eloqua （V2）をExperience Platformの UI に接続
description: Oracle Eloqua アカウントをExperience Platformに UI で接続する方法を説明します。
source-git-commit: 180754969d4ae8dbd1308dfc85dae73baf64f759
workflow-type: tm+mt
source-wordcount: '1187'
ht-degree: 7%

---

# [!DNL Oracle Eloqua] （V2）を UI でExperience Platformに接続

[!DNL Oracle Eloqua (V2)] ソースコネクタを使用すると、Oracle Eloqua アカウントをAdobe Experience Platformに接続し、連絡先、アカウント、キャンペーン、エンゲージメントアクティビティなどの主要な B2B マーケティングデータを自動的かつスケーラブルに取り込むことができます。

このソースコネクタを使用して、安全な認証を設定し、必要な正確な Eloqua データエンティティを選択し、標準化された Experience Data Model （XDM）スキーマにマッピングします。 柔軟なスケジュールオプションを使用すると、初期データの読み込みと繰り返しの増分同期の両方を設定でき、マーケティングデータを最新かつ実用的な状態に保つことができます。

Adobeのエンタープライズ取り込みフレームワークに基づいて構築された [!DNL Oracle Eloqua (V2)] ソースコネクタは、キャンペーンの最適化、パフォーマンスの測定、クロスチャネルパーソナライゼーションのための信頼性と拡張性の高い基盤を提供します。

このガイドでは、Experience Platform ユーザーインターフェイスのソースワークスペースを使用して [!DNL Oracle Eloqua] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な資格情報の収集 {#credentials}

認証について詳しくは、[[!DNL Eloqua]  概要 &#x200B;](../../../../connectors/marketing-automation/eloqua.md) を参照してください。

## ソースカタログのナビゲート {#catalog}

Experience Platformの UI で、左側のナビゲーションから「**[!UICONTROL Sources]**」を選択して、*[!UICONTROL Sources]* ワークスペースにアクセスします。 カテゴリを選択するか、検索バーを使用してソースを検索します。

[!DNL Eloqua] に接続するには、*[!UICONTROL Marketing Automation]* カテゴリに移動し、**[!UICONTROL (V2) Oracle Eloqua]** ソース カードを選択してから、[**[!UICONTROL Set up]**] を選択します。

>[!TIP]
>
>特定のソースがまだ認証済みのアカウントを持っていない場合、ソースカタログ内のソースに「**[!UICONTROL Set up]**」オプションが表示されます。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL Add data]** に変わります。

![&#x200B; ソースカタログ内の Eloqua ソースカードの「設定」ボタンがハイライト表示されています。](../../../../images/tutorials/create/eloqua/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL Existing account]**」を選択してから、使用する [!DNL Eloqua] アカウントを選択します。

![&#x200B; アカウント作成インターフェイスで選択された「既存のアカウント」オプション &#x200B;](../../../../images/tutorials/create/eloqua/existing.png)

## 新しいアカウントを作成 {#new}

新しいアカウントを作成するには、「**[!UICONTROL New account]**」を選択し、[!UICONTROL Source connection details] の下に名前と説明を入力します。 次に、[!UICONTROL Account authentication] で、**クライアント ID**、**クライアントシークレット**、**ユーザー名**、**パスワード**、**ベースエンドポイント** の値を指定します。 これらの資格情報について詳しくは、[&#x200B; 認証ガイド &#x200B;](../../../../connectors/marketing-automation/eloqua.md) を参照してください。 終了したら「**[!UICONTROL Connect to source]**」を選択し、接続が確立されるまで数秒間待ちます。

![&#x200B; ソース接続の詳細と認証資格情報のフィールドを含む新しいアカウントインターフェイス。](../../../../images/tutorials/create/eloqua/new.png)

## データの選択

データを選択インターフェイスを使用して、Experience Platformに取り込む [!DNL Eloqua] エンティティを選択します。

>[!TIP]
>
>データを選択すると、キャンペーンを除き、他のエンティティに代表的なサンプルデータが表示されます。 このアプローチでは、パブリック API が現在キャンペーン専用の実際のデータを取得す [!DNL Eloqua] ので、使用可能なフィールドと構造をプレビューできるようにします。 残りのエンティティについては、設定ワークフローをサポートするためのサンプルデータが提供されています。

![&#x200B; 使用可能な Eloqua データエンティティを表示するデータ選択インターフェイス &#x200B;](../../../../images/tutorials/create/eloqua/select-data.png)

## データセットとデータフローの詳細 {#details}

次に、データセットとデータフローに関する情報を指定する必要があります。 この手順では、既存のデータセットを使用するか、新しいデータセットを作成します。 さらに、オプションで、この手順でデータセットを有効にしてリアルタイム顧客プロファイルに取り込むこともできます。

![&#x200B; データセットプロパティを設定するためのオプションを備えたデータセットとデータフローの詳細インターフェイス &#x200B;](../../../../images/tutorials/create/eloqua/details.png)

## マッピング {#mapping}

[!DNL Eloqua] のマッピングは、次の 4 つの主なエンティティタイプに整理されています。

* **アカウント** - [!DNL Eloqua] からの会社/組織レコード。
* **アクティビティ** - [!DNL Eloqua] からのマーケティングアクティビティとエンゲージメントイベント。
* **キャンペーン** - [!DNL Eloqua] ークフローからのマーケティングキャンペーンレコード。
* **連絡先** - ア [!DNL Eloqua] ット内の個々の個人レコード。

デフォルトで提供される以外の追加フィールドにアクセスする必要がある場合は、Experience Platformのデータ準備マッピングプロセスを使用して、これらのフィールドを追加できます。 デフォルト（標準）スキーマが必須フィールドの一部をサポートしていない場合は、Experience Platformでカスタムスキーマを定義するオプションがあります。 この機能を使用して必要なフィールドを作成およびマッピングすると、関連するすべてのデータを [!DNL Eloqua] からExperience Platformにシームレスに取り込むことができます。

次の手順の概要：

* 統合で使用可能なデフォルトのマッピングされたフィールドを確認します。
* マッピング手順に、[!DNL Eloqua] から必要な追加フィールドを含めます。
* 標準スキーマに新しいフィールドが存在しない場合は、これらのフィールドを含むExperience Platformでカスタムスキーマを拡張または作成します。
* マッピングを完了して、必要なデータがすべて取り込まれるようにします。

外部 CRM 情報が正確に反映されるように、データ準備の計算フィールド関数を使用し、ソースデータフィールド内の特定の CRM インスタンス ID で `{CRM_INSTANCE_ID}` プレースホルダーを更新するだけです。 これにより、組織固有の設定に合わせて統合を柔軟にカスタマイズできます。

計算フィールドエディターを使用するには、更新するソースフィールドを選択します。

![&#x200B; 計算フィールドを含む Eloqua ソースフィールド。](../../../../images/tutorials/create/eloqua/select-calculated-field.png)

>[!BEGINTABS]

>[!TAB Salesforce]

[!DNL Salesforce] ユーザーの場合は、計算フィールドエディターを使用し、適切なインスタンス ID で `{CRM_INSTANCE_ID}` を更新します。

![Salesforceの計算フィールド &#x200B;](../../../../images/tutorials/create/eloqua/sf-field.png)

>[!TAB Microsoft Dynamics]

[!DNL Microsoft] ユーザーの場合は、計算フィールドエディターを使用し、適切なインスタンス ID で `{CRM_INSTANCE_ID}` を更新します。

![Dynamics の計算フィールド。](../../../../images/tutorials/create/eloqua/dynamics-field.png)

>[!ENDTABS]

計算フィールドの更新が完了したら、「**[!UICONTROL Next]**」を選択して続行します。

![Eloqua データエンティティのフィールドマッピングを表示するマッピングインターフェイス &#x200B;](../../../../images/tutorials/create/eloqua/mapping.png)

## スケジュール設定

>[!NOTE]
>
>増分データの読み込みに内部的に使用される差分フィールドを次に示します。
>
>* **連絡先：** `C_DateModified`
>* **アカウント：** `M_DateModified`
>* **アクティビティ：** `CreatedAt`
>* **カスタム オブジェクト：** `UpdatedAt`
>* **Campaign:** `updatedAt`

マッピングが完了したので、データフローの取り込みスケジュールを設定できるようになりました。 [!UICONTROL Frequency] を `Once` に設定して、1 回限りの取り込みを実行します。 増分取り込みの場合は、[!UICONTROL Frequency] を `Hour`、`Day` または `Week` に設定できます。 増分取り込みを使用する場合は、取り込みの実行間隔を定義するように [!UICONTROL Interval] を設定する必要もあります。 例えば、取り込み頻度を `Day` に設定し、間隔を `15` に設定すると、データフローは 15 日ごとにデータを取り込むようにスケジュールされます。

1 分あたりの取り込み頻度は、[!DNL Eloqua] ソースでは使用できません。 最も頻繁に選択できるスケジュールは 1 時間ごとです。 データの鮮度のニーズに合ったスケジュールを選択します。 スケジュールを頻繁に選択すると、計算コストが増加することに留意してください。

![&#x200B; 取り込みの頻度と間隔を設定するオプションを備えたスケジュールインターフェイス &#x200B;](../../../../images/tutorials/create/eloqua/scheduling.png)

## レビュー

取り込みスケジュールを設定した状態で、[!UICONTROL Review] インターフェイスを使用してデータフローの詳細を確認します。 「**[!UICONTROL Finish]**」を選択して設定を完了し、データフローが開始されるまでしばらく待ちます。

![&#x200B; 完了前にデータフロー設定の概要を表示するレビューインターフェイス &#x200B;](../../../../images/tutorials/create/eloqua/review.png)

## 監視

データフローを選択すると、データが 1 回だけバックフィルされ、その後は指定したスケジュールで増分同期が行われます。 同期のステータスは、データフローに移動して監視できます。 詳しくは、[UI でのソースデータフローの監視 &#x200B;](../../../../../dataflows/ui/monitor-sources.md) に関するガイドを参照してください。

## 次の手順

これで、Experience Platformでの [!DNL Eloqua] ソースのセットアップと設定が完了しました。 データフローが確立されると、選択したスケジュールに従って [!DNL Eloqua] データが取り込まれ、標準の Experience Data Model （XDM）スキーマにマッピングされます。 データフローの監視を続け、取り込んだデータを Platform 内で調査して、インサイトを促進し、マーケティングのユースケースをアクティブ化します。 より高度な設定とトラブルシューティングについては、関連するドキュメントを参照するか、Adobe サポートリソースにお問い合わせください。

詳しくは、次のドキュメントを参照してください。

* [ソースの概要](../../../../home.md)
* [Real-Time CDP B2B エディション](../../../../../rtcdp/b2b-overview.md)
