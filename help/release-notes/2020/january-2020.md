---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年1月16日）
doc-type: release notes
last-update: January 15, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: f881c1365684b1ca9e6bf9a8ce866d234dc54128
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 10%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 1 月 15 日**

Adobe Experience Platform内の既存の機能の更新：

* [!DNL Experience Data Model (XDM) System](#xdm)
* [!DNL Privacy Service](#privacy)
* [!DNL Sources](#sources)
* [!DNL Destinations](#destinations)

## [!DNL Experience Data Model] (XDM)システム {#xdm}

標準化と相互運用性は、重要な概念 [!DNL Experience Platform]です。 [!DNL Experience Data Model] (XDM)は、アドビが推進する機能で、カスタマーエクスペリエンスデータを標準化し、カスタマーエクスペリエンス管理のスキーマを定義するための取り組みです。

XDMは、デジタルエクスペリエンスのパワーを向上させるために設計された、公開された仕様です。 Adobe Experience Platform上のサービスと通信するアプリケーションの共通の構造と定義を提供します。 XDM標準を守ることで、すべての顧客体験データを共通の表現に組み込むことができ、より迅速で統合的な方法でインサイトを提供できます。 顧客のアクションから貴重なインサイトを得たり、セグメントを通して顧客オーディエンスを定義したり、顧客属性を使用してパーソナライズを図ることができます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 同じ階層のフィールドに対するフィールドタイプの制限 | XDMフィールドを特定の型として定義した後は、同じ名前と階層の他のフィールドは、それらが使用されるクラスやミックスインに関係なく、同じフィールド型を使用する必要があります。 例えば、XDM [!DNL Profile] クラスのミックスインに&quot;integer&quot;型の `profile.age` フィールドが含まれている場合、XDMの同様のミックスインに&quot;string&quot;型の [!DNL ExperienceEvent] フィールドを含めることはで `profile.age` きません。 別のフィールドタイプを利用するには、フィールドが、以前に定義したフィールドとは異なる階層(例えば、 `profile.person.age`)になっている必要があります。 この機能は、スキーマを和集合にまとめたときの競合を防ぐためのものです。 制約は既存のスキーマに遡って影響を与えませんが、フィールドタイプの競合に関するスキーマを確認し、必要に応じて編集することを強くお勧めします。 |
| 大文字と小文字が区別されるフィールドの検証 | 大文字と小文字が区別されず、同じレベルのカスタムフィールドの名前は異なる必要があります。 例えば、「Email」という名前のカスタムフィールドを追加した場合、「email」という名前の同じレベルに別のカスタムフィールドを追加することはできません。 |

**既知の問題**

* None

APIとユーザーインターフェイスを使用したXDMの操作について詳しくは、 [!DNL Schema Registry] XDMシステムのドキュメントを読んでください [!DNL Schema Editor][](../../xdm/home.md)。

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を持つようになります。 Adobe Experience Platform [!DNL Privacy Service] は、RESTful APIとユーザーインターフェイスを提供し、顧客からのこれらのデータリクエストを管理するのに役立ちます。 また、Adobe Experience Cloudアプリケーションから個人または個人の顧客データにアクセスする要求を送信したり、Adobe Experience Cloudアプリケーションから削除したりできるので、法律や組織のプライバシーに関する規制への準拠を自動化できます。 [!DNL Privacy Service]

**新機能**

| 機能 | 説明 |
|--- | ---|
| [!DNL Privacy Service] 商標変更 | GDPRに加え、他の規制をサポートするようになったため、従来の「GDPRサービス」 [!DNL Privacy Service] の名称を変更。 |
| 新しいAPIエンドポイント | APIのベースパスがからに更新さ [!DNL Privacy Service] れ `/data/privacy/gdpr``/data/core/privacy/jobs`ました。 |
| 新しい必須 `regulation` プロパティ | APIで新しいジョブを作成する場合、要求ペイロードで [!DNL Privacy Service]`regulation` プロパティを指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は `gdpr` とで `ccpa`す。 |
| サポート対象 [!DNL Adobe Primetime Authentication] | [!DNL Privacy Service] 製品の値としてを使用し、アドビ [!DNL Primetime Authentication]からのアクセス/削除要求 `primetimeAuthentication` を受け入れるようになりました。 |
| Privacy ServiceUIの強化 | GDPRおよびCCPA規制に関する個別のジョブトラッキングページ。 GDPRとCCPAのトラッキングデータを切り替えるための新しい _Regulation Type_ （規則タイプ）ドロップダウン。 |

**既知の問題**

* None

詳細については、 [!DNL Privacy Service]Privacy Serviceの概要を読んで開始してください [](../../privacy-service/home.md)。

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込みながら、サー [!DNL Platform] ビスを使用してデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、外部のストレージシステムやCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 顧客属性データのサポート | 顧客属性データを取り込むためのストリーミングコネクタの作成に対するUIおよびAPIのサポート。 |
| クラウドストレージ向けの追加ファイル形式のサポート | クラウドストレージからのファイル取り込みで、XDM準拠のParketとJSONファイル形式がサポートされるようになりました。 |
| アクセス制御権限のサポート | Adobe Experience Platformのアクセス制御フレームワークは、データ取り込み時にソースへのアクセスを許可するために必要な権限を提供します。 ユーザーは、権限レベルに応じて、表示ソースの管理、ソースの管理を行ったり、アクセスを完全に拒否したりできます。 |

**アクセス制御権限**

| カテゴリ | 権限 | 説明 |
|--- | --- | ---|
| データ収集 | ソースの管理 | ソースの読み取り、作成、編集および無効化を行うことができます。 |
| データ収集 | 表示ソース | 「 *[!UICONTROL カタログ]* 」タブの使用可能なソースおよび「 *[!UICONTROL 参照]* 」タブの認証されたソースへの読み取り専用アクセス権。 |

**既知の問題**

* None

For more information about sources, see the [sources overview](../../sources/home.md)

## 宛先 {#destinations}

アド [ビのリアルタイムCDPでは](../../rtcdp/overview.md)、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する宛先プラットフォームとの事前に構築された統合です。

**新機能**

| 機能 | 説明 |
|--- | ---|
| アクセス制御権限のサポート | Real-time CDP の宛先機能は、Adobe Experience Platform のアクセス制御権限と連携します。ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。 |

**アクセス制御権限**

| カテゴリ | 権限 | 説明 |
|--- | --- | ---|
| 宛先 | 宛先の管理 | 読み取り先、作成先、編集先および無効化先へのアクセス権。 |
| 宛先 | 表示先 | 「 [!UICONTROL _カタログ&#x200B;_]」タブの使用可能な宛先および「_&#x200B;参照&#x200B;_」タブの認証済み宛先への読み取り専用アクセス権。 |
| 宛先 | 宛先のアクティブ化 | 宛先へのデータのアクティブ化機能 この権限を持つには、製品プロファイルに「表示先を管理」または「宛先を管理」を追加する必要があります。 |

**既知の問題**

* None

詳しくは、「[宛先の概要](../../rtcdp/destinations/destinations-overview.md)」を参照してください。