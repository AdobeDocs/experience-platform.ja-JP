---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: アラート UI ガイド
description: Experience Platform のユーザーインターフェイスでアラートを管理する方法を説明します。
feature: Alerts
exl-id: 4ba3ef2b-7394-405e-979d-0e5e1fe676f3
source-git-commit: 8d63e9fa4c7eb09ffb90edca612a6e6d44dd18fa
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 40%

---

# アラート UI ガイド

Adobe Experience Platform ユーザーインターフェイスを使用すると、Adobe Experience Platform の Observability Insights で表示される指標に基づいて、受信したアラートの履歴を表示できます。 また、UI を使用すると、使用可能なアラートルールの表示、有効化、無効化、登録をおこなうこともできます。

>[!NOTE]
>
>Experience Platform のアラートの概要については、[アラートの概要](./overview.md)を参照してください。

開始するには、左側のナビゲーションで「**[!UICONTROL アラート]**」を選択します。

![アラートページのハイライト [!UICONTROL アラート] をクリックします。](../images/alerts/ui/workspace.png)

## アラートルールの管理

「**[!UICONTROL 参照]**」タブには、アラートをトリガーする可能性のある使用可能なルールが一覧表示されます。

![使用可能なアラートのリストは、 [!UICONTROL 参照] タブをクリックします。](../images/alerts/ui/rules.png)

リストからルールを選択し、その説明と設定パラメーター（しきい値や重大度を含む）を右側のパネルに表示します。

![右側のパネルに詳細を示すアラートルールがハイライトされています。](../images/alerts/ui/rule-details.png)

ルール名の横にある省略記号（**...**）を選択すると、ドロップダウンにアラートの有効／無効（現在のステータスに応じて異なる）と、アラートのメール通知の登録／登録解除を切り替えるためのコントロールが表示されます。

![選択した省略記号にドロップダウンメニューが表示されます。](../images/alerts/ui/disable-subscribe.png)

## アラート購読者の管理

>[!NOTE]
>
> アラートをAdobeユーザー ID、外部の電子メールアドレス、または電子メールグループリストに割り当てるには、管理者である必要があります。

「**[!UICONTROL 参照]**」タブには、アラートをトリガーする可能性のある使用可能なルールが一覧表示されます。

![使用可能なアラートルールのリスト ( [!UICONTROL 参照] タブをクリックします。](../images/alerts/ui/rules.png)

省略記号 (**...**) をクリックし、ルール名の横にコントロールを表示するドロップダウンが表示されます。 選択 **[!UICONTROL アラート購読者の管理]**.

![省略記号を選択してドロップダウンメニューを表示します。 The [!UICONTROL アラート購読者の管理] オプションがハイライト表示されます。](../images/alerts/ui/manage-alert-subscribers.png)

The [!UICONTROL アラート購読者の管理] ページが表示されます。 特定のユーザーに通知を割り当てるには、Adobeのユーザー ID、外部の電子メールアドレス、または電子メールグループのリストを入力し、Enter キーを押します。

>[!NOTE]
>
>この通知を複数のユーザーに一度に送信するには、ユーザー ID または電子メールアドレスのリストをコンマで区切って指定します。

![「アラート購読者の管理」ページに、入力した電子メールアドレスが表示されます。](../images/alerts/ui/manage-alert-add-email.png)

E メールアドレスは、現在の購読者のリストに表示されます。 「**[!UICONTROL 更新]**」を選択します。

![「アラート購読者の管理」ページで、購読者を強調表示し、 [!UICONTROL 更新].](../images/alerts/ui/manage-alert-subscribers-added-email.png)

ユーザーがアラート通知リストに正常に追加されました。 送信されたユーザーは、次の画像に示すように、このアラートに関する電子メール通知を受け取ります。

![受信したアラート通知の電子メールの例。](../images/alerts/ui/manage-alert-subscribers-email.png)

## E メールアラートを有効にする

アラート通知は、電子メールに直接配信できます。

ベルアイコン (![ベルアイコン](../images/alerts/ui/bell-icon.png)) は、通知とお知らせを表示する右上の上部のリボンに配置されます。 表示されるドロップダウンで、コグアイコン (![コグアイコン](../images/alerts/ui/cog-icon.png)) をクリックして、Experience Cloud環境設定ページにアクセスします。

![ベルのアイコンと歯車のアイコンをハイライト表示するアラートのリストが表示されます。](../images/alerts/ui/edit-preferences.png)

The **プロファイル** ページが表示されます。 を選択します。 **[!UICONTROL 通知]** （左側のナビゲーション）をクリックして、e メールアラートの環境設定にアクセスします。

![プロファイルページのハイライト [!UICONTROL 通知] をクリックします。](../images/alerts/ui/profile.png)

をスクロールします。 **E メール** 」セクションをクリックし、 **[!UICONTROL インスタント通知]**

![プロファイルページで強調表示されている「 E メール」セクション。](../images/alerts/ui/notifications.png)

これで、Adobe IDアカウントに接続されている電子メールアドレスに、購読しているすべてのアラートが配信されます。

## アラート履歴の表示

「**[!UICONTROL 履歴]**」タブには、アラートをトリガーしたルール、トリガー日、解決日（該当する場合）など、組織で受信したアラートの履歴が表示されます。

![受信したアラートのリストは、 [!UICONTROL 履歴] タブをクリックします。](../images/alerts/ui/history.png)

リストに表示されたアラートを選択すると、右側のパネルに詳細（そのアラートをトリガーしたイベントの概要を含む）が表示されます。

![右側のパネルに詳細を示すアラートがハイライト表示されます。](../images/alerts/ui/history-details.png)

## 次の手順

このドキュメントでは、Platform UI でアラートを表示および管理する方法の概要を示しました。 サービスの機能について詳しくは、[Observability Insights](../home.md) の概要を参照してください。
