---
title: Web SDK インストールの概要
description: Experience PlatformWeb SDK のインストール方法を説明します。
keywords: web sdk のインストール；web sdk のインストール；internet explorer;promise;npm パッケージ
exl-id: b1de7ca1-d0d2-4661-a273-a1acf29afcd5
source-git-commit: f75dcfc945be2f45c1638bdd4d670288aef6e1e6
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---

# Web SDK インストールの概要

Adobe Experience Platform Web SDK の使用方法は 3 つあります。

1. **[Web SDK タグ拡張機能](extension.md)**:Adobeでは、この方法を使用することをお勧めします。 サイトにタグローダーをインストールし、Adobe Experience Platform Data Collection UI を使用して実装を設定します。
1. **[Web SDK JavaScript ライブラリ](library.md)**:CDN でホストされるライブラリファイルを参照するか、独自のインフラストラクチャを使用してライブラリファイルをホストします。 サイトのコード内でライブラリに対して呼び出しを行います。
1. **[NPM](npm.md)**: NPM パッケージマネージャーを使用して、web SDK をサイトにインストールします。

## 前提条件

Web SDK を使用またはインストールする前に、次の要件を満たす必要があります。

* 最初に、Adobe Experience Platformのアーキテクチャを設定する必要があります。 これらの設定には、必要なスキーマ、ID およびデータストリームが含まれます。
* 適切なツールにアクセスするための適切な権限が設定されている必要があります。 例えば、組織がタグ拡張機能の使用を決定した場合、データ収集 UI にアクセスするための適切な権限が必要になります。 詳しくは、[ データ収集権限の管理 ](https://experienceleague.adobe.com/docs/experience-platform/collection/permissions.html?lang=ja) を参照してください。
* ファーストパーティドメイン（CNAME）を使用することをお勧めします。 既にAdobe Analyticsの CNAME がある場合は、その CNAME を使用できます。 Adobe環境でのテストは CNAME なしでも機能しますが、実稼動環境に公開する前に実施することをお勧めします。 詳しくは、[ ファーストパーティデバイス ID](../identity/first-party-device-ids.md) を参照してください。
