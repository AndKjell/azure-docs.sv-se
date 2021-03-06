---
title: 'Självstudier: Azure Active Directory-integration med Front | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och längst fram.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: d0bdf3ff282152f92e1b661bf19768489d1a029b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438621"
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Självstudier: Azure Active Directory-integration med längst fram

Lär dig hur du integrerar Front med Azure Active Directory (AD Azure) i den här självstudien.

Integrera Front med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till längst fram.
- Du kan aktivera användarna att automatiskt få inloggade längst fram (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton på en central plats – Azure portal.

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Front, behöver du följande objekt:

- En Azure AD-prenumeration
- En Front enkel inloggning aktiverat prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö, kan du [få en månads utvärdering](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Front från galleriet
1. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-front-from-the-gallery"></a>Att lägga till Front från galleriet
Om du vill konfigurera integreringen av fram till Azure AD, som du behöver lägga till Front från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Front från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Azure Active Directory-knappen][1]

1. Gå till **företagsprogram**. Gå till **alla program**.

    ![Bladet för Enterprise-program][2]
    
1. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Knappen Nytt program][3]

1. I sökrutan skriver **främre**väljer **främre** resultatet panelen klickar **Lägg till** för att lägga till programmet.

    ![Front i resultatlistan](./media/front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Front baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du känna till motsvarande användaren fram till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren framför upprättas.

Framför, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Front, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
1. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
1. **[Skapa en testanvändare Front](#create-a-front-test-user)**  – du har en motsvarighet för Britta Simon framför som är länkad till en Azure AD-representation av användaren.
1. **[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
1. **[Testa enkel inloggning](#test-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt första program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Front:**

1. I Azure-portalen på den **främre** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning för länken][4]

1. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Enkel inloggning för dialogrutan](./media/front-tutorial/tutorial_front_samlbase.png)

1. På den **Front domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/front-tutorial/tutorial_front_url1.png)

    a. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: `https://<companyname>.frontapp.com`

    b. I den **svars-URL** textrutan anger du ett URL med hjälp av följande mönster: `https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifierare och svars-URL som beskrivs senare i självstudien eller kontakta [Front klienten supportteamet](mailto:support@frontapp.com) att hämta dessa värden. 

1. På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.

    ![Konfigurera enkel inloggning](./media/front-tutorial/tutorial_front_certificate.png) 

1. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/front-tutorial/tutorial_general_400.png)
    
1. På den **Front Configuration** klickar du på **konfigurera främre** att öppna **konfigurera inloggning** fönster. Kopiera den **URL för utloggning, SAML entitets-ID och SAML enkel inloggning för tjänst-URL** från den **Snabbreferens avsnittet.**

    ![Konfigurera enkel inloggning](./media/front-tutorial/tutorial_front_configure.png) 

1. Inloggning till Front-klienten som administratör.

1. Gå till **inställningar (kugghjulsikonen längst ned i den vänstra sidorutan) > Inställningar**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_000.png)

1. Klicka på **Single Sign On** länk.
   
    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_001.png)

1. Välj **SAML** i listrutan för **Single Sign On**.
   
    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_002.png)

1. I den **startpunkt** textrutan anger du värdet för **enkel inloggnings-URL för** från konfigurationsguiden för Azure AD-program.
    
    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_003.png)

1. Öppna din hämtade **Certificate(Base64)** filen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **signeringscertifikat** textrutan.
    
    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_004.png)

1. På den **tjänstinställningar providern** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning på App-sida](./media/front-tutorial/tutorial_front_005.png)

    a. Kopiera värdet för **entitets-ID** och klistra in den i den **identifierare** -textrutan i **Front domän och URL: er** avsnitt i Azure-portalen.

    b. Kopiera värdet för **ACS URL** och klistra in den i den **svars-URL** -textrutan i **Front domän och URL: er** avsnitt i Azure-portalen.
    
1. Klicka på **spara** knappen.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

   ![Skapa en Azure AD-testanvändare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I Azure-portalen, i den vänstra rutan klickar du på den **Azure Active Directory** knappen.

    ![Azure Active Directory-knappen](./media/front-tutorial/create_aaduser_01.png)

1. Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.

    ![”Användare och grupper” och ”alla användare”-länkar](./media/front-tutorial/create_aaduser_02.png)

1. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.

    ![Knappen Lägg till](./media/front-tutorial/create_aaduser_03.png)

1. I den **användaren** dialogrutan utför följande steg:

    ![Dialogrutan användare](./media/front-tutorial/create_aaduser_04.png)

    a. I den **namn** skriver **BrittaSimon**.

    b. I den **användarnamn** skriver användarens Britta Simon e-postadress.

    c. Välj den **visa lösenord** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** box.

    d. Klicka på **Skapa**.
 
### <a name="create-a-front-test-user"></a>Skapa en testanvändare längst fram

I det här avsnittet skapar du en användare som kallas Britta Simon framför. Arbeta med [Front klienten supportteamet](mailto:support@frontapp.com) att lägga till användare i Front-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning om du beviljar åtkomst längst fram.

![Tilldela rollen][200] 

**Om du vill tilldela Britta Simon längst fram, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

1. I listan med program väljer **främre**.

    ![Länken längst fram i listan med program](./media/front-tutorial/tutorial_front_app.png)  

1. I menyn till vänster, klickar du på **användare och grupper**.

    ![Länken ”användare och grupper”][202]

1. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Fönstret Lägg till tilldelning][203]

1. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

1. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

1. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

Målet med det här avsnittet är att testa din Azure AD-SSOconfiguration med hjälp av åtkomstpanelen.

När du klickar på panelen Front i åtkomstpanelen du bör få automatiskt loggat in på ditt första program. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/front-tutorial/tutorial_general_01.png
[2]: ./media/front-tutorial/tutorial_general_02.png
[3]: ./media/front-tutorial/tutorial_general_03.png
[4]: ./media/front-tutorial/tutorial_general_04.png

[100]: ./media/front-tutorial/tutorial_general_100.png

[200]: ./media/front-tutorial/tutorial_general_200.png
[201]: ./media/front-tutorial/tutorial_general_201.png
[202]: ./media/front-tutorial/tutorial_general_202.png
[203]: ./media/front-tutorial/tutorial_general_203.png

