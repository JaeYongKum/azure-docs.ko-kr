---
title: "Azure Container Instances 컨테이너 그룹"
description: "컨테이너 그룹이 Azure Container Instances에서 작동하는 방법 이해"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 25eab41c3f0c986bcce33123f86f4c9638b77191
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Azure Container Instances의 컨테이너 그룹

Azure Container Instances의 최상위 리소스는 컨테이너 그룹입니다. 이 문서에서는 컨테이너 그룹의 정의와 이들이 지원하는 시나리오 유형에 대해 설명합니다.

## <a name="how-a-container-group-works"></a>컨테이너 그룹 작동 방식

컨테이너 그룹은 같은 호스트 컴퓨터에서 예약되어 있고 라이프사이클, 로컬 네트워크 및 저장소 볼륨을 공유하는 컨테이너 컬렉션입니다. [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 및 [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/)의 *Pod* 개념과 유사합니다.

다음 다이어그램은 여러 컨테이너가 포함된 컨테이너 그룹의 예를 보여 줍니다.

![컨테이너 그룹 예][container-groups-example]

다음 사항에 유의하세요.

- 그룹은 단일 호스트 컴퓨터에서 예약됩니다.
- 그룹에는 1개의 노출 포트가 있는 단일 공용 IP 주소를 노출합니다.
- 이 그룹은 2개의 컨테이너로 구성되어 있습니다. 하나의 컨테이너는 포트 80을 수신하고, 다른 컨테이너는 포트 5000을 수신합니다.
- 그룹에는 볼륨 탑재로 2개의 Azure 파일 공유가 포함되어 있으며, 각 컨테이너는 공유 중 하나를 로컬에서 탑재합니다.

### <a name="networking"></a>네트워킹

컨테이너 그룹은 IP 주소와 해당 IP 주소에서 포트 네임스페이스를 공유합니다. 외부 클라이언트가 그룹 내 컨테이너에 도달하게 지원하려면 IP 주소와 컨테이너에서 해당 포트를 공개해야 합니다. 그룹 내의 컨테이너가 포트 네임스페이스를 공유하고 있으므로 포트 매핑이 지원되지 않습니다. 그룹 내 컨테이너는 해당 포트가 그룹의 IP 주소에 외부적으로 노출되어 있지 않은 경우에도 이들이 노출한 포트의 localhost를 통해 서로 도달할 수 있습니다.

### <a name="storage"></a>저장소

컨테이너 그룹 내에서 탑재할 외부 볼륨을 지정할 수 있습니다. 해당 볼륨을 그룹의 개별 컨테이너 내에 있는 특정 경로에 매핑할 수 있습니다.

## <a name="common-scenarios"></a>일반적인 시나리오

다중 컨테이너 그룹은 단일한 기능적 작업을 다른 팀을 통해 전달하고 별도의 리소스 요구 사항을 갖도록 작은 여러 컨테이너 이미지로 나누려는 경우에 유용합니다. 사용 예는 다음과 같습니다.

- 응용 프로그램 컨테이너 및 로깅 컨테이너. 로깅 컨테이너는 주 응용 프로그램에서 출력한 로그 및 메트릭을 수집하여 장기 저장소로 기록합니다.
- 응용 프로그램 및 모니터링 컨테이너. 모니터링 컨테이너는 응용 프로그램이 올바르게 실행 중이고 응답하고 있는지 확인하기 위해 응용 프로그램에 정기적으로 요청을 보내고, 그렇지 않은 경우 경고를 올립니다.
- 웹 응용 프로그램을 처리하는 컨테이너와 원본 제어에서 최신 콘텐츠를 풀링하는 컨테이너

## <a name="next-steps"></a>다음 단계

Azure Resource Manager 템플릿을 통해 [다중 컨테이너 그룹 배포](container-instances-multi-container-group.md) 방법을 알아 봅니다.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png