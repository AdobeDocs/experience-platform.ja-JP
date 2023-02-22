---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platformの 2023 年 2 月のリリースノート。
source-git-commit: 38c9325e2eb5d396472ea55ca082083040d6e590
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 35%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年2月22日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time CDP B2B Edition の関連するアカウント](#related-accounts)
- [ソース](#sources)

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**
&#x200B; |機能 |説明 | | — | — | | UI を通じたフィールドの廃止 |データの取り込み後に、スキーマのフィールドを非推奨にできるようになりました。 XDM フィールドの廃止により、UI ビューからフィールドを削除しながら、使用するためにフィールドを保持できます。 廃止されたフィールドを必要に応じて再度表示できます。また、そのフィールドを参照するセグメント、クエリ、ダウンストリームソリューションは、通常どおりに実行されます。 |

{style=&quot;table-layout:auto&quot;} Platform の XDM について詳しく&#x200B;は、 [XDM システムの概要](../../xdm/home.md).&#x200B;
<!-- Field deprecation: https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/field-deprecation.html -->

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合し、クエリ結果を新しいデータセットとして取り込んで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**
&#x200B; |機能 |説明 | | — | — | | SQL を使用したプロファイルのデータセットの有効化 | CTAS クエリで LABEL を使用してデータセットを「プロファイルを有効にする」か、ALTER を使用して既存のデータセットを更新し、プロファイルに対して有効にします。 | |スケジュールクエリの監視 | 「スケジュール済みクエリ」タブを使用して、クエリの実行に関する重要な情報を見つけ、アラートを購読します。 クエリを監視して、スケジュールの詳細、ステータス、エラーメッセージ/コードが失敗した場合に表示されます。  | |オートコンプリート機能を切り替え |クエリエディターのオートコンプリート機能を切り替えることで、特定のメタデータコマンドをなくし、処理時間を短縮します。 この機能では、クエリの記述時に、SQL キーワードの候補と表の詳細が自動的に提示されます。 | |データセットのサンプル |クエリでサンプリングレートを指定し、データセットサンプルを使用して均一なランダムサンプルを作成するか、特定の条件に基づいて条件付きサンプルを作成します。 |

{style=&quot;table-layout:auto&quot;} クエリサービスについ&#x200B;て詳しくは、 [クエリサービスの概要](../../query-service/home.md).&#x200B;
<!-- Links for QS feature docs after release day: -->
<!-- Enable datasets for profile with SQL link: https://experienceleague.adobe.com/docs/experience-platform/query/sql/syntax.html#create-table-as-select -->
<!-- Monitor scheduled queries link: https://experienceleague.adobe.com/docs/experience-platform/query/monitor-queries.html  -->
<!-- Toggle auto-complete feature link: https://experienceleague.adobe.com/docs/experience-platform/query/ui/user-guide.html#auto-complete -->
<!-- dataset samples: https://experienceleague.adobe.com/docs/experience-platform/query/essential-concepts/dataset-samples.html -->

## Real-Time CDP B2B Edition の関連するアカウント {#related-accounts}

>[!NOTE]
>
>関連するアカウント機能は、Real-Time CDP B2B Edition のお客様のみ使用できます。

関連するアカウント [!DNL Real-Time CDP B2B] を使用すると、参照しているアカウントに類似したアカウントのリストを表示できます。 関連するアカウントをセグメント定義に含めることができるので、リーチを広げたり、セグメントでより広い条件を適用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 関連アカウントサービスを有効にする | 新しい切り替え機能を使用すると、お使いのアカウントで関連アカウントサービスを有効にできます。 詳しくは、 [関連アカウントサービスの有効化](../../rtcdp/b2b-ai-ml-services/related-accounts.md#enable). |

{style=&quot;table-layout:auto&quot;}

関連するアカウントの機能について詳しくは、次のドキュメントページを参照してください。

- [Real-Time CDP B2B Edition の関連するアカウントの概要](../../rtcdp/b2b-ai-ml-services/related-accounts.md)
- [アカウントプロファイル UI ガイドの「関連するアカウント」タブ](../../rtcdp/accounts/account-profile-ui-guide.md#related-accounts-tab)
- [セグメント定義での関連するアカウントの使用方法](../../rtcdp/segmentation/b2b.md#related-accounts)

Real-Time CDP B2B Edition の詳細については、 [Real-Time CDP B2B Edition の概要](../../rtcdp/overview.md).

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込むことができ、Platform Services を使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 次を使用してサブスクリプションレベルのアクセスを指定 [!DNL Google PubSub] | を使用する際に、特定のトピック購読へのアクセスを定義できるようになりました。 [!DNL Google PubSub] 認証時に購読 ID を指定してソースを作成する。 詳しくは、 [!DNL Google PubSub] 認証チュートリアル [フローサービス API の使用](../../sources/tutorials/api/create/cloud-storage/google-pubsub.md) または [Platform UI](../../sources/tutorials/ui/create/cloud-storage/google-pubsub.md). |
| カスタムアクティビティデータの取り込み元 [!DNL Marketo] | これで、 [!DNL Marketo] インスタンスからExperience Platformへ。 カスタムアクティビティデータを取り込むには、B2B アクティビティスキーマでカスタムアクティビティフィールドグループを設定し、アクティビティデータセットを使用してデータフローを作成する必要があります。 データフローが完了すると、取り込まれたデータセットには、 [!DNL Marketo] インスタンス。 その後、 [クエリサービス](../../query-service/home.md) をクリックして、Platform のカスタムアクティビティレコードにアクセスします。 詳しくは、 [カスタムアクティビティデータのデータフローの作成](../../sources/tutorials/ui/create/adobe-applications/marketo-custom-activities.md). |
| 次の場所から未要求のアカウントを除外 [!DNL Marketo] | 会社データのデータフローを作成する際に、取得から要求されていないアカウントを除外するか、取得から含めるかを設定できるようになりました。 詳しくは、 [のソース接続とデータフローの作成 [!DNL Marketo]](../../sources/tutorials/ui/create/adobe-applications/marketo.md). |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。