---
description: このページでは、「/authoring/sample-profiles」 API エンドポイントを使用して実行できる、宛先テストで使用するサンプルプロファイルを生成する API 操作の一覧と説明を示します。
title: プロファイル生成 API 操作の例
exl-id: 5f1cd00a-8eee-4454-bcae-07b05afa54af
source-git-commit: 2ed132cd16db64b5921c5632445956f750fead56
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 3%

---

# プロファイル生成 API 操作の例 {#sample-profile-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/sample-profiles`

このページでは、`/authoring/sample-profiles` API エンドポイントを使用して実行できるすべての API 操作について説明します。

この API エンドポイントを使用すると、使用するサンプルプロファイルを生成できます。
* メッセージ変換テンプレートのテスト時。 詳しくは、[ メッセージ変換テンプレート ](./create-template.md) の作成とテストを参照してください。
* 宛先が正しく設定されているかどうかをテストする場合。 詳しくは、[ 宛先の設定 ](./test-destination.md) をテストしてください。

サンプルプロファイルは、AdobeXDM ソーススキーマか、宛先でサポートされているターゲットスキーマのどちらかに基づいて生成できます。 Adobeの XDM ソーススキーマとターゲットスキーマの違いを理解するには、[ メッセージ形式 ](./message-format.md) の記事を参照してください。

## サンプルプロファイル生成 API 操作の概要 {#get-started}

続行する前に、[ はじめに ](./getting-started.md) を参照して、必要な宛先オーサリング権限や必要なヘッダーの取得方法など、API を正しく呼び出すために知っておく必要がある重要な情報を確認してください。

## ソーススキーマに基づくサンプルプロファイルの生成 {#generate-sample-profiles-source-schema}

>[!IMPORTANT]
>
>[ 宛先 ](./test-destination.md) をテストする際に、ここで生成したサンプルプロファイルを HTTP 呼び出しに追加します。

`authoring/sample-profiles/` エンドポイントにGETリクエストを送信し、テストする宛先設定に基づいて作成した宛先インスタンスの ID を指定することで、ソーススキーマに基づいてサンプルプロファイルを生成できます。

>[!TIP]
>
>* 宛先との接続を参照する際に、URL からここで使用する必要がある宛先インスタンス ID を取得します。
   >![UI 画像宛先インスタンス ID の取得方法](./assets/get-destination-instance-id.png)


**API 形式**


```http
GET authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}&count={COUNT}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルを生成する際の基になる宛先インスタンスの ID。 |
| `{COUNT}` | *オプション*. 生成するサンプルプロファイルの数。 このパラメーターは `1 - 1000` の間の値を取ることができます。 <br> count パラメーターを指定しない場合、生成されるプロファイルのデフォルト数は、宛先サーバー `maxUsersPerRequest` 設定の値 [によって決まります](./destination-server-api.md#create)。このプロパティを定義しない場合、Adobeは 1 つのサンプルプロファイルを生成します。 |

{style=&quot;table-layout:auto&quot;}


**リクエスト**

次のリクエストは、`{DESTINATION_INSTANCE_ID}` および `{COUNT}` クエリパラメーターで設定されたサンプルプロファイルを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationInstanceId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された数のサンプルプロファイル、およびソース XDM スキーマに対応するセグメントメンバーシップ、ID、プロファイル属性を返します。

>[!TIP]
>
> 応答は、宛先インスタンスで使用されるセグメントのメンバーシップ、ID、プロファイル属性のみを返します。 ソーススキーマに他のフィールドが含まれている場合でも、それらは無視されます。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-7VEsJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Y55JJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Nd9GK"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    }
]
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentMembership` | 個人のセグメントメンバーシップを表す map オブジェクト。 `segmentMembership` の詳細については、[Segment Membership Details](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html) を参照してください。 |
| `lastQualificationTime` | このプロファイルが最後にセグメントに適合した時刻のタイムスタンプ。 |
| `xdm:status` | 現在のリクエストの一部としてセグメントのメンバーシップが認識されたかどうかを示します。 次の値を使用できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントの一部であり、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントに入っています。</li><li>`exited`:プロファイルは、現在のリクエストの一部としてセグメントから退出しています。</li></ul> |
| `identityMap` | 個人の様々な ID 値と、関連する名前空間を示す map-type フィールド。 `identityMap` の詳細は、[ スキーマ構成の基礎 ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=en#identityMap) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## ターゲットスキーマに基づくサンプルプロファイルの生成 {#generate-sample-profiles-target-schema}

>[!IMPORTANT]
>
>テンプレートの作成時にここで生成したサンプルプロファイルを、[ レンダリングテンプレートの手順 ](./render-template-api.md#multiple-profiles-with-body) で使用します。

`authoring/sample-profiles/` エンドポイントに対してGETリクエストをおこなうターゲットスキーマに基づいて、サンプルプロファイルを生成し、テンプレートを作成するターゲット設定に基づいて宛先設定の宛先 ID を指定できます。

>[!TIP]
>
>* ここで使用する宛先 ID は、`/destinations` エンドポイントを使用して作成された、宛先設定に対応する `instanceId` です。 [ 宛先設定 API リファレンス ](./destination-configuration-api.md#retrieve-list) を参照してください。


**API 形式**


```http
GET authoring/sample-profiles?destinationId={DESTINATION_ID}&count={COUNT}
```

| クエリパラメーター | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | サンプルプロファイルを生成する場所に基づく宛先設定の ID。 |
| `{COUNT}` | *オプション*. 生成するサンプルプロファイルの数。 このパラメーターは `1 - 1000` の間の値を取ることができます。 <br> count パラメーターを指定しない場合、生成されるプロファイルのデフォルト数は、宛先サーバー `maxUsersPerRequest` 設定の値 [によって決まります](./destination-server-api.md#create)。このプロパティを定義しない場合、Adobeは 1 つのサンプルプロファイルを生成します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、`{DESTINATION_ID}` および `{COUNT}` クエリパラメーターで設定されたサンプルプロファイルを生成します。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、HTTP ステータス 200 と、指定された数のサンプルプロファイル、およびターゲット XDM スキーマに対応するセグメントメンバーシップ、ID、プロファイル属性を返します。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609326Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-vizii"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-adKYs"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-t4sKv"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-C3enB"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-bfnbs"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609626Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-6YjGc"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-SfJ21"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-eQMWS"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-d3WzP"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-eWfFn"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609823Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-2PMjZ"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-3aLez"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-D2H1J"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-i6PsF"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-VPUtZ"
                }
            ]
        }
    }
]
```

## API エラー処理 {#api-error-handling}

宛先 SDK API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 Platform トラブルシューティングガイドの [API ステータスコード ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#api-status-codes) および [ リクエストヘッダーのエラー ](https://experienceleague.adobe.com/docs/experience-platform/landing/troubleshooting.html?lang=en#request-header-errors) を参照してください。

## 次の手順

このドキュメントを読むと、[ メッセージ変換テンプレート ](./create-template.md) のテスト時、または [ テスト時に、](./test-destination.md) 宛先が正しく設定されているかどうかをテストする際に使用するサンプルプロファイルを生成できます。
