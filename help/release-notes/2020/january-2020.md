---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 1 月 15 日
doc-type: release notes
last-update: January 15, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: adf8e8457c8ffef263223a38d3f9c345cf7c6ab2
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 70%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 1 月 15 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

* [[!DNL Experience Data Model (XDM) System]](#xdm)
* [[!DNL Privacy Service]](#privacy)
* [[!DNL Sources]](#sources)
* [[!DNL Destinations]](#destinations)

## [!DNL Experience Data Model] (XDM)システム  {#xdm}

標準化と相互運用性は、[!DNL Experience Platform]の背後にある重要な概念です。 [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 同じ階層のフィールドに対するフィールドタイプの制限 | XDM フィールドを特定のタイプとして定義した後、名前と階層が同じ他のフィールドに対して、それらのフィールドが使用されているクラスや Mixin に関係なく、同じフィールドタイプを使用する必要があります。例えば、XDM [!DNL Profile]クラスのミックスインに&quot;integer&quot;型の`profile.age`フィールドが含まれている場合、XDM [!DNL ExperienceEvent]の同様のミックスインには&quot;string&quot;型の`profile.age`フィールドを含めることはできません。 別のフィールドタイプを利用するには、そのフィールドが、以前に定義したフィールド（例えば、`profile.person.age`）とは異なる階層のフィールドである必要があります。この機能は、和集合でスキーマを統合する際の競合を防ぐためのものです。この制限は既存のスキーマにさかのぼって影響を与えませんが、フィールドタイプの競合に関してスキーマを確認し、必要に応じてスキーマを編集することを強くお勧めします。 |
| 大文字と小文字を区別するフィールドの検証 | 大文字と小文字の区別なく、同じレベルのカスタムフィールドに同じ名前を付けることはできません。例えば、「Email」という名前のカスタムフィールドを追加する場合、同じレベルで「email」という名前の別のカスタムフィールドを追加することはできません。 |

**既知の問題**

* なし

[!DNL Schema Registry] APIと[!DNL Schema Editor]ユーザーインターフェイスを使用したXDMの使い方の詳細については、[XDMシステムドキュメント](../../xdm/home.md)を読んでください。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform[!DNL Privacy Service]は、RESTful APIとユーザーインターフェイスを提供し、お客様からのこれらのデータリクエストを管理するのに役立ちます。 [!DNL Privacy Service]を使うと、個人顧客データや個人顧客データにアクセスして削除する要求をAdobe Experience Cloudのアプリケーションから送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
|--- | ---|
| [!DNL Privacy Service] 商標変更 | GDPRに加え、他の規制をサポートするようになったため、以前の名称は[!DNL Privacy Service]に変更された。 |
| 新しい API エンドポイント | [!DNL Privacy Service] APIの基本パスが`/data/privacy/gdpr`から`/data/core/privacy/jobs`に更新されました。 |
| 新しい必須の `regulation` プロパティ | [!DNL Privacy Service] APIで新しいジョブを作成する場合、`regulation`プロパティを要求ペイロードに指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は、`gdpr` と `ccpa` です。 |
| サポート対象 [!DNL Adobe Primetime Authentication] | [!DNL Privacy Service] 製品の値としてを使用し、Adobe [!DNL Primetime Authentication]からアクセス/削除 `primetimeAuthentication` の要求を受け入れるようになりました。 |
| Privacy Service UI の強化 | GDPR および CCPA 規制に関する個別のジョブトラッキングページ。新しい**Regulation Type **ドロップダウンを使用して、GDPRとCCPAのトラッキングデータを切り替えます。 |

**既知の問題**

* なし

[!DNL Privacy Service]の詳細については、[Privacy Serviceの概要](../../privacy-service/home.md)を読んで開始してください。

## ソース {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 顧客属性データのサポート | 顧客属性データを取り込むストリーミングコネクタを作成するための UI と API のサポート。 |
| クラウドストレージの追加のファイル形式のサポート | クラウドストレージからのファイル取得で、XDM 準拠の Parquet および JSON ファイル形式がサポートされるようになりました。 |
| アクセス制御権限のサポート | Adobe Experience Platform のアクセス制御フレームワークは、データ取得の際にソースへのアクセスを許可するために必要な権限を提供します。ユーザーは、権限レベルに応じて、ソースの表示や管理行ったり、アクセスが拒否されたりします。 |

**アクセス制御権限**

| カテゴリ | 権限 | 説明 |
|--- | --- | ---|
| データ取得 | ソースの管理 | ソースへの読み取り、作成、編集、無効化アクセス |
| データ取得 | ソースの表示 | 「**[!UICONTROL カタログ]**」タブでの使用可能なソースおよび「**[!UICONTROL 参照]**」タブでの認証済みのソースへの読み取り専用アクセス |

**既知の問題**

* なし

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## 宛先 {#destinations}

[リアルタイムCDP](../../rtcdp/overview.md)では、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する宛先プラットフォームとの事前に構築された統合です。

**新機能**

| 機能 | 説明 |
|--- | ---|
| アクセス制御権限のサポート | リアルタイム CDP の宛先機能は、Adobe Experience Platform のアクセス制御権限と連携します。ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。 |

**アクセス制御権限**

| カテゴリ | 権限 | 説明 |
|--- | --- | ---|
| 宛先 | 宛先の管理 | 宛先の読み取り、作成、編集、および無効化へのアクセス。 |
| 宛先 | 宛先の表示 | 「**[!UICONTROL カタログ]**」タブの使用可能な宛先と「**参照**」タブの認証済みの宛先への読み取り専用アクセス。 |
| 宛先 | 宛先のアクティブ化 | 宛先に対してデータをアクティブ化する機能。この権限では、製品プロファイルに「宛先の管理」または「宛先の表示」を追加する必要があります。 |

**既知の問題**

* なし

詳しくは、「[宛先の概要](../../destinations/home.md)」を参照してください。