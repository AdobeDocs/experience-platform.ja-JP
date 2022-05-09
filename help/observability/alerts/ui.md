---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: アラート UI ガイド
description: Experience Platform のユーザーインターフェイスでアラートを管理する方法を説明します。
feature: Alerts
exl-id: 4ba3ef2b-7394-405e-979d-0e5e1fe676f3
source-git-commit: ed18ecea98497e0c20d44617436a013bf83b69d2
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 69%

---

# アラート UI ガイド

Adobe Experience Platform ユーザーインターフェイスを使用すると、Adobe Experience Platform の Observability Insights で表示される指標に基づいて、受信したアラートの履歴を表示できます。 また、UI を使用すると、使用可能なアラートルールの表示、有効化、無効化、登録をおこなうこともできます。

>[!NOTE]
>
>Experience Platform のアラートの概要については、[アラートの概要](./overview.md)を参照してください。

開始するには、左側のナビゲーションで「**[!UICONTROL アラート]**」を選択します。

![](../images/alerts/ui/workspace.png)

## アラートルールの管理

「**[!UICONTROL 参照]**」タブには、アラートをトリガーする可能性のある使用可能なルールが一覧表示されます。

![](../images/alerts/ui/rules.png)

リストからルールを選択し、その説明と設定パラメーター（しきい値や重大度を含む）を右側のパネルに表示します。

![](../images/alerts/ui/rule-details.png)

ルール名の横にある省略記号（**...**）を選択すると、ドロップダウンにアラートの有効／無効（現在のステータスに応じて異なる）と、アラートのメール通知の登録／登録解除を切り替えるためのコントロールが表示されます。

![](../images/alerts/ui/disable-subscribe.png)

## E メールアラートを有効にする

アラート通知は、電子メールに直接配信できます。

ベルアイコン (![ベルアイコン](../images/alerts/ui/bell-icon.png)) をクリックします。 表示されるドロップダウンで、コグアイコン (![コグアイコン](../images/alerts/ui/cog-icon.png)) をクリックして、Experience Cloud環境設定ページにアクセスします。

![](../images/alerts/ui/edit-preferences.png)

この **プロファイル** 」タブが表示されます。 を選択します。 **[!UICONTROL 通知]** （左側のナビゲーション）をクリックして、e メールアラートの環境設定にアクセスします。

![](../images/alerts/ui/profile.png)

スクロールして **電子メール** 」セクションをクリックし、 **[!UICONTROL インスタント通知]**

![](../images/alerts/ui/notifications.png)

これで、Adobe IDアカウントに接続されている電子メールアドレスに、購読しているすべてのアラートが配信されます。

## アラート履歴の表示

「**[!UICONTROL 履歴]**」タブには、アラートをトリガーしたルール、トリガー日、解決日（該当する場合）など、組織で受信したアラートの履歴が表示されます。

![](../images/alerts/ui/history.png)

リストに表示されたアラートを選択すると、右側のパネルに詳細（そのアラートをトリガーしたイベントの概要を含む）が表示されます。

![](../images/alerts/ui/history-details.png)

## 次の手順

このドキュメントでは、Platform UI でアラートを表示および管理する方法の概要を示しました。 サービスの機能について詳しくは、[Observability Insights](../home.md) の概要を参照してください。
