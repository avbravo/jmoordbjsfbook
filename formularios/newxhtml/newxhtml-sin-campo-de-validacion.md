# Sin campo de validación

Algunos formularios no necesitamos validar nada hasta que se presione el botón guardar

Para estos casos:

* Usar toolbarnewsavereturn
* Quitar las validaciones de rendered por el atributo writable de los paneles principales
* El método save incluir las validaciones necesarias.

![](/assets/form.png)

* Remover los atributos del rendered  writable

![](/assets/panel.png)

## Codigo .xhtml

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
                xmlns:e="http://xmlns.jcp.org/jsf/composite/extensions" 
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

        <b:form id="form"  prependId="false"  rendered="#{loginController.loggedIn and applicationMenu.solicitudDocentePorAdministrador.create}" onkeypress="if (event.keyCode == 13) {
                    return false;
                }">
            <h:panelGroup id="content" layout="block"> 

                <jmoordbjsf:messages id="msg"/>


                <b:panel title="#{msg['titleview.solicitudmanualdocente']}" look="primary" >


                    <b:panelGrid id="panel" colSpans="2,10" size="xs" > 

                        <p:outputLabel  value="#{msg['field.solicitadopor']}" />
                        <b:column>
                            <jmoordbjsf:autocomplete converter="#{usuarioConverter}"
                                                     completeMethod="#{usuarioController.usuarioServices.complete}"
                                                     labeltip1="#{msg['field.username']} #{p.username}"
                                                     labeltip2="#{msg['field.nombre']} #{p.nombre}"    listener="#{solicitudManualDocenteController.handleSelect}"
                                                     value="#{solicitudManualDocenteController.solicita}"
                                                     itemLabel=" #{p.nombre}"
                                                     field="nombre"
                                                     dropdown="true"
                                                     minQueryLength="0"

                                                     /> 

                            <p:outputLabel  value="#{msg['field.copiardesde']}" />


                            <jmoordbjsf:autocompleteWithCalendarDateTime
                                converter="#{solicitudConverter}"
                                completeMethod="#{solicitudManualDocenteController.completeSolicitudParaCopiar}"    
                                labeltip1="#{msg['field.idsolicitud']}"
                                labelvalue1="#{p.idsolicitud}"   
                                labeltip2="#{msg['field.solicitadopor']}" 
                                labelvalue2="#{p.usuario.get(0).nombre}"
                                labeltip3="#{msg['field.responsable']}"
                                labelvalue3="#{p.usuario.get(1).nombre}"
                                labeltip4="#{msg['field.mision']} "
                                labelvalue4="#{p.mision}" 
                                labeltip5="#{msg['field.lugares']}" 
                                labelvalue5="#{p.lugares}"
                                calendarsize="20"
                                columnpaneltip="2"
                                calendardatelabel1="#{msg['field.fechapartida']}" 
                                calendardatevalue1="#{p.fechahorapartida}" 

                                calendartimelabel1="#{msg['field.horapartida']}"
                                calendartimevalue1="#{p.fechahorapartida}"  

                                calendardatelabel2="#{msg['field.fecharegreso']}" 
                                calendardatevalue2="#{p.fechahoraregreso}"  


                                calendartimelabel2="#{msg['field.horaregreso']}" 
                                calendartimevalue2="#{p.fechahoraregreso}"  


                                listener="#{solicitudManualDocenteController.handleSelectCopiarDesde}"
                                value="#{solicitudManualDocenteController.solicitudCopiar}"
                                itemLabel="#{p.mision}"
                                field="mision"
                                dropdown="true"
                                minQueryLength="0"
                                update=":form:panel"
                                />

                        </b:column>


                        <p:outputLabel value="#{msg['field.responsable']}"/>
                        <jmoordbjsf:autocomplete converter="#{usuarioConverter}"
                                                 completeMethod="#{usuarioController.usuarioServices.complete}"
                                                 labeltip1="#{msg['field.username']} #{p.username}"
                                                 labeltip2="#{msg['field.nombre']} #{p.nombre}"    listener="#{solicitudManualDocenteController.handleSelect}"
                                                 value="#{solicitudManualDocenteController.responsable}"
                                                 itemLabel=" #{p.nombre}"
                                                 field="nombre"
                                                 dropdown="true"
                                                 minQueryLength="0"
                                                 update=":form:panel"
                                                 />


                        <p:outputLabel  value="#{msg['field.telefono']}" />
                        <jmoordbjsf:inputText span="4"  value="#{solicitudManualDocenteController.responsable.celular}" id="celular" label="#{msg['field.celular']}" />

                        <p:outputLabel  value="#{msg['field.email']}" />
                        <jmoordbjsf:email span="8" value="#{solicitudManualDocenteController.responsable.email}" id="email"  label="#{msg['field.email']}" />


                        <p:outputLabel  value="#{msg['field.semestre']}" />
                        <jmoordbjsf:autocomplete converter="#{semestreConverter}"
                                                 completeMethod="#{semestreController.semestreServices.complete}"
                                                 labeltip1="#{p.idsemestre}"
                                                 labeltip2="#{p.descripcion}"     listener="#{solicitudManualDocenteController.handleSelect}"
                                                 value="#{solicitudManualDocenteController.solicitud.semestre}"
                                                 itemLabel="#{p.idsemestre}"
                                                 dropdown="true"
                                                 required="true"
                                                 minQueryLength="0"
                                                 field="descripcion"
                                                 />

                        <p:outputLabel  value="#{msg['field.periodoacademico']}" />
                        <jmoordbjsf:inputText span="4" value="#{solicitudManualDocenteController.solicitud.periodoacademico}" id="periodoacademico"  label="#{msg['field.periodoacademico']}" />








                        <p:outputLabel  value="#{msg['field.facultad']}" />
                        <p:autoComplete dropdown="true"
                                        multiple="true"
                                        scrollHeight="250"
                                        size="55"
                                        emptyMessage="#{app['info.nohayregistros']}"
                                        value="#{solicitudManualDocenteController.facultadList}"
                                        completeMethod="#{solicitudManualDocenteController.completeFiltradoFacultad}"
                                        var="p"
                                        required="false"


                                        itemLabel="#{p.descripcion}"
                                        itemValue="#{p}" forceSelection="true">
                            <f:converter binding="#{facultadConverter}"/>
                            <f:attribute name="field" value="descripcion"/>
                            <f:attribute name="fromstart" value="false"/>
                            <f:attribute name="fielddropdown" value="false"/>
                            <f:attribute name="fieldquerylenth" value="0"/>
                            <p:ajax event="itemSelect" listener="#{solicitudManualDocenteController.handleSelect}"
                                    update="carrera"     />
                            <p:ajax event="itemUnselect" listener="#{solicitudManualDocenteController.itemUnselect}"
                                    update="carrera"
                                    />
                            <!--                            <f:facet name="itemtip">
                                                            <h:panelGrid columns="1" cellpadding="5">
                                                                <h:outputText value="#{msg['field.descripcion']} #{p.descripcion}" />


                                                            </h:panelGrid>
                                                        </f:facet>-->

                        </p:autoComplete>



                        <p:outputLabel  value="#{msg['field.carrera']}" />
                        <p:autoComplete dropdown="true"
                                        id="carrera"
                                        multiple="true"
                                        scrollHeight="250"
                                        size="55"
                                        emptyMessage="#{app['info.nohayregistros']}"
                                        value="#{solicitudManualDocenteController.carreraList}"
                                        completeMethod="#{solicitudManualDocenteController.completeFiltradoCarrera}"
                                        var="p"
                                        required="false"
                                        itemLabel="#{p.descripcion}"
                                        itemValue="#{p}" forceSelection="true">
                            <f:converter binding="#{carreraConverter}"/>
                            <f:attribute name="field" value="descripcion"/>
                            <f:attribute name="fielddropdown" value="false"/>
                            <f:attribute name="fieldquerylenth" value="0"/>
                            <p:ajax event="itemSelect" listener="#{solicitudManualDocenteController.handleSelect}"
                                    update=""
                                    />
                            <p:ajax event="itemUnselect" listener="#{solicitudManualDocenteController.itemUnselect}"
                                    update=""
                                    />
                            <!--                            <f:facet name="itemtip">
                                                            <h:panelGrid columns="1" cellpadding="5">
                                                                <h:outputText value="#{msg['field.descripcion']} #{p.descripcion}" />


                                                            </h:panelGrid>
                                                        </f:facet>-->

                        </p:autoComplete>



                        <p:outputLabel  value="#{msg['field.numerogrupos']}" />
                        <p:chips id="numerogrupo" value="#{solicitudManualDocenteController.solicitud.numerogrupo}" title="#{msg['field.chips']}"/>



                        <p:spacer/>
                        <p:spacer/>

                        <p:outputLabel value="#{msg['field.idtipogira']}"/>
                        <jmoordbjsf:autocomplete converter="#{tipogiraConverter}"
                                                 completeMethod="#{tipogiraController.tipogiraServices.complete}" 
                                                 labeltip1="#{msg['field.tipogira']} #{p.idtipogira}"     listener="#{solicitudManualDocenteController.handleSelect}"
                                                 value="#{solicitudManualDocenteController.solicitud.tipogira}"
                                                 itemLabel=" #{p.idtipogira}"
                                                 field="idtipogira"
                                                 dropdown="true"
                                                 fromstart="true"
                                                 minQueryLength="0"
                                                 update=""/>

                        <p:outputLabel  value="#{msg['field.objetivo']}" />
                        <b:inputTextarea rows="2" span="8" value="#{solicitudManualDocenteController.solicitud.objetivo}" id="objetivo"  label="#{msg['field.objetivo']}" />


                        <p:outputLabel  value="#{msg['field.lugares']}" />
                        <p:chips value="#{solicitudManualDocenteController.solicitud.lugares}" id="lugares"  title="#{msg['field.chips']}" />

                        <p:outputLabel  value="#{msg['field.numerodevehiculos']}" />
                        <b:touchSpin  col-md="3" value="#{solicitudManualDocenteController.solicitud.numerodevehiculos}" min="1" max="20" step="1" />


                        <p:outputLabel  value="#{msg['field.pasajeros']}" />
                        <b:touchSpin  col-md="3"  min="1"  step="1"  value="#{solicitudManualDocenteController.solicitud.pasajeros}" />



                        <p:outputLabel  value="#{msg['field.fechahorapartida']}" />
                        <jmoordbjsf:calendar 
                            pattern="dd/MM/yyyy HH:mm a" value="#{solicitudManualDocenteController.solicitud.fechahorapartida}" id="fechahorapartida"  label="#{msg['field.fechahorapartida']}" />


                        <p:outputLabel  value="#{msg['field.lugarpartida']}" />
                        <jmoordbjsf:inputText span="8" value="#{solicitudManualDocenteController.solicitud.lugarpartida}" id="lugarpartida"  label="#{msg['field.lugarpartida']}" />



                        <p:outputLabel  value="#{msg['field.fechahoraregreso']}" />
                        <jmoordbjsf:calendar pattern="dd/MM/yyyy HH:mm a" value="#{solicitudManualDocenteController.solicitud.fechahoraregreso}" id="fechahoraregreso" 

                                             label="#{msg['field.fechahoraregreso']}" />


                        <p:outputLabel  value="#{msg['field.lugarllegada']}" />
                        <jmoordbjsf:inputText span="8" value="#{solicitudManualDocenteController.solicitud.lugarllegada}" id="lugarllegada"  label="#{msg['field.lugarllegada']}" />



                        <p:outputLabel  value="#{msg['field.recursossolicitados']}" />
                        <p:chips value="#{solicitudManualDocenteController.solicitud.recursossolicitados}" id="recursossolicitados"  title="#{msg['field.chips']}" />

                        <p:outputLabel  value="#{msg['field.observaciones']}" />
                        <b:inputTextarea rows="2" span="8" value="#{solicitudManualDocenteController.solicitud.observaciones}" id="observaciones"  label="#{msg['field.observaciones']}" />




                        <p:outputLabel  value="#{msg['field.sugerencia']}" style="font-size: 25;" />
                        <b:dataTable value="#{sugerenciaController.sugerenciaDataModel}"
                                     var="item"
                                     id="dataTable2"
                                     paginated="false"
                                     onpage="console.log('page');">

                            <b:dataTableColumn value="#{item.descripcion}" label="#{msg['field.descripcion']}"/>


                        </b:dataTable>




                        <jmoordbjsf:toolbarnewsavereturn
                            renderednew ="#{applicationMenu.solicitudDocentePorAdministrador.create}"

                            new="#{solicitudManualDocenteController.prepare('new',solicitudManualDocenteController.solicitud)}"
                            rendererList="#{applicationMenu.solicitudDocentePorAdministrador.list}"
                            list="#{solicitudManualDocenteController.prepare('golist',solicitudManualDocenteController.solicitud)}"
                            renderedsave="#{applicationMenu.solicitudDocentePorAdministrador.create}"
                            save="#{solicitudManualDocenteController.save()}"
                            />


                    </b:panelGrid>


                </b:panel>
            </h:panelGroup>
        </b:form>
        <jmoordbjsf:denegado renderedcondition="#{!loginController.loggedIn or !applicationMenu.solicitudDocentePorAdministrador.create}" />

        <br/><br/><br/>
    </ui:define>
</ui:composition>
```



