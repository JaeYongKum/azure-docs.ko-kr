---
title: "Azure Data Factory를 사용하여 Sybase에서 데이터 복사 | Microsoft Docs"
description: "Azure Data Factory 파이프라인의 복사 작업을 사용하여 Sybase에서 지원되는 싱크 데이터 저장소로 데이터를 복사하는 방법에 대해 알아봅니다."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: jingwang
ms.openlocfilehash: ff008b6fdfe9e248a0588f24a1cb87b39ca8d90c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2017
---
# <a name="copy-data-from-sybase-using-azure-data-factory"></a>Azure Data Factory를 사용하여 Sybase에서 데이터 복사
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [버전 1 - GA](v1/data-factory-onprem-sybase-connector.md)
> * [버전 2 - 미리 보기](connector-sybase.md)

이 문서에서는 Azure Data Factory의 복사 작업을 사용하여 Sybase 데이터베이스에서 데이터를 복사하는 방법을 설명합니다. 이 문서는 복사 작업에 대한 일반적인 개요를 제공하는 [복사 작업 개요](copy-activity-overview.md) 문서를 기반으로 합니다.

> [!NOTE]
> 이 문서는 현재 미리 보기 상태인 Data Factory 버전 2에 적용됩니다. GA(일반 공급) 상태인 Data Factory 버전 1 서비스를 사용 중인 경우 [V1의 Sybase 커넥터](v1/data-factory-onprem-sybase-connector.md)를 참조하세요.

## <a name="supported-capabilities"></a>지원되는 기능

Sybase 데이터베이스에서 지원되는 모든 싱크 데이터 저장소로 데이터를 복사할 수 있습니다. 복사 작업의 원본/싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats) 표를 참조하세요.

특히 이 Sybase 커넥터는 다음을 지원합니다.

- Sybase **버전 16 이상**
- **Basic** 또는 **Windows** 인증을 사용한 데이터 복사

## <a name="prerequisites"></a>필수 조건

이 Sybase 커넥터를 사용하려면 다음을 수행해야 합니다.

- 자체 호스팅 Integration Runtime을 설정합니다. 자세한 내용은 [자체 호스팅 Integration Runtime](create-self-hosted-integration-runtime.md)을 참조하세요.
- Integration Runtime 컴퓨터에 [Sybase iAnywhere.Data.SQLAnywhere 데이터 공급자](http://go.microsoft.com/fwlink/?linkid=324846) 16 이상을 설치합니다.

## <a name="getting-started"></a>시작
.NET SDK, Python SDK, Azure PowerShell, REST API 또는 Azure Resource Manager 템플릿을 사용하여 복사 작업으로 파이프라인을 만들 수 있습니다. 복사 작업을 사용하여 파이프라인을 만드는 단계별 지침은 [복사 작업 자습서](quickstart-create-data-factory-dot-net.md)를 참조하세요.

다음 섹션에서는 Sybase 커넥터에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

Sybase 연결된 서비스에 다음 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 형식 속성은 **Sybase**로 설정되어야 합니다. | 예 |
| server | Sybase 서버의 이름입니다. |예 |
| database | Sybase 데이터베이스의 이름입니다. |예 |
| schema | 데이터베이스에서 스키마의 이름입니다. |아니요 |
| authenticationType | Sybase 데이터베이스에 연결하는 데 사용되는 인증 형식입니다.<br/>허용되는 값은 **Basic** 및 **Windows**입니다. |예 |
| username | Sybase 데이터베이스에 연결할 사용자 이름을 지정합니다. |예 |
| 암호 | 사용자 이름에 지정한 사용자 계정의 암호를 지정합니다. 이 필드를 SecureString으로 표시합니다. |예 |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md)입니다. [필수 조건](#prerequisites)에 설명된 대로 자체 호스팅 Integration Runtime이 필요합니다. |예 |

**예제:**

```json
{
    "name": "SybaseLinkedService",
    "properties": {
        "type": "Sybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 집합 속성

데이터 집합 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 데이터 집합 문서를 참조하세요. 이 섹션에는 Sybase 데이터 집합에서 지원하는 속성의 목록을 제공합니다.

Sybase에서 데이터를 복사하려면 데이터 집합의 type 속성을 **RelationalTable**로 설정합니다. 다음과 같은 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 집합의 type 속성을 **RelationalTable**로 설정해야 합니다. | 예 |
| tableName | Sybase 데이터베이스의 테이블 이름입니다. | 아니요(작업 원본에서 "query"가 지정된 경우) |

**예제**

```json
{
    "name": "SybaseDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Sybase linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에는 Sybase 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="sybase-as-source"></a>Sybase를 원본으로

Sybase에서 데이터를 복사하려면 복사 작업의 원본 형식을 **RelationalSource**로 설정합니다. 복사 작업 **source** 섹션에서 다음 속성이 지원됩니다.

| 속성 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 형식 속성을 **RelationalSource**로 설정해야 합니다. | 예 |
| 쿼리 | 사용자 지정 SQL 쿼리를 사용하여 데이터를 읽습니다. 예: `"SELECT * FROM MyTable"` | 아니요(데이터 집합의 "tableName"이 지정된 경우) |

**예제:**

```json
"activities":[
    {
        "name": "CopyFromSybase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Sybase input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sybase"></a>Sybase에 대한 데이터 형식 매핑

Sybase에서 데이터를 복사할 경우 Sybase 데이터 형식에서 Azure Data Factory 중간 데이터 형식으로 다음 매핑이 사용됩니다. 복사 작업에서 원본 스키마 및 데이터 형식을 싱크에 매핑하는 방법에 대한 자세한 내용은 [스키마 및 데이터 형식 매핑](copy-activity-schema-and-type-mapping.md)을 참조하세요.

Sybase는 T-SQL 형식을 지원합니다. SQL 형식의 매핑 테이블을 Azure Data Factory 중간 데이터 형식에 매핑하려면 [Azure SQL Database 커넥터 - 데이터 형식 매핑](connector-azure-sql-database.md#data-type-mapping-for-azure-sql-database) 섹션을 참조하세요.


## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats)를 참조하세요.