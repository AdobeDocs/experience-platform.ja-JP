---
title: Experience Platform UI を使用した Azure の顧客管理キーの設定と設定
description: Azure テナントで CMK アプリを設定し、暗号化キー ID をAdobe Experience Platformに送信する方法を説明します。
role: Developer
feature: Privacy
exl-id: 5f38997a-66f3-4f9d-9c2f-fb70266ec0a6
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 18%

---

# Experience Platform UI を使用した Azure の顧客管理キーの設定と設定

このドキュメントでは、UI を使用してExperience Platformで顧客管理キー（CMK）機能を有効にする Azure 固有の手順について説明します。 AWS固有の手順については、[AWS設定ガイド &#x200B;](../aws/ui-set-up.md) を参照してください。

API を使用して Azure でホストされるExperience Platform インスタンスのこのプロセスを完了する手順については、[API CMK 設定ドキュメント &#x200B;](./api-set-up.md) を参照してください。

## 前提条件

Adobe Experience Platformの「[!UICONTROL &#x200B; 暗号化 &#x200B;]」セクションを表示および訪問するには、役割を作成し、その役割に [!UICONTROL &#x200B; 顧客管理キーの管理 &#x200B;] 権限を割り当てておく必要があります。 [!UICONTROL &#x200B; 顧客管理キーの管理 &#x200B;] 権限を持つユーザーは組織で CMK を有効にできます。

Experience Platformでの役割と権限の割り当てについて詳しくは、[&#x200B; 権限の設定に関するドキュメント &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja) を参照してください。

CMK を有効にするには、次の設定で [[!DNL Azure] Key Vault](./azure-key-vault-config.md) を設定する必要があります。

* [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
* [&#x200B; ソフト削除を有効にする &#x200B;](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
* [&#x200B; 役割ベースのアクセス制御  [!DNL Azure]  使用したアクセスの設定 &#x200B;](https://learn.microsoft.com/en-us/azure/role-based-access-control/)
* [Key Vault [!DNL Azure]  設定](./azure-key-vault-config.md)

## CMK アプリのセットアップ {#register-app}

Key Vault を設定したら、次の手順は、[!DNL Azure] テナントにリンクする CMK アプリケーションを登録します。

### はじめに

[!UICONTROL &#x200B; 暗号化設定 &#x200B;] ダッシュボードを表示するには、左側のナビゲーションサイドバーの **[!UICONTROL 管理]** 見出しの下にある [!UICONTROL &#x200B; 暗号化 &#x200B;] を選択します。

![&#x200B; 暗号化と顧客管理キーカードがハイライト表示された暗号化設定ダッシュボード。](../../../images/governance-privacy-security/customer-managed-keys/encryption-configraion.png)

**[!UICONTROL 設定]** を選択して、[!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] 表示を開きます。 このワークスペースには、以下に説明する手順を完了し、Azure Key Vault との統合を実行するために必要なすべての値が含まれています。

### 認証 URL をコピー {#copy-authentication-url}

登録プロセスを開始するには、[!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビューから組織のアプリケーション認証 URL をコピーし、[!DNL Azure] 環境 **[!DNL Key Vault Crypto Service Encryption User]** ージに貼り付けます。 [&#x200B; 役割の割り当て &#x200B;](#assign-to-role) 方法について詳しくは、次の節を参照してください。

コピーアイコン（![&#x200B; コピーアイコンを選択します。](../../../../images/icons/copy.png)）を選択します [!UICONTROL &#x200B; アプリケーション認証 URL]。

![&#x200B; アプリケーション認証 URL セクションがハイライト表示された [!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビュー。](../../../images/governance-privacy-security/customer-managed-keys/application-authentication-url.png)

[!UICONTROL &#x200B; アプリケーション認証 URL] をコピーしてブラウザーに貼り付け、認証ダイアログを開きます。 「**[!DNL Accept]**」を選択して、CMK アプリサービスプリンシパルを [!DNL Azure] テナントに追加します。 認証を確定すると、Experience Cloud ランディングページにリダイレクトされます。

![&#x200B; 「同意する [!UICONTROL &#x200B; がハイライト表示されたMicrosoft権限リクエストダイアログ &#x200B;]](../../../images/governance-privacy-security/customer-managed-keys/app-permission.png)

>[!IMPORTANT]
>
>複数の [!DNL Microsoft Azure] 購読がある場合、Experience Platform インスタンスを間違った Key Vault に接続できる可能性があります。 この場合、CMK ディレクトリ ID のアプリケーション認証 URL 名の `common` セクションを入れ替える必要があります。<br>[!DNL Microsoft Azure] アプリケーションのポータル設定、ディレクトリ、購読ページから CMK ディレクトリ ID をコピーします <br>![&#x200B; ディレクトリ ID が強調表示された [!DNL Microsoft Azure] アプリケーションのポータル設定、ディレクトリ、購読ページ。](../../../images/governance-privacy-security/customer-managed-keys/directory-id.png)<br> 次に、ブラウザーのアドレスバーにペーストします。<br>![&#x200B; アプリケーション認証 URL の「共通」セクションがハイライト表示されたGoogle ブラウザーページ。](../../../images/governance-privacy-security/customer-managed-keys/common-url-section.png)

### CMK アプリを役割に割り当てます。 {#assign-to-role}

認証プロセスが完了したら、[!DNL Azure] Key Vault に戻り、左側のナビゲーションで **[!DNL Access control]** を選択します。 ここから **[!DNL Add]** を選択し、続けて **[!DNL Add role assignment]** を選択します。

![[!DNL Add] と [!DNL Add role assignment] がハイライト表示された [!DNL Microsoft Azure] ダッシュボード。](../../../images/governance-privacy-security/customer-managed-keys/add-role-assignment.png)

次の画面では、この割り当ての役割を選択するように求められます。**[!DNL Key Vault Crypto Service Encryption User]** を選択してから **[!DNL Next]** を選択し、続行します。

>[!NOTE]
>
>[!DNL Managed-HSM Key Vault] 層がある場合は、**[!DNL Managed HSM Crypto Service Encryption User]** ユーザーの役割を選択する必要があります。

![[!DNL Key Vault Crypto Service Encryption User] がハイライト表示された [!DNL Microsoft Azure] ダッシュボード。](../../../images/governance-privacy-security/customer-managed-keys/select-role.png)

次の画面で、「**[!DNL Select members]**」 を選択して、右側のパネルでダイアログを開きます。 検索バーを使用して CMK アプリケーションのサービスプリンシパルを見つけ、リストから選択します。 終了したら「**[!DNL Save]**」を選択します。

>[!NOTE]
>
>リストにアプリケーションが見つからない場合は、サービスプリンシパルがテナントに受け入れられていません。 適切な権限を持っていることを確認するには、[!DNL Azure] 管理者または担当者に相談してください。

[!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビューで提供される [!UICONTROL &#x200B; アプリケーション ID] と、[!DNL Microsoft Azure] アプリケーションの概要で提供される [!DNL Application ID] を比較することで、アプリケーションを検証できます。

![&#x200B; アプリケーション ID がハイライト表示された [!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビュー &#x200B;](../../../images/governance-privacy-security/customer-managed-keys/application-id.png)

Azure ツールの検証に必要な詳細がすべてExperience Platform UI に含まれています。 このレベルの精度は、他の Azure ツールを使用して、これらのアプリケーションの監視と Key Vault へのアクセスをログに記録する機能を強化したい多くのユーザーに提供されます。 これらの識別子を理解することは、その目的のために、またAdobe サービスが鍵にアクセスするのを助けるために重要です。

## Experience Platform の暗号化キー設定を有効にする {#send-to-adobe}

CMK アプリを [!DNL Azure] にインストールすると、暗号化キー識別子をアドビに送信できます。 左側のナビゲーションで「**[!DNL Keys]**」を選択し、次に送信するキーの名前を選択します。

![[!DNL Keys] オブジェクトとキー名がハイライト表示されたMicrosoft Azure ダッシュボード。](../../../images/governance-privacy-security/customer-managed-keys/select-key.png)

キーの最新バージョンを選択すると、その詳細ページが表示されます。ここから、オプションでキーに対して許可する操作を設定できます。

>[!IMPORTANT]
>
>キーに許可される最低限必要な操作は、**[!DNL Wrap Key]** と **[!DNL Unwrap Key]** の権限です。 必要に応じて、[!DNL Encrypt]、[!DNL Decrypt]、[!DNL Sign]、[!DNL Verify] を含めることができます。

「**[!UICONTROL キー識別子]**」フィールドには、キーの URI 識別子が表示されます。次の手順で使用するために、この URI 値をコピーします。

![[!DNL Permitted operations] とキー URL をコピーセクションがハイライト表示されたMicrosoft Azure ダッシュボードキーの詳細。](../../../images/governance-privacy-security/customer-managed-keys/copy-key-url.png)

[!DNL Key vault URI] を取得したら、[!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] 表示に戻り、わかりやすい **[!UICONTROL 設定名]** を入力します。 次に、Azure キーの詳細ページから取得した [!DNL Key Identifier] を **[!UICONTROL Key Vault キー識別子]** に追加し、「保存 **」** 選択します。

![[!UICONTROL Configuration name] および [!UICONTROL Key Vault キー識別子 &#x200B;] セクションがハイライト表示された [!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビュー &#x200B;](../../../images/governance-privacy-security/customer-managed-keys/configuration-name.png)

[!UICONTROL &#x200B; 暗号化設定ダッシュボード &#x200B;] に戻ります。 [!UICONTROL &#x200B; 顧客管理キー &#x200B;] 設定のステータスが [!UICONTROL &#x200B; 処理中 &#x200B;] と表示されます。

![&#x200B; 顧客管理キー  カード上にハイライト表示された [!UICONTROL &#x200B; 処理中 &#x200B;] を含む [!UICONTROL &#x200B; 暗号化設定 &#x200B;] ダッシュボード &#x200B;](../../../images/governance-privacy-security/customer-managed-keys/processing.png)

## 設定のステータスの確認 {#check-status}

処理にかなりの時間を割くことができます。 設定のステータスを確認するには、[!UICONTROL &#x200B; 顧客管理キー設定 &#x200B;] ビューに戻り、下にスクロールして [!UICONTROL &#x200B; 設定ステータス &#x200B;] を表示します。 プログレスバーがステップ 3 まで進み、Experience Platformがキーと Key Vault にアクセスできることをシステムが検証していることを示します。

CMK 設定には、次の 4 つのステータスがあります。 次の 3 つがあります。

* 手順 1:Experience Platformがキーと Key Vault にアクセスできることを検証します。
* 手順 2: Key Vault とキー名は、組織全体のすべてのデータストアに追加する処理中です。
* 手順 3: Key Vault とキー名がデータストアに正常に追加された。
* `FAILED`：問題が発生しました。主にキー、Key Vault、またはマルチテナントのアプリ設定に関連しています。

## 次の手順

上記の手順を完了すると、Azure でホストされる組織の CMK が正常に有効になります。 プライマリデータストアに取り込まれたデータは、[!DNL Azure] Key Vault のキーを使用して暗号化および復号化されるようになりました。

Azure にホストされている組織に対して CMK を有効にした後、キーの使用状況を監視し、セキュリティを強化するためにキーローテーションポリシーを実装し、組織のポリシーに準拠していることを確認します。
