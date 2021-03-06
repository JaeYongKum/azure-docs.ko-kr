---
title: "가용성 영역 개요 | Microsoft Docs"
description: "이 문서는 Azure의 가용성 영역 개요를 제공합니다."
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 53c5c340dc4e91db29fbbb45893fabe6eec2bc5d
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2017
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Azure(미리 보기)의 가용성 영역 개요

가용성 영역은 데이터 센터 수준 오류로부터 사용자를 보호하는 데 도움이 됩니다. 이 기능은 Azure 지역 내에 있으며 각각 자체 독립적인 원본, 네트워크 및 냉각을 갖습니다. 복원력을 보장하려면 활성화된 모든 지역에서 최소한 세 개의 별도 영역이 필요합니다. 지역 내에서 가용성 영역을 물리적 및 논리적으로 분리하면 응용 프로그램 및 데이터를 영역 수준 오류로부터 보호할 수 있습니다. 

![지역에서 차례로 한 영역의 개념 보기](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>가용성 영역을 지원하는 지역

- 미국 동부 2
- 서유럽

## <a name="services-that-support-availability-zones"></a>가용성 영역을 지원하는 서비스

가용성 영역을 지원하는 Azure 서비스는 다음과 같습니다.

- Linux Virtual Machines
- Windows Virtual Machines
- 영역 가상 컴퓨터 확장 집합
- Managed Disks
- 부하 분산 장치

## <a name="supported-virtual-machine-size-families"></a>지원되는 가상 컴퓨터 크기 제품군

- Av2
- Dv2
- DSv2


## <a name="get-started-with-the-availability-zones-preview"></a>가용성 영역 미리 보기 시작

특정 Azure 서비스의 경우 미국 동부 2 및 유럽 서부 지역에서 가용성 영역 미리 보기를 사용할 수 있습니다. 

1. [가용성 영역 미리 보기에 등록합니다](http://aka.ms/azenroll). 
2. Azure 구독에 로그인합니다.
3. 가용성 영역을 지원하는 지역을 선택합니다.
4. 다음 링크 중 하나를 사용하여 서비스에서 가용성 영역을 사용하기 시작합니다. 
    - [가상 컴퓨터 만들기](../virtual-machines/windows/create-portal-availability-zone.md)
    - [영역 가상 컴퓨터 확장 집합 만들기](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md)
    - [PowerShell을 사용하여 Managed Disk 추가](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [부하 분산 장치](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>다음 단계
- [빠른 시작 템플릿](http://aka.ms/azqs)
