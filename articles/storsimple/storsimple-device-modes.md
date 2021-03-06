---
title: "StorSimple 장치 모드 변경 | Microsoft Docs"
description: "StorSimple 장치 모드 및 StorSimple용 Windows PowerShell을 사용하여 장치 모드를 변경하는 방법을 설명합니다."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: e9d7d277-8a2f-45eb-9fef-355486e14cbc
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: alkohli
ms.openlocfilehash: 815d4bd539db8aebf2ee1eb62aef13b554b9cc47
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/07/2017
---
# <a name="change-the-device-mode-on-your-storsimple-device"></a>StorSimple 장치에서 장치 모드 변경
> [!NOTE]
> StorSimple의 클래식 포털은 사용되지 않습니다. StorSimple 장치 관리자는 사용 중단 일정에 따라 자동으로 새 Azure Portal로 이동합니다. 이 이동에 대한 메일 및 포털 알림을 받게 됩니다. 이 문서도 곧 사용 중지됩니다. 이 문서의 새 Azure Portal용 버전을 보려면 [StorSimple 장치에서 장치 모드 변경](storsimple-8000-device-modes.md)으로 이동하세요. 이동과 관련된 자세한 내용은 [FAQ: Azure Portal로 이동](storsimple-8000-move-azure-portal-faq.md)을 참조하세요.

이 문서에서는 StorSimple 장치에서 작동할 수 있는 다양한 모드에 대한 간략한 설명을 제공합니다. StorSimple 장치는 표준, 유지 관리 및 복구의 세 가지 모드로 작동할 수 있습니다. 

이 문서를 읽은 후 다음에 대해 알 수 있습니다.

* StorSimple 장치 모드 정의
* StorSimple 장치의 현재 모드를 파악하는 방법
* 표준에서 유지 관리 모드로, 그리고 *그 반대로*

위의 관리 작업은 StorSimple 장치의 Windows PowerShell 인터페이스를 통해만 수행할 수 있습니다.

## <a name="about-storsimple-device-modes"></a>StorSimple 장치 모드
StorSimple 장치는 표준, 유지 관리 또는 복구 모드로 작동할 수 있습니다. 각 모드는 아래에 간단하게 설명되어 있습니다.

### <a name="normal-mode"></a>표준 모드
완전히 구성된 StorSimple 장치에 대한 표준 작동 모드로 정의됩니다. 기본적으로 장치는 표준 모드에 있어야 합니다.

### <a name="maintenance-mode"></a>유지 관리 모드
때로는 StorSimple 장치가 유지 관리 모드에 있어야 합니다. 이 모드를 사용하면 장치에서 유지 관리를 수행하고 디스크 펌웨어 관련 업데이트와 같은 중단 업데이트를 설치할 수 있습니다.

StorSimple용 Windows PowerShell을 통해서만 시스템을 유지 관리 모드로 전환할 수 있습니다. 모든 I/O 요청은 이 모드에서 일시 중지됩니다. 비휘발성 임의 액세스 메모리 (NVRAM) 등의 서비스 또는 클러스터링 서비스도 중지됩니다. 이 모드로 들어가거나 종료하면 두 컨트롤러가 모두 다시 시작됩니다. 유지 관리 모드를 종료하면 모든 서비스가 다시 시작되고 정상 상태여야 합니다. 몇 분이 걸릴 수 있습니다.

> [!NOTE]
> **유지 관리 모드는 정상적으로 작동하는 장치에만 지원됩니다. 하나 또는 모든 컨트롤러가 작동하지 않는 장치에서 지원되지 않습니다.**
> </br>
> 
> 

### <a name="recovery-mode"></a>복구 모드
복구 모드는 "네트워크를 지원하는 Windows 안전 모드"로 설명될 수 있습니다. 복구 모드는 Microsoft 기술 지원 서비스 팀을 통해 시스템에서 진단을 수행할 수 있습니다. 복구 모드의 기본 목표는 시스템 로그를 검색하는 것입니다.

시스템이 복구 모드로 전환되는 경우 Microsoft 지원에 다음 단계를 문의해야 합니다. 자세한 내용은 [Microsoft 지원에 문의](storsimple-contact-microsoft-support.md)로 이동하세요.

> [!NOTE]
> **장치를 복구 모드로 유지할 수 없습니다. 장치가 잘못된 상태에 있으면 복구 모드는 Microsoft 고객 지원 담당자가 검사할 수 있는 상태로 장치를 전환하려고 합니다.**
> 
> 

## <a name="determine-storsimple-device-mode"></a>StorSimple 장치 모드 확인
#### <a name="to-determine-the-current-device-mode"></a>현재 장치 모드를 확인하려면
1. [장치 직렬 콘솔 연결에 PuTTY 사용](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)단계를 수행하여 장치 직렬 콘솔에 로그온합니다.
2. 장치의 직렬 콘솔 메뉴에 있는 배너 메시지를 살펴봅니다. 이 메시지는 장치가 유지 관리 또는 복구 모드에 있는지를 명시적으로 나타냅니다. 메시지에 시스템 모드에 대한 특정 정보가 없는 경우 장치가 표준 모드에 있습니다.

## <a name="change-the-storsimple-device-mode"></a>StorSimple 장치 모드 변경
StorSimple 장치를 유지 관리 모드(표준 모드에서)에 배치하여 유지 관리를 수행하거나 유지 관리 모드 업데이트를 설치할 수 있습니다. 유지 관리 모드로 들어가거나 종료하려면 다음 절차를 수행하세요.

> [!IMPORTANT]
> 유지 관리 모드에 들어가기 전에 Azure 클래식 포털의 **유지 관리** 페이지에서 **하드웨어 상태**에 액세스하여 두 장치 컨트롤러 모두가 정상 상태인지 확인합니다. 하나 또는 모든 컨트롤러가 정상 상태가 아니면 다음 단계는 Microsoft 지원에 문의하세요. 자세한 내용은 [Microsoft 지원에 문의](storsimple-contact-microsoft-support.md)로 이동하세요.
> 
> 

#### <a name="to-enter-maintenance-mode"></a>유지 관리 모드로 전환하려면
1. [장치 직렬 콘솔 연결에 PuTTY 사용](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)단계를 수행하여 장치 직렬 콘솔에 로그온합니다.
2. 직렬 콘솔 메뉴에서 옵션 1, **모든 권한으로 로그인**을 선택합니다. 메시지가 표시되면 **장치 관리자 암호**를 제공합니다. 기본 암호는 `Password1`입니다.
3. 명령 프롬프트에 다음을 입력합니다. 
   
    `Enter-HcsMaintenanceMode`
4. 유지 관리 모드에서는 모든 I/O 요청이 중단되며 Azure 클래식 포털에 대한 연결이 끊어짐을 알리는 경고 메시지와 확인 요청 메시지가 표시됩니다. **Y** 를 입력하여 유지 관리 모드로 전환합니다.
5. 두 컨트롤러가 모두 다시 시작됩니다. 다시 시작이 완료되면 장치가 유지 관리 모드임을 나타내는 다른 메시지가 표시됩니다.

#### <a name="to-exit-maintenance-mode"></a>유지 관리 모드를 종료하려면
1. 장치 직렬 콘솔에 로그온합니다. 배너 메시지에서 장치가 유지 관리 모드에 있는지 확인합니다.
2. 명령 프롬프트에 다음을 입력합니다.
   
    `Exit-HcsMaintenanceMode`
3. 경고 메시지와 확인 메시지가 표시됩니다. **Y** 를 입력하여 유지 관리 모드를 종료합니다.
4. 두 컨트롤러가 모두 다시 시작됩니다. 다시 시작이 완료되면 장치가 표준 모드임을 나타내는 다른 메시지가 표시됩니다.

## <a name="next-steps"></a>다음 단계
StorSimple 장치에서 [표준 및 유지 관리 모드 업데이트를 적용](storsimple-update-device.md) 하는 방법을 알아봅니다.

