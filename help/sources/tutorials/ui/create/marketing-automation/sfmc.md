---
title: UI を使用したSalesforce Marketing Cloud（V2）アカウントのExperience Platformへの接続
description: UI を通じてSalesforce Marketing Cloud（V2）アカウントをExperience Platformに接続する方法を説明します。
source-git-commit: 91bf8bf6e6f7030574d693484ce9797800c2d5dd
workflow-type: tm+mt
source-wordcount: '834'
ht-degree: 10%

---

# [!DNL Salesforce Marketing Cloud] をExperience Platformに接続

このガイドでは、Experience Platform ユーザーインターフェイスのソースワークスペースを使用して [!DNL Salesforce Marketing Cloud] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Salesforce Marketing Cloud]  概要 &#x200B;](../../../../connectors/marketing-automation/sfmc.md#prerequisites) を参照してください。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから「**[!UICONTROL Sources]**」を選択して、*[!UICONTROL Sources]* ワークスペースにアクセスします。 カテゴリを選択するか、検索バーを使用してソースを検索します。

[!DNL Salesforce Marketing Cloud] に接続するには、*[!UICONTROL Marketing Automation]* カテゴリに移動し、**[!UICONTROL (V2) Salesforce Marketing Cloud]** ソース カードを選択してから、[**[!UICONTROL Set up]**] を選択します。

>[!TIP]
>
>特定のソースがまだ認証済みのアカウントを持っていない場合、ソースカタログ内のソースに「**[!UICONTROL Set up]**」オプションが表示されます。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL Add data]** に変わります。

![&#x200B; ソースカタログとSalesforce Marketing Cloud ソースが選択されています。](../../../../images/tutorials/create/sfmc/catalog.png)

## 既存のアカウントを使用 {#existing}

既存のアカウントを使用するには、「**[!UICONTROL Existing account]**」を選択してから、使用する [!DNL Salesforce Marketing Cloud] アカウントを選択します。

![&#x200B; ソースワークフローの既存のアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/sfmc/existing.png)

## 新しいアカウントを作成 {#new}

新しいアカウントを作成するには、「**[!UICONTROL New account]**」を選択し、[!UICONTROL Source connection details] の下に名前と説明を入力します。 次に、[!UICONTROL Account authentication] で、**クライアント ID**、**クライアントシークレット**、**ベースエンドポイント** の値を指定します。 これらの資格情報について詳しくは、[&#x200B; 認証ガイド &#x200B;](../../../../connectors/marketing-automation/sfmc.md#gather-required-credentials) を参照してください。 終了したら「**[!UICONTROL Connect to source]**」を選択し、接続が確立されるまで数秒間待ちます。

![&#x200B; ソースワークフローの新しいアカウントインターフェイス &#x200B;](../../../../images/tutorials/create/sfmc/new.png)

## データの選択

[!DNL Salesforce Marketing Cloud] ソースでは、[!DNL Salesforce Marketing Cloud] のデータ拡張機能からのデータ取り込みのみをサポートしています。

[!UICONTROL Select data] インターフェイスを使用して、[!DNL Salesforce Marketing Cloud] インスタンスから取り込むデータ拡張機能を選択します。 データ拡張機能を選択したら、プレビューパネルを使用して、続行する前に、データセットに想定されたフィールドが含まれていることを確認できます。

![&#x200B; ソースワークフローのデータを選択ステップ &#x200B;](../../../../images/tutorials/create/sfmc/select-data.png)

## データセットとデータフローの詳細

次に、データセットとデータフローに関する情報を指定する必要があります。 この手順では、既存のデータセットを使用するか、新しいデータセットを作成します。 さらに、オプションで、この手順でデータセットを有効にしてリアルタイム顧客プロファイルに取り込むこともできます。

![&#x200B; ソースワークフローのデータセットとデータフローの詳細手順。](../../../../images/tutorials/create/sfmc/dataset-details.png)

## マッピング

ま [!DNL Salesforce Marketing Cloud]、データ拡張機能は標準オブジェクトとは見なされません。 したがって、Experience Platform スキーマへの定義済みまたは固定のマッピングフィールドはありません。 Experience Platformのデータ準備は、[!DNL Salesforce Marketing Cloud] のソースフィールドとターゲットのエクスペリエンスデータモデル（XDM）スキーマの間でベストエフォートの整合性を実行しますが、マッピングされていないフィールドやエラーのあるフィールドを解決するために、手動の確認や調整が必要な場合もあります。

![&#x200B; ソースワークフローのマッピングステップ &#x200B;](../../../../images/tutorials/create/sfmc/mapping.png)

## データフローのスケジュール設定

マッピングが完了したので、データフローの取り込みスケジュールを設定できるようになりました。 [!UICONTROL Frequency] を `Once` に設定して、1 回限りの取り込みを実行します。 増分取り込みの場合は、[!UICONTROL Frequency] を `Hour`、`Day` または `Week` に設定できます。 増分取り込みを使用する場合は、取り込みの実行間隔を定義するように [!UICONTROL Interval] を設定する必要もあります。 例えば、取り込み頻度を `Day` に設定し、間隔を `15` に設定すると、データフローは 15 日ごとにデータを取り込むようにスケジュールされます。

>[!TIP]
>
> 1 分あたりの取り込み頻度は、[!DNL Salesforce Marketing Cloud] ソースでは使用できません。 最も頻繁に選択できるスケジュールは 1 時間ごとです。 データの鮮度のニーズに合ったスケジュールを選択します。 スケジュールを頻繁に選択すると、計算コストが増加することに留意してください。

増分同期を有効にするには、データセットで差分（日時）フィールドを選択する必要があります。 データセットに適切なデルタフィールドが含まれていない場合、データフローを作成することはできません。

![&#x200B; ソースワークフローのスケジュール手順 &#x200B;](../../../../images/tutorials/create/sfmc/schedule.png)

## レビュー

取り込みスケジュールを設定した状態で、[!UICONTROL Review] インターフェイスを使用してデータフローの詳細を確認します。 「**[!UICONTROL Finish]**」を選択して設定を完了し、データフローが開始されるまでしばらく待ちます。

![&#x200B; ソースワークフローのレビュー手順。](../../../../images/tutorials/create/sfmc/review.png)

## 監視

データフローを選択すると、データが 1 回だけバックフィルされ、その後は指定したスケジュールで増分同期が行われます。 同期のステータスは、データフローに移動して監視できます。 詳しくは、[UI でのソースデータフローの監視 &#x200B;](../../../../../dataflows/ui/monitor-sources.md) に関するガイドを参照してください。

## 次の手順

このチュートリアルでは、ユーザーインターフェイスを使用して [!DNL Salesforce Marketing Cloud] （V2）アカウントをExperience Platformに接続する手順を説明しました。 ソースアカウントを選択または作成する方法、必要な資格情報を指定する方法、取り込むデータ拡張機能を選択する方法、データセットとデータフローの詳細を指定する方法、データをマッピングする方法、データ取り込みのスケジュールを設定する方法、データフローを監視する方法を学びました。 これらの手順に従うと、アクティベーションおよび分析用に [!DNL Salesforce Marketing Cloud] データをExperience Platformと正常に統合できます。

詳しくは、次のドキュメントを参照してください。

* [ソースの概要](../../../../home.md)
* [Real-Time CDP B2B エディション](../../../../../rtcdp/b2b-overview.md)

