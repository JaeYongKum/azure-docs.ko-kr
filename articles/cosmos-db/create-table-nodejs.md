---
title: "빠른 시작: Node.js가 있는 Table API - Azure Cosmos DB | Microsoft Docs"
description: "이 빠른 시작은 Azure Cosmos DB Table API를 사용하여 Azure Portal과 Node.js를 통해 응용 프로그램을 만드는 방법을 보여 줍니다."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 99f3ddb165fa548ca1d65676bb1f945632c72dd3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-build-a-table-api-app-with-nodejs-and-azure-cosmos-db"></a>빠른 시작: Node.js 및 Azure Cosmos DB를 사용하여 Table API 앱 빌드

이 빠른 시작에서는 GitHub에서 예제를 복제하여 앱을 빌드하는 데 Node.js와 Azure Cosmos DB [Table API](table-introduction.md)를 사용하는 방법을 보여 줍니다. 또한 Azure Cosmos DB 계정을 만드는 방법 및 데이터 탐색기를 사용하여 웹 기반 Azure Portal에 테이블과 엔터티를 만드는 방법도 보여줍니다.

Azure Cosmos DB는 전 세계에 배포된 Microsoft의 다중 모델 데이터베이스 서비스입니다. Azure Cosmos DB의 핵심인 전역 배포 및 수평 크기 조정 기능의 이점을 모두 활용하여 문서, 키/값, 전체 열 및 그래프 데이터베이스를 빠르게 만들고 쿼리할 수 있습니다. 

## <a name="prerequisites"></a>필수 조건

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

또한,

* [Node.js](https://nodejs.org/en/) 버전 v0.10.29 이상
* [Git](http://git-scm.com/)

## <a name="create-a-database-account"></a>데이터베이스 계정 만들기

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>테이블 추가

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>샘플 데이터 추가

이제 데이터 탐색기를 사용하여 새 테이블에 데이터를 추가할 수 있습니다.

1. 데이터 탐색기에서 **sample-table**, **엔터티**를 클릭한 다음 **엔터티 추가**를 클릭합니다.

   ![Azure Portal의 데이터 탐색기에서 새 엔터티 만들기](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. 이제 PartitionKey 값 상자 및 RowKey 값 상자에 데이터를 추가하고 **엔터티 추가**를 클릭합니다.

   ![새 엔터티에 대한 분할 키와 행 키 설정](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    이제 테이블에 더 많은 엔터티를 추가하고 엔터티를 편집하거나 데이터 탐색기에서 데이터를 쿼리할 수 있습니다. 데이터 탐색기에서는 처리량을 확장하고 테이블에 저장된 프로시저, 사용자 정의 함수 및 트리거를 추가할 수 있습니다.

## <a name="clone-the-sample-application"></a>샘플 응용 프로그램 복제

이제 github에서 Table 앱을 복제하고 연결 문자열을 설정한 다음 실행해 보겠습니다. 프로그래밍 방식으로 데이터를 사용하여 얼마나 쉽게 작업할 수 있는지 알게 될 것입니다. 

1. Git Bash와 같은 Git 터미널 창을 열고, `cd` 명령을 사용하여 샘플 앱을 설치할 폴더를 변경합니다. 

    ```bash
    cd "C:\git-samples"
    ```

2. 다음 명령을 실행하여 샘플 리포지토리를 복제합니다. 이 명령은 컴퓨터에서 샘플 앱의 복사본을 만듭니다. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-node-getting-started.git
    ```

3. 그런 다음 Visual Studio에서 솔루션을 엽니다. 

## <a name="update-your-connection-string"></a>연결 문자열 업데이트

이제 Azure Portal로 다시 이동하여 연결 문자열 정보를 가져와서 앱에 복사합니다. 이를 통해 앱이 호스팅된 데이터베이스와 통신할 수 있게 됩니다. 

1. [Azure Portal](http://portal.azure.com/)에서 **연결 문자열**을 클릭합니다. 

    ![[연결 문자열] 창에서 필요한 연결 문자열 정보 보기 및 복사](./media/create-table-nodejs/connection-string.png)

2. app.config 파일을 열고, 필요한 연결 문자열 속성을 구성 파일에 복사합니다.

3. app.config 파일을 저장합니다.

이제 Azure Cosmos DB와 통신하는 데 필요한 모든 정보로 앱이 업데이트되었습니다. 

## <a name="run-the-app"></a>앱 실행

1. git 터미널 창에서 storage-table-java-getting-started 폴더로 `cd`를 실행합니다.

    ```git
    cd "C:\git-samples\
storage-table-node-getting-started"
    ```

2. git 터미널 창에서 다음 명령을 실행하여 Java 응용 프로그램 시작을 실행합니다.

    ```git
    node ./tableSample.js 
    ```

    Azure Cosmos DB에서 새 테이블 데이터베이스에 추가되는 테이블 데이터가 콘솔 창에 표시됩니다.

    이제 데이터 탐색기로 돌아가서 이 새 데이터를 쿼리 및 수정하고 작업에 사용할 수 있습니다. 

## <a name="review-slas-in-the-azure-portal"></a>Azure Portal에서 SLA 검토

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>리소스 정리

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서, Azure Cosmos DB 계정을 만들고, 데이터 탐색기를 사용하여 테이블을 만들고, 앱을 실행하는 방법을 알아보았습니다.  이제 테이블 API를 사용하여 데이터를 쿼리할 수 있습니다.  

> [!div class="nextstepaction"]
> [Table API로 테이블 데이터 가져오기](table-import.md)
