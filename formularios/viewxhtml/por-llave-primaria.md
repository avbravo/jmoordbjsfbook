# Rol view.xhtml

![](/assets/rolview.png)

Formulario simple para el view.xhtml

```java
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html>
<ui:composition template="/layout/template.xhtml" 
                xmlns="http://www.w3.org/1999/xhtml"
                xmlns:h="http://java.sun.com/jsf/html"
                xmlns:f="http://java.sun.com/jsf/core"
                xmlns:b="http://bootsfaces.net/ui"
                xmlns:ui="http://java.sun.com/jsf/facelets"
                xmlns:p="http://primefaces.org/ui"
                xmlns:jmoordbjsf="http://jmoordbjsf.com/taglib">
    <ui:define name="content">
        <!--<h:outputStylesheet library="bsf" name="css/thumbnails.css"/>-->

        <style>
            .thumbnail { max-width: 100%; }
            img.thumbnail:hover, img.thumbnail:focus {
                border: 1px solid;
                border-color: #428BCA;
            }
        </style>

         <b:form id="form"  prependId="false"  rendered="#{loginController.loggedIn and applicationMenu.rol.query}" onkeypress="if (event.keyCode == 13) {
                    return false;
                }">
            <h:panelGroup id="content" layout="block"> 

                <jmoordbjsf:messages id="msg"/>
                <b:panel title="#{msg['titleview.rol']}"  look="primary">
                    <b:panelGrid id="panel" colSpans="2,10" size="xs" rendered="#{rolController.writable}"> 

                        <p:outputLabel  value="#{msg['field.idrol']}" />
                        <p:outputLabel value="#{rolController.rol.idrol}" id="idrol"  />

                         <p:outputLabel  value="#{msg['field.rol']}" />
                        <jmoordbjsf:inputText value="#{rolController.rol.rol}" id="rol"  />



                        <p:outputLabel  value="#{msg['field.activo']}" />
                        <jmoordbjsf:yesno value="#{rolController.rol.activo}" id="activo"  required="true"/>




                    </b:panelGrid>
                    <jmoordbjsf:toolbarview 
                        renderedDelete="#{applicationMenu.rol.delete and rolController.writable }"
                        renderedEdit="#{applicationMenu.rol.edit and rolController.writable}" 
                        renderedList="#{applicationMenu.rol.list and rolController.writable}"                    
                        edit="#{rolController.edit()}"
                        delete="#{rolController.delete(rolController.rol,true)}"
                        print="#{rolController.print()}"
                        url="#{rolController.prepare('golist',rolController.rol)}"
                        />
                </b:panel>
            </h:panelGroup>
        </b:form>
 <jmoordbjsf:denegado renderedcondition="#{!loginController.loggedIn or !applicationMenu.rol.query}" />
        <br/><br/><br/>
    </ui:define>
</ui:composition>
```



