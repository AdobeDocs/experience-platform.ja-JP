---
title: Mutual Transport Layer Security （mTLS）の概要
description: イベント転送用にAdobeが発行する公開証明書を安全に取得するためにmTLSを使用する方法について説明します。
exl-id: e8ee8655-213d-4d2a-93d4-d62824b53b1d
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 2%

---

# 相互トランスポート層セキュリティ （[!DNL mTLS]）の概要

[!DNL mTLS]の相互トランスポート層セキュリティ （[!UICONTROL Environments UI]）証明書をバインドして、拡張機能のセキュリティを制御します。 [!DNL mTLS]証明書は、安全な通信におけるサーバーまたはクライアントのIDを証明するデジタル資格情報です。 [!DNL mTLS] サービス APIを使用する場合、これらの証明書は、Adobe Experience Platform イベント転送とのやり取りを検証および暗号化するのに役立ちます。 このプロセスにより、データを保護できるだけでなく、あらゆる接続を信頼できるパートナーから取得することができます。

## 新しい環境で[!DNL mTLS]を実装する {#implement-mtls}

イベント転送環境を設定して、ライブラリビルドがエッジネットワークに正しくデプロイされていることを確認します。 セットアップ時に、デプロイメントのニーズに最適なホスティングオプションを選択できます。 また、[!DNL mTLS]証明書が安全な通信のために新しい環境に自動的に追加されます。

新しい環境を作成するには、イベント転送プロパティの左側のパネルで「**[!UICONTROL Environments]**」タブを選択し、**[!UICONTROL Add Environment]**&#x200B;を選択します。

![既存の環境を表示するイベント転送プロパティ、[!UICONTROL Add Environment]を強調表示。](../../../images/extensions/server/cloud-connector/add-environment.png)

次のページで、この設定に使用する環境を選択します。 次の3つの環境を使用できます。

>[!NOTE]
>
>プロパティは、1つの開発環境、1つのステージング環境、1つの実稼動環境に制限されます。

| 環境 | 説明 |
| --- | --- |
| 開発 | 開発環境は、チームメンバーがライブラリやイベント転送の変更をテストするためのものです。 |
| ステージング | ステージング環境はオプションで、承認済みチームメンバーは公開前にライブラリをテストおよび承認できます。 |
| 本番 | 実稼動環境は、ライブ実稼動データに使用されます。 |

![開発用の[!UICONTROL Select]を強調表示する環境選択画面。](../../../images/extensions/server/cloud-connector/select-environment.png)

**[!UICONTROL Create Environment]** ページで、**[!UICONTROL Name]**&#x200B;と入力し、***ドロップダウンメニューから*** Adobe Managed **[!UICONTROL Select Host]**&#x200B;を選択します。 **[!UICONTROL Certificate]**&#x200B;は&#x200B;***自動的に***&#x200B;追加されます。 最後に、**[!UICONTROL Save]**&#x200B;を選択します。

![開発環境の作成ページ、[!UICONTROL Name]、[!UICONTROL Select Host]、[!UICONTROL Save]を強調表示します。](../../../images/extensions/server/cloud-connector/create-environment.png)

環境が正常に作成され、新しい環境を表示する「**[!UICONTROL Environments]**」タブに戻ります。

![開発環境を強調表示する「[!UICONTROL Environments]」タブ。](../../../images/extensions/server/cloud-connector/new-environment-created.png)

## 環境証明書の詳細の表示 {#view-certificate}

環境の証明書の詳細を表示するには、イベント転送プロパティの左側のパネルにある「**[!UICONTROL Environments]**」タブを選択し、環境を選択して詳細を表示します。

次の証明書の詳細が表示されます。

| フィールド名 | 説明 |
| --- | --- |
| 証明書 | 証明書の詳細（以下を含む）:<ul><li>**名前**：証明書の名前。</li><li>**作成日**：証明書が作成された日付。</li><li>**ステータス**：証明書の現在のステータス：<ul><li>**現在**：証明書はアクティブに使用中です。</li><li>**廃止**：証明書は使用されていませんが、有効期限がまだ切れていません。 まだ使用するために選択できます。</li><li>**期限切れ**：証明書は期限切れでグレー表示になり、使用できなくなります。</li></ul></ul> |
| 有効期限 | 証明書の有効期限が切れる日付。 |
| Variable Name | 証明書の変数名。 |
| ステータス | 証明書の現在のステータス：<ul><li>**デプロイ済み**：証明書は正常にデプロイされ、アクティブです。</li><li>**デプロイ**：証明書はデプロイ中です。</li><li>**デプロイメントが必要**：このステータスは、古い証明書が選択されている場合に表示されます。</li></ul> |

![開発環境を編集ページです。[!UICONTROL Certificate]の詳細がハイライトされます。](../../../images/extensions/server/cloud-connector/certificate-details.png)

### 古い証明書を選択してデプロイする {#deploy-obsolete-certificate}

古い証明書を使用するには、イベント転送プロパティの左側のパネルにある「**[!UICONTROL Environments]**」タブに移動し、環境を選択して詳細を表示します。

![開発環境を強調表示する「[!UICONTROL Environments]」タブ。](../../../images/extensions/server/cloud-connector/new-environment-created.png)

**[!UICONTROL Certificate]** ドロップダウンから、古い証明書を選択し、**[!UICONTROL Save]**&#x200B;を選択します。

![開発環境を編集ページ。古い証明書と保存がハイライト表示された[!UICONTROL Certificate] ドロップダウンがハイライト表示されます。](../../../images/extensions/server/cloud-connector/obsolete-certificate.png)

証明書をデプロイするには、**[!UICONTROL Save and deploy]** ダイアログで&#x200B;**[!UICONTROL Deploy Certificate]**&#x200B;を選択します。

![保存とデプロイがハイライト表示された証明書のデプロイ ダイアログ。](../../../images/extensions/server/cloud-connector/obsolete-certificate-deploy.png)


## 次の手順 {#next-steps}

このドキュメントでは、イベント転送プロパティの環境を作成し、証明書を追加し、古い証明書を使用する方法を説明しました。 [!DNL mTLS]証明書について詳しくは、[[!DNL mTLS]  サービス APIの概要](../../../../data-governance/mtls-api/overview.md)を参照してください

イベント転送ルールで[!DNL mTLS]証明書を使用する方法については、[Cloud Connector拡張機能の概要](../cloud-connector/overview.md#mtls-rules)を参照してください。
