---
keywords: Experience Platform；ホーム；人気の高いトピック；Veeva CRM;veeva
solution: Experience Platform
title: UI での Veeva CRM ソース接続の作成
topic-legacy: overview
type: Tutorial
description: Adobe Experience Platform UI を使用して Veeva CRM ソース接続を作成する方法を説明します。
exl-id: 4ef76c28-9bd2-4e54-a3d6-dceb89162337
source-git-commit: ea20a850a5d83f648c699119913aa31e2ea16233
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 78%

---

# UI での [!DNL Veeva CRM] ソース接続の作成

Adobe Experience Platform のソースコネクタには、外部ソースの CRM データを設定したスケジュールに従って取り込む機能が用意されています。 このチュートリアルでは、[!DNL Platform] のユーザーインターフェイスを使用して [!DNL Veeva CRM] ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL Veeva CRM] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/crm.md)に関するチュートリアルに進んでください。

### 必要な認証情報の収集

| 認証情報 | 説明 |
| ---------- | ----------- |
| `environmentUrl` | の URL [!DNL Veeva CRM] ソースインスタンス。 |
| `username` | のユーザー名 [!DNL Veeva CRM] ユーザーアカウント。 |
| `password` | のパスワード [!DNL Veeva CRM] ユーザーアカウント。 |
| `securityToken` | のセキュリティトークン [!DNL Veeva CRM] ユーザーアカウント。 |

導入の詳細については、 [[!DNL Veeva CRM] 文書](https://developer.veevacrm.com/doc/Content/rest-api.htm).

## [!DNL Veeva CRM] アカウントの接続

必要な認証情報が揃ったら、次の手順に従って、[!DNL Veeva CRM] アカウントを [!DNL Platform] にリンクします。

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 [!UICONTROL CRM] カテゴリ、選択 **[!UICONTROL Veeva CRM]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/veeva/catalog.png)

この **[!UICONTROL Veeva CRM アカウントの接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL Veeva CRM] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![既存](../../../../images/tutorials/create/veeva/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、[!DNL Veeva CRM] の認証情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../../../../images/tutorials/create/veeva/new.png)

## 次の手順

このチュートリアルでは、[!DNL Veeva CRM] アカウントとの接続を確立しました。次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](../../dataflow/crm.md)を行いましょう。
