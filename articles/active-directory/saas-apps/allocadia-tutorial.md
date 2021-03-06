---
title: 'Självstudier: Azure Active Directory-integration med Allocadia | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Allocadia.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c415fc55-6dc1-49f2-a8a2-2fc6e3790d65
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: jeedes
ms.openlocfilehash: a25fbd4daaa4d1fb09d788ba7ab61392ffb100f2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043922"
---
# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Självstudier: Azure Active Directory-integration med Allocadia

I den här självstudien får du lära dig hur du integrerar Allocadia med Azure Active Directory (AD Azure).

Integrera Allocadia med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till Allocadia
- Du kan aktivera användarna att automatiskt få loggat in på Allocadia (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton på en central plats – Azure portal

Om du vill veta mer om integrering av SaaS-app med Azure AD finns i [vad är programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med Allocadia, behöver du följande objekt:

- En Azure AD-prenumeration
- En Allocadia enkel inloggning aktiverad prenumeration

> [!NOTE]
> Om du vill testa stegen i den här självstudien rekommenderar vi inte med hjälp av en produktionsmiljö.

Om du vill testa stegen i den här självstudien bör du följa dessa rekommendationer:

- Använd inte din produktionsmiljö, om det inte behövs.
- Om du inte har en Azure AD-utvärderingsmiljö kan du få en månads utvärdering [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudien kan du testa Azure AD enkel inloggning i en testmiljö. Det scenario som beskrivs i den här självstudien består av två viktigaste byggstenarna:

1. Att lägga till Allocadia från galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-allocadia-from-the-gallery"></a>Att lägga till Allocadia från galleriet
För att konfigurera integrering av Allocadia i Azure AD, som du behöver lägga till Allocadia från galleriet i din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till Allocadia från galleriet:**

1. I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Gå till **företagsprogram**. Gå till **alla program**.

    ![Program][2]
    
3. Lägg till nytt program, klicka på **nytt program** knappen överst i dialogrutan.

    ![Program][3]

4. I sökrutan skriver **Allocadia**.

    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/tutorial_allocadia_search.png)

5. I resultatpanelen väljer **Allocadia**, och klicka sedan på **Lägg till** för att lägga till programmet.

    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/tutorial_allocadia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet ska du konfigurera och testa Azure AD enkel inloggning med Allocadia baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning att fungera, behöver Azure AD du veta vad användaren motsvarighet i Allocadia är till en användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och relaterade användaren i Allocadia upprättas.

I Allocadia, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** att upprätta länken-relation.

Om du vill konfigurera och testa Azure AD enkel inloggning med Allocadia, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  – om du vill ge användarna använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  – om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Allocadia](#creating-an-allocadia-test-user)**  – du har en motsvarighet för Britta Simon i Allocadia som är länkad till en Azure AD-representation av användaren.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  – om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  – om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Allocadia program.

**Utför följande steg för att konfigurera Azure AD enkel inloggning med Allocadia:**

1. I Azure-portalen på den **Allocadia** program integration-sidan klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På den **enkel inloggning** dialogrutan **läge** som **SAML-baserad inloggning** att aktivera enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_allocadia_samlbase.png)

3. På den **Allocadia domän och URL: er** avsnittet, utför följande steg:

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_allocadia_url.png)

    a. I den **identifierare** textrutan anger du ett URL med hjälp av följande mönster: 
       
     För testmiljö:  `https://na2standby.allocadia.com`
    
     För produktionsmiljö: `https://na2.allocadia.com`

    b. I den **svars-URL** textrutan anger du ett URL med hjälp av följande mönster: 
    
     För testmiljö: `https://na2standby.allocadia.com/allocadia/saml/SSO`
    
     För produktionsmiljö: `https://na2.allocadia.com/allocadia/saml/SSO`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med de faktiska identifierare och svars-URL. Kontakta [Allocadia supportteamet](mailTo:support@allocadia.com) att hämta dessa värden.

4. Allocadia program som förväntar SAML-intyg i ett visst format. Konfigurera följande anspråk för det här programmet. Du kan hantera värdena för dessa attribut från den ”**användarattribut**” på sidan för integrering av program. Följande skärmbild visar ett exempel för den här konfigurationen. 

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_allocadia_attributes.png)
    
5. I den **användarattribut** avsnittet på den **enkel inloggning** dialogrutan Konfigurera SAML-token attributet som visas i bilden och utför följande steg:
    
    | Attributnamn | Attributvärde |
    | ------------------- | -------------------- |    
    | Förnamn | User.givenName |
    | Efternamn | User.surname |
    | e-post | User.Mail |
    
    a. Klicka på **Lägg till attribut** att öppna den **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_attribute_04.png)

    b. I den **namn** textrutan skriver du attributnamnet som visas för den raden.

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_attribute_05.png)

    c. Från den **värdet** anger attributvärdet som visas för den raden.
 
    d. Klicka på **OK**.



6. På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_allocadia_certificate.png) 


7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_general_400.png)

8. Att konfigurera enkel inloggning på **Allocadia** sida, som du behöver skicka de hämtade **XML-Metadata för** till [Allocadia supportteamet](mailTo:support@allocadia.com). De ställer du in SAML SSO ansluta till korrekt inställda på båda sidorna.

> [!TIP]
> Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du ställer in appen!  När du lägger till den här appen från den **Active Directory > företagsprogram** bara klickar du på den **enkel inloggning** fliken och komma åt den inbäddade dokumentationen genom den  **Konfigurationen** avsnittet längst ned. Du kan läsa mer om här funktionen embedded-dokumentation: [Azure AD embedded-dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en Azure AD-användare för testning
Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.

![Skapa en Azure AD-användare][100]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **Azure-portalen**, i det vänstra navigeringsfönstret klickar du på **Azure Active Directory** ikon.

    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/create_aaduser_01.png) 

2. Om du vill visa en lista över användare, gå till **användare och grupper** och klicka på **alla användare**.
    
    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/create_aaduser_02.png) 

3. Öppna den **användaren** dialogrutan klickar du på **Lägg till** överst i dialogrutan.
 
    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/create_aaduser_03.png) 

4. På den **användaren** dialogrutan utför följande steg:
 
    ![Skapa en Azure AD-användare för testning](./media/allocadia-tutorial/create_aaduser_04.png) 

    a. I den **namn** textrutan typ **BrittaSimon**.

    b. I den **användarnamn** textrutan skriver den **e-postadress** av BrittaSimon.

    c. Välj **visa lösenord** och anteckna värdet för den **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-allocadia-test-user"></a>Skapa en Allocadia testanvändare

Program stöder bara i tid användaretablering. Efter autentisering skapas användare i programmet automatiskt.

### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

I det här avsnittet ska aktivera du Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Allocadia.

![Tilldela användare][200] 

**Om du vill tilldela Britta Simon Allocadia, utför du följande steg:**

1. Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** klickar **alla program**.

    ![Tilldela användare][201] 

2. I listan med program väljer **Allocadia**.

    ![Konfigurera enkel inloggning](./media/allocadia-tutorial/tutorial_allocadia_app.png) 

3. I menyn till vänster, klickar du på **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg till tilldelning** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** dialogrutan **Britta Simon** på listan användare.

6. Klicka på **Välj** knappen **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen **Lägg till tilldelning** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet ska testa du Azure AD enkel inloggning för konfigurationen med hjälp av åtkomstpanelen.

När du klickar på panelen Allocadia i åtkomstpanelen du bör få automatiskt loggat in på ditt Allocadia program.
Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över guider om hur du integrerar SaaS-appar med Azure Active Directory](tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/allocadia-tutorial/tutorial_general_01.png
[2]: ./media/allocadia-tutorial/tutorial_general_02.png
[3]: ./media/allocadia-tutorial/tutorial_general_03.png
[4]: ./media/allocadia-tutorial/tutorial_general_04.png

[100]: ./media/allocadia-tutorial/tutorial_general_100.png

[200]: ./media/allocadia-tutorial/tutorial_general_200.png
[201]: ./media/allocadia-tutorial/tutorial_general_201.png
[202]: ./media/allocadia-tutorial/tutorial_general_202.png
[203]: ./media/allocadia-tutorial/tutorial_general_203.png

