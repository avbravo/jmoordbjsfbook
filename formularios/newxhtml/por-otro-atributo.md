# Por otro atributo

## Formulario para Facultad

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

        <b:form id="form"  prependId="false"  rendered="#{loginController.loggedIn and applicationMenu.facultad.create}" onkeypress="if (event.keyCode == 13) {
                    return false;
                }">
            <h:panelGroup id="content" layout="block"> 

                <jmoordbjsf:messages id="msg"/>
                <jmoordbjsf:toolbarnew label="#{msg['field.descripcion']}"
                       title="#{msg['titleview.facultad']}"
                       value="#{facultadController.facultad.descripcion}"
                       isnew="#{facultadController.isNew()}"
                       disabled="#{facultadController.writable}"
                       new="#{facultadController.prepare('new',facultadController.facultad)}"
                       rendererList="#{applicationMenu.facultad.list}"
                       list="#{facultadController.prepare('golist',facultadController.facultad)}"

                       />
                <b:panel title="#{app['title.data']}" look="primary" rendered="#{facultadController.writable}">



                    <b:panelGrid id="panel" colSpans="2,10" size="xs" rendered="#{facultadController.writable}"> 



                        <p:outputLabel  value="#{msg['field.activo']}" />
                        <jmoordbjsf:yesno value="#{facultadController.facultad.activo}" id="activo"  required="true"/>

                        <jmoordbjsf:save rendered="#{facultadController.writable and applicationMenu.facultad.create}"
                                save="#{facultadController.save()}" />


                    </b:panelGrid>


                </b:panel>
            </h:panelGroup>
        </b:form>
        <jmoordbjsf:denegado renderedcondition="#{!loginController.loggedIn or !applicationMenu.facultad.create}" />

        <br/><br/><br/>
    </ui:define>
</ui:composition>
```



