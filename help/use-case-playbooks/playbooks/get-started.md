---
solution: Experience Platform
title: 基本を学ぶ
description: ユースケースプレイブック機能の基本を学ぶ方法を説明します。
badgeBeta: label="ベータ版" type="Informative"
exl-id: 1c39792e-49fe-4c5f-9796-fa29f60b7461
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 100%

---

# （ベータ版）基本を学ぶ

>[!AVAILABILITY]
>
>この機能は現在ベータ版で、すべてのユーザーが利用できるわけではありません。ドキュメントと機能は変更される場合があります。

## 開発サンドボックスを作成 {#create-development-sandbox}

[[!UICONTROL ユースケースプレイブック]](/help/use-case-playbooks/playbooks/overview.md)機能の基本を学び、アクセスするには、下記に示すように、サフィックスに `-ucp` または `-UCP` を含む名前（タイトルではない）を付けて[新しい開発サンドボックスを作成](/help/sandboxes/ui/user-guide.md#create)します（実稼動用サンドボックスを選択しないようにしてください）。

![ユースケースプレイブック用の開発サンドボックスの作成](/help/use-case-playbooks/assets/playbooks/get-started/create-sandbox-ucp.png)

左側のパネルの[!UICONTROL ユースケースプレイブック]または[!UICONTROL マーケタープレイグラウンド]の下に[!UICONTROL プレイブック]が表示されます。

![サンドボックス作成後の UI でのユースケースプレイブック。](/help/use-case-playbooks/assets/playbooks/get-started/ucp-sandbox-in-ui.png)

上記に示すように、左側のパネルに[!UICONTROL プレイブック]が表示されない場合は、このリンク `https://experience.adobe.com/#/@<YOUR_ORG>/sname:<YOUR_SANDBOX_NAME>/platform/mexp/templates` を使用して直接移動します。リンク内の `<YOUR_ORG>` は組織の名前、`<YOUR_SANDBOX_NAME>` は作成した開発サンドボックスの名前です。

### Adobe Journey Optimizer ユーザー用のサンドボックス設定 {#sandbox-configuration-journey-optimizer}

組織が [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) でライセンス済みの場合は、メッセージに必要な技術パラメーターを定義するチャネルプリセットをサンドボックスで設定する必要があります。[詳しくは、Adobe Journey Optimizer でのチャネルサーフェスの設定方法を参照してください](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/channel-surfaces.html?lang=ja)。

## 必要なアクセス権限をチームに付与 {#grant-access-permissions}

[!UICONTROL ユースケースプレイブック]の基本を学ぶには、マーケティング運用チームのメンバーに適切な権限が必要です。次のようにして、チームに権限を付与できます。

* プレイブックを参照するだけのマーケティング運用チームのメンバーは、**読み取り**&#x200B;権限を取得できます。
* プレイブックからインスタンスを作成するマーケティング運用チームのメンバーは、**読み取りおよび書き込み**&#x200B;権限を取得できます。

## 次の手順 {#next-steps}

このドキュメントを参照すると、[!UICONTROL ユースケースプレイブック]の基本を学ぶことができます。次に、自分に合った[適切なプレイブックを見つけて](/help/use-case-playbooks/playbooks/discover.md)、[そこからインスタンスを作成する](/help/use-case-playbooks/playbooks/create-share-reuse.md)方法を参照してください。
