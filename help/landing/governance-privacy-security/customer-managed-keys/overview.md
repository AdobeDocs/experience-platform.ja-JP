---
title: Adobe Experience Platform の顧客管理キー
description: Adobe Experience Platform に保存されたデータ用に独自の暗号化キーを設定する方法を説明します。
exl-id: cd33e6c2-8189-4b68-a99b-ec7fccdc9b91
source-git-commit: 930c786db51063c55f731dc90f2ee66e98624555
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 36%

---

# Adobe Experience Platform の顧客管理キー

Adobe Experience Platform に保存されたデータは、システムレベルのキーを使用して保存時に暗号化されます。 Platform 上に構築されたアプリケーションを使用している場合は、代わりに独自の暗号化キーを使用するよう選択すると、データのセキュリティをより詳細に制御できます。

>[!NOTE]
>
>Adobe Experience Platformのデータレイクとプロファイルストアのデータは、CMK を使用して暗号化されます。 これらは主要なデータストアと見なされます。

このドキュメントでは、Platform で顧客管理キー (CMK) 機能を有効にするプロセスの概要と、これらの手順を完了するために必要な前提条件の情報を示します。

>[!NOTE]
>
>Customer Journey Analyticsのお客様の場合は、 [Customer Journey Analytics文書](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-privacy/cmk.html?lang=en).

## 前提条件

を表示して、 [!UICONTROL 暗号化] Adobe Experience Platformのセクションで、役割を作成し、 [!UICONTROL 顧客管理キーの管理] その役割に対する権限。 次の項目を持つユーザー [!UICONTROL 顧客管理キーの管理] 権限により、組織で CMK を有効にすることができます。

Experience Platformでの役割および権限の割り当てについて詳しくは、 [権限に関するドキュメントの設定](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja).

CMK を有効にするには、 [!DNL Azure] Key Vault は、次の設定で構成する必要があります。

* [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
* [ソフトデリートを有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
* [次を使用してアクセスを設定 [!DNL Azure] ロールベースのアクセス制御](https://learn.microsoft.com/en-us/azure/role-based-access-control/)

プロセスの理解を深めるには、リンクされたドキュメントをお読みください。

## プロセスの概要 {#process-summary}

CMK は、アドビの Healthcare Shield サービスおよび Privacy and Security Shield サービスに含まれています。お客様の組織がこれらの製品の 1 つに対するライセンスを購入すると、1 回限りのプロセスで機能を設定できます。

>[!WARNING]
>
>CMK を設定した後は、システム管理キーに戻すことはできません。 キーを安全に管理し、[!DNL Azure] 内で Key Vault、キー、CMK アプリへのアクセスを提供して、データへのアクセスが失われないようにする責任があります。

プロセスは以下のようになります。

1. [組織のポリシーに基づく  [!DNL Azure] Key Vault](./azure-key-vault-config.md) を設定してから、最終的にアドビと共有される[暗号化キーを生成](./azure-key-vault-config.md#generate-a-key)します。
1. を使用して CMK アプリを設定する [!DNL Azure] どちらかを借りる [API 呼び出し](./api-set-up.md#register-app) または [UI](./ui-set-up.md#register-app).
1. 暗号化キー ID をAdobeに送信し、次のいずれかの方法でこの機能の有効化プロセスを開始します。 [UI 内](./ui-set-up.md#send-to-adobe) または [API 呼び出し](./api-set-up.md#send-to-adobe).
1. 設定のステータスをチェックして、CMK が有効になっているかどうかを確認します。 [UI 内](./ui-set-up.md#check-status) または [API 呼び出し](./api-set-up.md#check-status).

設定プロセスが完了すると、すべてのサンドボックスをまたいで Platform にオンボードされたデータがすべて、[!DNL Azure] キーの設定を使用して暗号化されます。CMK を使用するには、[公開プレビュープログラム](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)の一部である [!DNL Microsoft Azure] 機能を活用します。

## アクセスを取り消し {#revoke-access}

データへの Platform アクセスを取り消す場合は、アプリケーションに関連付けられているユーザーの役割を [!DNL Azure] 内の Key Vault から削除できます。

>[!WARNING]
>
>キー Vault、キー、または CMK アプリを無効にすると、重大な変更がおこなわれる場合があります。 Platform でキー Vault、キー、または CMK アプリが無効になり、データにアクセスできなくなると、そのデータに関連するダウンストリーム操作はできなくなります。 設定を変更する前に、Platform のアクセスを取り消すことによるダウンストリームの影響を理解しておく必要があります。

キーアクセスを削除した後、またはキーを [!DNL Azure] キー vault、この設定がプライマリデータストアに反映されるまでに、数分から 24 時間かかる場合があります。 Platform のワークフローには、パフォーマンスとコアアプリケーションの機能に必要なキャッシュおよび一時的なデータストアも含まれます。 このようなキャッシュされたストアと一時的なストアを通じて CMK 失効を伝達するには、データ処理ワークフローの判断に従って、最大 7 日間かかる場合があります。 例えば、プロファイルダッシュボードはキャッシュデータストアのデータを保持して表示し、更新サイクルの一環としてキャッシュデータストアに保持されているデータの有効期限を 7 日間に設定します。 アプリケーションへのアクセスを再度有効にすると、データが再び使用可能になるまで、同じように遅延時間が発生します。

>[!NOTE]
>
>非プライマリ（キャッシュ/一時的）データの 7 日間のデータセット有効期限には、2 つのユースケース固有の例外があります。 これらの機能について詳しくは、それぞれのドキュメントを参照してください。<ul><li>[Adobe Journey Optimizer URL 短縮サービス](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms)</li><li>[エッジ投影](https://experienceleague.adobe.com/docs/experience-platform/profile/home.html#edge-projections)</li></ul>

## 次の手順

プロセスを開始するには、まず [設定 [!DNL Azure] Key Vault](./azure-key-vault-config.md) および [暗号化キーを生成](./azure-key-vault-config.md#generate-a-key) をクリックして、Adobeと共有します。
