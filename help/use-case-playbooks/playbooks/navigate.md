---
solution: Experience Platform
title: ユースケースプレイブックに移動
description: プレイブックのギャラリーをナビゲートし、感動的なサンドボックスの使用を開始する方法を説明します。
role: User
exl-id: 1f5dae75-1136-4be3-9132-01d36a4066ca
source-git-commit: 54b3d2ef22f7afb47fa8c9430c5c1645c94c837d
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 2%

---

# ユースケースプレイブックに移動

ユースケースプレイブックは、すべてのAdobe Experience Platformのお客様が追加費用なしで利用できます。 Experience PlatformUI で豊富なユースケースプレイブックのギャラリーにアクセスするには、次を選択します **[!UICONTROL プレイブック]** 左側のナビゲーションから。

![ユースケースプレイブックギャラリー。](/help/use-case-playbooks/assets/playbooks/discover/playbooks-gallery.png)

![左側のナビゲーションバーのユースケースプレイブックに直接アクセスします。](/help/use-case-playbooks/assets/playbooks/discover/left-nav-playbooks.png)

任意のプレイブックを選択して詳細ページに移動し、以下を選択します **[!UICONTROL インスピレーションの得られるサンドボックスに移動]**. 確認モーダルが表示されます。 を選択 **確認** 様々なユースケースを探索し、試すことができる感動的なサンドボックスに移動します。

サンドボックスを作成する権限がない場合は、管理者に連絡して、感動的なサンドボックスの作成についてサポートを受けてください。

>[!TIP]
>
>感動的なサンドボックスは、Adobe Experience Platform内の開発用サンドボックスで、ライブ実稼動環境に実装する前に、様々なユースケースを作成、テスト、実験できます。

![感動的なサンドボックスに移動します。](/help/use-case-playbooks/assets/playbooks/discover/inspirational-sandbox.png)

インスピレーションを得られるサンドボックスをまだ設定していない場合は、次を選択します。 **[!UICONTROL 感動的なサンドボックスの作成]**. モーダルが表示されます。 を入力 **名前** および **タイトル** 必須フィールドボックスでを選択し、 **作成**. 感動的なサンドボックスを作成したら、必ず次の操作を行います [権限の定義](/help/access-control/home.md) ユースケースプレイブックの詳細ページに戻ってインスタンスを作成する前に。

![感動的なサンドボックスを作成します。](/help/use-case-playbooks/assets/playbooks/discover/create-inspirational-sandbox.png)

![名前とタイトルを入力して、感動的なサンドボックスを作成します。](/help/use-case-playbooks/assets/playbooks/discover/create-inspirational-sandbox-modal.png)

感動的なサンドボックスの外部からユースケースプレイブックを選択した場合は、インスタンスを作成できません。 詳細ページで、 **感動的なサンドボックスに移動** 既存の感動的サンドボックスに移動して、次を選択します： **[!UICONTROL インスタンスを作成]**.

サンドボックスを作成する権限がない場合は、管理者に連絡して、感動的なサンドボックスの作成についてサポートを受けてください。

![サンドボックスを作成する権限がありません。](/help/use-case-playbooks/assets/playbooks/discover/no-permissions-to-create-sandbox.png)

割り当てられるサンドボックスの数が上限に達した場合は、組織の管理者に連絡して、上限を増やすか、アクティブなサンドボックスの一部を非アクティブにするか削除するかを尋ねるメッセージが表示されます。 サンドボックスの制限を調整するか、アクティブなサンドボックスの数を減らしたら、感動的なサンドボックスの作成に進むことができます。

![サンドボックスの制限に達しました。](/help/use-case-playbooks/assets/playbooks/discover/sandbox-limit-reached.png)

インスピレーションの得られるサンドボックスを作成する場合、メール、プッシュおよび SMS 通知用のチャネルサーフェスは、自動的には設定されません。 IT 管理者に手動で設定するように依頼してください。そうでない場合は、インスタンスの作成に失敗することがあります。

![チャネルプリセットの設定](/help/use-case-playbooks/assets/playbooks/discover/configure-channel-presets.png)

## Journey Optimizerでのサンドボックスおよびチャネルサーフェスの設定 {#configure-channel-surfaces}

組織がにライセンスを取得している場合 [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja)また、Journey Optimizer用に設計されたプレイブックを使用する場合は、サンドボックスでチャネルプリセットを設定する必要があります。これは、メッセージに必要な技術的パラメーターを定義します。 [詳しくは、Adobe Journey Optimizer でのチャネルサーフェスの設定方法を参照してください](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/channel-surfaces.html?lang=ja)。

Journey Optimizerでプレイブックのインスタンスを作成するには、メール、プッシュ、SMS 通知用のチャネルサーフェスを設定する必要があります。

### メールチャネルサーフェス

に移動 `Channels` Journey Optimizerのインターフェイスで上書きできます。 まだ設定されていない場合は、マーケティングメールとトランザクションメッセージ用に個別のサブドメインと IP プールを設定します。 これらは、注文確認メールなどのトランザクションメッセージを顧客に確実に送信するためのベストプラクティスです。 名前、メールアドレスおよび追加設定を入力します。 を選択 **Submit** ページの右上で、マーケティングチャネルサーフェスを作成します。 のドキュメントを参照してください。 [メールチャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/configure-email/email-settings.html).

### SMS チャネルサーフェス

SMS チャネルサーフェスを作成するには、まず SMS API 認証情報を作成し、優先ベンダー（Sinch など）を選択します。 SMS チャネルサーフェスに名前を付け（例：SMS マーケティング）、設定を選択し、送信者番号を入力します。 を選択 **Submit** をクリックし、SMS チャネルサーフェスを保存します。 のドキュメントを参照してください。 [sms チャネルサーフェスの設定方法](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms).

また、注文確認などのトランザクションメッセージを含むプレイブックのチャネルを設定します。

### チャネルサーフェスをプッシュ

アプリサーフェスがExperience Platformまたはデータ収集インターフェイスのいずれかから設定されていることを確認します。 データ収集環境では、アプリのサーフェスはこのように表示されます。

## 次の手順 {#next-steps}

このドキュメントを読み、感動的なサンドボックスの設定方法を理解し、Platform 内のユースケースプレイブックにアクセスする様々な方法に精通する必要があります。 次の手順として、その方法を確認します。 [を選択](/help/use-case-playbooks/playbooks/choose.md) 適切なプレイブック。
