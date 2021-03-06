---
title: "자습서: Azure Active Directory와 Zendesk 통합 | Microsoft Docs"
description: "Azure Active Directory와 Zendesk 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>자습서:Azure Active Directory와 Zendesk 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 Zendesk를 통합하는 방법에 대해 알아봅니다.

Zendesk와 Azure AD를 통합하면 다음과 같은 이점이 제공됩니다.

- Zendesk에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 Zendesk에 자동으로 로그온(Single Sign-on)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](active-directory-appssoaccess-whatis.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

Zendesk와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- Zendesk Single Sign-On이 설정된 구독


> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.


이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.


## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 Zendesk 추가
2. Azure AD Single Sign-on 구성 및 테스트


## <a name="adding-zendesk-from-the-gallery"></a>갤러리에서 Zendesk 추가
Zendesk의 Azure AD 통합을 구성하려면 갤러리의 Zendesk를 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 Zendesk를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![응용 프로그램][2]
    
3. 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![응용 프로그램][3]

4. 검색 상자에 **Zendesk**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. 결과 패널에서 **Zendesk**를 선택하고 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 Zendesk에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 Zendesk 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 Zendesk의 관련 사용자 간에 연결이 형성되어야 합니다.

이 연결 관계는 Azure AD의 **사용자 이름** 값을 Zendesk의 **Username** 값으로 할당하여 설정합니다.

Zendesk에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[Zendesk 테스트 사용자 만들기](#creating-a-zendesk-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 Zendesk에 만듭니다.
4. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 Zendesk 응용 프로그램에서 Single Sign-On을 구성합니다.

**Zendesk에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **Zendesk** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-on 구성][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. **Zendesk 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. **로그온 URL** 텍스트 상자에 다음 패턴으로 값을 입력합니다. `https://<subdomain>.zendesk.com` 

    b. **식별자** 텍스트 상자에 `https://<subdomain>.zendesk.com` 패턴으로 값을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자 URL로 이러한 값을 업데이트합니다. 이러한 값을 얻으려면 [Zendesk 지원 팀](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise)에 문의하세요. 

4. Zendesk에는 특정 형식의 SAML 어설션이 필요합니다. 필수 SAML 특성은 없지만 필요에 따라 다음 단계를 수행하여 **사용자 특성** 섹션에서 특성을 추가할 수 있습니다. 

     ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. **기타 모든 사용자 특성 보기 및 편집** 확인란을 클릭합니다.
     
    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. **특성 추가**를 클릭하여 **특성 추가** 대화 상자를 엽니다.
    
    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. **이름** 텍스트 상자에 특성 이름(예: **emailaddress**)을 입력합니다.
    
    d. **값** 목록에서 특성 값을 **user.mail**로 선택합니다.
    
    e. **확인**을 클릭합니다.
 
    > [!NOTE] 
    > 기본적으로 Azure AD에 없는 특성을 추가하려면 확장 특성을 사용합니다. **Zendesk**에서 허용하는 전체 SAML 특성 목록을 가져오려면 [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-)(SAML에서 설정할 수 있는 사용자 특성)을 클릭하세요. 

5. **SAML 서명 인증서** 섹션에서 인증서의 **지문** 값을 복사합니다.

    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. **Zendesk 구성** 섹션에서 **Zendesk 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **로그아웃 URL 및 SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. 다른 웹 브라우저 창에서 관리자 권한으로 Zendesk 회사 사이트에 로그인합니다.

8. **Admin**을 클릭합니다.

9. **설정**을 클릭하고 왼쪽 탐색 창에서 **보안**을 클릭합니다.

10. **보안** 페이지에서 다음 단계를 수행합니다. 
   
     ![보안](./media/active-directory-saas-zendesk-tutorial/ic773089.png "보안")

    ![Single Sign-On](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single Sign-On")

     a. **관리자 및 에이전트** 탭을 클릭합니다.

     b. **SSO(Single Sign-On) 및 SAML**을 선택하고**SAML**을 선택합니다.

     c. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL** 값을 **SAML SSO URL** 텍스트 상자에 붙여넣습니다. 

     d. Azure Portal에서 복사한 **로그아웃 URL** 값을 **원격 로그아웃 URL** 텍스트 상자에 붙여넣습니다.
        
     e. Azure Portal에서 복사한 인증서의 **지문** 값을 **인증서 지문** 텍스트 상자에 붙여넣습니다.
     
     f. **Save**를 클릭합니다.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. 대화 상자 위쪽에서 **추가**를 클릭하여 **사용자** 대화 상자를 엽니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    c. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다. 

### <a name="creating-a-zendesk-test-user"></a>Zendesk 테스트 사용자 만들기

Azure AD 사용자가 **Zendesk**에 로그인할 수 있도록 하려면 **Zendesk**로 프로비전되어야 합니다.  
앱에서 할당된 역할에 따라 예상된 동작입니다.

 1. **최종 사용자** 계정은 로그인할 때 자동으로 프로비전됩니다.
 2. **에이전트** 및 **관리자** 계정은 로그인하기 전에 **Zendesk**에서 수동으로 프로비전해야 합니다.
 
**사용자 계정을 프로비전하려면 다음 단계를 수행합니다.**

1. **Zendesk** 테넌트에 로그인합니다.

2. **고객 목록** 탭을 선택합니다.

3. **사용자** 탭을 선택하고 **추가**를 클릭합니다.
   
    ![사용자 추가](./media/active-directory-saas-zendesk-tutorial/ic773632.png "사용자 추가")
4. 프로비전 하려는 기존 Azure AD 계정의 이메일 주소를 입력하고, **저장**을 클릭합니다.
   
    ![새 사용자](./media/active-directory-saas-zendesk-tutorial/ic773633.png "새 사용자")

> [!NOTE]
> 다른 Zendesk 사용자 계정 생성 도구 또는 Zendesk에서 제공하는 APIs를 사용하여 AAD 사용자 계정을 프로비전할 수 있습니다.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 Zendesk에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 Zendesk에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **Zendesk**를 선택합니다.

    ![Single Sign-on 구성](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 Zendesk 타일을 클릭하면 Zendesk 응용 프로그램에 자동으로 로그온됩니다.
액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](active-directory-saas-access-panel-introduction.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](active-directory-saas-tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On이란 무엇입니까?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
