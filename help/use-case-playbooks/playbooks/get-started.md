---
solution: Experience Platform
title: ユースケースプレイブックの概要
description: ユースケースプレイブック機能の基本を学ぶ方法を説明します。
role: Admin
exl-id: 1c39792e-49fe-4c5f-9796-fa29f60b7461
source-git-commit: d6b62b9539a04be2d2adc7aa66436a294e08303a
workflow-type: tm+mt
source-wordcount: '942'
ht-degree: 8%

---


# 基本を学ぶ

Real-Time Customer Data PlatformおよびAdobe Journey Optimizer向けに設計されたユースケースプレイブック用のアカウントが自動的に設定されない場合は、その設定方法を説明します。 主な設定手順は次の 3 つです。

* サンドボックスの作成
* ユーザー権限の設定
* メール、プッシュ、SMS 通知用のJourney Optimizer チャネルサーフェスの設定（Journey Optimizer プレイブックを使用する予定の場合）

Experience Platform UI で豊富なユースケースプレイブックのギャラリーにアクセスするには、左側のナビゲーションから「**[!UICONTROL Playbooks]**」を選択します。 [&#x200B; ユースケースプレイブックを操作する &#x200B;](../playbooks/navigate.md) 方法、および [&#x200B; インスピレーションにつながるサンドボックス &#x200B;](../playbooks/navigate.md) の基本を学ぶ方法については、ドキュメントをお読みください。

## ユースケースプレイブックの設定 – ビデオチュートリアル {#video}

このビデオでは、サンドボックスの作成、権限の設定、Journey Optimizerでのメール、プッシュおよび SMS 通知用のチャネルサーフェスの設定に必要な手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3449827?captions=jpn&learn=on)

## 開発サンドボックスを作成 {#create-development-sandbox}

ユースケースプレイブックは、特別なタイプの開発用サンドボックスを使用します。 [[!UICONTROL Use Case Playbooks]](/help/use-case-playbooks/playbooks/overview.md) 機能を使い始めて新しい機能にアクセスするには、次に示すように、[&#x200B; または &#x200B;](/help/sandboxes/ui/user-guide.md#create) のいずれかをサフィックスに含んだ名前（タイトルではありません）を持つ `-ucp` 新しい開発サンドボックスを作成 `-UCP` を選択してください（実稼動サンドボックスは選択しないでください）。

>[!IMPORTANT]
>
>新しい開発用サンドボックスを作成する場合は、名前のサフィックスに `-ucp` または `-UCP` が含まれていることを確認します。


![ユースケースプレイブック用の開発サンドボックスの作成](/help/use-case-playbooks/assets/playbooks/get-started/create-sandbox-ucp.png)

これで、[!UICONTROL Playbooks] の下の左側のレールに [!UICONTROL Use Case Playbooks] が表示されます。

![サンドボックス作成後の UI でのユースケースプレイブック。](/help/use-case-playbooks/assets/playbooks/get-started/ucp-sandbox-in-ui.png)

上記の左側のレールに [!UICONTROL Playbooks] が表示されない場合は、このリンク `https://experience.adobe.com/#/@<YOUR_ORG>/sname:<YOUR_SANDBOX_NAME>/platform/mexp/templates` を使用してそこに直接移動します。 リンク内の `<YOUR_ORG>` は組織の名前、`<YOUR_SANDBOX_NAME>` は作成した開発サンドボックスの名前です。

## 必要なアクセス権限をチームに付与 {#grant-access-permissions}

[!UICONTROL Use Case Playbooks] の使用を開始するには、マーケティングチームのメンバーが作成したプレイブックのリストを表示したり、自分でプレイブックを作成したりできるように、適切な権限が必要です。

**必要な権限**

必要な権限を追加するには、権限 UI で、他の開発用サンドボックスに使用される権限を含め、新しいユースケースプレイブックサンドボックスを [&#x200B; 設定済みの役割 &#x200B;](/help/access-control/abac/ui/permissions.md#managing-sandboxes-for-role) に含めます。

![&#x200B; 役割のプレイブックサンドボックスは既に設定済み &#x200B;](/help/use-case-playbooks/assets/playbooks/get-started/permissions-to-existing-roles.png)

**プレイブックの役割の設定：**

または、[&#x200B; 必要な権限 &#x200B;](/help/access-control/home.md#sandboxes-and-permissions) を持つ新しい役割を追加することも検討できます。

基本的なプレイブックタスクに必要な権限を持つ [&#x200B; 新しい役割を設定 &#x200B;](/help/access-control/abac/ui/permissions.md) します。 以下に示すように、役割を作成し、それに新しいサンドボックスを追加します。

![&#x200B; 役割を作成してサンドボックスに追加する &#x200B;](/help/use-case-playbooks/assets/playbooks/get-started/create-new-role.png)

**プレイブックインスタンスの権限**

ユースケースプレイブックの一部として、スキーマ、オーディエンス、宛先、ジャーニーなどの様々なアセットを作成します。 自身および他のユーザーは、これらのオブジェクトを作成するための適切な権限が必要になります。

**スキーマの権限**

スキーマを作成および管理するには、データモデリング権限（**[!UICONTROL Manage Schemas]**、**[!UICONTROL View Schemas]**、**[!UICONTROL Manage Relationships]**、**[!UICONTROL Manage Identity Metadata]**）を利用します

**宛先の権限**

宛先を作成および管理するには、宛先権限（**[!UICONTROL Manage]**、**[!UICONTROL Destinations]**、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL Activate Segment without Mapping]**、**[!UICONTROL Manage and Activate Dataset Destination]**、**[!UICONTROL Destination Authoring]**）を使用します。

**ジャーニーの権限**

ジャーニーを作成および管理するには、ジャーニー（**[!UICONTROL Manage Journeys]**、**[!UICONTROL View Journeys]**、**[!UICONTROL View Journeys Report]**、**[!UICONTROL Manage Journeys]**、**[!UICONTROL Events]**、**[!UICONTROL Data Sources and Actions]**、**[!UICONTROL View Journeys]**、**[!UICONTROL Events]**、**[!UICONTROL Data Sources and Actions, Publish Journeys]**）を使用します。

次の画像は、プレイブックとプレイブックによって生成されたアセットの表示、作成、管理にユーザーに推奨される権限のスナップショットを示しています。

![&#x200B; プレイブックのすべてのインスタンスを作成するために必要なすべての権限項目のスナップショット &#x200B;](/help/use-case-playbooks/assets/playbooks/get-started/permission-snapshot.png)

**役割にユーザーを追加**

上記のように [&#x200B; 新しい役割を作成 &#x200B;](/help/access-control/abac/ui/permissions.md#managing-users-for-role) したら、自分自身をユーザーとして追加します。 表示のみのアクセス権を持つ別のユーザーセットに対して、制限付きアクセスの役割を作成する場合、これらの権限に関連付けられている必要なビュー項目のみを含めます。

## Journey Optimizerでのサンドボックスおよびチャネルサーフェスの設定 {#configure-channel-surfaces}

組織が [Adobe Journey Optimizerのライセンスを取得していて &#x200B;](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/ajo-home)Journey Optimizer用に設計されたプレイブックを使用する場合は、サンドボックスでチャネルサーフェスを設定します。 チャネルサーフェスは、メールのタイプ、送信者のメールと名前、モバイルアプリ、SMS 設定など、メッセージに必要なすべての技術パラメーターを定義します。 [詳しくは、Adobe Journey Optimizer でのチャネルサーフェスの設定方法を参照してください](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/configuration/channel-surfaces)。

Journey Optimizerでプレイブックのインスタンスを作成するには、メール、プッシュ、SMS 通知用のチャネルサーフェスを設定する必要があります。

### メールチャネルサーフェス

Journey Optimizer インターフェイスの `Channels` に移動します。 まだ設定されていない場合は、マーケティングメールとトランザクションメッセージ用に個別のサブドメインと IP プールを設定します。 これらは、注文確認メールなどのトランザクションメッセージを顧客に確実に送信するためのベストプラクティスです。 名前、メールアドレスおよび追加設定を入力します。 ページの右上にある「**送信**」を選択して、マーケティングチャネルサーフェスを作成します。 詳しくは、[&#x200B; メールチャネルサーフェスの設定方法 &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/configure-email/email-settings.html?lang=ja) のドキュメントを参照してください。

### SMS チャネルサーフェス

SMS チャネルサーフェスを作成するには、まず SMS API 認証情報を作成し、優先ベンダー（Sinch など）を選択します。 SMS チャネルサーフェスに名前を付け（例：SMS マーケティング）、設定を選択し、送信者番号を入力します。 ページの右上にある「**送信**」を選択して、SMS チャネルサーフェスを保存します。 [SMS チャネルサーフェスの設定方法 &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/sms/sms-configuration.html?lang=ja#message-preset-sms) に関するドキュメントを参照してください。

また、注文確認などのトランザクションメッセージを含むプレイブックのチャネルを設定します。

### チャネルサーフェスをプッシュ

チャネル設定がExperience Platform インターフェイスまたはデータコレクションインターフェイスのいずれかから設定されていることを確認します。 データ収集環境では、チャネル設定は次のようになります。

<!-- ![Channel configurations in Data collections](/help/use-case-playbooks/assets/playbooks/get-started/.png) -->

次に、チャネル設定で確認したチャネル、プラットフォームおよびアプリを選択します。 **送信** を選択して、プッシュチャネルサーフェスを作成します。

[&#x200B; プッシュチャネルサーフェスの設定方法 &#x200B;](https://experienceleague.adobe.com/docs/journey-optimizer/using/push/push-config/push-configuration.html?lang=ja) に関するドキュメントを参照してください。

## 次の手順 {#next-steps}

このドキュメントのすべての手順を完了したので、左側のナビゲーションにユースケースプレイブックが表示された開発用サンドボックスを作成しました。 また、プレイブックの表示と管理、アセットの生成に必要な権限をチームメンバーに付与する方法についても理解しました。 次のステップとして、[&#x200B; 適切なプレイブックを選択する &#x200B;](/help/use-case-playbooks/playbooks/choose.md) 方法と、それから [&#x200B; インスタンスを作成する &#x200B;](/help/use-case-playbooks/playbooks/create-share-reuse.md) 方法を参照してください。
