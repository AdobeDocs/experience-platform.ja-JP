---
title: Google Cloud Platform イベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能は、Edge ネットワークイベントをGoogle Cloud Platform に送信します。
last-substantial-update: 2023-06-21T00:00:00Z
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 4%

---

# [!DNL Google Cloud Platform] イベント転送拡張機能

[[!DNL Google Cloud Platform]](https://cloud.google.com/) は、顧客関係管理 (CRM) およびエンタープライズリソース計画 (ERP) 向けの、分散コンピューティング、データベースストレージ、コンテンツ配信、SaaS(Software-as-a-S) 統合サービスなど、幅広いサービスを提供するクラウドコンピューティングプラットフォームです。

The [!DNL Google Cloud Platform] [イベント転送](../../../ui/event-forwarding/overview.md) 拡張機能の活用 [[!DNL Cloud Pub/Sub]](https://cloud.google.com/pubsub) を追加して、Adobe Experience Platform Edge Network からにイベントを送信します。 [!DNL Google Cloud Platform] を参照してください。 このガイドでは、イベント転送ルールでの拡張機能のインストール方法とその機能の使用方法について説明します。

## 前提条件

この拡張機能を使用するには、 [!DNL Google Cloud Platform] 既存のアカウント [!DNL Cloud Pub/Sub] トピック。 既存のトピックがない場合は、 [[!DNL Google Cloud Platform]](https://cloud.google.com/pubsub/docs/create-topic) トピックの作成と管理に関するドキュメント。

### シークレットとデータ要素の作成

最初に、新しい `Google OAuth 2` [イベント転送秘密鍵](../../../ui/event-forwarding/secrets.md)：値のセキュリティを維持しながら、アカウントへの接続を認証するために使用されます。

次に、 [データ要素の作成](../../../ui/managing-resources/data-elements.md#create-a-data-element) の使用 **[!UICONTROL コア]** 拡張機能と **[!UICONTROL 秘密鍵]** を参照するデータ要素タイプ `Google OAuth 2` 作成した秘密鍵。

## のインストールと設定 [!DNL Google Cloud Platform] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。Adobe Analytics の **[!UICONTROL カタログ]** タブ、選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Google Cloud Platform] 拡張子。

![カタログ [!DNL Google Cloud Platform] 拡張機能ハイライトインストール。](../../../images/extensions/server/google-cloud-platform/install-extension.png)

設定画面で、前の手順で作成したデータ要素シークレットを、 **[!UICONTROL アクセストークン]** フィールドに入力します。 データ要素の秘密鍵には、 [!DNL Google Cloud Platform] OAuth 2 トークン。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![The [!DNL Google Cloud Platform] 拡張機能の設定ページ。](../../../images/extensions/server/google-cloud-platform/configure-extension.png)

## の作成 [!DNL Send Data to Cloud Pub/Sub] ルール {#tracking-rule}

拡張機能がインストールされたら、新しいイベント転送を作成します。 [ルール](../../../ui/managing-resources/rules.md) 必要に応じて、条件を設定します。 ルールのアクションを設定する際に、 **[!UICONTROL Google Cloud Platform]** 拡張機能、「 **[!UICONTROL Cloud Pub/Sub にデータを送信]** （アクションタイプ用）。

![のアクション設定表示 [!UICONTROL Google Cloud Platform]( アクションがハイライトされ、 [!UICONTROL Cloud Pub/Sub にデータを送信].](../../../images/extensions/server/google-cloud-platform/event-action.png)

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL トピック] | イベント転送からイベントを受け取るトピック。 値は形式にする必要があります `projects/{projectName}/topics/{topicName}`. |
| [!UICONTROL データ] | このフィールドには、 [!DNL Cloud Pub/Sub] JSON 形式のトピックです。<br><br>の下 **[!UICONTROL 生データ]** 」オプションを使用する場合は、指定したテキストフィールドに JSON オブジェクトを直接貼り付けるか、データ要素アイコン (![データセットアイコン](../../../images/extensions/server/aws/data-element-icon.png)) をクリックして、データを表す既存のデータ要素のリストから選択します。<br><br>また、 **[!UICONTROL JSON キーと値のペアエディター]** UI エディターを使用して各キーと値のペアを手動で追加するオプションが追加されました。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |
| [!UICONTROL 属性] | このフィールドには、メッセージと共に送信される追加の属性を含む JSON オブジェクトが含まれています。<br><br>の下 **[!UICONTROL 生データ]** 」オプションを使用する場合は、指定したテキストフィールドに JSON オブジェクトを直接貼り付けるか、データ要素アイコン (![データセットアイコン](../../../images/extensions/server/aws/data-element-icon.png)) をクリックして、データを表す既存のデータ要素のリストから選択します。<br><br>また、 **[!UICONTROL JSON キーと値のペアエディター]** UI エディターを使用して各キーと値のペアを手動で追加するオプションが追加されました。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |

{style="table-layout:auto"}

## 次の手順

このガイドでは、にデータを送信する方法を説明します。 [!DNL Cloud Pub/Sub] の使用 [!DNL Google Cloud Platform] イベント転送拡張機能。 Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).