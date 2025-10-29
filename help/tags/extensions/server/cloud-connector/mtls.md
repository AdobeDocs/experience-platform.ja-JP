---
title: mTLS （Mutual Transport Layer Security）の概要
description: mTLS を使用して、イベント転送用にAdobeが発行した公開証明書を安全に取得する方法を説明します。
exl-id: e8ee8655-213d-4d2a-93d4-d62824b53b1d
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '605'
ht-degree: 2%

---

# 相互トランスポート層セキュリティ（[!DNL mTLS]）の概要

拡張機能のセキュリティを制御するために、[!DNL mTLS] で相互トランスポート層セキュリティ （[!UICONTROL Environments UI]）証明書をバインドします。 [!DNL mTLS] 証明書は、安全な通信におけるサーバーまたはクライアントの ID を証明するデジタル証明書です。 [!DNL mTLS] Service API を使用する場合、これらの証明書は、Adobe Experience Platform イベント転送とのインタラクションを検証し、暗号化するのに役立ちます。 このプロセスは、データを保護するだけでなく、すべての接続が信頼できるパートナーからのものであることを保証します。

## 新しい環境への [!DNL mTLS] の実装 {#implement-mtls}

イベント転送環境を設定して、ライブラリビルドが Edge ネットワークに正しくデプロイされていることを確認します。 セットアップ時に、デプロイメントのニーズに最適なホスティングオプションを選択できます。 また、安全な通信のために、[!DNL mTLS] 証明書が新しい環境に自動的に追加されます。

新しい環境を作成するには、イベント転送プロパティの左側のパネルで「**[!UICONTROL Environments]**」タブを選択し、「**[!UICONTROL Add Environment]**」を選択します。

![ 既存の環境を示すイベント転送プロパティ。[!UICONTROL Add Environment] がハイライト表示されています。](../../../images/extensions/server/cloud-connector/add-environment.png)

次のページで、この設定に使用する環境を選択します。 次の 3 つの環境を使用できます。

>[!NOTE]
>
>プロパティは、1 つの開発環境、1 つのステージング環境、1 つの実稼動環境に制限されます。

| 環境 | 説明 |
| --- | --- |
| 開発 | 開発環境は、チームメンバーがイベント転送でライブラリまたは変更をテストするためのものです。 |
| ステージング | ステージング環境はオプションで、承認済みチームメンバーがライブラリを公開前にテストおよび承認できます。 |
| 実稼動 | 実稼動環境は、実稼動データに使用されます。 |

![ 環境の選択画面。開発用の [!UICONTROL Select] がハイライト表示されています。](../../../images/extensions/server/cloud-connector/select-environment.png)

**[!UICONTROL Create Environment]** ページで **[!UICONTROL Name]** を入力し、***ドロップダウンメニューから「*** Adobe管理 **[!UICONTROL Select Host]**」を選択します。 **[!UICONTROL Certificate]** は ***自動的に追加*** されます。 最後に、「**[!UICONTROL Save]**」を選択します。

![[!UICONTROL Name]、[!UICONTROL Select Host]、[!UICONTROL Save] がハイライト表示された開発環境を作成ページ ](../../../images/extensions/server/cloud-connector/create-environment.png)

環境が正常に作成され、「**[!UICONTROL Environments]**」タブに戻ります。このタブには新しい環境が表示されます。

![ 「開 [!UICONTROL Environments]」タブ。開発環境がハイライト表示されています。](../../../images/extensions/server/cloud-connector/new-environment-created.png)

## 環境証明書の詳細の表示 {#view-certificate}

環境の証明書の詳細を表示するには、イベント転送プロパティの左側のパネルで「**[!UICONTROL Environments]**」タブを選択し、詳細を表示する環境を選択します。

次の証明書の詳細が表示されます。

| フィールド名 | 説明 |
| --- | --- |
| 証明書 | 証明書の詳細。これには次が含まれます。<ul><li>**名前**：証明書の名前。</li><li>**作成日**：証明書が作成された日付。</li><li>**ステータス**：証明書の現在のステータス：<ul><li>**現在**：証明書はアクティブに使用されています。</li><li>**古い形式**：証明書は使用されていませんが、まだ有効期限が切れていません。 引き続き使用するために選択できます。</li><li>**期限切れ**：証明書の有効期限が切れ、グレー表示され、使用できなくなっています。</li></ul></ul> |
| Expires | 証明書の有効期限。 |
| Variable Name | 証明書の変数名。 |
| ステータス | 証明書の現在のステータス：<ul><li>**デプロイ**：証明書は正常にデプロイされ、アクティブになっています。</li><li>**デプロイ**：証明書はデプロイ中です。</li><li>**デプロイメントが必要**：このステータスは、古い証明書が選択された場合に表示されます。</li></ul> |

![ 開発環境を編集ページが開き、[!UICONTROL Certificate] 細がハイライト表示されます。](../../../images/extensions/server/cloud-connector/certificate-details.png)

### 古い証明書を選択して展開する {#deploy-obsolete-certificate}

古い証明書を使用するには、イベント転送プロパティの左パネルにある「**[!UICONTROL Environments]**」タブに移動し、詳細を表示する環境を選択します。

![ 「開 [!UICONTROL Environments]」タブ。開発環境がハイライト表示されています。](../../../images/extensions/server/cloud-connector/new-environment-created.png)

**[!UICONTROL Certificate]** ドロップダウンから古い証明書を選択し、「**[!UICONTROL Save]**」を選択します。

![ 開発環境を編集ページで、古い証明書と「保存」 [!UICONTROL Certificate] ハイライト表示された状態でドロップダウンがハイライト表示されます。](../../../images/extensions/server/cloud-connector/obsolete-certificate.png)

証明書をデプロイするには、**[!UICONTROL Save and deploy]** ダイアログで「**[!UICONTROL Deploy Certificate]**」を選択します。

![ 「保存してデプロイ」がハイライト表示された証明書をデプロイ ](../../../images/extensions/server/cloud-connector/obsolete-certificate-deploy.png) ダイアログ


## 次の手順 {#next-steps}

このドキュメントでは、イベント転送プロパティの環境を作成し、証明書を追加して、古い証明書を使用する方法を説明しました。 [!DNL mTLS] 証明書について詳しくは、[[!DNL mTLS] Service API の概要 ](../../../../data-governance/mtls-api/overview.md) を参照してください

イベント転送ルールで [!DNL mTLS] 証明書を使用する方法については、[Cloud Connector 拡張機能の概要 ](../cloud-connector/overview.md#mtls-rules) を参照してください。
