---
solution: Experience Platform
title: 外部オーディエンスの読み込みと使用
description: このチュートリアルでは、外部オーディエンスをAdobe Experience Platformで使用する方法について説明します。
exl-id: 56fc8bd3-3e62-4a09-bb9c-6caf0523f3fe
hide: true
hidefromtoc: true
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 4%

---

# 外部オーディエンスの読み込みと使用

>[!IMPORTANT]
>
>このドキュメントには、以前のバージョンの Audiences ドキュメントの情報が含まれているため、は古くなっています。

Adobe Experience Platformは、外部オーディエンスを読み込む機能をサポートしています。この機能は、その後新しいオーディエンスのコンポーネントとして使用できます。 このドキュメントでは、外部オーディエンスを読み込んで使用するためのExperience Platformの設定に関するチュートリアルを提供します。

## はじめに

このチュートリアルでは、オーディエンスの作成に関連する様々な [!DNL Adobe Experience Platform] サービスについての十分な知識が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [ セグメント化サービス ](../home.md)：リアルタイム顧客プロファイルデータからオーディエンスを作成できます。
- [リアルタイム顧客プロファイル](../../profile/home.md)：複数のソースから集計したデータに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [ エクスペリエンスデータモデル（XDM） ](../../xdm/home.md):Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。 セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。
- [データセット](../../catalog/datasets/overview.md)：Experience Platform のデータ永続化のためのストレージと管理の構成。
- [ ストリーミング取得 ](../../ingestion/streaming-ingestion/overview.md):Experience Platformがクライアントサイドおよびサーバーサイドのデバイスからリアルタイムにデータを取得して保存する方法。

### オーディエンスとセグメントの定義

外部オーディエンスの読み込みと使用を開始する前に、オーディエンスとセグメント定義の違いを理解しておくことが重要です。

オーディエンスとは、フィルタリング対象のプロファイルのグループを指します。 セグメント定義を使用する場合は、セグメントの選定条件を満たすサブセットにプロファイルをフィルタリングするセグメント定義を作成して、オーディエンスを作成できます。

セグメント定義には、名前、説明、式（該当する場合）、作成日、最終変更日、ID などの情報が含まれます。 ID は、セグメントメタデータを、セグメントの選定を満たし、結果として生じるオーディエンスの一部である個々のプロファイルにリンクします。

| オーディエンス | セグメント定義 |
| --------- | ---------------- |
| 検索しようとしているプロファイルのグループ。 セグメント定義を使用する場合、これは、セグメントの選定を満たすプロファイルのグループになります。 | 検索するオーディエンスをセグメント化するために使用されるルールのグループ。 |

## 外部オーディエンスの ID 名前空間の作成

外部オーディエンスを使用する最初の手順は、ID 名前空間を作成することです。 ID 名前空間を使用すると、Platform はオーディエンスの発信元を関連付けることができます。

ID 名前空間を作成するには、[ID 名前空間ガイド ](../../identity-service/features/namespaces.md#manage-namespaces) の手順に従います。 ID 名前空間を作成する際に、ソースの詳細を ID 名前空間に追加し、その [!UICONTROL  タイプ ] を **[!UICONTROL 人物以外の識別子]** としてマークします。

![ 人物以外の識別子は、ID 名前空間を作成モーダルでハイライト表示されます ](../images/tutorials/external-audiences/identity-namespace-info.png)。

## セグメントメタデータのスキーマの作成

ID 名前空間を作成したら、作成するセグメントの新しいスキーマを作成する必要があります。

スキーマの作成を開始するには、まず左側のナビゲーションバーで **[!UICONTROL スキーマ]** を選択したあと、スキーマ ワークスペースの右上隅にある **[!UICONTROL スキーマを作成]** を選択します。 ここから **[!UICONTROL 参照]** を選択すると、使用可能なスキーマタイプの完全な選択が表示されます。

![ 「スキーマを作成」と「参照」の両方がハイライト表示されている様子 ](../images/tutorials/external-audiences/create-schema-browse.png)

セグメント定義（事前定義済みのクラス）を作成するので、「**[!UICONTROL 既存のクラスを使用]**」を選択します。 次に、**[!UICONTROL セグメント定義]** クラス、**[!UICONTROL 割り当てクラス]** の順に選択します。

![ セグメント定義クラスがハイライト表示されている様子 ](../images/tutorials/external-audiences/assign-class.png)

スキーマが作成されたので、セグメント ID を含むフィールドを指定する必要があります。 このフィールドは、プライマリ ID としてマークされ、以前に作成した名前空間に割り当てられている必要があります。

![ スキーマエディターで、選択したフィールドをプライマリ ID としてマークするチェックボックスがハイライト表示されます。](../images/tutorials/external-audiences/mark-primary-identifier.png)

`_id` フィールドをプライマリ ID としてマークした後、スキーマのタイトルを選択し、続けて **[!UICONTROL プロファイル]** というラベルの付いた切り替えを選択します。 「**[!UICONTROL 有効]**」を選択して、[!DNL Real-Time Customer Profile] のスキーマを有効にします。

![ プロファイルのスキーマを有効にする切替スイッチがスキーマエディターでハイライト表示されています。](../images/tutorials/external-audiences/schema-profile.png)

これで、このスキーマがプロファイルに対して有効になり、プライマリ ID が、作成した非人物 ID 名前空間に割り当てられます。 その結果、このスキーマを使用して Platform に読み込まれたセグメントメタデータは、他の人物に関連するプロファイルデータと結合されずに、プロファイルに取り込まれます。

## スキーマのデータセットの作成

スキーマを設定したら、セグメントメタデータのデータセットを作成する必要があります。

データセットを作成するには、[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#create) の手順に従ってください。 前に作成したスキーマを使用して、「**[!UICONTROL スキーマからデータセットを作成]**」オプションに従う必要があります。

![ データセットのベースにするスキーマがハイライト表示されます。](../images/tutorials/external-audiences/select-schema.png)

データセットを作成したら、[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#enable-profile) の手順に従って続行し、このデータセットをリアルタイム顧客プロファイルに対して有効にします。

![ データセットアクティビティページで、プロファイルのスキーマを有効にする切替スイッチがハイライト表示されています。](../images/tutorials/external-audiences/dataset-profile.png)

## オーディエンスデータの設定と読み込み

データセットを有効にすると、UI またはExperience PlatformAPI を使用してデータを Platform に送信できるようになります。 このデータは、バッチ接続またはストリーミング接続を使用して取り込むことができます。

### バッチ接続を使用したデータの取り込み

バッチ接続を作成するには、汎用の [ ローカルファイルアップロード UI ガイド ](../../sources/tutorials/ui/create/local-system/local-file-upload.md) の手順に従ってください。 データの取り込みに使用できる使用可能なソースの完全なリストについては、[ ソースの概要 ](../../sources/home.md) を参照してください。

### ストリーミング接続を使用したデータの取り込み

ストリーミング接続を作成するには、[API チュートリアル ](../../sources/tutorials/api/create/streaming/http.md) または [UI チュートリアル ](../../sources/tutorials/ui/create/streaming/http.md) の手順に従います。

ストリーミング接続を作成すると、データの送信先となる一意のストリーミングエンドポイントにアクセスできるようになります。 これらのエンドポイントにデータを送信する方法については、[ レコードデータのストリーミングに関するチュートリアル ](../../ingestion/tutorials/streaming-record-data.md#ingest-data) を参照してください。

![ ソースの詳細ページで、ストリーミング接続のストリーミングエンドポイントがハイライト表示されています。](../images/tutorials/external-audiences/get-streaming-endpoint.png)

## オーディエンスメタデータの構造

接続を作成したら、データを Platform に取り込むことができます。

外部オーディエンスペイロードのメタデータのサンプルを以下に示します。

```json
{
    "header": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        },
        "imsOrgId": "{ORG_ID}",
        "datasetId": "{DATASET_ID}",
        "source": {
            "name": "Sample External Audience"
        }
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "xdmEntity": {
            "_id": "{SEGMENT_ID}",
            "description": "Sample description",
            "identityMap": {
                "{IDENTITY_NAMESPACE}": [{
                    "id": "{}"
                }]
            },
            "segmentName" : "{SEGMENT_NAME}",
            "segmentStatus": "ACTIVE",
            "version": "1.0"
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef` | スキーマは、セグメントメタデータ用に以前に作成したスキーマを参照する **必要があります**。 |
| `datasetId` | データセット ID **必須** は、先ほど作成したスキーマ用に以前に作成したデータセットを参照します。 |
| `xdmEntity._id` | ID **必須** は、外部オーディエンスとして使用しているのと同じセグメント ID を参照します。 |
| `xdmEntity.identityMap` | このセクションには **必ず**、以前に作成した名前空間の作成時に使用される ID ラベルを含めます。 |
| `{IDENTITY_NAMESPACE}` | これは、以前に作成した ID 名前空間のラベルです。 例えば、ID 名前空間を「externalAudience」と呼んだ場合は、それを配列のキーとして使用します。 |
| `segmentName` | 外部オーディエンスのセグメント化元となるセグメントの名前。 |

## 読み込んだオーディエンスを使用したセグメントの作成

読み込んだオーディエンスを設定したら、セグメント化プロセスの一部として使用できます。 外部オーディエンスを検索するには、セグメントビルダーに移動し、「**[!UICONTROL フィールド]**」セクションの「**[!UICONTROL オーディエンス]**」タブを選択します。

![ セグメントビルダーの外部オーディエンスセレクターがハイライト表示されています。](../images/tutorials/external-audiences/external-audiences.png)

## 次の手順

セグメントで外部オーディエンスを使用できるようになったので、セグメントビルダーを使用してセグメントを作成できます。 セグメントの作成方法について詳しくは、[ セグメントの作成に関するチュートリアル ](./create-a-segment.md) を参照してください。

## 付録

読み込んだ外部オーディエンスメタデータを使用してセグメントの作成に使用するほか、外部セグメントメンバーシップを Platform に読み込むこともできます。

### 外部セグメントメンバーシップの宛先スキーマの設定

スキーマの作成を開始するには、まず左側のナビゲーションバーで **[!UICONTROL スキーマ]** を選択したあと、スキーマ ワークスペースの右上隅にある **[!UICONTROL スキーマを作成]** を選択します。 ここから、「**[!UICONTROL XDM 個人プロファイル]**」を選択します。

![XDM 個人プロファイル領域がハイライト表示されています。](../images/tutorials/external-audiences/create-schema-profile.png)

スキーマを作成したら、セグメントメンバーシップフィールドグループをスキーマの一部として追加する必要があります。 これを行うには、「[!UICONTROL  セグメントメンバーシップの詳細 ]」を選択し、続いて「[!UICONTROL  フィールドグループの追加 ] を選択します。

![ セグメントメンバーシップの詳細フィールドグループがハイライト表示されています。](../images/tutorials/external-audiences/segment-membership-details.png)

さらに、スキーマが **[!UICONTROL プロファイル]** 用にマークされていることを確認します。 これを行うには、フィールドをプライマリ ID としてマークする必要があります。

![ プロファイルのスキーマを有効にする切替スイッチがスキーマエディターでハイライト表示されています。](../images/tutorials/external-audiences/external-segment-profile.png)

### データセットの設定

スキーマを作成したら、データセットを作成する必要があります。

データセットを作成するには、[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#create) の手順に従ってください。 前に作成したスキーマを使用して、「**[!UICONTROL スキーマからデータセットを作成]**」オプションに従う必要があります。

![ データベースの作成に使用するスキーマがハイライト表示されています。](../images/tutorials/external-audiences/select-schema.png)

データセットを作成したら、[ データセットユーザーガイド ](../../catalog/datasets/user-guide.md#enable-profile) の手順に従って続行し、このデータセットをリアルタイム顧客プロファイルに対して有効にします。

![ データセットを作成ワークフローで、プロファイルのスキーマを有効にする切替スイッチがハイライト表示されています。](../images/tutorials/external-audiences/dataset-profile.png)

## 外部オーディエンスメンバーシップデータの設定と読み込み

データセットを有効にすると、UI またはExperience PlatformAPI を使用してデータを Platform に送信できるようになります。 このデータは、バッチ接続またはストリーミング接続を使用して取り込むことができます。

### バッチ接続を使用したデータの取り込み

バッチ接続を作成するには、汎用の [ ローカルファイルアップロード UI ガイド ](../../sources/tutorials/ui/create/local-system/local-file-upload.md) の手順に従ってください。 データの取り込みに使用できる使用可能なソースの完全なリストについては、[ ソースの概要 ](../../sources/home.md) を参照してください。

### ストリーミング接続を使用したデータの取り込み

ストリーミング接続を作成するには、[API チュートリアル ](../../sources/tutorials/api/create/streaming/http.md) または [UI チュートリアル ](../../sources/tutorials/ui/create/streaming/http.md) の手順に従います。

ストリーミング接続を作成すると、データの送信先となる一意のストリーミングエンドポイントにアクセスできるようになります。 これらのエンドポイントにデータを送信する方法については、[ レコードデータのストリーミングに関するチュートリアル ](../../ingestion/tutorials/streaming-record-data.md#ingest-data) を参照してください。

![ ソースの詳細ページで、ストリーミング接続のストリーミングエンドポイントがハイライト表示されています。](../images/tutorials/external-audiences/get-streaming-endpoint.png)

## セグメントメンバーシップ構造

接続を作成したら、データを Platform に取り込むことができます。

外部オーディエンスメンバーシップペイロードの例を次に示します。

```json
{
    "header": {
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;version=1"
        },
        "imsOrgId": "{ORG_ID}",
        "datasetId": "{DATASET_ID}",
        "source": {
            "name": "Sample External Audience Membership"
        }
    },
    "body": {
        "xdmMeta": {
            "schemaRef": {
                "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
                "contentType": "application/vnd.adobe.xed-full+json;version=1"
            }
        },
        "xdmEntity": {
            "_id": "{UNIQUE_ID}",
            "description": "Sample description",
            "{TENANT_NAME}": {
                "identities": {
                    "{SCHEMA_IDENTITY}": "sample-id"
                }
            },
            "personId" : "sample-name",
            "segmentMembership": {
                "{IDENTITY_NAMESPACE}": {
                    "{EXTERNAL_IDENTITY}": {
                        "status": "realized",
                        "lastQualificationTime": "2022-03-14T:00:00:00Z"
                    }
                }
            }
        }
    }
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `schemaRef` | スキーマは **必ず**、セグメントメンバーシップデータ用に以前に作成したスキーマを参照します。 |
| `datasetId` | データセット ID **必須** は、作成したばかりのメンバーシップスキーマの作成済みデータセットを参照します。 |
| `xdmEntity._id` | データセット内のレコードを一意に識別するために使用される適切な ID。 |
| `{TENANT_NAME}.identities` | このセクションは、カスタム ID のフィールドグループを、以前に読み込んだユーザーと接続するために使用されます。 |
| `segmentMembership.{IDENTITY_NAMESPACE}` | これは、以前に作成したカスタム ID 名前空間のラベルです。 例えば、ID 名前空間を「externalAudience」と呼んだ場合は、それを配列のキーとして使用します。 |

>[!NOTE]
>
>デフォルトでは、外部オーディエンスのメンバーシップは 30 日後に削除されます。 削除を防ぎ、30 日以上保持するには、オーディエンスデータを取り込む際に `validUntil` フィールドを使用してください。 このフィールドについて詳しくは、[ セグメントメンバーシップの詳細スキーマフィールドグループ ](../../xdm/field-groups/profile/segmentation.md) に関するガイドを参照してください。
