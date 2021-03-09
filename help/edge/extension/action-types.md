---
title: Adobe Experience PlatformWeb SDK Extensionのアクションタイプ
description: Adobe Experience Platform LaunchのAdobe Experience PlatformWeb SDK Extensionが提供する様々なアクションタイプについて説明します。
translation-type: tm+mt
source-git-commit: ff261c507d310b8132912680b6ddd1e7d5675d08
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 6%

---


# アクションタイプ

[Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/launch.html)用の[Adobe Experience PlatformWeb SDK拡張機能](web-sdk-extension.md)を設定した後、アクションタイプを設定します。

このページでは、使用可能なアクションのタイプについて説明します。

## イベントの送信

Adobe Experience Platformが送信したデータを収集し、その情報に基づいて行動できるように、Adobe[!DNL Experience Platform]にイベントを送信します。 インスタンスを選択します（複数ある場合）。 ページの読み込みの開始時、または単一ページアプリでの表示の変更時にイベントが発生した場合は、「**[!UICONTROL 表示の開始時に発生します]**」を選択します。

**[!UICONTROL XDM Data]**&#x200B;フィールドには、任意の送信データを格納できます。 XDMスキーマの構造に準拠するJSONオブジェクトを使用します。 このオブジェクトは、ページ上に作成するか、**[!UICONTROL カスタムコード]** **[!UICONTROL データ要素]**&#x200B;を使用して作成できます。

## 同意の設定

ユーザーから同意を受けた後、この同意は、「同意の設定」アクションタイプを使用して、Adobe Experience PlatformWeb SDKに伝える必要があります。 現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。「[顧客の同意に関する基本設定のサポート](../consent/supporting-consent.md)」を参照してください。 Adobeバージョン2.0を使用する場合は、データ要素の値のみがサポートされます。 同意オブジェクトに解決されるデータ要素を作成する必要があります。

この操作では、承認を受けた後でIDを同期できるように、IDマップを含めるオプションのフィールドも提供されます。 同期は、同意が「保留」または「送信」として設定されている場合に便利です。同意の呼び出しが最初に実行される呼び出しである可能性が高いからです。

## イベント結合 ID のリセット

ページ上のイベント結合IDをリセットする場合は、この操作を行って行います。 IDをリセットするには、リセットするマージIDを選択し、必要に応じてアクションを実行します。

## 次の作業

アクションの種類を設定した後、[データ要素の種類](data-element-types.md)を設定します。