---
title: Adobe Experience Platformの顧客管理キー
description: Adobe Experience Platform に保存されたデータ用に独自の暗号化キーを設定する方法を説明します。
role: Developer
feature: Privacy
exl-id: cd33e6c2-8189-4b68-a99b-ec7fccdc9b91
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1111'
ht-degree: 6%

---

# Adobe Experience Platformの顧客管理キー

Adobe Experience Platform に保存されたデータは、システムレベルのキーを使用して保存時に暗号化されます。 Experience Platform上に構築されたアプリケーションを使用している場合は、代わりに独自の暗号化キーを使用するように選択すると、データのセキュリティをより詳細に制御できます。

>[!AVAILABILITY]
>
>Adobe Experience Platformは、Microsoft Azure とAmazon Web Services（AWS）の両方で顧客管理キー（CMK）をサポートします。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 実装がAWSで実行されている場合は、Experience Platform データ暗号化に Key Management Service （KMS）を使用することができます。 サポートされるインフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud) を参照してください。
>
>AWS KMS での暗号化キーの作成と管理については、[AWS KMS データ暗号化ガイド ](./aws/configure-kms.md) を参照してください。 Azure の実装については、[Azure Key Vault 設定ガイド ](./azure/azure-key-vault-config.md) を参照してください。

>[!NOTE]
>
>ホステッド型Experience Platform インスタンス [!DNL Azure] 場合、Experience Platform [!DNL Azure Data Lake] および [!DNL Azure Cosmos DB] プロファイルストアに保存された顧客プロファイルデータは、有効にすると、CMK を使用してのみ暗号化されます。 プライマリデータストアでのキーの失効には、一時的なデータストアまたはセカンダリデータストアの場合は **数分から 24 時間** および **最大 7 日** かかる場合があります。 詳しくは、[ キーアクセスの取り消しの影響 ](#revoke-access) の節を参照してください。

このドキュメントでは、[!DNL Azure] およびAWSをまたいでExperience Platformの顧客管理キー（CMK）機能を有効にするためのプロセスの概要と、これらの手順を実行するために必要な前提条件について説明します。

>[!NOTE]
>
>Customer Journey Analyticsのお客様は、[Customer Journey Analytics ドキュメント ](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-privacy/cmk.html?lang=ja) の手順に従ってください。

## 前提条件

CMK を有効にするには、お使いのプラットフォームのホスティング環境（[!DNL Azure] またはAWS）が特定の設定要件を満たしている必要があります。

### 一般的な前提条件

Adobe Experience Platformの「[!UICONTROL &#x200B; 暗号化 &#x200B;]」セクションを表示してアクセスするには、役割を作成し、その役割に [!UICONTROL &#x200B; 顧客管理キーの管理 &#x200B;] 権限を割り当てる必要があります。  [!UICONTROL &#x200B; 顧客管理キーの管理 &#x200B;] 権限を持つユーザーは組織で CMK を有効にできます。

Experience Platformでの役割と権限の割り当てについて詳しくは、[ 権限の設定に関するドキュメント ](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja) を参照してください。

### Azure 固有の前提条件

Azure でホストされる実装の場合、次の設定を使用して [!DNL Azure] Key Vault を設定します。

- [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
- [ ソフト削除を有効にする ](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
- [ 役割ベースのアクセス制御  [!DNL Azure]  使用したアクセスの設定 ](https://learn.microsoft.com/en-us/azure/role-based-access-control/)

### AWS固有の前提条件

AWSでホストされる実装の場合は、次のようにAWS環境を設定します。

- AWS Identity and Access Management （IAM）を使用して暗号化キーを管理する権限があることを確認します。 詳しくは、[AWS KMS の IAM ポリシー ](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) を参照してください。
- CMK をサポートするAWS KMS を設定します。 [AWS KMS データ暗号化ガイド ](./aws/configure-kms.md) を参照してください。

## プロセスの概要 {#process-summary}

顧客管理キー（CMK）は、Adobeの Healthcare Shield およびプライバシーとセキュリティシールド サービスを通じて利用できます。 Azure では、CMK は Healthcare Shield とプライバシーおよびセキュリティシールドの両方でサポートされています。 AWSでは、CMK は Privacy and Security Shield でのみサポートされ、Healthcare Shield では使用できません。 組織がこれらの製品の 1 つに対するライセンスを購入したら、CMK を有効にするための 1 回限りの設定プロセスを開始できます。

>[!WARNING]
>
>CMK を設定した後は、システム管理キーに戻すことはできません。 キーを安全に管理して、データへのアクセスが失われないようにする責任があります。

プロセスは以下のようになります。

### Azure の場合 {#azure-process-summary}

1. 組織のポリシーに基づいて [ [!DNL Azure]  Key Vault を設定 ](./azure/azure-key-vault-config.md) してから、[ 暗号化キーを生成 ](./azure/azure-key-vault-config.md#generate-a-key) してAdobeと共有します。
1. [API 呼び出し ](./azure/api-set-up.md#register-app) または [UI](./azure/ui-set-up.md#register-app) を使用して、[!DNL Azure] テナントとの CMK アプリを設定します。
1. 暗号化キー ID をAdobeに送信し、[UI 内 ](./azure/ui-set-up.md#send-to-adobe) または [API 呼び出し ](./azure/api-set-up.md#send-to-adobe) によって機能のイネーブルメントプロセスを開始します。
1. 設定のステータスの確認：CMK が（UI または [API 呼び出し ](./azure/ui-set-up.md#check-status) で有効になっているかどうかを確認し [ す ](./azure/api-set-up.md#check-status)。

Azure がホストするExperience Platform インスタンスの設定プロセスが完了すると、すべてのサンドボックスをまたいでExperience Platformにオンボードされたデータがすべて、[!DNL Azure] キーの設定を使用して暗号化されます。 CMK を使用するには、[公開プレビュープログラム](https://azure.microsoft.com/ja-jp/support/legal/preview-supplemental-terms/)の一部である [!DNL Microsoft Azure] 機能を活用します。

### AWS用 {#aws-process-summary}

1. Adobeと共有する暗号化キーを設定して [AWS KMS をセットアップ ](./aws/configure-kms.md) します。
2. [UI セットアップガイド ](./aws/ui-set-up.md) に記載されているAWS固有の手順に従います。
3. 設定を検証し、AWSがホストするキーを使用してExperience Platform データが暗号化されていることを確認します。

<!--  Pending: or [API setup guide]() -->

AWSでホストされるExperience Platform インスタンスのセットアッププロセスが完了すると、すべてのサンドボックスをまたいでExperience Platformにオンボードされたデータがすべて、AWS Key Management Service （KMS）の設定を使用して暗号化されます。 AWSで CMK を使用するには、AWS Key Management Service を使用して、組織のセキュリティ要件に合わせて暗号化キーを作成および管理します。

## キーアクセスの取り消しの影響 {#revoke-access}

Azure の Key Vault、キー、CMK アプリまたはAWSの暗号化キーへのアクセスを取り消したり無効にしたりすると、Experience Platformの操作に重大な変更が加えられるなど、重大な中断が生じる可能性があります。 キーを無効にすると、Experience Platformのデータにアクセスできなくなる可能性があり、このデータに依存するダウンストリーム操作は機能しなくなります。 主要な設定を変更する前に、ダウンストリームの影響を十分に理解することが重要です。

[!DNL Azure] のデータへのExperience Platform アクセスを取り消すには、アプリケーションに関連付けられているユーザーの役割を Key Vault から削除します。 AWSの場合、このキーを無効にするか、ポリシーステートメントを更新できます。 AWS プロセスの手順について詳しくは、[ 鍵失効の節 ](./aws/ui-set-up.md#key-revocation) を参照してください。


### 伝播タイムライン {#propagation-timelines}

[!DNL Azure] Key Vault からキーアクセスが取り消されると、変更は次のように反映されます。

| **ストアタイプ** | **説明** | **タイムライン** |
|---|---|---|
| プライマリデータストア | データレイク（Azure Data Lake、AWS S3）および Azure Cosmos DB プロファイルストアが含まれます。 キーアクセスが取り消されると、データにアクセスできなくなります。 | **数分～24 時間**。 |
| キャッシュ/一時的なデータストア | パフォーマンスおよびコアアプリケーション機能に使用されるセカンダリデータストアが含まれます。 キーの失効の影響は遅延します。 | **最長 7 日間**。 |

例えば、プロファイルダッシュボードには、データの有効期限が切れて更新されるまでの、最大 7 日間、キャッシュのデータが引き続き表示されます。 同様に、アプリケーションへのアクセスを再度有効にすると、これらのストア間でデータの可用性をリストアするのに同じ時間がかかります。

>[!NOTE]
>
>アプリケーションへのアクセスの再有効化には、これらのストア間でデータの可用性を復元するための失効と同じ時間がかかる場合があります。

>[!TIP]
>
>非プライマリ（キャッシュ/一時的）データの 7 日間のデータセット有効期限には、ユースケース固有の例外が 2 つあります。 これらの機能について詳しくは、それぞれのドキュメントを参照してください。<ul><li>[Adobe Journey Optimizerの URL 短縮機能 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms)</li><li>[Edge見込み ](https://experienceleague.adobe.com/docs/experience-platform/profile/home.html#edge-projections)</li></ul>

## 次の手順

プロセスを開始するには：

- Azure の場合：まず [Key Vault の設定 ](./azure/azure-key-vault-config.md) [!DNL Azure]  および [ 暗号化キーの生成 ](./azure/azure-key-vault-config.md#generate-a-key) を行い、Adobeと共有します。
- AWSの場合：[AWS KMS を設定する ](./aws/configure-kms.md) と、UI または API 設定ガイドに進む前に、IAM および KMS の適切な設定を確認します。
