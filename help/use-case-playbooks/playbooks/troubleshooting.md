---
solution: Experience Platform
title: プレイブックの既知の制限と問題のトラブルシューティング
description: プレイブックの既知の問題とよくある問題の詳細とトラブルシューティング方法について説明します
role: User, Developer, Admin
exl-id: 2604ce26-bcf9-46e1-bc10-30252a113159
source-git-commit: 0faf3187c0b32e0be70033e501939412ade37d7e
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---


# トラブルシューティング {#troubleshooting}

ユースケースプレイブックを操作する際の一般的なエラーに関するトラブルシューティングの提案を表示します

## Adobe Journey Optimizerサーフェスが設定されていません {#surfaces-not-configured}

プレイブックのインスタンスを作成すると、次のメッセージが表示される場合があります。

![トラブルシューティング](/help/use-case-playbooks/assets/playbooks/troubleshooting/troubleshooting-ajo.png)

これは、Journey Optimizer プレイブックがメール、プッシュ、SMS チャネルのメッセージを作成するからです。 様々なサーフェスの設定については、[&#x200B; 基本を学ぶ &#x200B;](/help/use-case-playbooks/playbooks/get-started.md#configure-sandbox-and-channel-surfaces-in-journey-optimizer) ガイドを参照してください。

## 新しいインスタンスの作成時のステータス *失敗* {#status-failed}

インスタンスを作成しようとしたときに失敗メッセージが表示された場合は、管理者から必要なユーザー権限が付与されていない可能性があります。 プレイブックには様々なアセットが多数含まれており、プレイブックのインスタンスを正常に作成するには、ユーザーがこれらのアセットを作成する権限が必要です。 権限の設定方法については、このガイドの [&#x200B; 権限 &#x200B;](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions) の節を参照してください。

## 読み込み失敗 {#import-failure}

お客様は様々なテスト環境内で作業しますが、インスタンスを環境からAdobeサンドボックスに読み込む際に失敗する場合もあります。 これらの読み込みのステータスを表示するには、左側のナビゲーションから「サンドボックス」を選択したあと、「ジョブ」を選択します。 ここでは、読み込まれたファイルのすべての詳細を表示できます。 失敗ステータスのファイルを選択し、「ジョブの詳細を表示」を選択します。 モーダルが表示されます。 「JSON ファイルを表示」を選択し、下にスクロールして「メッセージ」の下に表示されるエラーメッセージをコピーします。 複数のエラーメッセージが表示される可能性があるので、必ずすべてをコピーします。 バグ チケットをログに記録する際に、Adobeチームに送信します。 これにより、解決プロセスが迅速化され、起こっていることについてのチームのコンテキストが増えます。
