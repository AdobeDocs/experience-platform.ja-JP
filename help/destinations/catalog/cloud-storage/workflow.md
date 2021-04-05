---
keywords: クラウドのストレージ先；クラウドのストレージ
title: クラウドストレージの保存先の作成
type: チュートリアル
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
exl-id: 58003c1e-2f70-4e28-8a38-3be00da7cc3c
translation-type: tm+mt
source-git-commit: 1e33a7b48e20d7afe9f10b206a6fd68433b205db
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 33%

---

# クラウドストレージの保存先の作成

## 概要 {#overview}

このページでは、Adobe Experience Platformのクラウドストレージの場所に接続する方法を説明します。

**[!UICONTROL ストレージ]**/**[!UICONTROL 宛先]**&#x200B;で、希望するクラウド接続先を選択し、**[!UICONTROL 設定]**&#x200B;を選択します。

![クラウドストレージの宛先に接続](../../assets/catalog/cloud-storage/workflow/connect.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

## アカウントステップ{#account}

**[!UICONTROL アカウント]**&#x200B;の手順で、クラウドストレージの接続先への接続を事前に設定している場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。 または、「**[!UICONTROL 新しいアカウント]**」を選択して、クラウドストレージの宛先への新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。必要に応じて、RSA形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。

**認証**&#x200B;手順の資格情報の入力に関する詳細は、[AmazonS3](./amazon-s3.md)宛先、[[!DNL Amazon Kinesis]](./amazon-kinesis.md)宛先、[[!DNL Azure Event Hubs]](./azure-event-hubs.md)宛先、[SFTP](./sftp.md)宛先を参照してください。

>[!NOTE]
>
>プラットフォームは、認証プロセスで資格情報の検証をサポートしており、クラウドストレージの場所に誤った資格情報を入力すると、エラーメッセージを表示します。 このため、間違った資格情報を使用すると、ワークフローを完了することができません。

![クラウドストレージの接続先に接続 — アカウント手順](../../assets/catalog/cloud-storage/workflow/destination-account.png)

## 認証手順{#authentication}

**[!UICONTROL 認証]**&#x200B;手順で、アクティベーションフローの&#x200B;**[!UICONTROL 名前]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;を入力します。

この手順では、この宛先に適用する&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;を選択することもできます。 マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。


Amazon S3 の宛先の場合は、ファイルが配信されるクラウドストレージの宛先に「**[!UICONTROL バケット名]**」と「**[!UICONTROL フォルダーパス]**」を挿入します。上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![Amazon S3 クラウドストレージの宛先への接続 - 認証手順](../../assets/catalog/cloud-storage/workflow/amazon-s3-setup.png)

SFTP の宛先の場合は、ファイルが配信される「**[!UICONTROL フォルダーパス]**」を挿入します。上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![SFTP クラウドストレージの宛先への接続 - 認証手順](../../assets/catalog/cloud-storage/workflow/sftp-setup.png)

[!DNL Amazon Kinesis]宛先には、[!DNL Amazon Kinesis]アカウント内の既存のデータストリームの名前を指定します。 プラットフォームは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![Kinesisクラウドのストレージ先に接続 — 認証手順](../../assets/catalog/cloud-storage/workflow/kinesis-setup.png)

[!DNL Azure Event Hubs]宛先には、[!DNL Amazon Event Hubs]アカウント内の既存のデータストリームの名前を指定します。 プラットフォームは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![イベントハブクラウドのストレージ先への接続 — 認証手順](../../assets/catalog/cloud-storage/workflow/event-hubs-setup.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。残りのワークフローでデータをエクスポートする場合は、[セグメントをアクティブ化](#activate-segments)の節を読みます。

## マクロを使用してストレージーの場所にフォルダーを作成{#use-macros}

ストレージーの場所にあるセグメントファイルごとにカスタムフォルダーを作成するには、フォルダーパスの入力フィールドにマクロを使用します。 以下に示すように、入力フィールドの末尾にマクロを挿入します。

![マクロを使用してストレージーにフォルダーを作成する方法](../../assets/catalog/cloud-storage/workflow/macros-folder-path.png)

以下の例では、ID `25768be6-ebd5-45cc-8913-12fb3f348615`のサンプルセグメント`Luxury Audience`を参照しています。

### マクロ1 - `%SEGMENT_NAME%`

入力：`acme/campaigns/2021/%SEGMENT_NAME%`

ストレージーの場所のフォルダーパス：`acme/campaigns/2021/Luxury Audience`

### マクロ2 - `%SEGMENT_ID%`

入力：`acme/campaigns/2021/%SEGMENT_ID%`

ストレージーの場所のフォルダーパス：`acme/campaigns/2021/25768be6-ebd5-45cc-8913-12fb3f348615`

### マクロ3 - `%SEGMENT_NAME%/%SEGMENT_ID%`

入力：`acme/campaigns/2021/%SEGMENT_NAME%/%SEGMENT_ID%`

ストレージーの場所のフォルダーパス：`acme/campaigns/2021/Luxury Audience/25768be6-ebd5-45cc-8913-12fb3f348615`



## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。
