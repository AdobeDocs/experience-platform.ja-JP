---
title: Adobe Experience Platform リリースノート 2023年10月
description: Adobe Experience Platform の 2023年10月のリリースノート。
exl-id: e9cf5299-8350-4b40-8f56-05e598846875
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1057'
ht-degree: 37%

---

# Adobe Experience Platform リリースノート

**リリース日：2023 年 10 月 25 日**

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
| 宛先の使用状況指標 | ライセンス使用状況ダッシュボードに新しい計測指標が追加されました。 **[!UICONTROL Audience Activationのサイズ]** と **[!UICONTROL データ書き出しのサイズ]** の指標は、ライセンス使用権限に関して、Experience Platformから書き出したデータの量を追跡する便利な方法を提供します。 これらを始めとするライセンス使用状況指標の説明については、[&#x200B; 利用可能な指標 &#x200B;](../../dashboards/guides/license-usage.md#available-metrics) のドキュメントを参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Meta] Conversions API の機能強化 | [Meta Conversions API](/help/tags/extensions/server/meta/overview.md) 拡張機能は 3 つ強化されました。 <ul><li>[[!DNL Meta Business Extension (MBE)]](/help/tags/extensions/server/meta/overview.md#integration-with-meta-business-extension-mbe) との統合：Conversions API とAdobeの統合の pixelID およびアクセストークンを共有できるようにすることで、シームレスなログインエクスペリエンスを作成します。</li><li>[[!DNL Event Match Quality Score (EMQ)]](/help/tags/extensions/server/meta/overview.md#integration-with-event-quality-match-score-emq) との統合：目的のアクションを完了する可能性が高い人物に広告を配信し、アクションを配信された広告にリンクすることができます。</li><li>[[!DNL LiveRamp (Alpha)]](/help/tags/extensions/server/meta/overview.md#integration-with-liveramp-alpha) との統合：LiveRamp の RampID を CIP フィールドに渡すことができるため、パートナーや Meta と直接 PII を共有する必要がなくなります。 </li></ul> |
| 拡張機能 | [!DNL LinkedIn] Conversions API | [[!DNL LinkedIn] Conversions API](../../tags/extensions/server/linkedin/overview.md) 拡張機能を使用すると、Experience Platform イベントデータを LinkedIn に転送することで、LinkedIn マーケティングキャンペーンの有効性を評価できます。 |
| 秘密鍵 | [!DNL LinkedIn] OAuth2 シークレット | [[!DNL LinkedIn] OAuth 2 シークレット &#x200B;](../../tags/ui/event-forwarding/secrets.md#linkedin-oauth-2) を使用すると、イベント転送で、サーバー間インタラクションを [!DNL LinkedIn] に送信できます。 |
| イベント転送 | タグとイベント転送の更新 | Experience Platformの [&#x200B; タグ &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja) と [&#x200B; イベント転送 &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/event-forwarding/overview.html?lang=ja) のパフォーマンスを保持するために、成功と失敗の両方の最新の Development ビルドと Stage ビルドのみが保持されます。 使用されなくなったビルドはすべて削除されます。 さらに、スロットルとレート制限が実装され、少数の大量の API 使用によって他のユーザーの API パフォーマンスが低下することがなくなりました。 |
| 拡張機能 | 要素、ルールおよび拡張機能 | [&#x200B; 要素、ルール、拡張機能 &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/overview.html?lang=ja) がライブラリ出力で並べ替えられ、複数のビルドと同じライブラリのデプロイメント間の一貫性が確保されるようになりました。 |

データ収集について詳しくは、[データ収集の概要](../../tags/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 新規／アップデート | 説明 |
| ----------- |----------------|----------- |
| [[!DNL MoEngage]](/help/destinations/catalog/mobile-engagement/moengage.md) | 新規 | Moengage の宛先を使用して、Adobe データ（ユーザー属性、セグメント、イベント）を MoEngage にリアルタイムに接続およびマッピングします。 その後、顧客はこのデータに基づいて行動し、パーソナライズされたターゲット設定されたエクスペリエンスを提供できます。 |
| [[!DNL Qualtrics Automations]](/help/destinations/catalog/survey/qualtrics-automations.md) | 新規 | Adobe Experience Platformの複数のオペレーショナルデータソースの集計を Qualtrics Experience ID の入力として使用することで、顧客をより深く理解し、目的、感情、エクスペリエンスの要因を理解する際に、ターゲットを絞ったアウトリーチによってギャップを埋めることができます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| （Beta）計算フィールドでのハッシュ関数のサポート | 配列または配列から要素を書き出すための [&#x200B; 特定の &#x200B;](../../destinations/ui/export-arrays-maps-objects.md) 関数に加えて、追加の [&#x200B; ハッシュ関数 &#x200B;](../../destinations/ui/export-arrays-maps-objects.md#hashing-functions) を使用して、書き出されたファイルの属性をハッシュ化できるようになりました。 サポートされているハッシュ関数は、`sha`、`sha256`、`sha512`、`hash`、`md5`、`crc32` です。 |
| （限定的な GA）特定の宛先に対するアカウントオーディエンスのアクティブ化 | Real-Time CDP B2B のお客様は、特定の宛先に対して [&#x200B; アカウントオーディエンス &#x200B;](../../segmentation/types/account-audiences.md) をアクティブ化できるようになりました。 この機能について詳しくは、[&#x200B; アカウントオーディエンスの有効化チュートリアル &#x200B;](/help/destinations/ui/activate-account-audiences.md) を参照してください。 |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | サンドボックスツール機能を使用すると、サンドボックス間の設定精度を向上させ、サンドボックス設定の書き出しと読み込みをサンドボックス間でシームレスに行うことができます。 サンドボックスツール機能を使用して、様々なオブジェクトを選択し、それらをパッケージに書き出すことができます。 詳しくは、[&#x200B; サンドボックスツール UI ガイド &#x200B;](../../sandboxes/ui/sandbox-tooling.md) を参照してください。 |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス （限定 GA） | Real-Time Customer Data Platform B2B editionでは、アカウントセグメント化を使用して、ユーザーベースのオーディエンスからアカウントベースのオーディエンスまで、マーケティングセグメント化エクスペリエンスを完全に簡単かつ高度なものにすることができます。 この機能について詳しくは、[&#x200B; アカウントオーディエンスの概要 &#x200B;](../../segmentation/types/account-audiences.md) を参照してください。 |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データランディングゾーンの認証を更新しました | 資格情報を表示する際に、データランディングゾーンに指定された有効期限を表示できるようになりました。 アプリケーションで引き続き使用するには、有効期限より前にトークンを更新する必要があります。 指定された有効期限の前にトークンを手動で更新しない場合、次回資格情報を取得する際に自動的に更新され、新しいトークンが提供されます。 詳しくは、[&#x200B; データランディングゾーンの使用 &#x200B;](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md) に関するドキュメントを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[&#x200B; ソースの概要 &#x200B;](../../sources/home.md) を参照してください。
