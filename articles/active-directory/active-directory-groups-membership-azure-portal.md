---
title: "Azure Active Directory에서 그룹이 속하는 그룹 관리 | Microsoft Docs"
description: "Azure Active Directory에서 그룹은 다른 그룹을 포함할 수 있습니다. 해당 멤버 자격을 관리하는 방법은 다음과 같습니다."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 44911ec2c278f7af7b8ec4bd971bc97b342a8bf6
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Azure Active Directory 테넌트에서 그룹이 속하는 그룹 관리
Azure Active Directory에서 그룹은 다른 그룹을 포함할 수 있습니다. 해당 멤버 자격을 관리하는 방법은 다음과 같습니다.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>내 그룹이 멤버인 그룹을 찾으려면 어떻게 할까요?
1. 디렉터리에 대한 전역 관리자인 계정으로 [Azure AD 관리 센터](https://aad.portal.azure.com)에 로그인합니다.
2. **사용자 및 그룹**을 선택합니다.

   ![사용자 및 그룹 이미지 열기](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. **모든 그룹**을 선택합니다.

   ![그룹 이미지 선택 ](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. 그룹을 선택합니다.
2. **그룹 멤버 자격**을 선택합니다.

   ![그룹 멤버 자격 열기 이미지](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. 다른 그룹의 멤버로 그룹을 추가하려면 **그룹 - 그룹 멤버 자격** 블레이드에서 **추가** 명령을 선택합니다.
2. **그룹 선택** 블레이드에서 그룹을 선택한 다음 블레이드 아래의 **선택** 단추를 선택합니다. 한 번에 하나의 그룹에만 그룹에 추가할 수 있습니다. **사용자** 상자는 사용자 또는 장치 이름 부분에 대한 항목 일치를 기반으로 한 표시를 필터링합니다. 와일드카드 문자는 해당 상자에서 허용되지 않습니다.

   ![그룹 멤버 자격 추가](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. 다른 그룹의 멤버로 그룹을 제거하려면 **그룹 - 그룹 멤버 자격** 블레이드에서 그룹을 선택합니다.
9. **제거** 명령을 선택하고 프롬프트에서 선택 내용을 확인합니다.

   ![멤버 자격 제거 명령](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. 그룹에 대한 그룹 멤버 자격 변경을 마치면 **저장**을 선택합니다.

## <a name="additional-information"></a>추가 정보
이러한 문서는 Azure Active Directory에 대한 추가 정보를 제공합니다.

* [기존 그룹 보기](active-directory-groups-view-azure-portal.md)
* [새 그룹을 만들고 멤버 추가](active-directory-groups-create-azure-portal.md)
* [그룹의 설정 관리](active-directory-groups-settings-azure-portal.md)
* [그룹의 멤버 관리](active-directory-groups-members-azure-portal.md)
* [그룹의 사용자에 대한 동적 규칙 관리](active-directory-groups-dynamic-membership-azure-portal.md)
