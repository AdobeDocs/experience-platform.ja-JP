---
title: Web SDK インストールの概要
description: Experience Platform Web SDKのインストール方法について説明します。
keywords: web sdk インストール；web sdkのインストール；internet explorer;promise;npm パッケージ
exl-id: b1de7ca1-d0d2-4661-a273-a1acf29afcd5
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Web SDK インストールの概要

Adobe Experience Platform Web SDKの使用には、次の3つのサポートされている方法があります。

1. **[Web SDK タグ拡張機能](/help/tags/extensions/client/web-sdk/overview.md)**:Adobeでは、このメソッドを使用することをお勧めします。 サイトにタグローダーをインストールし、Adobe Experience Platform Data Collection UIを使用して実装を設定します。
1. **[Web SDK JavaScript ライブラリ](library.md)**: CDN ホストのライブラリ ファイルを参照するか、独自のインフラストラクチャを使用してライブラリ ファイルをホストします。 サイトのコード内でライブラリを呼び出します。
1. **[NPM](npm.md)**: NPM パッケージマネージャーを使用して、Web SDKをサイトにインストールします。

## 前提条件

Web SDKを使用またはインストールする前に、次の要件を満たす必要があります。

* Adobe Experience Platformのアーキテクチャを最初に設定する必要があります。 これらの設定には、必要なスキーマ、ID、データストリームが含まれます。
* 適切なツールにアクセスするには、適切な権限を設定する必要があります。 例えば、組織でタグ拡張機能を使用する場合は、データ収集UIにアクセスするための適切な権限が必要です。 詳しくは、[ データ収集の権限](../../permissions.md)を参照してください。
* 1st パーティドメイン（CNAME）を持つことをお勧めします。 Adobe AnalyticsのCNAMEを既にお持ちの場合は、そのCNAMEを使用できます。 開発中のテストはCNAMEなしで機能しますが、Adobeでは本番環境への公開前にCNAMEを持つことを推奨しています。 詳しくは、[ ファーストパーティデバイス ID](../../identity/fpid.md)を参照してください。
