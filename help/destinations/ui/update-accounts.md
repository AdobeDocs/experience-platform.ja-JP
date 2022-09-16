---
keywords: 宛先アカウントの更新、宛先アカウント、アカウントの更新、宛先の更新
title: 宛先アカウントの更新
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform UI で宛先アカウントを更新する手順を示します
exl-id: afb41878-4205-4c64-af4d-e2740f852785
source-git-commit: f31b54622c63f96fb8fa727f80dda295a926e2c7
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 3%

---

# 宛先アカウントの更新

## 概要 {#overview}

この **[!UICONTROL アカウント]** 「 」タブには、様々な宛先との接続を確立した場合の詳細が表示されます。 詳しくは、 [アカウントの概要](../ui/destinations-workspace.md#accounts) を参照してください。

このチュートリアルでは、Experience PlatformUI を使用して宛先アカウントの詳細を更新する手順を説明します。

宛先アカウントの詳細を更新して、現在使用している宛先の現在または期限切れのアカウントの資格情報を更新し、再認証できます。 通常、OAuth および bearer トークンの有効期間は、宛先プラットフォームに応じて制限されます。 これらのトークンが期限切れになったら、後述のワークフローで更新できます。 このワークフローは、OAuth ワークフローを進むか、トークンを再挿入するように指示します。 同様に、ダウンストリームプラットフォームでパスワードまたはユーザーアクセスが変更された場合は、資格情報を更新できます。

バッチ保存先の場合、アクセスまたは秘密鍵のいずれかが変更されている場合は更新できます。 また、今後のファイルを暗号化する場合は、RSA 公開鍵を挿入すると、書き出したファイルは暗号化されます。

![「アカウント」タブ](../assets/ui/update-accounts/destination-accounts.png)

## アカウントの更新 {#update}

既存の宛先への接続の詳細を更新するには、以下の手順に従います。

1. にログインします。 [Experience PlatformUI](https://platform.adobe.com/) を選択し、 **[!UICONTROL 宛先]** をクリックします。 選択 **[!UICONTROL アカウント]** 上部のヘッダーから既存のアカウントを表示します。

   ![「アカウント」タブ](../assets/ui/update-accounts/accounts-tab.png)

2. フィルターアイコンを選択します。 ![フィルターアイコン](../assets/ui/update-accounts/filter.png) をクリックして、並べ替えパネルを起動します。 並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択すると、選択した宛先に関連付けられたアカウントのフィルターされた選択を表示できます。

   ![宛先アカウントのフィルター](../assets/ui/update-accounts/filter-accounts.png)

3. 省略記号 (`...`) をクリックします。 ポップアップパネルが表示され、次のオプションが表示されます。 **[!UICONTROL セグメントのアクティブ化]**, **[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]** アカウント を選択します。 ![「詳細を編集」ボタン](../assets/ui/workspace/pencil-icon.png) **[!UICONTROL 詳細を編集]** ボタンをクリックして、アカウント情報を編集します。

   ![アカウントを編集](../assets/ui/update-accounts/accounts-edit.png)

4. 更新されたアカウント資格情報を入力します。

   * を使用するアカウントの場合、 `OAuth1` または `OAuth2` 接続タイプ、選択 **[!UICONTROL OAuth を再接続]** をクリックして、アカウントの資格情報を更新します。 また、アカウントの名前と説明を更新することもできます。

   ![詳細 OAuth を編集](../assets/ui/update-accounts/edit-details-oauth.png)

   * を使用するアカウントの場合、 `Access Key` または `ConnectionString` 接続タイプを使用すると、アクセス ID、秘密鍵、接続文字列などの情報を含むアカウント認証情報を編集できます。 また、アカウントの名前と説明を更新することもできます。

   ![詳細アクセスキーを編集](../assets/ui/update-accounts/edit-details-key.png)

   * を使用するアカウントの場合、 `Bearer token` 接続タイプを指定する場合、必要に応じて新しい bearer トークンを入力できます。 また、アカウントの名前と説明を更新することもできます。

   ![詳細 Bearer トークンを編集](../assets/ui/update-accounts/edit-details-bearer.png)

   * を使用するアカウントの場合、 `Server to server` 接続タイプを指定する場合は、アカウントの名前と説明を更新できます。

   ![詳細を編集サーバー間](../assets/ui/update-accounts/edit-details-s2s.png)

5. 選択 **[!UICONTROL 保存]** をクリックして、アカウントの詳細の更新を完了します。

## 次の手順

このチュートリアルでは、 **[!UICONTROL 宛先]** ワークスペースを使用して、既存のアカウントを更新します。

宛先について詳しくは、 [宛先の概要](../catalog/overview.md).