---
title: Azure Key Vault の設定
description: Azure で新しいエンタープライズアカウントを作成する方法、または既存のエンタープライズアカウントを使用して Key Vault を作成する方法について説明します。
role: Developer
feature: Privacy
exl-id: 670e3ca3-a833-4b28-9ad4-73685fa5d74d
source-git-commit: c0eb5b5c3a1968cae2bc19b7669f70a97379239b
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 24%

---

# [!DNL Azure] Key Vault の設定

顧客管理キー（CMK）は、[!DNL Microsoft Azure] Key Vault からのキーのみをサポートします。 作業を開始するには、[!DNL Azure] を使用して新しいエンタープライズアカウントを作成するか、既存のエンタープライズアカウントを使用して、以下の手順に従って Key Vault を作成する必要があります。

>[!IMPORTANT]
>
>[!DNL Azure] Key Vault の Standard、Premium、Managed HSM レベルのみがサポートされています。 [!DNL Azure Dedicated HSM] および [!DNL Azure Payments HSM] はサポートされていません。 提供されるキー管理サービスについて詳しくは、[[!DNL Azure] ドキュメント](https://learn.microsoft.com/ja-jp/azure/security/fundamentals/key-management#azure-key-management-services)を参照してください。

>[!NOTE]
>
>以下のドキュメントでは、Key Vault を作成する基本的な手順についてのみ説明します。 このガイダンス以外では、組織のポリシーに従って Key Vault を設定する必要があります。

[!DNL Azure] ポータルにログインし、検索バーを使用してサービスのリストの下にある **[!DNL Key vaults]** を見つけます。

![ 検索結果で [!DNL Key vaults] がハイライト表示されている [!DNL Microsoft Azure] の検索機能 ](../../images/governance-privacy-security/customer-managed-keys/access-key-vaults.png)

サービスを選択すると、**[!DNL Key vaults]** ページが表示されます。 ここから **[!DNL Create]** を選択します。 

![[!DNL Create] がハイライト表示された [!DNL Microsoft Azure] の [!DNL Key vaults] ダッシュボード ](../../images/governance-privacy-security/customer-managed-keys/create-key-vault.png)

提供されたフォームを使用して、名前や割り当てられたリソースグループなど、Key Vault の基本的な詳細を入力します。

>[!WARNING]
>
>ほとんどのオプションはデフォルト値のままでかまいませんが、**必ずソフト削除および消去保護オプションを有効にしてください**。これらの機能を有効にしないと、Key Vault が削除された場合、データへのアクセスが失われる可能性があります。
>
>![ ソフト削除および消去保護がハイライト表示された [!DNL Microsoft Azure] [!DNL Create a Key Vault] ワークフロー ](../../images/governance-privacy-security/customer-managed-keys/basic-config.png)

ここから、Key Vault の作成ワークフローを続け、組織のポリシーに従って様々なオプションを設定します。

**[!DNL Review + create]** の手順に達したら、検証中に Key Vault の詳細を確認できます。 検証に問題がなければ、「**[!DNL Create]**」を選択してプロセスを完了します。

![ 「作成」がハイライト表示されたMicrosoft Azure Key Vault の「レビューと作成」ページ ](../../images/governance-privacy-security/customer-managed-keys/finish-creation.png)

## アクセスを設定 {#configure-access}

次に、Key Vault の Azure ロールベースのアクセス制御を有効にします。 左側のナビゲーションの「[!DNL Settings]」セクションで「**[!DNL Access configuration]**」を選択したあと、「**[!DNL Azure role-based access control]**」を選択して設定を有効にします。 CMK アプリは後で Azure ロールに関連付ける必要があるので、この手順は必須です。 役割の割り当ては、[API](./api-set-up.md#assign-to-role) ワークフローと [UI](./ui-set-up.md#assign-to-role) ワークフローの両方に記載されています。

![[!DNL Access configuration] と [!DNL Azure role-based access control] がハイライト表示された [!DNL Microsoft Azure] ダッシュボード。](../../images/governance-privacy-security/customer-managed-keys/access-configuration.png)

## ネットワークオプションの設定 {#configure-network-options}

公開アクセスを特定の仮想ネットワークに制限するように Key Vault が設定されている場合、または公開アクセスを完全に無効にする場合は、ファイアウォールの例外を付与する必要 [!DNL Microsoft] あります。

左側のナビゲーションの「**[!DNL Networking]**」を選択します。 **[!DNL Firewalls and virtual networks]** の下で、「**[!DNL Allow trusted Microsoft services to bypass this firewall]**」チェックボックスを選択し、「**[!DNL Apply]**」を選択します。

![[!DNL Networking] と [!DNL Allow trusted Microsoft surfaces to bypass this firewall] の例外がハイライト表示された [!DNL Microsoft Azure] の「[!DNL Networking]」タブ ](../../images/governance-privacy-security/customer-managed-keys/networking.png)

### キーの生成 {#generate-a-key}

Key Vault を作成したら、新しいキーを生成できます。 「**[!DNL Keys]**」タブに移動し、「**[!DNL Generate/Import]**」を選択します。

![[!DNL Generate import] がハイライト表示された [!DNL Azure] の「[!DNL Keys]」タブ ](../../images/governance-privacy-security/customer-managed-keys/view-keys.png)

提供されたフォームを使用してキーの名前を指定し、キータイプに **RSA** または **RSA-HSM** のいずれかを選択します。 少なくとも、[!DNL Cosmos DB] で要求されているように、**[!DNL RSA key size]** は少なくとも **3072** ビットである必要があります。 [!DNL Azure Data Lake Storage] は、RSA 3027 とも互換性があります。

>[!NOTE]
>
>キーをAdobeに送るために必要なので、キーに指定した名前を覚えておきます。

残りのコントロールを使用して、必要に応じて生成または読み込むキーを設定します。終了したら「**[!DNL Create]**」を選択します。

![[!DNL 3072] ビットがハイライト表示された [!DNL Create a key] ダッシュボード。](../../images/governance-privacy-security/customer-managed-keys/configure-key.png)

設定されたキーが、Vault のキーのリストに表示されます。

![ キー名がハイライト表示された [!DNL Keys] ワークスペース。](../../images/governance-privacy-security/customer-managed-keys/key-added.png)

## 次の手順

顧客管理キー機能の設定の 1 回限りのプロセスを続行するには、顧客管理キー設定ガイド [API](./api-set-up.md) または [UI](./ui-set-up.md) のいずれかに進みます。
