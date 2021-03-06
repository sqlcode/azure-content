<properties 
	pageTitle="How to Scale an App in an App Service Environment" 
	description="Scaling an app in an App Service Environment" 
	services="app-service" 
	documentationCenter="" 
	authors="ccompy" 
	manager="stefsch" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.workload="na" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="01/14/2016" 
	ms.author="ccompy"/>

# Scaling apps in an App Service Environment #

At a high level, App Service Environments are essentially personal deployments of the Azure App Service into your VNET and only manageable by your subscription. They offer new networking capabilities because they are in your VNET and can also be scaled beyond what is normally available in the Azure App Service environments.  If you need more information around what an App Service Environment(ASE) is then see [What is an App Service Environment][WhatisASE].  For details around creating an App Service Environment or creating a web app in an App Service Environment see [How to Create an App Service Environment][HowtoCreateASE] and [How to create a web app in an App Service Environment][CreateWebappinASE]

As a quick reminder, when you normally change a scale attribute for a web, mobile or API app in the Azure App Service, you are changing that at an App Service Plan(ASP) level.  For details around scaling App Service Plans or just for details on App Service Plans outside of the App Service Environments see [Scale a web app in Azure App Service][ScaleWebapp] and [App Service Plans in depth overview][Appserviceplans].

Scaling an app in an App Service Environment is very similar to scaling apps normally.  In the Azure App Service there are normally three things you can scale:

- pricing plan
- worker size (for dedicated instances)
- number of instances.

In an ASE there is no need to select or change the pricing plan.  In terms of capabilities it is already at a Premium pricing capability level.  In an App Service Environment there are also no shared workers.  They are all dedicated workers.  

With respect to worker sizes, the ASE admin can assign the size of the compute resource to be used for each worker pool.  That means you can have Worker Pool 1 with P4 compute resources and Worker Pool 2 with P1 compute resources, if desired.  They do not have to be in size order.  For details around the sizes and their pricing see the document here [Azure App Service Pricing][AppServicePricing].  This leaves the scaling options for web apps and App Service Plans in an App Service Environment to be:

- worker pool selection
- number of instances

Changing either item is done through the appropriate UI shown for your ASE hosted App Service Plans.  You won't be able to scale up your ASP beyond the number of available compute resources in the worker pool that your ASP is in.  If you need more you need to get your ASE administrator to add more compute resources to the worker pool that you need them in.  For information around re-configuring your ASE read the information here: [How to Configure an App Service environment][HowtoConfigureASE].  You may also want to take advantage of the ASE autoscale features to add capacity based on schedule or metrics.  To get more details on configuring autoscale for the ASE environment itself see [How to configure autoscale for an App Service Environment][ASEAutoscale].

![][1]

### Scaling the number of instances ###

When you first create your web app in an App Service Environment you should scale it up to at least 2 instances to provide fault tolerance.   

If your ASE has enough capacity then this is pretty simple.  You go to your App Service Plan that holds the sites you want to scale up and select Scale.  This opens the UI where you can manually set the scale for your ASP or configure autoscale rules for your ASP.  To manually scale your app simply set ***Scale by*** to ***an instance count that I enter manually***.  From here either drag the slider to the desired quantity or enter it in the box next to the slider.  

![][2] 

The autoscale rules for an ASP in an ASE work the same as they do normally.  You can select ***CPU Percentage*** under ***Scale by*** and create autoscale rules for your ASP based on CPU Percentage or you can create more complex rules using ***schedule and performance rules***.  To see more complete details on configuring autoscale use the guide here [Scale an app in Azure App Service][AppScale]. 


### Worker Pool selection ###

As noted earlier, the worker pool selection is accessed from the ASP UI.  Open the blade for the ASP that you want to scale and select worker pool.  You will see all of the worker pools which you have configured in your App Service Environment.  If you have only one worker pool then you will only see the one pool listed.  To change what worker pool your ASP is in, you simply select the worker pool you want your App Service Plan to move to.  

![][3]

Before moving your ASP from one worker pool to another it is important to make sure you will have adequate capacity for your ASP.  In the list of worker pools, not only is the worker pool name listed but you can also see how many workers are available in that worker pool.  Make sure that there are enough instances available to contain your App Service Plan.  If you need more compute resources in the worker pool you wish to move to, then get your ASE administrator to add them.  

> [AZURE.NOTE] Moving an ASP from one worker pool will cause a restart of the apps in that ASP.  This can cause a small amount of downtime for your app depending on how long it takes your apps to restart.  

## Getting started

To get started with App Service Environments, see [How To Create An App Service Environment][HowtoCreateASE]

For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
