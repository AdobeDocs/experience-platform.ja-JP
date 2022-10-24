---
title: Adobe Experience Platformリリースノート 2020 年 1 月
description: Adobe Experience Platformの 2020 年 1 月のリリースノート。
doc-type: release notes
last-update: January 15, 2020
author: crhoades, ens28527
exl-id: e488a50c-2a87-4649-b3a4-f9d45cb12fcb
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 66%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 01 月 15 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

* [[!DNL Experience Data Model (XDM) System]](#xdm)
* [[!DNL Privacy Service]](#privacy)
* [[!DNL Sources]](#sources)
* [[!DNL Destinations]](#destinations)

## [!DNL Experience Data Model]（XDM）システム {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
|--- | ---|
| 同じ階層のフィールドに対するフィールドタイプの制限 | XDM フィールドを特定のタイプとして定義した後、同じ名前と階層の他のフィールドは、それらを使用するクラスやスキーマフィールドグループに関係なく、同じフィールドタイプを使用する必要があります。 例えば、XDM のフィールドグループ [!DNL Profile] クラスに次を含む `profile.age` 「integer」タイプのフィールド。XDM 用の類似のフィールドグループ [!DNL ExperienceEvent] が `profile.age` 「string」型のフィールド。 別のフィールドタイプを利用するには、そのフィールドが、以前に定義したフィールド（例えば、`profile.person.age`）とは異なる階層のフィールドである必要があります。この機能は、和集合でスキーマを統合する際の競合を防ぐためのものです。この制限は既存のスキーマにさかのぼって影響を与えませんが、フィールドタイプの競合に関してスキーマを確認し、必要に応じてスキーマを編集することを強くお勧めします。 |
| 大文字と小文字を区別するフィールドの検証 | 大文字と小文字の区別なく、同じレベルのカスタムフィールドに同じ名前を付けることはできません。例えば、「Email」という名前のカスタムフィールドを追加する場合、同じレベルで「email」という名前の別のカスタムフィールドを追加することはできません。 |

**既知の問題**

* なし

を使用した XDM の操作について詳しくは、以下を参照してください。 [!DNL Schema Registry] API および [!DNL Schema Editor] ユーザーインターフェイス、 [XDM システムドキュメント](../../xdm/home.md).

## [!DNL Privacy Service] {#privacy}

新しい法規制や組織の規制により、ユーザーはリクエストによってデータストアから個人データにアクセスしたり削除したりする権利を与えられています。Adobe Experience Platform [!DNL Privacy Service] には、顧客からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスが用意されています。 を使用 [!DNL Privacy Service]を使用すると、Adobe Experience Cloudアプリケーションから非公開または個人の顧客データに対するアクセスおよび削除のリクエストを送信でき、法規制や組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
|--- | ---|
| [!DNL Privacy Service] 再ブランディング | 以前の名称「GDPR サービス」は、 [!DNL Privacy Service] は、GDPR に加えて他の規制もサポートするようになったためです。 |
| 新しい API エンドポイント | のベースパス [!DNL Privacy Service] API は次のように更新されました： `/data/privacy/gdpr` から `/data/core/privacy/jobs`. |
| 新しい必須の `regulation` プロパティ | で新しいジョブを作成する場合 [!DNL Privacy Service] API, a `regulation` プロパティをリクエストペイロードで指定して、ジョブを追跡する規則を示す必要があります。 指定できる値は、`gdpr` と `ccpa` です。 |
| [!DNL Adobe Primetime Authentication] のサポート | [!DNL Privacy Service] Adobeからのアクセス/削除リクエストを受け入れるようになりました [!DNL Primetime Authentication]，次を使用 `primetimeAuthentication` を製品値として設定します。 |
| Privacy Service UI の強化 | GDPR および CCPA 規制に関する個別のジョブトラッキングページ。GDPR と CCPA のトラッキングデータを切り替える新しい「**規制タイプ**」ドロップダウン。 |

**既知の問題**

* なし

詳しくは、 [!DNL Privacy Service]次を読んでください： [Privacy Serviceの概要](../../privacy-service/home.md).

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

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

In [Real-Time CDP](../../rtcdp/overview.md)の宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新機能**

| 機能 | 説明 |
|--- | ---|
| アクセス制御権限のサポート | Real-Time CDPの宛先機能は、Adobe Experience Platformのアクセス制御権限と連携します。 ユーザーの権限レベルに応じて、宛先を表示、管理、アクティブ化できます。 |

**アクセス制御権限**

| カテゴリ | 権限 | 説明 |
|--- | --- | ---|
| 宛先 | 宛先の管理 | 宛先の読み取り、作成、編集、および無効化へのアクセス。 |
| 宛先 | 宛先の表示 | 「**[!UICONTROL カタログ]**」タブの使用可能な宛先と「**参照**」タブの認証済みの宛先への読み取り専用アクセス。 |
| 宛先 | 宛先のアクティブ化 | 宛先に対してデータをアクティブ化する機能。この権限では、製品プロファイルに「宛先の管理」または「宛先の表示」を追加する必要があります。 |

**既知の問題**

* なし

詳しくは、「[宛先の概要](../../destinations/home.md)」を参照してください。
