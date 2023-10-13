---
title: Azure Key Vault を設定する
description: Azure で新しいエンタープライズアカウントを作成する方法、または既存のエンタープライズアカウントを使用して Key Vault を作成する方法を説明します。
exl-id: 670e3ca3-a833-4b28-9ad4-73685fa5d74d
source-git-commit: 4ec87482c5a38404217ecd910b6a27ee2d0e00eb
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 36%

---

# [!DNL Azure] Key Vault の設定

顧客管理キー (CMK) は、 [!DNL Microsoft Azure] キー Vault。 作業を開始するには、[!DNL Azure] を使用して新しいエンタープライズアカウントを作成するか、既存のエンタープライズアカウントを使用して、以下の手順に従って Key Vault を作成する必要があります。

>[!IMPORTANT]
>
>[!DNL Azure] Key Vault の Premium と Standard のサービスレベルのみがサポートされています。[!DNL Azure Managed HSM]、[!DNL Azure Dedicated HSM] および [!DNL Azure Payments HSM] ではサポートされていません。提供されるキー管理サービスについて詳しくは、[[!DNL Azure] ドキュメント](https://learn.microsoft.com/ja-jp/azure/security/fundamentals/key-management#azure-key-management-services)を参照してください。

>[!NOTE]
>
>以下のドキュメントでは、Key Vault を作成する基本的な手順についてのみ説明します。 このガイダンス以外では、組織のポリシーに従って Key Vault を設定する必要があります。

[!DNL Azure] ポータルにログインし、検索バーを使用してサービスのリストの下にある **[!DNL Key vaults]** を見つけます。

![の検索機能 [!DNL Microsoft Azure] 次を使用 [!DNL Key vaults] 検索結果内でハイライト表示されます。](../../images/governance-privacy-security/customer-managed-keys/access-key-vaults.png)

サービスを選択すると、**[!DNL Key vaults]** ページが表示されます。 ここから **[!DNL Create]** を選択します。 

![The [!DNL Key vaults] ダッシュボード [!DNL Microsoft Azure] 次を使用 [!DNL Create] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/create-key-vault.png)

提供されたフォームを使用して、名前や割り当てられたリソースグループなど、Key Vault の基本的な詳細を入力します。

>[!WARNING]
>
>ほとんどのオプションはデフォルト値のままでかまいませんが、**必ずソフト削除および消去保護オプションを有効にしてください**。これらの機能をオンにしないと、Key Vault が削除された場合に、データへのアクセスが失われる可能性があります。
>
>![The [!DNL Microsoft Azure] [!DNL Create a Key Vault] ソフト削除およびパージ保護がハイライトされたワークフロー。](../../images/governance-privacy-security/customer-managed-keys/basic-config.png)

ここから、Key Vault の作成ワークフローを続け、組織のポリシーに従って様々なオプションを設定します。

いったん **[!DNL Review + create]** 手順では、Key Vault の詳細を確認しながら、検証を行うことができます。 検証に問題がなければ、「**[!DNL Create]**」を選択してプロセスを完了します。

![Microsoft Azure Key Vault の [ レビューと作成 ] がハイライト表示されたページを作成します。](../../images/governance-privacy-security/customer-managed-keys/finish-creation.png)

## アクセスの設定 {#configure-access}

次に、Azure の役割に基づくアクセス制御をキー Vault に対して有効にします。 選択 **[!DNL Access configuration]** （内） [!DNL Settings] 左側のナビゲーションの「 」セクションで、「 」を選択します。 **[!DNL Azure role-based access control]** をクリックして設定を有効にします。 CMK アプリは後で Azure の役割に関連付ける必要があるので、この手順は必須です。 ロールの割り当てについては、 [API](./api-set-up.md#assign-to-role) および [UI](./ui-set-up.md#assign-to-role) ワークフロー。

![The [!DNL Microsoft Azure] 次のダッシュボード [!DNL Access configuration] および [!DNL Azure role-based access control] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/access-configuration.png)

## ネットワークオプションの設定 {#configure-network-options}

Key Vault が、特定の仮想ネットワークへのパブリックアクセスを制限するように設定されている場合、またはパブリックアクセスを完全に無効にする場合は、 [!DNL Microsoft] ファイアウォールの例外。

左側のナビゲーションの「**[!DNL Networking]**」を選択します。 **[!DNL Firewalls and virtual networks]** の下で、「**[!DNL Allow trusted Microsoft services to bypass this firewall]**」チェックボックスを選択し、「**[!DNL Apply]**」を選択します。

![The [!DNL Networking] タブ [!DNL Microsoft Azure] 次を使用 [!DNL Networking] および [!DNL Allow trusted Microsoft surfaces to bypass this firewall] 例外がハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/networking.png)

### キーの生成 {#generate-a-key}

Key Vault を作成したら、新しいキーを生成できます。 「**[!DNL Keys]**」タブに移動し、「**[!DNL Generate/Import]**」を選択します。

![The [!DNL Keys] タブ [!DNL Azure] 次を使用 [!DNL Generate import] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/view-keys.png)

提供されたフォームを使用してキーの名前を指定し、キータイプに「**RSA**」を選択します。少なくとも、 **[!DNL RSA key size]** は、少なくとも **3072** 必要なビット数 [!DNL Cosmos DB]. [!DNL Azure Data Lake Storage] は、RSA 3027 とも互換性があります。

>[!NOTE]
>
>キーをAdobeに送信するために必要な、キーに指定した名前を記憶します。

残りのコントロールを使用して、必要に応じて生成または読み込むキーを設定します。終了したら「**[!DNL Create]**」を選択します。

![キーダッシュボードの作成 [!DNL 3072] ハイライト表示されたビット。](../../images/governance-privacy-security/customer-managed-keys/configure-key.png)

設定されたキーが、Vault のキーのリストに表示されます。

![The [!DNL Keys] キー名がハイライト表示されたワークスペース。](../../images/governance-privacy-security/customer-managed-keys/key-added.png)

## 次の手順

顧客が管理するキー機能を 1 回限りで設定する手順を続行するには、 [API](./api-set-up.md) または [UI](./ui-set-up.md) 顧客管理キーの設定ガイド。
