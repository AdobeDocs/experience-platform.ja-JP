---
title: Adobe Experience Platform Assurance拡張機能の実装
description: このガイドでは、Adobe Experience Platform Assurance拡張機能の実装およびインストール方法を説明します。
exl-id: b7bd1bb1-1606-4d00-97e0-c329c86d8ca4
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 1%

---

# Adobe Experience Platform Assurance拡張機能の実装

このチュートリアルでは、モバイル SDKにExperience Platform Assurance拡張機能をインストールして実装する方法について説明します。 Assurance拡張機能をアプリケーションに追加する方法については、[Adobe Experience Platform Assurance拡張機能の概要 ](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app) を参照してください。

## はじめに

Assurance拡張機能をインストールして実装するには、次のサービスにアクセスする必要があります。

- [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform Assurance](https://experience.adobe.com/assurance)

## モバイルプロパティの作成

>[!NOTE]
>
>既にモバイルプロパティがある場合は、次の手順に進むことができます。

データ収集 UI で、「**[!UICONTROL タグ]**」を選択します。 モバイルおよび web プロパティのリストが表示され、組織に属するプロパティに関する情報が表示されます。 **[!UICONTROL 新しいプロパティ]** を選択して、新しいプロパティを作成します。

![ 「新しいプロパティ」ボタンがハイライト表示され、新しいプロパティを作成するために選択した項目が示されている様子 ](./images/implement-assurance/create-new-property.png)

**[!UICONTROL プロパティを作成]** ページが表示されます。 新しいプロパティの名前を入力し、プラットフォームとして **[!UICONTROL モバイル]** を選択します。 詳細を挿入したら、「**[!UICONTROL 保存]**」を選択してモバイルプロパティを作成します。

>[!NOTE]
>
>モバイルプロパティの **[!UICONTROL プライバシー]** 設定は、Assuranceのデータ収集に影響 **与えません**。

![ プロパティを作成ページが表示されます。 ここにモバイルプロパティに関する情報を挿入できます。](./images/implement-assurance/create-property.png)

## Assurance拡張機能のインストール

Assurance拡張機能をインストールするモバイルプロパティを選択します。

![ 選択したモバイルプロパティがハイライト表示されたタグプロパティページが表示されます。](./images/implement-assurance/select-mobile-property.png)

**モバイルプロパティの詳細** ページが表示されます。 **[!UICONTROL 拡張機能]** を選択して、モバイルプロパティに現在関連付けられている拡張機能のリストを表示します。

![ モバイルプロパティの詳細ページが表示されます。 最近のアクティビティに関する情報が表示されます。 「拡張機能」タブがハイライト表示されている様子 ](./images/implement-assurance/tag-properties.png)

「**[!UICONTROL カタログ]**」を選択して、モバイルプロパティに追加できる拡張機能のリストを表示します。 フィルターを使用して、**[!UICONTROL AEP Assurance]** 拡張機能を見つけて、「**[!UICONTROL インストール]**」を選択します。

![ 拡張機能カタログが表示されます。 Assurance拡張機能がにフィルタリングされ、表示されます。「インストール」ボタンがハイライト表示されます。](./images/implement-assurance/assurance-extension.png)

## 次の手順

これで、モバイルプロパティにAssurance拡張機能をインストールしたので、アプリケーション内でAssuranceの使用を開始できます。 Assurance拡張機能をアプリケーションに追加する方法については、[Adobe Experience Platform Assurance拡張機能の概要 ](https://developer.adobe.com/client-sdks/documentation/platform-assurance-sdk/#add-the-aep-assurance-extension-to-your-app) を参照してください。 Assuranceの使用方法については、[Assuranceの使用ガイド ](./using-assurance.md) を参照してください。
