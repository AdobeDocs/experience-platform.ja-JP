---
title: Google Cloud Platform イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能は、Edge NetworkイベントをGoogle Cloud Platform に送信します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: c5da1889-f917-42aa-b3a4-9557c31d6ee8
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '562'
ht-degree: 3%

---

# [!DNL Google Cloud Platform] イベント転送拡張機能

[[!DNL Google Cloud Platform]](https://cloud.google.com/) は、分散コンピューティング、データベースストレージ、コンテンツ配信、顧客関係管理（CRM）やエンタープライズリソースプランニング（ERP）向けの SaaS （Software-as-a-Service）統合サービスなど、幅広いサービスを提供するクラウドコンピューティングプラットフォームです。

[!DNL Google Cloud Platform] [&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) 拡張機能は、[[!DNL Cloud Pub/Sub]](https://cloud.google.com/pubsub) を活用して、イベントをAdobe Experience Platform Edge Networkから [!DNL Google Cloud Platform] に送信して、さらに処理できるようにします。 このガイドでは、拡張機能をインストールし、イベント転送ルールでその機能を使用する方法について説明します。

## 前提条件

この拡張機能を使用するには、既存の [!DNL Cloud Pub/Sub] トピックを持つ [!DNL Google Cloud Platform] アカウントが必要です。 既存のトピックがない場合は、トピックの作成と管理に関する [[!DNL Google Cloud Platform]](https://cloud.google.com/pubsub/docs/create-topic) ドキュメントを参照してください。

### 秘密鍵およびデータ要素の作成

まず、新しい `Google OAuth 2`[&#x200B; イベント転送の秘密鍵 &#x200B;](../../../ui/event-forwarding/secrets.md) を作成します。これは、値を安全に保ちながら、アカウントへの接続を認証するために使用されます。

次に、[Core ]&#x200B;**拡張機能と**&#x200B;[!UICONTROL &#x200B; Secret &#x200B;]&#x200B;**データ要素タイプを使用して (../../../ui/managing-resources/data-elements.md#create-a-data-element) データ要素を作成** し、作成した `Google OAuth 2` シークレットを参照します。

## [!DNL Google Cloud Platform] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティを作成 &#x200B;](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで、[!DNL Google Cloud Platform] 拡張機能のカードの **[!UICONTROL インストール]** を選択します。

![&#x200B; インストールをハイライト表示したカタログ [!DNL Google Cloud Platform] ース拡張機能。](../../../images/extensions/server/google-cloud-platform/install-extension.png)

設定画面で、前に作成したデータ要素の秘密鍵を「**[!UICONTROL アクセストークン]**」フィールドに入力します。 データ要素の秘密鍵には、[!DNL Google Cloud Platform] の OAuth 2 トークンが含まれます。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![[!DNL Google Cloud Platform] 拡張機能の設定ページ &#x200B;](../../../images/extensions/server/google-cloud-platform/configure-extension.png)

## [!DNL Send Data to Cloud Pub/Sub] ルールの作成 {#tracking-rule}

拡張機能がインストールされたら、新しいイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成し、必要に応じてその条件を設定します。 ルールのアクションを設定する際に、**[!UICONTROL Google Cloud Platform]** 拡張機能を選択してから、アクションタイプとして「**[!UICONTROL Send Data to Cloud Pub/Sub]**」を選択します。

![Cloud Pub/Sub にデータを送信 ] アクションがハイライト表示された [!UICONTROL 1&rbrace;Google Cloud Platform] のアクション設定ビュー (../../../images/extensions/server/google-cloud-platform/event-action.png)

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; トピック &#x200B;] | イベント転送からイベントを受信するトピック。 値の形式は `projects/{projectName}/topics/{topicName}` である必要があります。 |
| [!UICONTROL データ] | このフィールドには、[!DNL Cloud Pub/Sub] トピックに JSON 形式で転送されるデータが含まれています。<br><br> 「**[!UICONTROL Raw]**」オプションで、JSON オブジェクトを指定されたテキストフィールドに直接貼り付けるか、データ要素アイコン（![&#x200B; データセットアイコン &#x200B;](/help/images/icons/database.png)）を選択して、データを表す既存のデータ要素のリストから選択できます。<br><br> また、「**[!UICONTROL JSON キーと値のペア エディター]**」オプションを使用し、UI エディターを使用して各キーと値のペアを手動で追加することもできます。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |
| [!UICONTROL &#x200B; 属性 &#x200B;] | このフィールドには、メッセージと共に送信される追加の属性を持つ JSON オブジェクトが含まれています。<br><br> 「**[!UICONTROL Raw]**」オプションで、JSON オブジェクトを指定されたテキストフィールドに直接貼り付けるか、データ要素アイコン（![&#x200B; データセットアイコン &#x200B;](/help/images/icons/database.png)）を選択して、データを表す既存のデータ要素のリストから選択できます。<br><br> また、「**[!UICONTROL JSON キーと値のペア エディター]**」オプションを使用し、UI エディターを使用して各キーと値のペアを手動で追加することもできます。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |

{style="table-layout:auto"}

## 次の手順

このガイドでは、[!DNL Google Cloud Platform] イベント転送拡張機能を使用して [!DNL Cloud Pub/Sub] にデータを送信する方法について説明します。 Experience Platformのイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。
