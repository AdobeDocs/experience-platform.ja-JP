---
description: このページでは、メッセージ変換テンプレートに基づいて、宛先に対して書き出されたデータをレンダリングするために、「/authoring/testing/template/render」 API エンドポイントを使用して実行できるすべての API 操作について説明します。
title: レンダリングテンプレート API 操作
exl-id: e64ea89e-6064-4a05-9730-e0f7d7a3e1db
source-git-commit: 6dd8a94e46b9bee6d1407e7ec945a722d8d7ecdb
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 1%

---

# レンダリングテンプレート API 操作 {#render-template-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**: `https://platform.adobe.io/data/core/activation/authoring/testing/template/render`

このページでは、 `/authoring/testing/template/render` API エンドポイント：宛先の想定される形式に一致する、書き出されたプロファイルを、 [メッセージ変換テンプレート](./message-format.md#using-templating). このエンドポイントでサポートされる機能については、 [テンプレートを作成](./create-template.md).

## レンダリングテンプレート API 操作の概要 {#get-started}

続行する前に、 [入門ガイド](./getting-started.md) を参照してください。

## メッセージ変換テンプレートに基づいて、書き出されたプロファイルをレンダリング {#render-exported-data}

書き出されたプロファイルをレンダリングするには、 `authoring/testing/template/render` エンドポイントを作成し、宛先設定の宛先 ID と、 [サンプルテンプレート API エンドポイント](./sample-template-api.md).

まず、変換を適用せずに生のプロファイルを書き出す単純なテンプレートを使用し、次に、変換をプロファイルに適用するより複雑なテンプレートに進むことができます。 シンプルなテンプレートの構文は次のとおりです。 <br> `"template": "{% for profile in input.profiles %}{{profile|raw}}{% endfor %}}"`

>[!TIP]
>
>* ここで使用する必要がある宛先 ID は、です。 `instanceId` を使用して作成された、宛先の設定に対応する `/destinations` endpoint. 詳しくは、 [宛先設定 API 操作](./destination-configuration-api.md#retrieve-list).



**API 形式**


```http
POST authoring/testing/template/render
```

| リクエストパラメーター | 説明 |
| -------- | ----------- |
| `destinationId` | 書き出したプロファイルをレンダリングする宛先設定の ID。 |
| `template` | 書き出されたプロファイルをレンダリングする際に基づく、文字がエスケープされたバージョンのテンプレート。 |
| `profiles` | *オプション*. リクエスト本文にプロファイルを追加できます。 プロファイルを追加しない場合、Experience Platformは自動的にプロファイルを生成し、リクエストに追加します。 <br> 呼び出しの本文にプロファイルを追加する場合は、 [サンプルプロファイル生成 API](./sample-profile-generation-api.md). |

{style=&quot;table-layout:auto&quot;}

レンダリングテンプレート API エンドポイントから返される応答は、宛先の集計ポリシーに基づいて異なることに注意してください。 宛先に設定可能な集計ポリシーがある場合、プロファイルの集計方法を決定する集計キーも応答で返されます。 詳細を表示 [集計ポリシー](./destination-configuration.md#aggregation) を設定します。

| 応答パラメーター | 説明 |
| -------- | ----------- |
| `aggregationKey` | 宛先へのエクスポートでプロファイルを集計するポリシーを表します。 このパラメータはオプションで、宛先の集計ポリシーがに設定されている場合にのみ存在します。 `CONFIGURABLE_AGGREGATION`. |
| `profiles` | リクエストで提供されたプロファイル、またはリクエストでプロファイルが指定されていない場合は自動生成されたプロファイルを表示します。 |
| `output` | 指定されたメッセージ変換テンプレートに基づいて、エスケープされた文字列としてレンダリングされたプロファイル |

以下の節では、上記の両方のケースに関する詳細な要求と応答を示します。

* [ベストエフォート集計とリクエスト本文に含まれるプロファイル](#best-effort)
* [リクエスト本文に含まれる、設定可能な集計とプロファイル](#configurable-aggregation)

### 書き出されたプロファイルを、ベストエフォート集計とリクエスト本文に含まれる単一のプロファイルでレンダリング {#best-effort}

**リクエスト**

次のリクエストでは、宛先で想定されている形式に一致する、書き出されたプロファイルがレンダリングされます。 この例では、宛先 ID は、ベストエフォート集計を使用した宛先設定に対応し、サンプルプロファイルがリクエストの本文に含まれています。

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '
{
    "destinationId": "947c1c46-008d-40b0-92ec-3af86eaf41c1",
    "template": "{#- THIS is an example template for a single profile -#}\r\n{#- A '\''-'\'' at the beginning or end of a tag removes all whitespace on that side of the tag. -#}\r\n{\r\n    \"identities\": [\r\n    {%- for idMapEntry in input.profile.identityMap -%}\r\n    {%- set namespace = idMapEntry.key -%}\r\n        {%- for identity in idMapEntry.value %}\r\n        {\r\n            \"type\": \"{{ namespace }}\",\r\n            \"id\": \"{{ identity.id }}\"\r\n        }{%- if not loop.last -%},{%- endif -%}\r\n        {%- endfor -%}{%- if not loop.last -%},{%- endif -%}\r\n    {% endfor %}\r\n    ],\r\n    \"AdobeExperiencePlatformSegments\": {\r\n        \"add\": [\r\n        {%- for segment in input.profile.segmentMembership.ups | added %}\r\n            \"{{ segment.key }}\"{%- if not loop.last -%},{%- endif -%}\r\n        {% endfor %}\r\n        ],\r\n        \"remove\": [\r\n        {#- Alternative syntax for filtering segments by status: -#}\r\n        {% for segment in removedSegments(input.profile.segmentMembership.ups) %}\r\n            \"{{ segment.key }}\"{%- if not loop.last -%},{%- endif -%}\r\n        {% endfor %}\r\n        ]\r\n    }\r\n}",
    "profiles": [
        {
            "segmentMembership": {
                "ups": {
                    "segmentid1": {
                        "lastQualificationTime": "2021-10-26T16:59:00.828461Z",
                        "status": "existing"
                    },
                    "segmentid3": {
                        "lastQualificationTime": "2021-10-26T16:59:00.828469Z",
                        "status": "exited"
                    },
                    "segmentid2": {
                        "lastQualificationTime": "2021-10-26T16:59:00.828468Z",
                        "status": "realized"
                    }
                }
            },
            "identityMap": {
                "gaid": [
                    {
                        "id": "gaid-BLAcJ"
                    }
                ],
                "idfa": [
                    {
                        "id": "idfa-Iv5AG"
                    }
                ],
                "email": [
                    {
                        "id": "email-rbN62"
                    }
                ]
            },
            "attributes": {
                "key": {
                    "value": "string"
                }
            }
        }
    ]
}'
```

**応答**

応答は、テンプレートのレンダリング結果、または発生したエラーを返します。
正常な応答は、HTTP ステータス 200 と、エクスポートされたデータの詳細を返します。 で、書き出されたプロファイルを見つけます。 `output` パラメーターをエスケープした文字列として渡します。
失敗した応答は、発生したエラーの説明と共に HTTP ステータス 400 を返します。

```json
{
    "results": [
        {
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T16:59:00.828461Z",
                                "status": "existing"
                            },
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T16:59:00.828469Z",
                                "status": "exited"
                            },
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T16:59:00.828468Z",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "gaid": [
                            {
                                "id": "gaid-BLAcJ"
                            }
                        ],
                        "idfa": [
                            {
                                "id": "idfa-Iv5AG"
                            }
                        ],
                        "email": [
                            {
                                "id": "email-rbN62"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"identities\": [\r\n        {\r\n            \"type\": \"gaid\",\r\n            \"id\": \"gaid-BLAcJ\"\r\n        },\r\n        {\r\n            \"type\": \"idfa\",\r\n            \"id\": \"idfa-Iv5AG\"\r\n        },\r\n        {\r\n            \"type\": \"email\",\r\n            \"id\": \"email-rbN62\"\r\n        }\r\n    ],\r\n    \"AdobeExperiencePlatformSegments\": {\r\n        \"add\": [\r\n            \"segmentid1\",\r\n            \"segmentid2\"\r\n        ],\r\n        \"remove\": [\r\n            \"segmentid3\"\r\n        ]\r\n    }\r\n}"
        }
    ]
}    
```

### リクエスト本文に含まれる設定可能な集計とプロファイルを使用して、書き出されたプロファイルをレンダリング {#configurable-aggregation}

**リクエスト**


次のリクエストでは、宛先で想定されている形式に一致する、複数の書き出されたプロファイルがレンダリングされます。 この例では、宛先 ID は、設定可能な集計を使用した宛先設定に対応しています。 リクエストの本文には、2 つのプロファイルが含まれ、それぞれに 3 つのセグメント認定と 5 つの ID が含まれます。 呼び出し時に送信するプロファイルは、 [サンプルプロファイル生成 API](./sample-profile-generation-api.md).

```shell
curl --location --request POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {IMS_ORG}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
--data-raw '{
    "destinationId": "c2bc84c5-589c-43a1-96ea-becfa941f5be",
    "template": "{#- THIS is an example template for multiple profiles -#}\r\n{#- A '\''-'\'' at the beginning or end of a tag removes all whitespace on that side of the tag. -#}\r\n{\r\n    \"profiles\": [\r\n    {%- for profile in input.profiles %}\r\n        {\r\n            \"identities\": [\r\n            {%- for idMapEntry in profile.identityMap -%}\r\n            {%- set namespace = idMapEntry.key -%}\r\n                {%- for identity in idMapEntry.value %}\r\n                {\r\n                    \"type\": \"{{ namespace }}\",\r\n                    \"id\": \"{{ customerData }}\"\r\n                }{%- if not loop.last -%},{%- endif -%}\r\n                {%- endfor -%}{%- if not loop.last -%},{%- endif -%}\r\n            {% endfor %}\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                {%- for segment in profile.segmentMembership.ups | added %}\r\n                    \"{{ segment.key }}\"{%- if not loop.last -%},{%- endif -%}\r\n                {% endfor %}\r\n                ],\r\n                \"remove\": [\r\n                {#- Alternative syntax for filtering segments by status: -#}\r\n                {% for segment in removedSegments(profile.segmentMembership.ups) %}\r\n                    \"{{ segment.key }}\"{%- if not loop.last -%},{%- endif -%}\r\n                {% endfor %}\r\n                ]\r\n            }\r\n        }{%- if not loop.last -%},{%- endif -%}\r\n    {% endfor %}\r\n    ]\r\n}",
    "profiles": [
        {
            "segmentMembership": {
                "ups": {
                    "segmentid1": {
                        "lastQualificationTime": "2021-10-26T17:41:55.947859Z",
                        "status": "existing"
                    },
                    "segmentid3": {
                        "lastQualificationTime": "2021-10-26T17:41:55.947860Z",
                        "status": "exited"
                    },
                    "segmentid2": {
                        "lastQualificationTime": "2021-10-26T17:41:55.947860Z",
                        "status": "realized"
                    }
                }
            },
            "identityMap": {
                "amazon_channel": [
                    {
                        "id": "amazon_channel-biCbJ"
                    }
                ],
                "named_user_id": [
                    {
                        "id": "named_user_id-0Q3hp"
                    }
                ],
                "channel": [
                    {
                        "id": "channel-mN1Hw"
                    }
                ],
                "android_channel": [
                    {
                        "id": "android_channel-MVw4L"
                    }
                ],
                "ios_channel": [
                    {
                        "id": "ios_channel-2OjnN"
                    }
                ]
            },
            "attributes": {
                "key": {
                    "value": "string"
                }
            }
        },
        {
            "segmentMembership": {
                "ups": {
                    "segmentid1": {
                        "lastQualificationTime": "2021-10-26T17:41:55.948187Z",
                        "status": "existing"
                    },
                    "segmentid3": {
                        "lastQualificationTime": "2021-10-26T17:41:55.948188Z",
                        "status": "exited"
                    },
                    "segmentid2": {
                        "lastQualificationTime": "2021-10-26T17:41:55.948188Z",
                        "status": "realized"
                    }
                }
            },
            "identityMap": {
                "amazon_channel": [
                    {
                        "id": "amazon_channel-fxt2p"
                    }
                ],
                "named_user_id": [
                    {
                        "id": "named_user_id-sboQe"
                    }
                ],
                "channel": [
                    {
                        "id": "channel-MRelR"
                    }
                ],
                "android_channel": [
                    {
                        "id": "android_channel-M46ze"
                    }
                ],
                "ios_channel": [
                    {
                        "id": "ios_channel-40Vrf"
                    }
                ]
            },
            "attributes": {
                "key": {
                    "value": "string"
                }
            }
        }
    ]
}'
```

**応答**

応答は、テンプレートのレンダリング結果、または発生したエラーを返します。
正常な応答は、HTTP ステータス 200 と、エクスポートされたデータの詳細を返します。 応答では、セグメントのメンバーシップと ID に基づいてプロファイルを集計する方法に注意します。 で、書き出されたプロファイルを見つけます。 `output` パラメーターをエスケープした文字列として渡します。
失敗した応答は、発生したエラーの説明と共に HTTP ステータス 400 を返します。

```json
{
    "results": [
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid3",
                "segmentStatus": "exited",
                "identityNamespaces": [
                    "android_channel",
                    "amazon_channel",
                    "ios_channel",
                    "channel"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-2OjnN"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-mN1Hw"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-biCbJ"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-MVw4L"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-40Vrf"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-MRelR"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-fxt2p"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-M46ze"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        },
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid1",
                "segmentStatus": "existing",
                "identityNamespaces": [
                    "android_channel",
                    "amazon_channel",
                    "ios_channel",
                    "channel"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-2OjnN"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-mN1Hw"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-biCbJ"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-MVw4L"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-40Vrf"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-MRelR"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-fxt2p"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-M46ze"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        },
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid2",
                "segmentStatus": "realized",
                "identityNamespaces": [
                    "android_channel",
                    "amazon_channel",
                    "ios_channel",
                    "channel"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-2OjnN"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-mN1Hw"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-biCbJ"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-MVw4L"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "ios_channel": [
                            {
                                "id": "ios_channel-40Vrf"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "channel": [
                            {
                                "id": "channel-MRelR"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "amazon_channel": [
                            {
                                "id": "amazon_channel-fxt2p"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "android_channel": [
                            {
                                "id": "android_channel-M46ze"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"ios_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"amazon_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"android_channel\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        },
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid3",
                "segmentStatus": "exited",
                "identityNamespaces": [
                    "named_user_id"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-0Q3hp"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid3": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "exited"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-sboQe"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                ],\r\n                \"remove\": [\r\n                    \"segmentid3\"\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        },
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid1",
                "segmentStatus": "existing",
                "identityNamespaces": [
                    "named_user_id"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-0Q3hp"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid1": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "existing"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-sboQe"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid1\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        },
        {
            "aggregationKey": {
                "destinationInstanceId": "49966037-32cd-4457-a105-2cbf9c01826a",
                "segmentId": "segmentid2",
                "segmentStatus": "realized",
                "identityNamespaces": [
                    "named_user_id"
                ]
            },
            "profiles": [
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.947+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-0Q3hp"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                },
                {
                    "segmentMembership": {
                        "ups": {
                            "segmentid2": {
                                "lastQualificationTime": "2021-10-26T17:41:55.948+0000",
                                "status": "realized"
                            }
                        }
                    },
                    "identityMap": {
                        "named_user_id": [
                            {
                                "id": "named_user_id-sboQe"
                            }
                        ]
                    },
                    "attributes": {
                        "key": {
                            "value": "string"
                        }
                    }
                }
            ],
            "output": "{\r\n    \"profiles\": [\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        },\r\n        {\r\n            \"identities\": [\r\n                {\r\n                    \"type\": \"named_user_id\",\r\n                    \"id\": \"{moviestar_region=dIqYn-moviestar_region}\"\r\n                }\r\n            ],\r\n            \"AdobeExperiencePlatformSegments\": {\r\n                \"add\": [\r\n                    \"segmentid2\"\r\n                ],\r\n                \"remove\": [\r\n                ]\r\n            }\r\n        }\r\n    ]\r\n}"
        }
    ]
}
```

## API エラー処理 {#api-error-handling}

Destination SDKAPI エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読み終えたら、メッセージ変換テンプレートを使用して、宛先で期待されるデータ形式に一致する、書き出されたプロファイルを生成する方法を理解できました。 読み取り [宛先の設定にDestination SDKを使用する方法](./configure-destination-instructions.md) を参照して、この手順が宛先を設定するプロセスに適した場所を把握します。
