# GetCallerOwnerService

Установить файл [GetCallerOwnerService.svc](GetCallerOwnerService.svc) в `Terrasoft.WebApp/ServiceModel/`

![GetCallerOwnerService.svc](img/1.png)

Добавить в файле `Terrasoft.WebApp/ServiceModel/https/services.config` код

```
	<service name="Terrasoft.Configuration.GetCallerOwnerService.GetCallerOwnerService">
		<endpoint name="GetCallerOwnerServiceEndPoint"
			address="" 
			binding="webHttpBinding"
			behaviorConfiguration="RestServiceBehavior"
			bindingNamespace="http://Terrasoft.WebApp.ServiceModel"
			contract="Terrasoft.Configuration.GetCallerOwnerService.GetCallerOwnerService" />
	</service>
```

![services.config](img/2.png)

Добавить в файле `Terrasoft.WebApp/Web.config` код:

```
  <location path="ServiceModel/GetCallerOwnerService.svc">
    <system.web>
      <authorization>
        <allow users="*" />
      </authorization>
    </system.web>
  </location>
```

![Web.config](img/3.png)
   

