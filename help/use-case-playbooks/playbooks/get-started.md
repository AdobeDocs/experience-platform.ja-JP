---
solution: Experience Platform
title: 基本を学ぶ
description: ユースケースプレイブック機能の使用を開始する方法を説明します。
badgeBeta: label="Beta" type="Informative"
source-git-commit: 297dc9d6252d401f805fa5ebb5cf5111910286cf
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 8%

---


# （ベータ版）はじめに

>[!AVAILABILITY]
>
>この機能は現在ベータ版で、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 開発サンドボックスの作成 {#create-development-sandbox}

使用を開始して、 [[!UICONTROL ユースケースプレイブック]](/help/use-case-playbooks/playbooks/overview.md) 機能 [新しい開発サンドボックスを作成する](/help/sandboxes/ui/user-guide.md#create) （タイトルではなく）名前が次のいずれかを含む（実稼動用サンドボックスを選択しないでください）。 `-ucp` または `-UCP` という名前を付けます。

![使用例のプレイブック用の開発サンドボックスの作成](/help/use-case-playbooks/assets/playbooks/get-started/create-sandbox-ucp.png)

これで、 [!UICONTROL プレイブック] の下の左側のレールに [!UICONTROL ユースケースプレイブック] または [!UICONTROL マーケタープレイグラウンド].

![サンドボックス作成後の UI での使用例プレイブック。](/help/use-case-playbooks/assets/playbooks/get-started/ucp-sandbox-in-ui.png)

表示されない [!UICONTROL プレイブック] 上に示すように、左側のレールで、このリンクを使用します。 `https://experience.adobe.com/#/@<YOUR_ORG>/sname:<YOUR_SANDBOX_NAME>/platform/mexp/templates` をクリックして直接移動します。 リンクで、 `<YOUR_ORG>` は組織の名前で、 `<YOUR_SANDBOX_NAME>` は、作成した開発用サンドボックスの名前です。

### Adobe Journey Optimizerユーザー用のサンドボックス設定 {#sandbox-configuration-journey-optimizer}

組織が [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja)の場合は、サンドボックスでチャネルプリセットを設定する必要があります。この設定は、メッセージに必要な技術的パラメーターを定義します。 [Adobe Journey Optimizerでチャネルサーフェスを設定する方法を説明します。](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/channel-surfaces.html?lang=ja).

## チームに必要なアクセス権限を付与する {#grant-access-permissions}

を使い始めるには、以下を実行します。 [!UICONTROL ユースケースプレイブック]、マーケティング運用チームのメンバーには適切な権限が必要です。 次の手順に従って、チームに権限を付与できます。

* マーケティング運用チームのメンバーは、プレイブックのみを参照したい場合、 **読み取り** 権限。
* マーケティング運用チームのメンバーがプレイブックからインスタンスを作成する場合は、 **読み取りと書き込み** 権限。

## 次の手順 {#next-steps}

このドキュメントを読むと、の基本を学ぶことができます。 [!UICONTROL ユースケースプレイブック]. 次に、 [適切なプレイブックを見つける](/help/use-case-playbooks/playbooks/discover.md) それから [そのインスタンスからインスタンスを作成します。](/help/use-case-playbooks/playbooks/create-share-reuse.md).
