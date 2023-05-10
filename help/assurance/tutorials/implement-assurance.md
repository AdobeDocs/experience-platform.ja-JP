---
title: Adobe Experience Platform Assurance 拡張機能の実装
description: このガイドでは、Adobe Experience Platform Assurance 拡張機能の実装およびインストール方法を説明します。
exl-id: b7bd1bb1-1606-4d00-97e0-c329c86d8ca4
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---

# Adobe Experience Platform Assurance 拡張機能の実装

このチュートリアルでは、Mobile SDK で Platform Assurance 拡張機能をインストールして実装する方法を説明します。 Assurance 拡張機能をアプリケーションに追加する手順については、 [Adobe Experience Platform Assurance 拡張機能の概要](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app).

## はじめに

Assurance 拡張機能をインストールして実装するには、次のサービスにアクセスする必要があります。

- この [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform Assurance](https://experience.adobe.com/assurance)

## モバイル プロパティの作成

>[!NOTE]
>
>既にモバイルプロパティがある場合は、次の手順に進むことができます。

データ収集 UI で、「 」を選択します。 **[!UICONTROL タグ]**. モバイルプロパティと Web プロパティのリストが表示され、組織に属するプロパティに関する情報が表示されます。 選択 **[!UICONTROL 新しいプロパティ]** をクリックして新しいプロパティを作成します。

![「新規プロパティ」ボタンがハイライト表示され、新しいプロパティを作成するために選択した内容が表示されます](./images/implement-assurance/create-new-property.png)

この **[!UICONTROL プロパティを作成]** ページが表示されます。 新しいプロパティの名前を入力し、「 」を選択します。 **[!UICONTROL モバイル]** をプラットフォームとして使用する。 詳細を挿入した後、 **[!UICONTROL 保存]** をクリックしてモバイルプロパティを作成します。

>[!NOTE]
>
>モバイルプロパティの **[!UICONTROL プライバシー]** 設定 **not** アシュランスのデータ収集に影響を与える。

![「プロパティを作成」ページが表示されます。 モバイルプロパティに関する情報は、ここに挿入できます。](./images/implement-assurance/create-property.png)

## Assurance 拡張機能のインストール

Assurance 拡張機能をインストールするモバイルプロパティを選択します。

![タグのプロパティページが表示され、選択したモバイルプロパティがハイライト表示されます。](./images/implement-assurance/select-mobile-property.png)

この **モバイルプロパティの詳細** ページが表示されます。 選択 **[!UICONTROL 拡張機能]** をクリックして、現在モバイルプロパティに関連付けられている拡張機能のリストを表示します。

![モバイルプロパティの詳細ページが表示されます。 最近のアクティビティに関する情報が表示されます。 「拡張機能」タブがハイライト表示されます。](./images/implement-assurance/tag-properties.png)

選択 **[!UICONTROL カタログ]** を参照して、モバイルプロパティに追加できる拡張機能のリストを確認します。 フィルターを使用して、 **[!UICONTROL AEP アシュランス]** 拡張機能と選択 **[!UICONTROL インストール]**.

![拡張機能カタログが表示されます。 Assurance 拡張機能は、インストールボタンがハイライト表示された状態で、フィルタリングされて表示されます。](./images/implement-assurance/assurance-extension.png)

## 次の手順

これで、モバイルプロパティに Assurance 拡張機能がインストールされ、アプリケーション内で Assurance の使用を開始できます。 Assurance 拡張機能をアプリケーションに追加する方法については、 [Adobe Experience Platform Assurance 拡張機能の概要](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app). アシュランスの使用方法については、 [アシュランスガイドの使用](./using-assurance.md).
