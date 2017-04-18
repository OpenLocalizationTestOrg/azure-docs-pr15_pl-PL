<properties
    pageTitle="Widok SAML zwracane przez usługę kontroli dostępu (Java)"
    description="Dowiedz się, jak wyświetlić SAML zwracane przez usługę kontroli dostępu w aplikacji Java znajdującej się na Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Jak wyświetlić SAML zwrócony przez usługę Azure kontroli dostępu

Ten przewodnik procedurach pokazano, jak wyświetlić źródłowych zabezpieczeń potwierdzenia SAML (Markup Language) Powrót do aplikacji przez usługi kontroli dostępu Azure (ACS). Przewodnik tworzy na temat [sposobu uwierzytelniania użytkowników sieci Web z Zaćmienie przy użyciu usługi kontroli dostępu Azure][] , dostarczając kod, który wyświetla informacje SAML. Wypełniony wniosek będzie wyglądać podobnie do następującej.

![Przykład SAML wyjścia][saml_output]

Aby uzyskać więcej informacji o ACS zobacz sekcję [Następne kroki](#next_steps) .

> [AZURE.NOTE]
> Filtr sterowania usług programu Access Azure jest podgląd technologii społeczności. Jako wersji wstępnej oprogramowania jest formalnego nieobsługiwany przez firmę Microsoft.

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać zadania w tym przewodniku, wypełnij próbki w [sposób uwierzytelniania użytkowników sieci Web z Zaćmienie przy użyciu usługi kontroli dostępu Azure][] i użyty jako punktu wyjścia do tego samouczka.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Dodawanie biblioteki JspWriter do kompilacji zestawu ścieżkę i wdrażania

Dodawanie biblioteki zawierającej klasy **javax.servlet.jsp.JspWriter** do kompilacji zestawu ścieżkę i wdrożenia. Jeśli używasz Tomcat biblioteka jest **jsp api.jar**, który znajduje się w folderze **Biblioteka** Apache.

1. W oknie Project Explorer Zaćmienie osoby kliknij prawym przyciskiem myszy **MyACSHelloWorld**, kliknij przycisk **Konstruuj ścieżkę**, kliknij pozycję **Konfigurowanie Tworzenie ścieżki**, kliknij kartę **biblioteki** , a następnie kliknij **Dodaj JARs zewnętrznych**.
2. W oknie dialogowym **Wybieranie JAR** przejdź do potrzeby SŁOIK, zaznacz go i kliknij przycisk **Otwórz**.
3. Przy użyciu okna dialogowego **Właściwości MyACSHelloWorld** jest nadal otwarty kliknij **Zestaw wdrożenia**.
4. W oknie dialogowym **Web wdrażania zestawu** kliknij przycisk **Dodaj**.
5. W oknie dialogowym **Nowy dyrektywy zestawu** kliknij **Pozycje Tworzenie ścieżki Java** , a następnie kliknij przycisk **Dalej**.
6. Wybierz odpowiednią bibliotekę, a następnie kliknij przycisk **Zakończ**.
7. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości dla MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Aby wyświetlić SAML zmodyfikuj JSP

Modyfikowanie **index.jsp** , aby użyć następującego kodu.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Uruchamianie aplikacji

1. Uruchamianie aplikacji w emulatorze komputera lub wdrażanie Azure, wykonując czynności opisane w [sposób uwierzytelniania użytkowników sieci Web z Zaćmienie przy użyciu usługi kontroli dostępu Azure][].
2. Uruchamianie przeglądarki i otwórz aplikacji sieci web. Po zalogowaniu się do aplikacji zostanie wyświetlona SAML informacji, takich jak potwierdzenia zabezpieczeń dostarczony przez dostawcę tożsamości.

## <a name="next-steps"></a>Następne kroki

Do dalszych Eksplorowanie funkcji ACS firmy i Aby poeksperymentować z bardziej zaawansowanych scenariuszy, zobacz [2.0 usługi kontroli dostępu][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Usługa sterowania programu Access 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Sposób uwierzytelniania użytkowników w sieci Web z usługą Kontrola dostępu Azure za pomocą Zaćmienie]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 