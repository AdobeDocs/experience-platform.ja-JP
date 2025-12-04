---
title: Web SDKのインストールの概要
description: Experience Platform Web SDKのインストール方法について説明します。
keywords: web sdk のインストール；web sdk のインストール；internet explorer;promise;npm パッケージ
exl-id: b1de7ca1-d0d2-4661-a273-a1acf29afcd5
source-git-commit: a490c429047f5e5997d69f30a51e6b78debe2d5d
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Web SDKのインストールの概要

Adobe Experience Platform Web SDKの使用方法として、次の 3 つがサポートされています。

1. **[Web SDK タグ拡張機能](/help/tags/extensions/client/web-sdk/overview.md)**:Adobeでは、この方法を使用することをお勧めします。 サイトにタグローダーをインストールし、Adobe Experience Platform Data Collection UI を使用して実装を設定します。
1. **[Web SDK JavaScript ライブラリ](library.md)**:CDN でホストされるライブラリファイルを参照するか、独自のインフラストラクチャを使用してライブラリファイルをホストします。 サイトのコード内でライブラリに対して呼び出しを行います。
1. **[NPM](npm.md)**:NPM パッケージマネージャーを使用して、サイトに web SDKをインストールします。

## 前提条件

Web SDKを使用またはインストールする前に、次の要件を満たす必要があります。

* 最初に、Adobe Experience Platformのアーキテクチャを設定する必要があります。 これらの設定には、必要なスキーマ、ID およびデータストリームが含まれます。
* 適切なツールにアクセスするための適切な権限が設定されている必要があります。 例えば、組織がタグ拡張機能の使用を決定した場合、データ収集 UI にアクセスするための適切な権限が必要になります。 詳しくは、[&#x200B; データ収集の権限 &#x200B;](../../permissions.md) を参照してください。
* ファーストパーティドメイン（CNAME）を使用することをお勧めします。 既にAdobe Analyticsの CNAME がある場合は、その CNAME を使用できます。 開発環境でのテストは CNAME なしでも機能しますが、Adobeでは実稼動環境に公開する前に CNAME を用意することをお勧めします。 詳しくは、[&#x200B; ファーストパーティデバイス ID](../../use-cases/identity/first-party-device-ids.md) を参照してください。
