---
keywords: 宛先アカウント、宛先アカウント、アカウントの更新方法、宛先の更新
title: 宛先アカウントの更新
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platform UI で宛先アカウントを更新する手順を示します
exl-id: afb41878-4205-4c64-af4d-e2740f852785
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 10%

---

# 宛先アカウントの更新

## 概要 {#overview}

「**[!UICONTROL アカウント]**」タブには、様々な宛先との接続を確立した場合の詳細が表示されます。 各宛先アカウントについて取得できるすべての情報については、[ アカウントの概要 ](../ui/destinations-workspace.md#accounts) を参照してください。

このチュートリアルでは、Experience PlatformUI を使用して、宛先アカウントの詳細を更新する手順を説明します。

宛先アカウントの詳細を更新して、現在使用している宛先の現在のアカウントまたは期限切れのアカウントの資格情報を更新し、再認証することができます。 通常、OAuth トークンとベアラートークンの有効期間は、宛先プラットフォームに応じて制限されています。 これらのトークンの有効期限が切れたら、後述するワークフローで更新できます。 このワークフローでは、OAuth ワークフローを確認するか、トークンを再挿入します。 同様に、ダウンストリームプラットフォームでパスワードまたはユーザーアクセスが変更された場合は、資格情報を更新できます。

バッチ宛先の場合、アクセスキーまたは秘密鍵が変更されていれば、それらを更新できます。 さらに、今後ファイルを暗号化する場合は、RSA 公開鍵を挿入すると、書き出されたファイルが暗号化されます。

![「アカウント」タブ](../assets/ui/update-accounts/destination-accounts.png)

## アカウントの更新 {#update}

既存の宛先への接続の詳細を更新するには、次の手順に従います。

1. [Experience Platform UI](https://platform.adobe.com/) にログインし、左側のナビゲーションバーから「**[!UICONTROL 宛先]**」を選択します。上部のヘッダーから **[!UICONTROL アカウント]** を選択して、既存のアカウントを表示します。

   ![「アカウント」タブ](../assets/ui/update-accounts/accounts-tab.png)

2. 左上のフィルターアイコン ![フィルターアイコン](../assets/ui/update-accounts/filter.png) を選択して、並べ替えパネルを開きます。並べ替えパネルには、すべての宛先のリストが表示されます。 リストから複数の宛先を選択して、選択した宛先に関連付けられたフィルターされたアカウントの選択を表示できます。

   ![ 宛先アカウントのフィルタリング ](../assets/ui/update-accounts/filter-accounts.png)

3. 更新するアカウント名の横にある省略記号（`...`）を選択します。 ポップアップパネルが表示され、アカウントの **[!UICONTROL オーディエンスをアクティブ化]**、**[!UICONTROL 詳細を編集]**、および **[!UICONTROL 削除]** するオプションが提供されます。 「![ 詳細を編集ボタン ](../assets/ui/workspace/pencil-icon.png)**[!UICONTROL 詳細を編集]** ボタンを選択して、アカウント情報を編集します。

   ![ アカウントを編集 ](../assets/ui/update-accounts/accounts-edit.png)

4. 更新したアカウント資格情報を入力します。

   * `OAuth1` または `OAuth2` の接続タイプを使用するアカウントの場合は、「**[!UICONTROL OAuth に再接続]**」を選択してアカウント資格情報を更新します。 また、アカウントの名前と説明を更新することもできます。

   ![ 詳細 OAuth を編集 ](../assets/ui/update-accounts/edit-details-oauth.png)

   * `Access Key` または `ConnectionString` の接続タイプを使用するアカウントの場合、アクセス ID、秘密鍵、接続文字列などの情報を含むアカウント認証情報を編集できます。 また、アカウントの名前と説明を更新することもできます。

   ![ 詳細アクセスキーを編集 ](../assets/ui/update-accounts/edit-details-key.png)

   * `Bearer token` 接続タイプを使用するアカウントの場合、必要に応じて、新しいベアラートークンを入力できます。 また、アカウントの名前と説明を更新することもできます。

   ![ 詳細ベアラートークンを編集 ](../assets/ui/update-accounts/edit-details-bearer.png)

   * `Server to server` 接続タイプを使用するアカウントの場合、アカウントの名前と説明を更新できます。

   ![ 詳細をサーバー間で編集 ](../assets/ui/update-accounts/edit-details-s2s.png)

5. 「**[!UICONTROL 保存]**」を選択して、アカウントの詳細の更新を終了します。

## 次の手順

このチュートリアルでは、**[!UICONTROL destinations]** ワークスペースを使用して既存のアカウントを正常に更新しました。

宛先について詳しくは、[ 宛先の概要 ](../catalog/overview.md) を参照してください。