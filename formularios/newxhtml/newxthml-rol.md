# Por llave primaria

## Rol

* Formulario simple con una llave primaria para usarse en el &lt;jmmordb:toolbarnew&gt;

![](/assets/rolnew.png)



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

        <b:form id="form"  prependId="false"  rendered="#{loginController.loggedIn and applicationMenu.sugerencia.create}" onkeypress="if (event.keyCode == 13) {
                     return false;
                 }">
            <h:panelGroup id="content" layout="block"> 

                <jmoordbjsf:messages id="msg"/>
                <jmoordbjsf:toolbarnew label="#{msg['field.idsugerencia']}"
                                       title="#{msg['titleview.sugerencia']}"
                                       value="#{sugerenciaController.sugerencia.idsugerencia}"
                                       isnew="#{sugerenciaController.isNew()}"
                                       disabled="#{sugerenciaController.writable}"
                                       new="#{sugerenciaController.prepare('new',sugerenciaController.sugerencia)}"
                                       rendererList="#{applicationMenu.sugerencia.list}"
                                       list="#{sugerenciaController.prepare('golist',sugerenciaController.sugerencia)}"

                                       />
                <b:panel title="#{app['title.data']}" look="primary" rendered="#{sugerenciaController.writable}">



                    <b:panelGrid id="panel" colSpans="2,10" size="xs" rendered="#{sugerenciaController.writable}"> 

                        <p:outputLabel  value="#{msg['field.descripcion']}" />
                        <b:inputTextarea rows="2" span="8" value="#{sugerenciaController.sugerencia.descripcion}" id="descripcion"  />

                        <p:outputLabel  value="#{msg['field.activo']}" />
                        <jmoordbjsf:yesno value="#{sugerenciaController.sugerencia.activo}" id="activo"  required="true"/>

                        <jmoordbjsf:save rendered="#{sugerenciaController.writable and applicationMenu.sugerencia.create}"
                                         save="#{sugerenciaController.save()}" />


                    </b:panelGrid>


                </b:panel>
            </h:panelGroup>
        </b:form>
        <jmoordbjsf:denegado renderedcondition="#{!loginController.loggedIn or !applicationMenu.sugerencia.create}" />

        <br/><br/><br/>
    </ui:define>
</ui:composition>
```



