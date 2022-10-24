---
keywords: Experience Platform;ホーム;人気のトピック;
description: Adobe Experience Platformには、データ取り込みプロセスを高速化するために使用できる、事前設定済みのテンプレートが用意されています。 テンプレートには、ソースからExperience Platformにデータを取り込む際に使用できる、スキーマ、データセット、マッピングルール、ID 名前空間、データフローなどの自動生成されたアセットが含まれます。
title: （アルファ）UI のテンプレートを使用してソースのデータフローを作成する
hide: true
hidefromtoc: true
source-git-commit: a0ca9cff43b6f8276268467fecf944c664992950
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 14%

---

# （アルファ）UI のテンプレートを使用してソースのデータフローを作成する

>[!IMPORTANT]
>
>テンプレートはアルファ版で、現在、 [[!DNL Marketo Engage] ソース](../../connectors/adobe-applications/marketo/marketo.md). ドキュメントと機能は変更される場合があります。

Adobe Experience Platformには、データ取り込みプロセスを高速化するために使用できる、事前設定済みのテンプレートが用意されています。 テンプレートには、ソースからExperience Platformにデータを取り込む際に使用できる、スキーマ、データセット、マッピングルール、ID 名前空間、データフローなどの自動生成されたアセットが含まれます。

テンプレートを使用すると、次のことが可能になります。

* ML ベースのアセット作成を高速化することで、取り込みの価値を低下させます。
* 手動データ取り込みプロセス中に発生する可能性のあるエラーを最小限に抑えます。
* ユースケースに合わせて、任意の時点で自動生成されたアセットを更新します。

次のチュートリアルでは、 [[!DNL Marketo Engage] ソース](../../connectors/adobe-applications/marketo/marketo.md).

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## Platform UI でのテンプレートの使用 {#use-templates-in-the-platform-ui}

>[!CONTEXTUALHELP]
>id="platform_sources_templates_accounttype"
>title="ビジネスタイプを選択"
>abstract="使用事例に適したビジネスタイプを選択します。 アクセス権は、Real-time Customer Data Platformサブスクリプションアカウントによって異なる場合があります。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=ja" text="Real-Time CDPの概要"

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] 画面には、アカウントの作成に使用できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

以下 [!UICONTROL Adobe] カテゴリ、選択 **[!UICONTROL Marketo Engage]** 次に、 **[!UICONTROL データを追加]**.

![ソースワークスペースのカタログで、Marketo Engageソースが強調表示されています。](../../images/tutorials/templates/catalog.png)

ポップアップウィンドウが表示され、テンプレートを参照するか、既存のスキーマおよびデータセットを使用するかを選択できます。 自動生成されたアセットを使用するには、 **[!UICONTROL テンプレートを参照]** 次に、 **[!UICONTROL 選択]**.

![テンプレートを参照するか、既存のアセットを使用するかを選択できるポップアップウィンドウです。](../../images/tutorials/templates/browse-templates.png)

### 認証

認証手順が表示され、新しいアカウントを作成するか、既存のアカウントを使用するかを尋ねられます。

#### 既存のアカウント

既存のアカウントを使用するには、「 」を選択します。 [!UICONTROL 既存のアカウント] 次に、表示されるリストから、使用するアカウントを選択します。

![アクセス可能な既存のアカウントのリストを含む既存のアカウントの選択ページ。](../../images/tutorials/templates/existing-account.png)

#### 新しいアカウント

新しいアカウントを作成するには、 **[!UICONTROL 新しいアカウント]**&#x200B;をクリックし、ソース接続の詳細とアカウント認証資格情報を入力します。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** そして、新しい接続が確立されるまでしばらく待ちます。

![ソース接続の詳細とアカウント認証資格情報を含む新しいアカウントの認証ページ。](../../images/tutorials/templates/new-account.png)

### テンプレートを選択

アカウントを認証して選択すると、テンプレートのリストが表示されます。 テンプレート名の横にあるプレビューアイコンを選択して、テンプレートのサンプルデータをプレビューします。

![プレビューアイコンがハイライトされたテンプレートのリストです。](../../images/tutorials/templates/templates.png)

プレビューウィンドウが表示され、テンプレートからサンプルデータを調べて検査できます。 終了したら、「 」を選択します。 **[!UICONTROL 取得]**.

![サンプルデータのプレビューウィンドウ。](../../images/tutorials/templates/preview-sample-data.png)

次に、使用するテンプレートをリストから選択します。 複数のテンプレートを選択し、一度に複数のデータフローを作成できます。 ただし、テンプレートはアカウントごとに 1 回だけ使用できます。 テンプレートを選択したら、「 **[!UICONTROL 完了]** アセットが生成されるまでしばらく待ちます。

![商談取引先責任者の役割テンプレートが選択されているテンプレートのリスト。](../../images/tutorials/templates/select-template.png)

### アセットのレビュー {#review-assets}

>[!CONTEXTUALHELP]
>id="platform_sources_templates_review"
>title="自動生成されたアセットのレビュー"
>abstract="すべてのアセットの生成には最大 5 分かかる場合があります。 ページを終了する場合は、アセットが完了した後に返される通知が届きます。 アセットは、生成後に確認し、いつでも追加の設定をおこなうことができます。"

この [!UICONTROL テンプレートアセットのレビュー] ページには、テンプレートの一部として自動生成されたアセットが表示されます。 このページでは、ソース接続に関連付けられた自動生成スキーマ、データセット、ID 名前空間、データフローを表示できます。

自動生成されたデータフローは、デフォルトで有効になっています。 省略記号 (`...`) をクリックし、「 」を選択します。 **[!UICONTROL マッピングをプレビュー]** を参照して、データフロー用に作成されたマッピングセットを確認します。

![「 Preview Mappings 」オプションが選択されたドロップダウンウィンドウ。](../../images/tutorials/templates/preview.png)

プレビューページが表示され、ソースデータフィールドとターゲットスキーマフィールドの間のマッピング関係を調べることができます。 データフローのマッピングを表示したら、 選択 **[!UICONTROL 分かった。]**

![マッピングのプレビューウィンドウ。](../../images/tutorials/templates/preview-mappings.png)

データフローは、実行後いつでも更新できます。 省略記号 (`...`) をクリックし、「 」を選択します。 **[!UICONTROL データフローを更新]**. ソースワークフローページに移動します。このページでは、部分取得、エラー診断、アラート通知の設定、データフローのマッピングなど、データフローの詳細を更新できます。

![「データフローを更新」オプションが選択されたドロップダウンウィンドウ。](../../images/tutorials/templates/update.png)

## 次の手順

このチュートリアルに従って、データフローと、テンプレートを使用したスキーマ、データセット、ID 名前空間などのアセットを作成しました。 ソースに関する一般的な情報については、 [ソースの概要](../../home.md).