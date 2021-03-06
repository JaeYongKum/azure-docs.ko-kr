---
title: "Azure Data Lake Tools: Azure Data Lake Tools for Visual Studio Code 사용 | Microsoft Docs"
description: "Azure Data Lake Tools for Visual Studio Code를 사용하여 U-SQL 스크립트를 만들고, 테스트하고, 실행하는 방법에 대해 알아봅니다. "
Keywords: "VScode,Azure Data Lake Tools,로컬 실행,로컬 디버그,로컬 디버그,저장소 파일 미리 보기,저장소 경로로 업로드,다운로드,업로드"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/10/2017
ms.author: jejiang
ms.openlocfilehash: e724a8db4424a1e608ae7ee5625cd4cc16f6078f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake Tools for Visual Studio Code 사용

Azure Data Lake Tools for Visual Studio Code(VS Code)를 사용하여 U-SQL 스크립트를 만들고, 테스트하고, 실행하는 방법을 알아봅니다. 정보는 또한 다음 비디오에서 설명합니다.

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>필수 조건

Azure Data Lake Tools for VSCode는 Windows, Linux 및 MacOS를 지원합니다.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

macOS 및 Linux:
- [.NET Core SDK 2.0](https://www.microsoft.com/net/download/core). 
- [Mono 5.2.x](http://www.mono-project.com/download/).

## <a name="install-data-lake-tools"></a>Data Lake Tools 설치

필수 구성 요소를 설치한 후에 Data Lake Tools for VS Code를 설치할 수 있습니다.

**Data Lake Tools를 설치하려면**

1. Visual Studio Code를 엽니다.
2. 왼쪽 창에서 **확장**을 클릭합니다. 검색 상자에 **Azure Data Lake**를 입력합니다.
3. **Azure Data Lake Tools** 옆에 있는 **설치**를 클릭합니다. 몇 초 후에 **설치** 단추가 **다시 로드**로 변경됩니다.
4. **다시 로드**를 클릭하여 **Azure Data Lake Tools** 확장을 활성화합니다.
5. **창 다시 로드**를 클릭하여 확인합니다. 확장 창에서 **Azure Data Lake Tools**를 볼 수 있습니다.

    ![Data Lake Tools for Visual Studio Code 확장 창](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake 도구 활성화
새 .usql 파일을 만들거나 기존 .usql 파일을 열어 확장을 활성화합니다. 

## <a name="open-the-sample-script"></a>샘플 스크립트 열기
명령 팔레트(Ctrl+Shift+P)를 열고 **ADL: Open Sample Script**를 입력합니다. 그러면 이 샘플의 다른 인스턴스가 열립니다. 이 인스턴스에서 스크립트를 편집, 구성, 제출할 수도 있습니다.

## <a name="work-with-u-sql"></a>U-SQL 사용

U-SQL을 사용하려면 U-SQL 파일이나 폴더를 열어야 합니다.

**U-SQL 프로젝트 폴더를 열려면**

1. Visual Studio Code에서 **파일** 메뉴를 선택한 다음 **폴더 열기**를 선택합니다.
2. 폴더를 지정한 다음 **폴더 선택**을 선택합니다.
3. **파일** 메뉴를 선택한 다음 **새 파일**을 선택합니다. 제목 없음-1 파일이 프로젝트에 추가됩니다.
4. 다음 코드를 제목 없음-1 파일에 입력합니다.

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    스크립트는 /output 폴더에 일부 데이터가 포함된 departments.csv 파일을 만듭니다.

5. 열린 폴더에서 파일을 **myUSQL.usql**로 저장합니다. adltools_settings.json 구성 파일도 프로젝트에 추가됩니다.
4. adltools_settings.json을 열고 다음 속성을 사용하여 구성합니다.

    - Account: Azure 구독에 있는 Data Lake Analytics 계정입니다.
    - Database: 사용자 계정의 데이터베이스입니다. 기본은 **master**입니다.
    - Schema: 데이터베이스의 스키마입니다. 기본은 **dbo**입니다.
    - 선택적 설정
        - Priority: 우선 순위의 범위는 1-1000이며, 가장 높은 우선 순위는 1입니다. 기본값은 **1000**입니다.
        - Parallelism: 병렬 처리의 범위는 1-150입니다. 기본값은 Azure Data Lake Analytics 계정에 허용되는 최대 병렬 처리입니다. 
        
        > [!NOTE] 
        > 설정이 유효하지 않으면 기본값이 사용됩니다.

        ![Data Lake Tools for Visual Studio Code 구성 파일](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

        Data Lake Analytics 계산 계정은 U-SQL 작업을 컴파일하고 실행하는 데 필요합니다. U-SQL 작업을 컴파일하고 실행하려면 먼저 컴퓨터 계정을 구성해야 합니다.
    
        구성이 저장되면 계정, 데이터베이스 및 스키마 정보가 해당 .usql 파일의 왼쪽 아래 모서리에 있는 상태 표시줄에 나타납니다. 
 
 
파일 열기와 비교하여 폴더를 열 때 다음이 가능합니다.

- 코드 숨김 파일 사용 단일 파일 모드에서는 코드 숨김이 지원되지 않습니다.
- 구성 파일 사용 폴더를 열면 작업 폴더의 스크립트에서 단일 구성 파일을 공유합니다.


U-SQL 스크립트는 Data Lake Analytics 서비스를 통해 원격으로 컴파일합니다. **컴파일** 명령을 실행하면 U-SQL 스크립트를 Data Lake Analytics 계정으로 전송하며, Visual Studio Code에서는 컴파일 결과를 늦게 수신합니다. 원격 컴파일로 인해 Visual Studio Code에는 구성 파일의 Data Lake Analytics 계정에 연결되는 정보를 나열해야 합니다.

**U-SQL 스크립트를 컴파일하려면**

1. Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다. 
2. **ADL:Compile Script**를 누릅니다. 컴파일 결과는 **출력** 창에 나타납니다. 또한 스크립트 파일을 마우스 오른쪽 단추로 클릭한 다음 **ADL: Compile Script**를 선택하여 U-SQL 작업을 컴파일할 수도 있습니다. 컴파일 결과가 **출력** 창에 나타납니다.
 

**U-SQL 스크립트를 제출하려면**

1. Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다. 
2. **ADL:Submit Job**을 누릅니다.  또한 스크립트 파일을 마우스 오른쪽 단추로 클릭한 다음 **ADL: Submit Job**을 선택할 수도 있습니다. 

U-SQL 작업을 제출한 후 전송 로그가 VS Code의 **출력** 창에 나타납니다. 성공적으로 제출되면 작업 URL도 함께 나타납니다. 웹 브라우저에서 작업 URL을 열어 실시간 작업 상태를 추적할 수 있습니다.

작업 세부 정보의 출력을 활성화하려면 **vs code for u-sql_settings.json** 파일에서 **jobInformationOutputPath**를 설정합니다.
 
## <a name="use-a-code-behind-file"></a>코드 숨김 파일 사용

코드 숨김 파일은 단일 U-SQL 스크립트와 연결되는 C# 파일입니다. 코드 숨김 파일에서는 UDO, UDA, UDT 및 UDF 전용 스크립트를 정의할 수 있습니다. 먼저 어셈블리를 등록하지 않고도 스크립트에서 UDO, UDA, UDT 및 UDF를 직접 사용할 수 있습니다. 코드 숨김 파일은 피어링 U-SQL 스크립트 파일과 동일한 폴더에 저장됩니다. 스크립트 파일 이름이 xxx.usql이면 코드 숨김 파일 이름은 xxx.usql.cs가 됩니다. 코드 숨김 파일을 수동으로 삭제하면 연결된 U-SQL 스크립트의 코드 숨김 기능을 사용할 수 없게 됩니다. U-SQL 스크립트의 고객 코드 만들기에 대한 자세한 내용은 [U-SQL에서 사용자 지정 코드 만들기 및 사용: 사용자 정의 함수]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/)를 참조하세요.

코드 숨김을 지원하려면 작업 폴더를 열어야 합니다. 

**코드 숨김 파일을 생성하려면**

1. 원본 파일을 엽니다. 
2. Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다.
3. **ADL:Generate Code Behind**를 누릅니다. 코드 숨김 파일이 동일한 폴더에 만들어집니다. 

또한 스크립트 파일을 마우스 오른쪽 단추로 클릭한 다음 **ADL: Generate Code Behind**를 선택할 수도 있습니다. 

코드 숨김을 사용하여 U-SQL 스크립트를 컴파일하고 제출하는 것은 독립 실행형 U-SQL 스크립트 파일과 동일합니다.

다음 두 스크린샷에서는 코드 숨김 파일 및 연결된 U-SQL 스크립트 파일을 보여 줍니다.
 
![Data Lake Tools for Visual Studio Code 코드 숨김](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Data Lake Tools for Visual Studio Code 코드 숨김 스크립트 파일](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

로컬 실행 및 로컬 디버그를 지원합니다. 지침에 대해서는 [Visual Studio Code로 U-SQL 로컬 실행 및 로컬 디버그](data-lake-tools-for-vscode-local-run-and-debug.md)를 참조하세요.

## <a name="use-assemblies"></a>어셈블리 사용

어셈블리를 개발에 대한 정보는 [Azure Data Lake Analytics 작업에 U-SQL 어셈블리 개발](data-lake-analytics-u-sql-develop-assemblies.md)을 참조하세요.

Data Lake Tools를 사용하여 사용자 지정 코드 어셈블리를 Data Lake Analytics 카탈로그에 등록할 수 있습니다.

**어셈블리를 등록하려면**

**ADL: Register Assembly** 또는 **ADL: Register Assembly through Configuration** 명령을 통해 어셈블리를 등록할 수 있습니다.

**ADL: Register Assembly 명령을 통해 등록하려면**
1.  Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다.
2.  **ADL:Register Assembly**를 입력합니다. 
3.  로컬 어셈블리 경로를 지정합니다. 
4.  Data Lake Analytics 계정을 선택합니다.
5.  데이터베이스를 선택합니다.

결과: 포털이 브라우저에서 열리고 어셈블리 등록 프로세스가 표시됩니다.  

**ADL: Register Assembly** 명령을 트리거하는 다른 편리한 방법은 파일 탐색기에서 .dll 파일을 마우스 오른쪽 단추로 클릭하는 것입니다. 

**ADL: Register Assembly through Configuration 명령을 통해 등록하려면**
1.  Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다.
2.  **ADL: Register Assembly through Configuration**을 입력합니다. 
3.  로컬 어셈블리 경로를 지정합니다. 
4.  JSON 파일이 표시됩니다. 필요한 경우 어셈블리 종속성 및 리소스 매개 변수를 검토하고 편집합니다. 지침이 **출력** 창에 표시됩니다. 어셈블리 등록을 계속하려면 JSON 파일을 저장(Ctrl+S)합니다.

![Data Lake Tools for Visual Studio Code 코드 숨김](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- 어셈블리 종속성: Azure Data Lake Tools는 DLL에 종속성이 있는지 여부를 자동으로 감지합니다. 종속성이 감지되면 JSON 파일에 표시됩니다. 
>- 리소스: 어셈블리 등록의 일환으로 DLL 리소스(예: txt, .png 및 .csv)를 업로드할 수 있습니다. 

**ADL: Register Assembly through Configuration** 명령을 트리거하는 다른 방법은 파일 탐색기에서 .dll 파일을 마우스 오른쪽 단추로 클릭하는 것입니다. 

다음 U-SQL 코드는 어셈블리를 호출하는 방법을 보여 줍니다. 이 샘플에서 어셈블리 이름은 *test*입니다.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```

## <a name="connect-to-azure"></a>Azure에 연결

Data Lake Analytics에서 U-SQL 스크립트를 컴파일하고 실행하기 전에 먼저 Azure 계정에 연결해야 합니다.

**Azure에 연결하려면**

1.  Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다. 
2.  **ADL: Login**을 입력합니다. 로그인 정보가 **출력** 창에 표시됩니다.

    ![Data Lake Tools for Visual Studio Code 명령 팔레트](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Data Lake Tools for Visual Studio Code 장치 로그인 정보](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Ctrl 키를 누른 채 로그인 URL: https://aka.ms/devicelogin을 클릭하여 선택하여 로그인 웹 페이지를 엽니다. 텍스트 상자에 코드 **G567LX42V**를 입력한 다음 **계속**을 선택합니다.

   ![Data Lake Tools for Visual Studio Code 로그인 코드 붙여넣기](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  지침에 따라 웹 페이지에서 로그인합니다. 연결되면 Azure 계정 이름이 **VS Code** 창의 왼쪽 아래 모서리에 있는 상태 표시줄에 나타납니다. 

    > [!NOTE] 
    > 계정에 활성화된 두 가지 요인이 있는 경우 PIN을 사용하는 것보다 전화 인증을 사용하는 것이 좋습니다.

로그아웃하려면 **ADL: Logout** 명령을 입력합니다.

## <a name="list-your-data-lake-analytics-accounts"></a>Data Lake Analytics 계정 나열

연결을 테스트하려면 Data Lake Analytics 계정의 목록을 가져옵니다.

**Azure 구독**에서 Data Lake Analytics 계정을 나열하려면

1. Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다.
2. **ADL: List Accounts**를 입력합니다. 계정이 **출력** 창에 표시됩니다.


## <a name="access-the-data-lake-analytics-catalog"></a>Data Lake Analytics 카탈로그 액세스

Azure에 연결한 후에는 다음 단계를 사용하여 U-SQL 카탈로그에 액세스할 수 있습니다.

**Azure Data Lake Analytics 메타데이터에 액세스하려면**

1.  Ctrl+Shift+P를 선택한 다음 **ADL: List Tables**를 입력합니다.
2.  Data Lake Analytics 계정 중 하나를 선택합니다.
3.  Data Lake Analytics 데이터베이스 중 하나를 선택합니다.
4.  스키마 중 하나를 선택합니다. 테이블의 목록을 볼 수 있습니다.

## <a name="view-data-lake-analytics-jobs"></a>Data Lake Analytics 작업 보기

**Data Lake Analytics 작업을 보려면**
1.  명령 팔레트(Ctrl+Shift+P)를 열고 **ADL: Show Job**을 선택합니다. 
2.  Data Lake Analytics 또는 로컬 계정을 선택합니다. 
3.  계정의 작업 목록이 표시되기를 기다립니다.
4.  작업 목록에서 작업을 선택하고 Data Lake Tools는 Azure Portal에서 작업 세부 정보를 열고 VS Code에서 JobInfo 파일을 표시합니다.

    ![Data Lake Tools for Visual Studio Code IntelliSense 개체 형식](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Azure Data Lake Storage 통합

다음 작업에 Azure Data Lake Storage 관련 명령을 사용할 수 있습니다.
 - Azure Data Lake Storage 리소스 찾기 [저장소 경로 나열](#list-the-storage-path) 
 - Azure Data Lake Storage 파일 미리 보기 [저장소 파일 미리 보기](#preview-the-storage-file) 
 - VS Code의 Azure Data Lake Storage에 직접 파일 업로드 [파일 업로드](#upload-file).
 - VS Code의 Azure Data Lake Storage에서 직접 파일 다운로드 [파일 다운로드](#download-file)

## <a name="list-the-storage-path"></a>저장소 경로 나열 

**명령 팔레트를 통해 저장소 경로를 나열하려면**

스크립트 편집기를 마우스 오른쪽 단추로 클릭하고 **ADL: List Storage Path**를 선택합니다.

목록에서 폴더를 선택하거나 **Enter Path** 또는 **Browse from Root**를 클릭합니다(Enter a path를 예제로 사용). -> **ADLA 계정**을 선택합니다. ->  저장소 폴더 경로로 이동하거나 입력합니다(예: /output/). -> 명령 팔레트는 입력한 항목에 따라 경로 정보를 나열합니다.

![Data Lake Tools for Visual Studio Code에서 저장소 경로 결과 나열](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

상대 경로를 나열하는 더 편리한 다른 방법은 바로 가기 메뉴를 마우스 오른쪽 단추로 클릭하는 것입니다.

**마우스 오른쪽 단추 클릭을 통해 저장소 경로를 나열하려면**

경로 문자열을 마우스 오른쪽 단추로 클릭하여 **List Storage Path**를 선택하여 계속 진행합니다.

![Data Lake Tools for Visual Studio Code에서 바로 가기 메뉴를 마우스 오른쪽 단추로 클릭](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>저장소 파일 미리 보기

스크립트 편집기를 마우스 오른쪽 단추로 클릭하고 **ADL: Preview Storage File**을 선택합니다.

**ADLA 계정**을 선택합니다. -> Azure Storage 파일 경로를 입력합니다(예: /output/SearchLog.txt). -> 결과: VSCode에서 파일이 열립니다.

   ![Data Lake Tools for Visual Studio Code 파일 미리 보기 결과](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

저장소 파일을 미리 보는 다른 방법은 스크립트 편집기에서 파일의 전체 경로 또는 파일의 상대 경로의 오른쪽 클릭 메뉴를 사용하는 것입니다. 

## <a name="upload-file"></a>파일 업로드 

**ADL: Upload File** 또는 **ADL: Upload File through Configuration** 명령을 입력하여 파일을 업로드할 수 있습니다.

**ADL: Upload File through Configuration 명령을 통해 파일을 업로드하려면**
1.  스크립트 편집기를 마우스 오른쪽 단추로 클릭한 다음 **Upload File through Configuration**을 선택합니다.
2.  VS Code에서 JSON 파일을 표시합니다. 파일 경로를 입력하고 여러 파일을 동시에 업로드할 수 있습니다. 지침이 **출력** 창에 표시됩니다. 파일 업로드를 계속하려면 JSON 파일을 저장(Ctrl+S)합니다.

       ![Data Lake Tools for Visual Studio Code 파일 경로](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  결과: **출력** 창은 파일 업로드 상태를 표시합니다.

       ![Data Lake Tools for Visual Studio Code에서 상태 업로드](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

동시에 [업로드 상태](#check-storage-tasks-status)를 모니터링할 수 있습니다.

**ADL: Upload File 명령을 통해 파일을 업로드하려면**

스크립트 편집기를 마우스 오른쪽 단추로 클릭하고 **Upload File**을 선택합니다.

**Local File Path**를 입력합니다. -> 목록에서 폴더를 선택하거나 **Enter Path** 또는 **Browse from Root**를 클릭합니다(Enter a path를 예제로 사용). -> **ADLA 계정**을 선택합니다. ->  저장소 폴더 경로로 이동하거나 입력합니다(예: /output/). -> **Choose Current Folder**를 클릭하여 업로드 위치를 지정합니다.

![Data Lake Tools for Visual Studio Code에서 상태 업로드](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


저장소에 파일을 업로드하는 다른 방법은 스크립트 편집기에서 파일의 전체 경로 또는 파일의 상대 경로의 오른쪽 클릭 메뉴를 사용하는 것입니다.

동시에 [업로드 상태](#check-storage-tasks-status)를 모니터링할 수 있습니다.

## <a name="download-file"></a>파일 다운로드 
**ADL: Download Storage File** 또는 **ADL: Download Storage File through Configuration** 명령을 입력하여 파일을 다운로드할 수 있습니다.

**ADL: Download File through Configuration 명령을 통해 파일을 다운로드하려면**
1. 스크립트 편집기를 마우스 오른쪽 단추로 클릭한 다음 **Download Storage File through Configuration**을 선택합니다.
2. VS Code에서 JSON 파일을 표시합니다. 파일 경로를 입력하고 여러 파일을 동시에 다운로드할 수 있습니다. 지침이 **출력** 창에 표시됩니다. 파일 다운로드를 계속하려면 JSON 파일을 저장(Ctrl+S)합니다.

    ![Data Lake Tools for Visual Studio Code의 구성 파일 다운로드](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  결과: **출력** 창은 파일 업로드 상태를 표시합니다.

    ![Data Lake Tools for Visual Studio Code의 여러 파일 결과 다운로드](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

동시에 [다운로드 상태](#check-storage-tasks-status)를 모니터링할 수 있습니다.

**ADL: Download Storage File 명령을 통해 파일을 다운로드하려면**

스크립트 편집기를 마우스 오른쪽 단추로 클릭한 다음 **Download Storage File**을 선택합니다.

목록에서 폴더를 선택하거나 **Enter Path** 또는 **Browse from Root**를 클릭합니다(Enter a path를 예제로 사용). -> **ADLA 계정**을 선택합니다. ->  저장소 폴더 경로로 이동하거나 입력하고(예: /output/) -> 다운로드할 파일을 선택합니다.

   ![Data Lake Tools for Visual Studio Code의 다운로드 상태](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   결과 그림에서 파일은 temp 폴더에 저장됩니다. VSCode 메뉴의 **파일** -> **기본 설정** -> **설정**을 통해 직접 **usql.defaultLocalFolderForDownload** 매개 변수의 기본 다운로드 경로를 설정할 수 있습니다.

저장소 파일을 다운로드하는 다른 방법은 스크립트 편집기에서 파일의 전체 경로 또는 파일의 상대 경로의 오른쪽 클릭 메뉴를 사용하는 것입니다.

동시에 [다운로드 상태](#check-storage-tasks-status)를 모니터링할 수 있습니다.

## <a name="check-storage-tasks-status"></a>저장소 작업의 상태 확인
다운로드 및 업로드를 완료하면 상태 표시줄의 아래쪽에 상태가 표시됩니다.
1. 아래의 상태 표시줄을 클릭하면 **OUTPUT** 패널에 다운로드 및 업로드 상태가 표시됩니다.

   ![Data Lake Tools for Visual Studio Code의 저장소 상태 확인](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="open-azure-storage-explorer"></a>Azure Storage Explorer 열기
**ADL: Open Web Azure Storage Explorer** 명령을 입력하거나 마우스 오른쪽 단추 클릭 바로 가기 메뉴에서 선택하여 **Azure Storage Explorer**를 열 수 있습니다.

**Azure Storage Explorer를 열려면**

1. Ctrl+Shift+P를 선택하여 명령 팔레트를 엽니다.
2. **Open Web Azure Storage Explorer**를 입력하거나 스크립트 편집기에서 상대 경로 또는 전체 경로를 마우스 오른쪽 단추로 클릭한 다음 **Open Web Azure Storage Explorer**를 선택합니다.
3. Data Lake Analytics 계정을 선택합니다.

Data Lake Tools가 Azure Portal에서 Azure 저장소 경로를 엽니다. 경로를 찾고 웹에서 파일을 미리 볼 수 있습니다.

## <a name="local-run-and-local-debug-for-windows-users"></a>Windows 사용자에 대한 로컬 실행 및 로컬 디버그
U-SQL 로컬 실행은 Data Lake Analytics에 코드가 게시되기 전에 로컬 데이터를 테스트하고 로컬에서 스크립트의 유효성을 검사합니다. 로컬 디버그 기능을 사용하면 Data Lake Analytics에 코드를 전송하기 전에 다음 작업을 완료할 수 있습니다. 
- C# 코드 숨김을 디버그합니다. 
- 코드를 단계별로 실행합니다. 
- 로컬에서 스크립트의 유효성을 검사합니다.

로컬 실행 및 로컬 디버그에 대한 지침은 [Visual Studio Code로 U-SQL 로컬 실행 및 로컬 디버그](data-lake-tools-for-vscode-local-run-and-debug.md)를 참조하세요.

## <a name="additional-features"></a>추가 기능

Data Lake Tools for VSCode에서 지원하는 기능은 다음과 같습니다.

-   IntelliSense 자동 완성: 키워드, 메서드 및 변수와 같은 제안이 항목 주위의 팝업 창에 표시됩니다. 다음과 같이 다양한 아이콘이 여러 형식의 개체를 나타냅니다.

    - Scala 데이터 형식
    - 복합 데이터 형식
    - 기본 제공 UDT
    - .NET 컬렉션 및 클래스
    - C# 식
    - 기본 제공 C# UDF, UDO 및 UDAAG 
    - U-SQL 함수
    - U-SQL 창 작업 함수
 
    ![Data Lake Tools for Visual Studio Code IntelliSense 개체 형식](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   Data Lake Analytics 메타데이터의 IntelliSense 자동 완성: Data Lake Tools는 Data Lake Analytics 메타데이터 정보를 로컬로 다운로드합니다. IntelliSense 기능은 Data Lake Analytics 메타데이터에서 데이터베이스, 스키마, 테이블, 보기, 테이블 반환 함수, 프로시저 및 C# 어셈블리 등의 개체를 자동으로 채웁니다.
 
    ![Data Lake Tools for Visual Studio Code IntelliSense 메타데이터](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense 오류 마커: Data Lake Tools는 U-SQL 및 C#의 편집 오류에 밑줄을 표시합니다. 
-   구문 강조 표시: Data Lake Tools는 변수, 키워드, 데이터 형식 및 함수 등의 항목을 구분하기 위해 서로 다른 색을 사용합니다. 

    ![Data Lake Tools for Visual Studio Code 구문 강조 표시](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>다음 단계

- Visual Studio Code로 U-SQL 로컬 실행 및 로컬 디버그는 [Visual Studio Code로 U-SQL 로컬 실행 및 로컬 디버그](data-lake-tools-for-vscode-local-run-and-debug.md)를 참조하세요.
- Data Lake Analytics 시작 정보는 [자습서: Azure Data Lake Analytics 시작](data-lake-analytics-get-started-portal.md)을 참조하세요.
- Data Lake Tools for Visual Studio에 대한 자세한 내용은 [자습서: Data Lake Tools for Visual Studio를 사용하여 U-SQL 스크립트 개발](data-lake-analytics-data-lake-tools-get-started.md)을 참조하세요.
- 어셈블리를 개발에 대한 정보는 [Azure Data Lake Analytics 작업에 U-SQL 어셈블리 개발](data-lake-analytics-u-sql-develop-assemblies.md)을 참조하세요.



