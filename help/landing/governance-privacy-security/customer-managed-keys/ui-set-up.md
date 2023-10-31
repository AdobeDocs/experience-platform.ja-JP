---
title: Platform UI を使用した顧客管理キーの設定
description: Azure テナントで CMK アプリを設定し、暗号化キー ID をAdobe Experience Platformに送信する方法を説明します。
exl-id: 5f38997a-66f3-4f9d-9c2f-fb70266ec0a6
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 22%

---

# Platform UI を使用した顧客管理キーの設定

このドキュメントでは、UI を使用して Platform で顧客管理キー (CMK) 機能を有効にするプロセスについて説明します。 API を使用してこのプロセスを完了する手順については、 [API CMK セットアップドキュメント](./api-set-up.md).

## 前提条件

を表示して、 [!UICONTROL 暗号化] Adobe Experience Platformのセクションで、役割を作成し、 [!UICONTROL 顧客管理キーの管理] その役割に対する権限。 次の項目を持つユーザー [!UICONTROL 顧客管理キーの管理] 権限により、組織で CMK を有効にすることができます。

Experience Platformでの役割および権限の割り当てについて詳しくは、 [権限に関するドキュメントの設定](https://experienceleague.adobe.com/docs/platform-learn/getting-started-for-data-architects-and-data-engineers/configure-permissions.html?lang=ja).

CMK を有効にするには、 [[!DNL Azure] Key Vault を設定する必要があります](./azure-key-vault-config.md) を次の設定で使用します。

* [消去保護を有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview#purge-protection)
* [ソフトデリートを有効にする](https://learn.microsoft.com/en-us/azure/key-vault/general/soft-delete-overview)
* [次を使用してアクセスを設定 [!DNL Azure] ロールベースのアクセス制御](https://learn.microsoft.com/en-us/azure/role-based-access-control/)
* [ [!DNL Azure]  Key Vault の設定](./azure-key-vault-config.md)

## CMK アプリのセットアップ {#register-app}

キー Vault を設定したら、次の手順で、次に、 [!DNL Azure] テナント。

### はじめに

次の手順で、 [!UICONTROL 暗号化設定] ダッシュボード、選択 **[!UICONTROL 暗号化]** の下に [!UICONTROL 管理] 左側のナビゲーションサイドバーの見出し。

![暗号化設定ダッシュボード（暗号化と顧客管理キーカードがハイライト表示されています）。](../../images/governance-privacy-security/customer-managed-keys/encryption-configraion.png)

選択 **[!UICONTROL 設定]** 開く [!UICONTROL 顧客管理キーの設定] 表示。 このワークスペースには、以下の手順を完了し、Azure Key Vault との統合を実行するために必要なすべての値が含まれています。

### 認証 URL をコピー {#copy-authentication-url}

登録プロセスを開始するには、組織のアプリケーション認証 URL を [!UICONTROL 顧客管理キーの設定] 表示して、 [!DNL Azure] 環境 **[!DNL Key Vault Crypto Service Encryption User]**. 方法の詳細 [役割を割り当てる](#assign-to-role) は次の節で提供されます。

コピーアイコン (![コピーアイコン。](../../images/governance-privacy-security/customer-managed-keys/copy-icon.png)) を [!UICONTROL アプリケーション認証 URL].

![The [!UICONTROL 顧客管理キーの設定] 「アプリケーション認証 url 」セクションをハイライト表示した状態で表示します。](../../images/governance-privacy-security/customer-managed-keys/application-authentication-url.png)

をコピーして貼り付けます。 [!UICONTROL アプリケーション認証 URL] をブラウザーにドラッグして、認証ダイアログを開きます。 **[!DNL Accept]** を選択して、CMK アプリサービスプリンシパルを [!DNL Azure] テナントに追加します。認証を確認すると、Experience Cloudのランディングページにリダイレクトされます。

![Microsoft権限リクエストダイアログ [!UICONTROL 確定] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/app-permission.png)

>[!IMPORTANT]
>
>複数の [!DNL Microsoft Azure] 購読すると、Platform インスタンスを誤ったキー Vault に接続する可能性があります。 この場合、 `common` CMK ディレクトリ ID のアプリケーション認証 URL 名のセクション。<br>CMK ディレクトリ ID を、 [!DNL Microsoft Azure] アプリ<br>![The [!DNL Microsoft Azure] ディレクトリ ID が強調表示されたアプリケーションポータル設定、ディレクトリと購読ページ。](../../images/governance-privacy-security/customer-managed-keys/directory-id.png)<br>次に、ブラウザーのアドレスバーに貼り付けます。<br>![アプリケーション認証 URL の「共通」セクションがハイライトされたGoogleブラウザーページ。](../../images/governance-privacy-security/customer-managed-keys/common-url-section.png)

### CMK アプリを役割に割り当てます。 {#assign-to-role}

認証プロセスが完了したら、[!DNL Azure] Key Vault に戻り、左側のナビゲーションで **[!DNL Access control]** を選択します。 ここから **[!DNL Add]** を選択し、続けて **[!DNL Add role assignment]** を選択します。

![The [!DNL Microsoft Azure] 次のダッシュボード [!DNL Add] および [!DNL Add role assignment] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/add-role-assignment.png)

次の画面では、この割り当ての役割を選択するように求められます。**[!DNL Key Vault Crypto Service Encryption User]** を選択してから **[!DNL Next]** を選択し、続行します。

![The [!DNL Microsoft Azure] ダッシュボードと [!DNL Key Vault Crypto Service Encryption User] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/select-role.png)

次の画面で、「**[!DNL Select members]**」 を選択して、右側のパネルでダイアログを開きます。 検索バーを使用して CMK アプリケーションのサービスプリンシパルを見つけ、リストから選択します。 終了したら「**[!DNL Save]**」を選択します。

>[!NOTE]
>
>リストにアプリケーションが見つからない場合は、サービスプリンシパルがテナントに受け入れられていません。 正しい権限を持っていることを確認するには、 [!DNL Azure] 管理者または担当者。

アプリケーションを検証するには、 [!UICONTROL アプリケーション ID] 指定日： [!UICONTROL 顧客管理キーの設定] 表示を [!DNL Application ID] 指定日： [!DNL Microsoft Azure] アプリケーションの概要。

![The [!UICONTROL 顧客管理キーの設定] 表示を [!UICONTROL アプリケーション ID] ハイライト表示されました。](../../images/governance-privacy-security/customer-managed-keys/application-id.png)

Azure ツールの検証に必要な詳細は、すべて Platform UI に含まれています。 このレベルの精度は、他の Azure ツールを使用して、これらのアプリケーションの主要なヴォールトへのアクセスを監視およびログに記録する機能を強化したいと考える多くのユーザーに対して提供されます。 これらの識別子を理解することは、その目的で重要で、Adobe サービスが鍵にアクセスするのに役立ちます。

## Experience Platform の暗号化キー設定を有効にする {#send-to-adobe}

CMK アプリを [!DNL Azure] にインストールすると、暗号化キー識別子をアドビに送信できます。 左側のナビゲーションで「**[!DNL Keys]**」を選択し、次に送信するキーの名前を選択します。

![Microsoft Azure ダッシュボードと [!DNL Keys] オブジェクトと、ハイライト表示されたキー名。](../../images/governance-privacy-security/customer-managed-keys/select-key.png)

キーの最新バージョンを選択すると、その詳細ページが表示されます。ここから、オプションでキーに対して許可する操作を設定できます。

>[!IMPORTANT]
>
>キーに対して許可する必要がある最小の操作は、 **[!DNL Wrap Key]** および **[!DNL Unwrap Key]** 権限。 以下を含めることができます。 [!DNL Encrypt], [!DNL Decrypt], [!DNL Sign]、および [!DNL Verify] 必要に応じて、

「**[!UICONTROL キー識別子]**」フィールドには、キーの URI 識別子が表示されます。次の手順で使用するために、この URI 値をコピーします。

![Microsoft Azure ダッシュボードのキーの詳細と [!DNL Permitted operations] と、ハイライト表示されているキー URL セクションをコピーします。](../../images/governance-privacy-security/customer-managed-keys/copy-key-url.png)

手に入れたら [!DNL Key vault URI]、に戻ります。 [!UICONTROL 顧客管理キーの設定] 説明的な **[!UICONTROL 設定名]**. 次に、 [!DNL Key Identifier] Azure キーの詳細ページから、 **[!UICONTROL キー Vault キー識別子]** を選択し、 **[!UICONTROL 保存]**.

![The [!UICONTROL 顧客管理キーの設定] 表示を [!UICONTROL 設定名] そして [!UICONTROL キー Vault キー識別子] 強調表示されたセクション。](../../images/governance-privacy-security/customer-managed-keys/configuration-name.png)

次の場所に戻ります。 [!UICONTROL 暗号化設定ダッシュボード]. のステータス [!UICONTROL 顧客管理キー] 設定は次のように表示されます [!UICONTROL 処理中].

![The [!UICONTROL 暗号化設定] 次のダッシュボード [!UICONTROL 処理中] ～で強調表示される [!UICONTROL 顧客管理キー] カード。](../../images/governance-privacy-security/customer-managed-keys/processing.png)

## 設定のステータスの確認 {#check-status}

処理にかなりの時間をかけます。 設定のステータスを確認するには、 [!UICONTROL 顧客管理キーの設定] 表示し、下にスクロールして [!UICONTROL 設定ステータス]. プログレスバーは、3 つの手順の 1 つに進み、Platform がキーとキーの Vault にアクセスできることを検証していることを説明します。

CMK 設定には、4 つの潜在的なステータスがあります。 次の 3 つがあります。

* 手順 1:Platform がキーとキーの Vault にアクセスできるかを検証します。
* 手順 2：キー Vault とキー名は、組織全体のすべてのデータストアに追加される処理中です。
* 手順 3：キー Vault とキー名がデータストアに正常に追加されました。
* `FAILED`：問題が発生しました。主にキー、Key Vault、またはマルチテナントのアプリ設定に関連しています。

## 次の手順

上記の手順を完了すると、組織で CMK が正常に有効になります。 プライマリデータストアに取り込まれたデータは、暗号化され、 [!DNL Azure] キー Vault。
