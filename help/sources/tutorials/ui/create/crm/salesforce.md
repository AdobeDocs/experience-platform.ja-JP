---
title: '[Experience Platform] ユーザインターフェイスを使用して Salesforce アカウントに接続'
description: Salesforce アカウントを接続し、ユーザーインターフェイスを使用して CRM データをExperience Platformに取り込む方法を説明します。
exl-id: b67fa4c4-d8ff-4d2d-aa76-5d9d32aa22d6
source-git-commit: 57cdcbd5018e7f57261f09c6bddf5e2a8dcfd0d5
workflow-type: tm+mt
source-wordcount: '524'
ht-degree: 31%

---

# 接続する [!DNL Salesforce] UI を使用してExperience Platformにアカウント

このチュートリアルでは、 [!DNL Salesforce] アカウントユーザーインターフェイスを使用して、CRM データをAdobe Experience PlatformにExperience Platformします。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL Salesforce] アカウントを使用する場合は、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください： [CRM データのデータフローの設定](../../dataflow/crm.md).

### 必要な資格情報の収集 {#gather-required-credentials}

を認証するために、 [!DNL Salesforce] Experience Platformを考慮するには、次に対応する値を指定する必要があります [!DNL Salesforce] 資格情報：

| 資格情報 | 説明 |
| --- | --- |
| `environmentUrl` | の URL [!DNL Salesforce] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Salesforce] ユーザーアカウント。 |
| `password` | のパスワード [!DNL Salesforce] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Salesforce] ユーザーアカウント。 |
| `apiVersion` | （オプション） [!DNL Salesforce] 使用しているインスタンス。 このフィールドを空白のままにすると、Experience Platformは利用可能な最新バージョンを自動的に使用します。 |

認証について詳しくは、 [この [!DNL Salesforce] 認証ガイド](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/quickstart_oauth.htm).

必要な資格情報を収集したら、次の手順に従って、 [!DNL Salesforce] アカウントからExperience Platformへ。

## [!DNL Salesforce] アカウントを接続

Platform UI で、「 」を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションからソースワークスペースにアクセスします。 The *[!UICONTROL カタログ]* 画面には、「ソース」カタログで使用可能な様々なソースがExperience Platformされます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索オプションを使用して特定のソースを検索できます。

選択 **[!UICONTROL CRM]** ソースカテゴリのリストから、「 」を選択します。 **[!UICONTROL データを追加]** から [!DNL Salesforce] カード。

![Salesforce ソースカードが選択されたExperience PlatformUI 上のソースカタログ。](../../../../images/tutorials/create/salesforce/catalog.png)

The **[!UICONTROL Salesforce に連携]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

>[!BEGINTABS]

>[!TAB 既存の Salesforce アカウントを使用]

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択し、表示されるリストから使用するアカウントを選択します。終了したら、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

![組織に既に存在する認証済み Salesforce アカウントのリスト。](../../../../images/tutorials/create/salesforce/existing.png)

>[!TAB 新しい Salesforce アカウントを作成]

新しいアカウントを使用するには、 **[!UICONTROL 新しいアカウント]** 名前、説明、 [!DNL Salesforce] 認証資格情報。 終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** そして、新しい接続が確立されるまで数秒間待ちます。

![適切な認証資格情報を指定して新しい Salesforce アカウントを作成できるインターフェイス。](../../../../images/tutorials/create/salesforce/new.png)

>[!ENDTABS]

## 次の手順

このチュートリアルでは、[!DNL Salesforce] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを に取り込むためのデータフローの設定 [!DNL Platform]](../../dataflow/crm.md)を行いましょう。
