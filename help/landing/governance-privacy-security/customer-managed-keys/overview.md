---
title: Adobe Experience Platform の顧客管理キー
description: Adobe Experience Platform に保存されたデータ用に独自の暗号化キーを設定する方法を説明します。
exl-id: cd33e6c2-8189-4b68-a99b-ec7fccdc9b91
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 36%

---

# Adobe Experience Platform の顧客管理キー

Adobe Experience Platform に保存されたデータは、システムレベルのキーを使用して保存時に暗号化されます。 Platform 上に構築されたアプリケーションを使用している場合は、代わりに独自の暗号化キーを使用するよう選択すると、データのセキュリティをより詳細に制御できます。

>[!NOTE]
>
>Adobe Experience Platform データレイクとプロファイルストアのデータは、CMK を使用して暗号化されます。 これらは、プライマリデータストアと見なされます。

このドキュメントでは、Platform で顧客管理キー（CMK）機能を有効にするプロセスの概要と、これらの手順を完了するために必要な前提条件について説明します。

>[!NOTE]
>
>Customer Journey Analytics版のお客様は、[Customer Journey Analyticsドキュメント ](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-privacy/cmk.html?lang=ja) の手順に従ってください。

## 前提条件

Adobe Experience Platformの「[!UICONTROL  暗号化 ]」セクションを表示および訪問するには、役割を作成し、その役割に [!UICONTROL  顧客管理キーの管理 ] 権限を割り当てておく必要があります。 [!UICONTROL  顧客管理キーの管理 ] 権限を持つユーザーは組織で CMK を有効にできます。

Experience Platformでの役割と権限の割り当てについて詳しくは、[ 権限の設定に関するドキュメント ](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja) を参照してください。

CMK を有効にするには、[!DNL Azure] Key Vault を次の設定で設定する必要があります。

* [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
* [ ソフト削除を有効にする ](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
* [ 役割ベースのアクセス制御  [!DNL Azure]  使用したアクセスの設定 ](https://learn.microsoft.com/en-us/azure/role-based-access-control/)

プロセスをより深く理解するには、リンクされたドキュメントをお読みください。

## プロセスの概要 {#process-summary}

CMK は、アドビの Healthcare Shield サービスおよび Privacy and Security Shield サービスに含まれています。お客様の組織がこれらの製品の 1 つに対するライセンスを購入すると、1 回限りのプロセスで機能を設定できます。

>[!WARNING]
>
>CMK を設定した後は、システム管理キーに戻すことはできません。 キーを安全に管理し、[!DNL Azure] 内で Key Vault、キー、CMK アプリへのアクセスを提供して、データへのアクセスが失われないようにする責任があります。

プロセスは以下のようになります。

1. [組織のポリシーに基づく  [!DNL Azure] Key Vault](./azure-key-vault-config.md) を設定してから、最終的にアドビと共有される[暗号化キーを生成](./azure-key-vault-config.md#generate-a-key)します。
1. [API 呼び出し ](./api-set-up.md#register-app) または [UI](./ui-set-up.md#register-app) を使用して、[!DNL Azure] テナントとの CMK アプリを設定します。
1. 暗号化キー ID をAdobeに送信し、[UI で ](./ui-set-up.md#send-to-adobe) または [API 呼び出し ](./api-set-up.md#send-to-adobe) を使用して、機能のイネーブルメントプロセスを開始します。
1. 設定のステータスの確認：CMK が [UI で ](./ui-set-up.md#check-status) 有効になっているか、[API 呼び出しで ](./api-set-up.md#check-status) 有効になっているかを確認します。

設定プロセスが完了すると、すべてのサンドボックスをまたいで Platform にオンボードされたデータがすべて、[!DNL Azure] キーの設定を使用して暗号化されます。CMK を使用するには、[公開プレビュープログラム](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)の一部である [!DNL Microsoft Azure] 機能を活用します。

## アクセスの取り消し {#revoke-access}

データへの Platform アクセスを取り消す場合は、アプリケーションに関連付けられているユーザーの役割を [!DNL Azure] 内の Key Vault から削除できます。

>[!WARNING]
>
>Key Vault、キー、または CMK アプリを無効にすると、重大な変更が発生する可能性があります。 Key Vault、キー、または CMK アプリが無効になり、Platform でデータにアクセスできなくなると、そのデータに関連するダウンストリーム操作ができなくなります。 設定を変更する前に、Platform でのキーへのアクセスができなくなることによるダウンストリームの影響を理解しておく必要があります。

キーアクセスを削除した後、または [!DNL Azure] key vault からキーを無効または削除した後、この設定がプライマリデータストアに反映されるまで、数分から 24 時間かかる場合があります。 Platform ワークフローには、パフォーマンスとコアアプリケーション機能に必要な、キャッシュされた一時的なデータストアも含まれます。 このようなキャッシュされた一時的なストアを介した CMK 失効の伝播には、データ処理ワークフローによって決定されるまでに最大 7 日かかる場合があります。 例えば、プロファイルダッシュボードではキャッシュデータストアのデータを保持および表示し、キャッシュデータストアに保持されているデータが更新サイクルの一環として期限切れになるまで 7 日かかります。 アプリケーションへのアクセスを再度有効にすると、データが再び使用可能になるまで、同じように遅延時間が発生します。

>[!NOTE]
>
>非プライマリ（キャッシュ/一時的）データの 7 日間のデータセット有効期限には、ユースケース固有の例外が 2 つあります。 これらの機能について詳しくは、それぞれのドキュメントを参照してください。<ul><li>[Adobe Journey Optimizerの URL 短縮機能 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms)</li><li>[Edge見込み ](https://experienceleague.adobe.com/docs/experience-platform/profile/home.html#edge-projections)</li></ul>

## 次の手順

プロセスを開始するには、まず [Key Vault の設定 ](./azure-key-vault-config.md) および [ 暗号化キーの生成 ](./azure-key-vault-config.md#generate-a-key) を行い、 [!DNL Azure] Adobeと共有します。
