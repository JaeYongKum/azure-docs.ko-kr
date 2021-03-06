---
title: "자습서: SAP Business ByDesign과 Azure Active Directory 통합 | Microsoft Docs"
description: "Azure Active Directory 및 SAP Business ByDesign 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ab76a0ac1ef954efd3c66e6f565514b889ed9444
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>자습서: SAP Business ByDesign과 Azure Active Directory 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 SAP Business ByDesign을 통합하는 방법에 대해 알아봅니다.

SAP Business ByDesign을 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- SAP Business ByDesign에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자가 해당 Azure AD 계정으로 SAP Business ByDesign에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory의 응용 프로그램 액세스 및 Single Sign-On이란 무엇인가요?](active-directory-appssoaccess-whatis.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

SAP Business ByDesign과 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- SAP Business ByDesign Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 SAP Business ByDesign 추가
2. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>갤러리에서 SAP Business ByDesign 추가
SAP Business ByDesign의 Azure AD 통합을 구성하려면 갤러리의 SAP Business ByDesign을 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 SAP Business ByDesign을 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Azure Active Directory 단추][1]

2. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 응용 프로그램**으로 이동합니다.

    ![엔터프라이즈 응용 프로그램 블레이드][2]
    
3. 새 응용 프로그램을 추가하려면 대화 상자 맨 위 있는 **새 응용 프로그램** 단추를 클릭합니다.

    ![새 응용 프로그램 단추][3]

4. 검색 상자에 **SAP Business ByDesign**을 입력하고 결과 패널에서 **SAP Business ByDesign**을 선택한 후 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![결과 목록의 SAP Business ByDesign](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성 및 테스트

이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 SAP Business ByDesign에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 SAP Business ByDesign 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 SAP Business ByDesign의 관련 사용자 간에 연결이 형성되어야 합니다.

SAP Business ByDesign에서 Azure AD의 **사용자 이름** 값을 **Username** 값으로 할당하여 연결 관계를 설정합니다.

SAP Business ByDesign에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Azure AD Single Sign-On 구성](#configure-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
2. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On을 테스트하는 데 사용합니다.
3. **[SAP Business ByDesign 테스트 사용자 만들기](#create-an-sap-business-bydesign-test-user)** - Azure AD를 대표하여 SAP Business ByDesign에서 Britta Simon에 해당하는 사용자가 있어야 합니다.
4. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
5. **[Single Sign-On 테스트](#test-single-sign-on)** - 구성이 작동하는지 여부를 확인합니다.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 SAP Business ByDesign 응용 프로그램에서 Single Sign-On을 구성합니다.

**SAP Business ByDesign에서 Azure AD Single Sign-on을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **SAP Business ByDesign** 응용 프로그램 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Single Sign-On 구성 링크][4]

2. **Single Sign-On** 대화 상자에서 **모드**를 **SAML 기반 로그온**으로 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Single Sign-On 대화 상자](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. **SAP Business ByDesign 도메인 및 URL** 섹션에서 다음 단계를 수행합니다.

    ![SAP Business ByDesign 도메인 및 URL Single Sign-On 정보](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. **로그온 URL** 텍스트 상자에서 다음 패턴으로 URL을 입력합니다. `https://<servername>.sapbydesign.com`

    b. **식별자** 텍스트 상자에서 `https://<servername>.sapbydesign.com` 패턴을 사용하여 URL을 입력합니다.

    > [!NOTE] 
    > 이러한 값은 실제 값이 아닙니다. 실제 로그온 URL 및 식별자로 값을 업데이트합니다. 이러한 값을 얻으려면 [SAP Business ByDesign 클라이언트 지원 팀](https://www.sap.com/products/cloud-analytics.support.html)에 문의하세요.

4. **사용자 특성** 섹션에서 다음 단계를 수행합니다.

    ![SAP Business ByDesign 특성 섹션](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. **사용자 ID** 목록에서 **ExtractMailPrefix()** 함수를 선택합니다.
    
    b. **메일** 목록에서 구현에 사용할 사용자 특성을 선택합니다. 예를 들어, EmployeeID를 고유한 사용자 식별자로 사용하고자 하고 ExtensionAttribute2에 특성 값을 저장했다면 user.extensionattribute2를 선택합니다.     

5. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 메타데이터 파일을 저장합니다.

    ![인증서 다운로드 링크](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. **저장** 단추를 클릭합니다.

    ![Single Sign-On 구성 저장 단추](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. **SAP Business ByDesign 구성** 섹션에서 **SAP Business ByDesign 구성**을 클릭하여 **로그온 구성** 창을 엽니다. **빠른 참조 섹션**에서 **SAML Single Sign-On 서비스 URL**을 복사합니다.

    ![SAP Business ByDesign 구성](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. 응용 프로그램에 SSO를 구성하려면 다음 단계를 수행합니다.
   
    a. 관리자 권한으로 SAP Business ByDesign 포털에 로그인합니다.
   
    b. **응용 프로그램 및 사용자 관리 일반 작업**으로 이동하여 **ID 공급자** 탭을 클릭합니다.
   
    c. **새 ID 공급자**를 클릭하고 Azure Portal에서 다운로드한 메타데이터 XML 파일을 선택합니다. 시스템은 메타데이터를 가져와서 필수 서명 인증서 및 암호화 인증서를 자동으로 업로드합니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. **어설션 소비자 서비스 URL**을 SAML 요청에 포함하려면 **어설션 소비자 서비스 URL 포함**을 선택합니다.
   
    e. **Single Sign-on 활성화**를 클릭합니다.
   
    f. 변경 내용을 저장합니다.
   
    g. **내 시스템** 탭을 클릭합니다.
   
    ![Single Sign-On 구성](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Azure Portal에서 복사한 **SAML Single Sign-On 서비스 URL**을 **Azure AD 로그온 URL** 텍스트 상자에 붙여넣습니다.
   
    ![Single Sign-on 구성](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. 직원이 **수동 ID 공급자 선택**을 선택하여 사용자 ID 및 암호 또는 SSO를 사용하는 로그온 중 하나를 수동으로 선택할 수 있는지 여부를 지정합니다.
   
    j. **SSO URL** 섹션에서 시스템에 로그온하려는 직원이 사용해야 하는 URL을 지정합니다. 
    직원에게 전송된 URL 드롭다운 목록에서 다음 옵션 중 하나를 선택할 수 있습니다.
   
    **SSO가 아닌 URL**
   
    시스템은 직원에게 정상적인 시스템 URL만을 보냅니다. 직원은 SSO를 사용하여 로그온할 수 없고 대신 암호 또는 인증서를 사용해야 합니다.
   
    **SSO URL** 
   
    시스템은 직원에게 SSO URL만을 보냅니다. 직원은 SSO를 사용하여 로그온할 수 있습니다. IdP를 통해 인증 요청이 리디렉션됩니다.
   
    **자동 선택**
   
    SSO가 활성 상태가 아닌 경우 시스템은 직원에게 정상적인 시스템 URL을 보냅니다. SSO가 활성 상태인 경우 시스템은 직원이 암호를 가지는지 여부를 확인합니다. 암호를 사용할 수 있는 경우 SSO URL와 SSO가 아닌 URL은 직원에게 전송됩니다. 그러나 직원에게 암호가 없는 경우 SSO URL만이 직원에게 전송됩니다.
   
    k. 변경 내용을 저장합니다.

> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory > 엔터프라이즈 응용 프로그램** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

   ![Azure AD 테스트 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory** 단추를 클릭합니다.

    ![Azure Active Directory 단추](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. 사용자 목록을 표시하려면 **사용자 및 그룹**으로 이동한 후 **모든 사용자**를 클릭합니다.

    !["사용자 및 그룹" 및 "모든 사용자" 링크](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. **사용자** 대화 상자를 열려면 **모든 사용자** 대화 상자 위쪽에서 **추가**를 클릭합니다.

    ![추가 단추](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. **사용자** 대화 상자에서 다음 단계를 수행합니다.

    ![사용자 대화 상자](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. **이름** 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 상자에 사용자인 Britta Simon의 전자 메일 주소를 입력합니다.

    c. **암호 표시** 확인란을 선택한 다음 **암호** 상자에 표시된 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>SAP Business ByDesign 테스트 사용자 만들기

이 섹션에서는 SAP Business ByDesign에서 Britta Simon이라는 사용자를 만듭니다. [SAP Business ByDesign 클라이언트 지원 팀](https://www.sap.com/products/cloud-analytics.support.html)과 협력하여 SAP Business ByDesign 플랫폼에 사용자를 추가합니다. 

> [!NOTE]
> NameID 값이 SAP Business ByDesign 플랫폼에서 사용자 이름 필드와 일치하는지 확인하세요.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 SAP Business ByDesign에 대한 액세스 권한을 부여합니다.

![사용자 역할 할당][200] 

**Britta Simon을 SAP Business ByDesign에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 응용 프로그램 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 응용 프로그램**으로 이동한 후 **모든 응용 프로그램**을 클릭합니다.

    ![사용자 할당][201] 

2. 응용 프로그램 목록에서 **SAP Business ByDesign**을 선택합니다.

    ![응용 프로그램 목록의 SAP Business ByDesign 연결](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    !["사용자 및 그룹" 링크][202]

4. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![할당 추가 창][203]

5. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

6. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

7. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="test-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 SAP Business ByDesign 타일을 클릭하면 SAP Business ByDesign 응용 프로그램에 자동으로 로그온됩니다.

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](active-directory-saas-tutorial-list.md)
* [Azure Active Directory로 응용 프로그램 액세스 및 Single Sign-On이란 무엇입니까?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

