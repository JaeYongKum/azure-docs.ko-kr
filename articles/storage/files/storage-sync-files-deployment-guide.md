---
title: "Azure File Sync 배포(미리 보기) | Microsoft Docs"
description: "Azure File Sync를 배포하는 방법을 처음부터 끝까지 알아봅니다."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 42a0e7a3816e0f1d96951feac210e5770add4fe1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2017
---
# <a name="deploy-azure-file-sync-preview"></a>Azure File Sync 배포(미리 보기)
Azure File Sync(미리 보기)를 사용하여 온-프레미스 파일 서버의 유연성, 성능 및 호환성을 유지하면서 Azure Files에서 조직의 파일 공유를 중앙 집중화합니다. Azure File Sync는 Windows Server를 Azure 파일 공유의 빠른 캐시로 변환합니다. SMB, NFS 및 FTPS를 포함하여 로컬로 데이터에 액세스하기 위해 Windows Server에서 사용할 수 있는 모든 프로토콜을 사용할 수 있습니다. 전 세계에서 필요한 만큼 많은 캐시를 가질 수 있습니다.

이 문서에 설명된 단계를 완료하기 전에 [Azure Files 배포에 대한 계획](storage-files-planning.md) 및 [Azure File Sync 배포에 대한 계획](storage-sync-files-planning.md)을 읽어보는 것이 좋습니다.

## <a name="prerequisites"></a>필수 조건
* Azure File Sync를 배포하려는 동일한 지역에 Azure Storage 계정 및 Azure 파일 공유가 있어야 합니다. 자세한 내용은 다음을 참조하세요.
    - [지역 가용성](storage-sync-files-planning.md#region-availability)에서 Azure File Sync를 참조하세요.
    - [저장소 계정 만들기](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)에서 저장소 계정을 만드는 방법에 대한 단계별 설명을 참조하세요.
    - [파일 공유 만들기](storage-how-to-create-file-share.md)에서 파일 공유를 만드는 방법에 대한 단계별 설명을 참조하세요.
* Azure File Sync와 동기화할 Windows Server 또는 Windows Server 클러스터의 지원되는 인스턴스가 하나 이상 있어야 합니다. 지원되는 Windows Server 버전에 대한 자세한 내용은 [Windows Server와의 상호 운용성](storage-sync-files-planning.md#azure-file-sync-interoperability)을 참조하세요.

## <a name="deploy-the-storage-sync-service"></a>저장소 동기화 서비스 배포 
저장소 동기화 서비스는 Azure File Sync의 최상위 Azure 리소스입니다. 저장소 동기화 서비스를 배포하려면 [Azure Portal](https://portal.azure.com/)로 이동한 후 Azure File Sync를 검색합니다. 검색 결과에서 **Azure File Sync(미리 보기)**를 선택한 후 **만들기**를 선택하여 **저장소 동기화 배포** 탭을 엽니다.

열리는 창에 다음 정보를 입력합니다.

- **이름**: 저장소 동기화 서비스의 고유 이름(구독별)입니다.
- **구독**: 저장소 동기화 서비스를 만들 구독입니다. 조직의 구성 전략에 따라 하나 이상의 구독에 대한 액세스 권한이 있을 수 있습니다. Azure 구독은 각 클라우드 서비스(예: Azure Files)에 대한 비용을 청구하는 가장 기본적인 컨테이너입니다.
- **리소스 그룹**: 리소스 그룹은 저장소 계정 또는 저장소 동기화 서비스와 같은 Azure 리소스의 논리적 그룹입니다. Azure File Sync를 위한 새 리소스 그룹을 생성하거나 기존 리소스 그룹을 사용할 수 있습니다. (HR 리소스 또는 특정 프로젝트의 리소스를 그룹화하듯이 리소스 그룹을 컨테이너로 사용하여 조직의 리소스를 논리적으로 격리하는 것이 좋습니다.)
- **위치**: Azure File Sync를 배포할 지역입니다. 이 목록에는 지원되는 지역만 표시됩니다.

작업이 끝나면 **만들기**를 선택하여 저장소 동기화 서비스를 배포합니다.

## <a name="prepare-windows-server-to-use-with-azure-file-sync"></a>Azure File Sync에 사용할 Windows Server 준비
장애 조치(Failover) 클러스터의 서버 노드를 비롯하여 Azure File Sync에 사용할 모든 서버에 대해 다음 단계를 완료합니다.

1. **Internet Explorer 보안 강화 구성**을 사용하지 않도록 설정합니다. 초기 서버 등록에만 필요합니다. 서버가 등록된 후에 사용하도록 다시 설정할 수 있습니다.
    1. [서버 관리자]를 엽니다.
    2. **로컬 서버**를 클릭합니다.  
        ![서버 관리자 UI 왼쪽에 있는 "로컬 서버"](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-1.PNG)
    3. **속성** 하위 창에서 **IE 보안 강화 구성** 링크를 선택합니다.  
        ![서버 관리자 UI의 "IE 보안 강화 구성" 창](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-2.PNG)
    4. **Internet Explorer 보안 강화 구성** 대화 상자에서 **관리자**와 **사용자**에 대해 **사용 안 함**을 선택합니다.  
        ![“해제"가 선택된 Internet Explorer 보안 강화 구성 팝업 창](media/storage-sync-files-deployment-guide/prepare-server-disable-IEESC-3.png)

2. PowerShell 5.1을 실행하고 있는지 확인합니다.\* (PowerShell 5.1은 Windows Server 2016에서 기본값입니다.) **$PSVersionTable** 개체의 **PSVersion** 속성 값을 확인하여 PowerShell 5.1.\*을 실행하고 있는지 확인할 수 있습니다.

    ```PowerShell
    $PSVersionTable.PSVersion
    ```

    대부분의 Windows Server 2012 R2 설치의 경우처럼 PSVersion 값이 5.1.\*보다 낮으면 [WMF(Windows Management Framework) 5.1](https://www.microsoft.com/download/details.aspx?id=54616)을 다운로드한 후 설치하여 쉽게 업그레이드할 수 있습니다. Windows Server 2012 R2에 대해 다운로드하고 설치할 적절한 패키지는 **Win8.1AndW2K12R2-KB\*\*\*\*\*\*\*-x64.msu**입니다.

3. [Azure PowerShell 설치 및 구성](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)합니다. 최신 버전의 Azure PowerShell 모듈을 사용하는 것이 좋습니다.

## <a name="install-the-azure-file-sync-agent"></a>Azure 파일 동기화 에이전트 설치
Azure File Sync 에이전트는 Windows Server가 Azure 파일 공유와 동기화되도록 하는 다운로드 가능 패키지입니다. [Microsoft 다운로드 센터](https://go.microsoft.com/fwlink/?linkid=858257)에서 에이전트를 다운로드할 수 있습니다. 다운로드가 완료되면 MSI 패키지를 두 번 클릭하여 Azure File Sync 에이전트 설치를 시작합니다.

> [!Important]  
> 장애 조치(Failover) 클러스터에서 Azure File Sync를 사용하려는 경우 Azure File Sync 에이전트를 클러스터의 모든 노드에 설치해야 합니다.

Azure File Sync 에이전트 설치 패키지는 너무 많은 추가 프롬프트 없이 비교적 빠르게 설치됩니다. 다음을 수행하는 것이 좋습니다.
- 문제 해결 및 서버 유지 관리를 간소화하려면 기본 설치 경로(C:\Program Files\Azure\StorageSyncAgent)를 유지하세요.
- Azure File Sync를 최신 상태로 유지하도록 Microsoft 업데이트를 사용하도록 설정하세요. 기능 업데이트 및 핫픽스를 비롯한 Azure File Sync 에이전트에 대한 모든 업데이트는 Microsoft 업데이트에서 수행됩니다. Azure File Sync에 대한 최신 업데이트를 설치하는 것이 좋습니다. 자세한 내용은 [Azure File Sync 업데이트 정책](storage-sync-files-planning.md#azure-file-sync-agent-update-policy)을 참조하세요.

Azure File Sync 에이전트 설치를 마치면 서버 등록 UI가 자동으로 열립니다. 이 서버를 Azure File Sync에 등록하는 방법을 알아보려면 다음 섹션을 참조하세요.

## <a name="register-windows-server-with-storage-sync-service"></a>저장소 동기화 서비스에 Windows Server 등록
저장소 동기화 서비스에 Windows Server를 등록하면 서버(또는 클러스터)와 저장소 동기화 서비스 간에 트러스트 관계가 설정됩니다. Azure File Sync 에이전트 설치 후 서버 등록 UI가 자동으로 열립니다. 그렇지 않은 경우 파일 위치(C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe)에서 수동으로 열 수 있습니다. 서버 등록 UI가 열리면 **로그인**을 선택하여 시작합니다.

로그인한 후에 다음 정보를 묻는 메시지가 나타납니다.

![서버 등록 UI 스크린샷](media/storage-sync-files-deployment-guide/register-server-scubed-1.png)

- **Azure 구독**: 저장소 동기화 서비스가 포함된 구독입니다([저장소 동기화 서비스 배포](#deploy-the-storage-sync-service) 참조). 
- **리소스 그룹**: 저장소 동기화 서비스를 포함하는 리소스 그룹입니다.
- **저장소 동기화 서비스**: 등록하려는 저장소 동기화 서비스의 이름입니다.

적절한 정보를 선택한 후 **등록**을 선택하여 서버 등록을 완료합니다. 등록 프로세스의 일부로 추가 로그인을 요구하는 메시지가 표시됩니다.

## <a name="create-a-sync-group"></a>동기화 그룹 만들기
동기화 그룹은 파일 집합에 대한 동기화 토폴로지를 정의합니다. 동기화 그룹 내 끝점은 서로 동기화된 상태를 유지합니다. 동기화 그룹은 Azure 파일 공유를 나타내는 하나 이상의 클라우드 끝점과 Windows Server의 경로를 나타내는 하나의 서버 끝점을 포함해야 합니다. 동기화 그룹을 만들려면 [Azure Portal](https://portal.azure.com/)에서 저장소 동기화 서비스로 이동한 후 **+동기화 그룹**을 선택합니다.

![Azure Portal에서 새 동기화 그룹 만들기](media/storage-sync-files-deployment-guide/create-sync-group-1.png)

열리는 창에서 다음 정보를 입력하여 클라우드 끝점이 있는 동기화 그룹을 만듭니다.

- **동기화 그룹 이름**: 만들 동기화 그룹의 이름입니다. 이 이름은 저장소 동기화 서비스 내에서 고유해야 하지만 사용자에게 논리적인 어떤 이름도 될 수 있습니다.
- **구독**: [저장소 동기화 서비스 배포](#deploy-the-storage-sync-service)에서 저장소 동기화 서비스를 배포한 구독입니다.
- **저장소 계정**: **저장소 계정 선택**을 선택하면 동기화할 Azure 파일 공유가 있는 저장소 계정을 선택할 수 있는 또 다른 창이 나타납니다.
- **Azure 파일 공유**: 동기화할 Azure 파일 공유의 이름입니다.

서버 끝점을 추가하려면 새로 만든 동기화 그룹으로 이동한 후 **서버 끝점 추가**를 클릭합니다.

![동기화 그룹 창에서 새 서버 끝점 추가](media/storage-sync-files-deployment-guide/create-sync-group-2.png)

**서버 끝점 추가** 창에 다음 정보를 입력하여 서버 끝점을 만듭니다.

- **등록된 서버**: 서버 끝점을 만들 서버 또는 클러스터의 이름입니다.
- **경로**: 동기화 그룹의 일부분으로 동기화할 Windows Server 경로입니다.
- **클라우드 계층화**: 클라우드 계층화를 사용하거나 사용하지 않도록 설정할 스위치입니다. 클라우드 계층화를 사용하면 드물게 사용하거나 액세스하는 파일은 Azure 파일로 계층화할 수 있습니다.
- **사용 가능한 볼륨 공간**: 서버 끝점이 있는 볼륨에서 예약할 여유 공간의 크기입니다. 예를 들어 단일 서버 끝점이 있는 볼륨에서 사용 가능한 볼륨 공간을 50%로 설정하는 경우 데이터 양의 절반 정도가 Azure Files로 계층화됩니다. 클라우드 계층화를 사용하도록 설정할지 여부에 관계없이, Azure 파일 공유는 항상 동기화 그룹에 데이터의 전체 복사본을 유지합니다.

서버 끝점을 추가하려면 **만들기**를 선택합니다. 이제 Azure 파일 공유 및 Windows Server에서 파일이 동기화 상태로 유지됩니다. 

> [!Important]  
> 동기화 그룹의 클라우드 끝점 또는 서버 끝점을 변경할 수 있고, 파일이 동기화 그룹의 다른 끝점과 동기화되도록 할 수 있습니다. 클라우드 끝점(Azure Files 공유)을 직접 변경하는 경우 변경 사항은 먼저 Azure File Sync 변경 내용 검색 작업으로 검색되어야 합니다. 변경 내용 검색 작업은 클라우드 끝점에 대해 24시간마다 한 번씩만 시작됩니다. 자세한 내용은 [Azure Files 질문과 대답](storage-files-faq.md#afs-change-detection)을 참조하세요.

## <a name="next-steps"></a>다음 단계
- [Azure File Sync 서버 끝점 추가 또는 제거](storage-sync-files-server-endpoint.md)
- [Azure File Sync로 서버 등록 또는 등록 취소](storage-sync-files-server-registration.md)
