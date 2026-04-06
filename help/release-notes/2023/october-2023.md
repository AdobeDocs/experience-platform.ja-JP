---
title: Adobe Experience Platform リリースノート 2023年10月
description: Adobe Experience Platform の 2023年10月のリリースノート。
exl-id: e9cf5299-8350-4b40-8f56-05e598846875
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 37%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年10月25日（PT）**

Experience Platformの既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先の使用状況メトリック | ライセンス使用状況ダッシュボードに新しい測定指標が追加されました。 **[!UICONTROL Audience Activation Size]**&#x200B;および&#x200B;**[!UICONTROL Data Export Size]**&#x200B;指標は、ライセンス使用権限に関して、Experience Platformから書き出したデータ量を追跡する便利な方法を提供します。 これらの指標とその他のライセンス使用状況の指標について詳しくは、[使用可能な指標](../../dashboards/guides/license-usage.md#available-metrics)のドキュメントを参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Meta] コンバージョン APIの機能強化 | [Meta Conversions API](/help/tags/extensions/server/meta/overview.md)拡張機能には、次の3つの機能強化が行われました。 <ul><li>[[!DNL Meta Business Extension (MBE)]](/help/tags/extensions/server/meta/overview.md#integration-with-meta-business-extension-mbe)との統合：AdobeとConversions API統合のpixelIDとアクセストークンを共有できるようにすることで、シームレスなログインエクスペリエンスを作成します。</li><li>[[!DNL Event Match Quality Score (EMQ)]](/help/tags/extensions/server/meta/overview.md#integration-with-event-quality-match-score-emq)との統合：目的のアクションを完了する可能性が高いユーザーに広告を配信し、そのアクションを配信された広告にリンクすることができます。</li><li>[[!DNL LiveRamp (Alpha)]](/help/tags/extensions/server/meta/overview.md#integration-with-liveramp-alpha)との統合：LiveRampのRampIDをCIP フィールドに渡すことができるため、パートナーやMetaと直接PIIを共有する必要がありません。 </li></ul> |
| 拡張機能 | [!DNL LinkedIn] コンバージョン API | [[!DNL LinkedIn] Conversions API](../../tags/extensions/server/linkedin/overview.md)拡張機能を使用すると、Experience Platform イベントデータをLinkedInに転送することで、LinkedIn マーケティングキャンペーンの効果を評価できます。 |
| 秘密鍵 | [!DNL LinkedIn] OAuth 2秘密鍵 | [[!DNL LinkedIn] OAuth 2 Secret](../../tags/ui/event-forwarding/secrets.md#linkedin-oauth-2)を使用すると、イベント転送時にサーバー間のインタラクションを[!DNL LinkedIn]に送信できます。 |
| イベント転送 | タグとイベント転送の更新 | Experience Platformで[&#x200B; タグ &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja)および[&#x200B; イベント転送](https://experienceleague.adobe.com/docs/experience-platform/tags/event-forwarding/overview.html?lang=ja)のパフォーマンスを保持するには、成功したビルドと失敗したビルドの両方の最新の開発ビルドとステージ ビルドのみが保持されます。 使用されなくなったすべてのビルドが削除されます。 さらに、いくつかの多量のAPI使用が他のAPIのパフォーマンスを低下させないように、スロットリングとレート制限が実装されています。 |
| 拡張機能 | 要素、ルール、拡張機能 | [要素、ルール、および拡張機能](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/overview.html)がライブラリ出力で並べ替えられるようになり、同じライブラリの複数のビルドとデプロイメント間の一貫性が高まりました。 |

データ収集について詳しくは、[データ収集の概要](../../tags/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 新規／アップデート | 説明 |
| ----------- |----------------|----------- |
| [[!DNL MoEngage]](/help/destinations/catalog/mobile-engagement/moengage.md) | 新規 | Moengageの宛先を使用して、Adobeデータ（ユーザー属性、セグメント、イベント）をリアルタイムでMoEngageに接続し、マッピングします。 顧客はこのデータにもとづいて行動し、パーソナライズされたエクスペリエンスを提供できます。 |
| [[!DNL Qualtrics Automations]](/help/destinations/catalog/survey/qualtrics-automations.md) | 新規 | Adobe Experience Platformの複数の運用データソースの集約をQualtrics Experience IDの入力として使用することで、顧客をより深く理解し、意図、感情、エクスペリエンスの要因を把握する際に、ターゲットを絞ったアウトリーチを実現できます。 |

{style="table-layout:auto"}

**新しい機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| （Beta）計算フィールドでのハッシュ関数のサポート | [配列](../../destinations/ui/export-arrays-maps-objects.md)または配列からの要素の書き出しに特化した関数に加えて、書き出したファイルの属性をハッシュ化するために、追加の[&#x200B; ハッシュ関数](../../destinations/ui/export-arrays-maps-objects.md#hashing-functions)を使用できるようになりました。 サポートされているハッシュ関数は、`sha`、`sha256`、`sha512`、`hash`、`md5`、`crc32`です。 |
| （一般提供の制限）特定の配信先でアカウントオーディエンスを活用する | Real-Time CDP B2Bのお客様は、特定の宛先に[&#x200B; アカウントオーディエンス &#x200B;](../../segmentation/types/account-audiences.md)をアクティブ化できるようになりました。 この機能について詳しくは、[&#x200B; アカウントオーディエンスの有効化のチュートリアル &#x200B;](/help/destinations/ui/activate-account-audiences.md)を参照してください。 |

{style="table-layout:auto"}

**修正と機能強化** {#destinations-fixes-and-enhancements}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformでは、単一のExperience Platformインスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と開発に役立つサンドボックスを提供しています。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | サンドボックスツール機能を使用すると、サンドボックス間の設定の精度を向上させ、サンドボックス間でサンドボックス設定をシームレスに書き出しおよび読み込むことができます。 サンドボックスツール機能を使用して、様々なオブジェクトを選択し、パッケージに書き出すことができます。 詳しくは、[&#x200B; サンドボックスツール UI ガイド &#x200B;](../../sandboxes/ui/sandbox-tooling.md)を参照してください。 |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス（限定GA） | Real-Time Customer Data Platform B2B editionでは、アカウントセグメンテーションを使用して、ピープルベースのオーディエンスからアカウントベースのオーディエンスに至るまで、マーケティングセグメンテーションのエクスペリエンスを容易かつ高度なものにすることができます。 この機能について詳しくは、[&#x200B; アカウントオーディエンスの概要](../../segmentation/types/account-audiences.md)を参照してください。 |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データランディングゾーンの認証を更新しました | 資格情報を表示する際に、データランディングゾーンの指定された有効期限を確認できるようになりました。 アプリケーションで引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定された有効期限より前にトークンを手動で更新しない場合は、次回の資格情報の取得時に自動的に更新され、新しいトークンが提供されます。 詳しくは、[&#x200B; データランディングゾーンの使用](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md)に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[&#x200B; ソースの概要](../../sources/home.md)を参照してください。
