---
keywords: Experience Platform;ホーム;人気のトピック;日付範囲
title: アラート UI ガイド
description: Experience Platform のユーザーインターフェイスでアラートを管理する方法を説明します。
feature: Alerts
exl-id: 4ba3ef2b-7394-405e-979d-0e5e1fe676f3
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
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

![ 左側のナビゲーションのアラートページがハイライト表示された [!UICONTROL  アラート ]。](../images/alerts/ui/workspace.png)

## アラートルールの管理

「**[!UICONTROL 参照]**」タブには、アラートをトリガーする可能性のある使用可能なルールが一覧表示されます。

![ 使用可能なアラートのリストが「[!UICONTROL  参照 ] タブに表示されます。](../images/alerts/ui/rules.png)

リストからルールを選択し、その説明と設定パラメーター（しきい値や重大度を含む）を右側のパネルに表示します。

![ 右側のパネルに詳細を表示するアラートルールがハイライト表示されている様子 ](../images/alerts/ui/rule-details.png)

ルール名の横にある省略記号（**...**）を選択すると、ドロップダウンにアラートの有効／無効（現在のステータスに応じて異なる）と、アラートのメール通知の登録／登録解除を切り替えるためのコントロールが表示されます。

![ 選択した省略記号によってドロップダウンメニューが表示されます。](../images/alerts/ui/disable-subscribe.png)

## アラートサブスクライバーを管理

>[!NOTE]
>
> アラートをAdobeユーザー ID、外部メールアドレス、メールグループリストのいずれかに割り当てるには、管理者である必要があります。

「**[!UICONTROL 参照]**」タブには、アラートをトリガーする可能性のある使用可能なルールが一覧表示されます。

![ 「参照 [!UICONTROL  タブに表示される使用可能なアラートルール ] リスト ](../images/alerts/ui/rules.png)

ルール名の横にある省略記号（**...**）を選択すると、ドロップダウンにコントロールが表示されます。 「**[!UICONTROL アラート購読者を管理]**」を選択します。

![ 省略記号を選択してドロップダウンメニューを表示します。 「[!UICONTROL  アラート購読者を管理 ]」オプションがハイライト表示されている様子 ](../images/alerts/ui/manage-alert-subscribers.png)

[!UICONTROL  アラート購読者の管理 ] ページが表示されます。 特定のユーザーに通知を割り当てるには、Adobeのユーザー ID、外部のメールアドレス、またはメールグループリストを入力し、Enter キーを押します。

>[!NOTE]
>
>この通知を複数のユーザーに一度に送信するには、ユーザー ID またはメールアドレスのリストをコンマで区切って指定します。

![ 入力されたメールアドレスを示すアラート購読者の管理ページ ](../images/alerts/ui/manage-alert-add-email.png)

これらのメールアドレスは、現在リストされている購読者のリストに表示されます。 「**[!UICONTROL 更新]**」を選択します。

![ アラート購読者を管理ページで購読者を強調表示して [!UICONTROL  更新 ] します。](../images/alerts/ui/manage-alert-subscribers-added-email.png)

アラート通知リストにユーザーが正常に追加されました。 送信されたユーザーは、このアラートに関するメール通知を受け取れるようになります（以下の画像を参照）。

![ 受信したアラート通知のメールの例。](../images/alerts/ui/manage-alert-subscribers-email.png)

## メールアラートを有効にする

アラート通知は、メールに直接配信できます。

通知とお知らせを表示するには、右側の上部のリボンにあるベルアイコン（![ ベルアイコン ](/help/images/icons/bell.png)）を選択します。 表示されるドロップダウンで、歯車アイコン（![ 歯車アイコン ](/help/images/icons/settings.png)）を選択して、Experience Cloudの環境設定ページにアクセスします。

![ ベルアイコンと歯車アイコンを強調表示したアラートのリスト。](../images/alerts/ui/edit-preferences.png)

**プロファイル** ページが表示されます。 左側のナビゲーションで **[!UICONTROL 通知]** を選択して、メールアラートの環境設定にアクセスします。

![ 左側のナビゲーションでハイライト表示されたプロファイルページ [!UICONTROL  通知 ]。](../images/alerts/ui/profile.png)

ページ下部の「**メール**」セクションまでスクロールし、「**[!UICONTROL インスタント通知]**」を選択します。

![ プロファイルページでハイライト表示された「メール」セクション。](../images/alerts/ui/notifications.png)

購読しているアラートは、Adobe ID アカウントに接続されているメールアドレスに配信されます。

## アラート履歴の表示

「**[!UICONTROL 履歴]**」タブには、アラートをトリガーしたルール、トリガー日、解決日（該当する場合）など、組織で受信したアラートの履歴が表示されます。

![ 受信したアラートのリストは「[!UICONTROL  履歴 ] タブに表示されます。](../images/alerts/ui/history.png)

リストに表示されたアラートを選択すると、右側のパネルに詳細（そのアラートをトリガーしたイベントの概要を含む）が表示されます。

![ 右側のパネルに詳細を示すハイライト表示されたアラート ](../images/alerts/ui/history-details.png)

## 次の手順

このドキュメントでは、Platform UI でアラートを表示および管理する方法の概要を示しました。 サービスの機能について詳しくは、[Observability Insights](../home.md) の概要を参照してください。
