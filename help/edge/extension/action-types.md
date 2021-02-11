---
title: プラットフォームWeb SDK拡張機能のアクションタイプ
description: Adobe Experience Platform LaunchのAdobe Experience PlatformWeb SDK拡張アクションタイプ
translation-type: tm+mt
source-git-commit: 473cc1f7617f1d65cdb70ff0e758178ea0174f00
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 54%

---


# アクションタイプ

[Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/launch.html)用の[Adobe Experience PlatformWeb SDK拡張機能](web-sdk-extension.md)を設定した後、アクションタイプを設定します。

このページでは、使用可能なアクションのタイプについて説明します。

## イベントの送信

Adobe Experience Platformが送信したデータを収集し、その情報に基づいて行動できるように、Adobe[!DNL Experience Platform]にイベントを送信します。 インスタンスを選択する必要があります（複数ある場合）。ページの読み込みの開始時、または単一ページアプリでの表示の変更時にイベントが発生した場合は、「**[!UICONTROL 表示の開始時に発生します]**」を選択します。

**[!UICONTROL XDM Data]**&#x200B;フィールドには、任意の送信データを格納できます。 XDM スキーマの構造に準拠する JSON オブジェクトを指定する必要があります。このオブジェクトは、ページ上に作成するか、**[!UICONTROL カスタムコード]** **[!UICONTROL データ要素]**&#x200B;を使用して作成できます。

## 同意の設定

ユーザーの同意を得たら、この情報をAdobe Experience PlatformWeb SDKに通信する必要があります。 それには、「同意の設定」アクションタイプを使用します。現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。Adobe 標準を使用する場合は、同意を「イン」または「アウト」に設定するか、データ要素を使用して指定できます。IAB TCF 標準を使用する場合は、使用するバージョンと値、および GDPR に関する追加情報を提供します。

このアクションでは、ID マップを含めるためのオプションのフィールドも提供されるので、同意を受けた後に ID を同期することができます。同意の呼び出しが最初に実行される可能性があるので、この設定は同意が「保留」に設定されている場合に便利です。

## イベント結合 ID のリセット

ページ上のイベント結合 ID をリセットする場合は、このアクションで実行できます。ID をリセットするには、リセットする結合 ID を選択し、必要に応じてアクションを実行する必要があります。

## 次の作業

アクションの種類を設定した後、[データ要素の種類](data-element-types.md)を設定します。