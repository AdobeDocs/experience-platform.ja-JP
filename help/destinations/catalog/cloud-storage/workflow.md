---
keywords: cloud storage destination;cloud storage
title: クラウドストレージの宛先ワークフロー
seo-title: クラウドストレージの宛先ワークフロー
type: Tutorial
description: クラウドストレージの場所への接続手順
seo-description: クラウドストレージの場所への接続手順
translation-type: tm+mt
source-git-commit: 24c8dd0f01d7ea14b2fa5827722e797bd209f50c
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 57%

---


# クラウドストレージの宛先を作成するためのワークフロー

## 概要

このページでは、 Real-time Customer Data Platform でクラウドストレージの場所に接続する方法について説明します。

**[!UICONTROL 接続]** / **[!UICONTROL 宛先]**&#x200B;で、目的のクラウドストレージの宛先を選択し、「 **[!UICONTROL 設定]**」を選択します。

![クラウドストレージの宛先に接続](../../assets/catalog/cloud-storage/workflow/connect.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに **[!UICONTROL 「アクティブ化]** 」ボタンが表示されます。 「 **[!UICONTROL アクティブ化]** 」と「 **[!UICONTROL 設定]**」の違いについて詳しくは、表示先ワークスペースのドキュメントの「 [カタログ](../../ui/destinations-workspace.md#catalog) 」セクションを参照してください。

クラウドストレージの宛先への接続を既に設定している場合は、**[!UICONTROL 認証]**&#x200B;手順で「**[!UICONTROL 既存のアカウント]**」を選択し、既存の接続を選択します。または、「**[!UICONTROL 新しいアカウント]**」を選択して、クラウドストレージの宛先への新しい接続を設定できます。アカウント認証資格情報を入力し、「**[!UICONTROL 宛先に接続]**」を選択します。必要に応じて、RSA形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 この公開鍵は、Base64エンコードされた文字列 **として書き込む** 必要があります。

See [Amazon S3](./amazon-s3.md) destination, [[!DNL Amazon Kinesis]](./amazon-kinesis.md) destination, [[!DNL Azure Event Hubs]](./azure-event-hubs.md) destination, and [SFTP](./sftp.md) destination for specifics around credentials input in the **Authentication** step.

>[!NOTE]
>
> Real-time CDP は、認証プロセスでの資格情報の検証をサポートし、クラウドのストレージの場所に誤った資格情報が入力されるとエラーメッセージを表示します。これにより、間違った資格情報を使用してワークフローを完了できなくします。

![クラウドストレージの宛先 - 認証手順](../../assets/catalog/cloud-storage/workflow/destination-account.png)

「**[!UICONTROL 設定]**」手順で、アクティベーションフローの「**[!UICONTROL 名前]**」と「**[!UICONTROL 説明]**」を入力します。

また、この手順では、この宛先に適用する **[!UICONTROL マーケティングの使用例]** を選択できます。 マーケティングの使用例は、データがエクスポート先にエクスポートされる意図を示します。 Adobe定義のマーケティングの使用例から選択するか、独自のマーケティングの使用例を作成することができます。 マーケティングの使用例の詳細については、Real-time CDP [（リアルタイムCDP）ページの「](../../../rtcdp/privacy/data-governance-overview.md#destinations) Data Governance（データ・ガバナンス）」を参照してください。 個々のAdobe定義マーケティングの使用例について詳しくは、 [データ使用ポリシーの概要を参照してください](../../../data-governance/policies/overview.md#core-actions)。


Amazon S3 の宛先の場合は、ファイルが配信されるクラウドストレージの宛先に「**[!UICONTROL バケット名]**」と「**[!UICONTROL フォルダーパス]**」を挿入します。上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![Amazon S3 クラウドストレージの宛先への接続 - 認証手順](../../assets/catalog/cloud-storage/workflow/amazon-s3-setup.png)

SFTP の宛先の場合は、ファイルが配信される「**[!UICONTROL フォルダーパス]**」を挿入します。上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![SFTP クラウドストレージの宛先への接続 - 認証手順](../../assets/catalog/cloud-storage/workflow/sftp-setup.png)

宛先の場合は、ア [!DNL Amazon Kinesis] カウント内の既存のデータストリームの名前を指定し [!DNL Amazon Kinesis] ます。 リアルタイムCDPは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![Kinesisクラウドのストレージ先に接続 — 認証手順](../../assets/catalog/cloud-storage/workflow/kinesis-setup.png)

宛先の場合は、ア [!DNL Azure Event Hubs] カウント内の既存のデータストリームの名前を指定し [!DNL Amazon Event Hubs] ます。 リアルタイムCDPは、このストリームにデータをエクスポートします。 上記のフィールドに入力した後、「**[!UICONTROL 宛先を作成]**」を選択します。

![イベントハブクラウドのストレージ先への接続 — 認証手順](../../assets/catalog/cloud-storage/workflow/event-hubs-setup.png)

これで宛先が作成されました。後でセグメントをアクティブにする場合は、「**[!UICONTROL 保存して終了]**」を選択します。また、「**[!UICONTROL 次へ]**」を選択してワークフローを続行し、アクティブ化するセグメントを選択することもできます。いずれの場合も、データをエクスポートする残りのワークフローについては、次の「[セグメントのアクティブ化](#activate-segments)」の節を参照してください。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。