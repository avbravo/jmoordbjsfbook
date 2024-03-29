# Rol list.xhtml

![](/assets/rolistç.png)

* ## Formulario simple para el view

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
                xmlns:jmoordbjsf="http://jmoordbjsf.com/taglib"
                xmlns:e="http://xmlns.jcp.org/jsf/composite/extensions">
    <ui:define name="content">


        <style>
            .thumbnail { max-width: 100%; }
            img.thumbnail:hover, img.thumbnail:focus {
                border: 1px solid;
                border-color: #428BCA;
            }
        </style>

        <b:form id="form"  prependId="false"  rendered="#{loginController.loggedIn and applicationMenu.rol.list}" onkeypress="if (event.keyCode == 13) {
                     return false;
                 }">

            <b:growl id="msgs"/>
            <b:panel title="#{msg['titlelist.rol']}" id="content"   look="primary" > 
                <!--<b:panelGrid colSpans="2,10" size="xs">--> 
                <b:panelGrid colSpans="1,4,1,6"  columns="4" size="xs"> 



                    <p:outputLabel value="#{msg['field.idrol']}"/>
                    <jmoordbjsf:autocomplete converter="#{rolConverter}"
                                             completeMethod="#{rolController.rolServices.complete}"
                                             labeltip1="#{msg['field.idrol']} #{p.idrol}"
                                             labeltip2="#{msg['field.rol']} #{p.rol}"    
                                             listener="#{rolController.handleAutocompleteOfListXhtml}"
                                             value="#{rolController.rolSelected}"
                                             itemLabel="#{p.idrol}"
                                             field="idrol"
                                             update=":form:dataTable"/>

                    <p:outputLabel value="#{msg['field.rol']}"/>
                    <jmoordbjsf:autocomplete converter="#{rolConverter}"
                                             completeMethod="#{rolController.rolServices.complete}"
                                             labeltip1="#{msg['field.idrol']} #{p.idrol}"
                                             labeltip2="#{msg['field.rol']} #{p.rol}"    listener="#{rolController.handleAutocompleteOfListXhtml}"
                                             value="#{rolController.rolSelected}"
                                             itemLabel=" #{p.rol}"
                                             field="rol"
                                             update=":form:dataTable"/>

                </b:panelGrid>
            </b:panel>
            <b:panel id="dataTable" look="primary">
                <jmoordbjsf:paginator 
                    rowPage="#{rolController.rowPage}"
                    clear="#{rolController.clear()}"
                    first="#{rolController.first()}"
                    back="#{rolController.back()}"
                    next="#{rolController.next()}"
                    last="#{rolController.last()}"
                    page="#{rolController.page}"
                    pages="#{rolController.pages}"
                    skip="ajax:rolController.skip(rolController.page)"  
                    new="#{rolController.prepare('gonew',rolController.rol)}"
                    printAll="#{rolController.printAll()}"
                    />
                <b:dataTable value="#{rolController.rolDataModel}"
                             var="item"
                             id="dataTable2"
                             paginated="false"
                             onpage="console.log('page');">

                    <b:dataTableColumn value="#{item.idrol}" label="#{msg['field.idrol']}"/>
                    <b:dataTableColumn value="#{item.rol}" label="#{msg['field.rol']}" />
                    <b:dataTableColumn value="#{item.activo}" label="#{msg['field.activo']}" />

                    <b:dataTableColumn label="">

                        <jmoordbjsf:column
                            edit="#{rolController.prepare('view',item)}"
                            delete="#{rolController.delete(item,false)}"
                            rendered="#{applicationMenu.rol.delete}"
                            />
                    </b:dataTableColumn>

                </b:dataTable>
            </b:panel>


        </b:form>

        <jmoordbjsf:denegado renderedcondition="#{!loginController.loggedIn or !applicationMenu.rol.list}" />
    </ui:define>
</ui:composition>
```



