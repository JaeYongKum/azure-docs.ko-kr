---
title: "Azure Portal을 사용하여 가상 컴퓨터 확장 집합 만들기 | Microsoft Docs"
description: "Azure Portal을 사용하여 확장 집합을 배포합니다."
keywords: "가상 컴퓨터 확장 집합"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Azure Portal을 사용하여 가상 컴퓨터 확장 집합을 만드는 방법
이 자습서에서는 Azure Portal을 사용하여 가상 컴퓨터 확장 집합을 몇 분 이내에 간편하게 만드는 방법을 보여 줍니다. Azure 구독이 아직 없는 경우 시작하기 전에 [무료 계정](https://azure.microsoft.com/free/) 을 만듭니다.

## <a name="choose-the-vm-image-from-the-marketplace"></a>마켓플레이스에서 VM 이미지를 선택합니다.
포털에서 CentOS, CoreOS, Debian, Ubuntu Server, 기타 Linux 이미지 및 Windows Server 이미지를 통해 확장 집합을 간편하게 배포할 수 있습니다.

먼저 웹 브라우저에서 [Azure Portal](https://portal.azure.com) 로 이동합니다. **새로 만들기**를 클릭하고 **확장 집합**을 검색한 다음 **가상 컴퓨터 확장 집합** 항목을 선택합니다.

![azure virtual machine 확장 집합 포털 검색](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>확장 집합 만들기
이제 기본 설정을 사용하여 신속하게 확장 집합을 만들 수 있습니다.

* 확장 집합의 이름을 입력합니다.  
이 이름은 확장 집합 앞에 있는 부하 분산 장치 FQDN의 기반이 됩니다. 따라서 모든 Azure에서 고유해야 합니다.

* 원하는 OS 종류를 선택합니다.

* 원하는 사용자 이름을 입력한 후 원하는 인증 유형을 선택합니다.  
암호를 선택하는 경우 12자 이상이어야 하고 1개의 소문자, 1개의 대문자, 1개의 숫자 및 1개의 특수 문자의 네 가지 복잡성 요구 사항 중 적어도 세 가지를 충족해야 합니다. 자세한 내용은 [사용자 이름 및 암호 요구 사항](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm)을 참조하세요. **SSH 공개 키**를 선택하는 경우 개인 키가 *아니라* 공개 키만 붙여넣어야 합니다.

* **100개 이상의 인스턴스로 확장 사용**에 대해 **예** 또는 **아니요**를 선택합니다.  
예를 선택하면 확장 집합이 여러 배치 그룹에 걸쳐 확장될 수 있습니다. 자세한 내용은 [SDK 설명서](./virtual-machine-scale-sets-placement-groups.md)를 참조하세요.

* 적합한 **인스턴스 크기**를 선택했는지 확인합니다.  
가상 컴퓨터 크기에 대한 자세한 내용은 [Windows VM 크기](..\virtual-machines\windows\sizes.md) 또는 [Linux VM 크기](..\virtual-machines\linux\sizes.md)를 참조하세요.

* 원하는 리소스 그룹 이름 및 위치를 입력합니다.  
지역 및 **인스턴스 크기**에서 가용성 영역을 지원할 경우 **가용성 영역** 필드가 활성화됩니다. 가용성 영역에 대한 자세한 내용은 [개요](../availability-zones/az-overview.md) 문서를 참조하세요.

* 원하는 도메인 이름 레이블(확장 집합 앞에 있는 부하 분산 장치 FQDN의 기준)을 입력합니다.  
이 레이블은 모든 Azure에서 고유해야 합니다.

* 원하는 운영 체제 디스크 이미지, 인스턴스 수 및 컴퓨터 크기를 선택합니다.

* 원하는 디스크 유형 선택: 관리 또는 관리 되지 않습니다.  
자세한 내용은 [SDK 설명서](./virtual-machine-scale-sets-managed-disks.md)를 참조하세요. 확장 집합을 여러 배치 그룹으로 확장하도록 선택한 경우 이 옵션은 확장 집합을 배치 그룹으로 확장하는 데 관리되는 디스크가 필요하기 때문에 이 옵션을 사용할 수 없습니다.

* 자동 크기 조정을 사용하거나 사용하지 않도록 설정하고 사용하도록 설정하는 경우 다음을 구성합니다.

![azure virtual machine 확장 집합 포털 만들기 프롬프트](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>확장 집합의 VM에 연결
단일 배치 그룹으로 설정된 규모를 제한하기로 선택한 경우에 확장 집합에 쉽게 연결할 수 있도록 구성하는 NAT 규칙과 함께 확장 집합이 배포됩니다(그렇지 않으면 확장 집합에 있는 가상 컴퓨터에 연결하려면 확장 집합과 동일한 가상 네트워크에 jumpbox를 만들어야 할 수 있습니다). 이를 보려면 확장 집합에 대한 부하 분산 장치의 `Inbound NAT Rules` 탭을 탐색합니다.

![azure virtual machine 확장 집합 포털 NAT 규칙](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

이러한 NAT 규칙을 사용하여 확장 집합의 각 VM에 연결할 수 있습니다. 예를 들어 Windows 확장 집합의 경우 수신 포트 50000에 대한 NAT 규칙이 있으면 `<load-balancer-ip-address>:50000`의 RDP를 통해 해당 컴퓨터에 연결할 수 있습니다. Linux 확장 집합의 경우 `ssh -p 50000 <username>@<load-balancer-ip-address>`명령을 사용하여 연결할 수 있습니다.

## <a name="next-steps"></a>다음 단계
CLI에서 확장 집합을 배포하는 방법에 대한 설명서를 보려면 [이 설명서](virtual-machine-scale-sets-cli-quick-create.md)를 참조하세요.

PowerShell에서 확장 집합을 배포하는 방법에 대한 설명서를 보려면 [이 설명서](virtual-machine-scale-sets-windows-create.md)를 참조하세요.

Visual Studio에서 확장 집합을 배포하는 방법에 대한 설명서를 보려면 [이 설명서](virtual-machine-scale-sets-vs-create.md)를 참조하세요.

일반 설명서는 [확장 집합에 대한 설명서 개요 페이지](virtual-machine-scale-sets-overview.md)를 참조하세요.

일반적인 정보는 [확장 집합에 대한 주 방문 페이지](https://azure.microsoft.com/services/virtual-machine-scale-sets/)를 확인하세요.

