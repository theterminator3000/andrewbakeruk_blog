---
title: "Building an Azure Static Web App"
date: 2021-03-16T23:40:15Z
toc: true
draft: false
tags: ['azure', 'devops', 'tutorial']
show_reading_time: true
---

> Azure Static Web Apps might be leaving preview soon. There are going to be two different SKUs Free and Standard.![Azure Static Web Apps pricing in West Europe](/ASWA/azurestaticwebapppricing.png)


Azure Static Web Apps is a service which is currently in preview on Azure. Because its currently in preview, the service is free until it leaves preview. This basically gives you a website, SSL cert (curtesy of Microsoft) and free web hosting!

Azure Static Web Apps automatically builds and deploys a full stack web app from just a GitHub repository. These apps only consist of a static web frontend, and a backend which uses Azure Functions.

When you create a Static Web Apps resource, Azure will set up a GitHub Action workflow in the repository and then publish the updated content straight to your web app.

![GitHub Action in GitHub running in the background](/ASWA/azurestaticwebapp_1.png)

# What do I need to get building?

* Web Domain 
* GitHub account ([Create a free account here](https://github.com/join
))
* Azure account ([Create a free account here](https://azure.microsoft.com/en-gb/free/))
* Git Client installed on your device (I recommend using [git-scm](https://git-scm.com/))

And thats pretty much all you need to create a Static Web App

# Great, I have everything now what do I do?
## Creating the GitHub repository
The first thing that you are going to do is visit the Static Web App teams [GitHub repository](https://github.com/staticwebdev?tab=repositories) and choose a template that you want to use.

![GitHub Repository for the Azure Static Web Apps team](/ASWA/azurestaticwebapp_2.png)

For this tutorial, we are just going to be using Vanilla-Basic. So we click the Vanilla-Basic repository and then the green "use this template" button.

![Azure Static Web Apps - Vanilla-Basic template](/ASWA/azurestaticwebapp_3.png)

This will then ask you to give a name for your cloned repository, this can be called anything that you want to but you need to remember what you called it for later when we start configuration of the Web App in Azure later on.

![Creating a clone of the ASWA template for use](/ASWA/azurestaticwebapp_4.png)

Once you have filled this out, feel free to click the "Create repository from this template". This will then bring over the resources from the repository to your fresh repository.

![Our fresh github repository using the templates resources](/ASWA/azurestaticwebapp_5.png)

## Creating your first Azure Static Web App
Now we have our repository set up we can move over to [the Azure portal](https://portal.azure.com) to set up the static web app.

When you login to the Azure portal, you will be presented with the Azure Homepage just like the one below:

![Azure Portal Homepage](/ASWA/azurestaticwebapp_6.png)

We are going to click **Create a Resource**, and then in the search box type **Static Web App**. When the dropdown appears, click the Static Web App (preview), followed by create.

![Creating the Resource from the Azure Marketplace](/ASWA/azurestaticwebapp_7.png)

![Static Web App (Preview) Marketplace Entry](/ASWA/azurestaticwebapp_8.png)

Now we are presented with the configuration screen for the web app.

![Configuring the resource](/ASWA/azurestaticwebapp_9.png)

Here you are asked to configure a couple of things...

* Resource Group - think of it as a container for all your resources for a project. These can be named anything, but it's easier to follow a naming convention e.g. **rg-staticwebapp-test**
* Name - the name used by Azure to create the object
* Region - where do you want your static web app to be running from. As of this moment, there are only a couple of regions available to choose from:
    * Central US
    * East US 2
    * West US 2
    * East Asia
    * West Europe
* SKU - how much its going to cost you...

![Configuring the resource - partially filled](/ASWA/azurestaticwebapp_9-5.png)

We can also see a button, which asks us to sign into our GitHub account. If you now click the button, and when prompted authorise **Azure Static Web Apps** to have access to your account.

![Authorising access to your GitHub tenant](/ASWA/azurestaticwebapp_10.png)

You then get returned to your resource configuration in Azure, and you will now have some more info to provide for your web app.

From the screenshot we can see that, we have to provide the repository that the Static Web App content is stored, the branch that you want to publish to your static web app.

![Configuring the resource some more](/ASWA/azurestaticwebapp_11.png)

There are some extra things we can configure here such as build presets, but for the purposes of this guide we will just choose Custom and leave it at the defaults.

When you have finished filling out the details, you can then click the Review + Create button at the bottom of the configuration screen.

![Configuring the resource](/ASWA/azurestaticwebapp_12.png)

Now we are on the Review screen we can double check that the details are correct. Once happy click the create button at the bottom of the page.

This will then create a deployment in Azure with the configuration provided above. This may take a few seconds

![Configuring the resource - deployment stage](/ASWA/azurestaticwebapp_13.png)

Once finished you are offered the option to go to your resource, if you click the option and then you are taken to the Static Web App resource we have just created

![The deployed Static Web App resource](/ASWA/azurestaticwebapp_14.png)

## Configuring your DNS provider

Now that we have our deployed web app, we can see that the URL looks a little funny (e.g. https://witty-ground-123456.azurestaticapps.net), whilst this url may work it isn't very friendly to any potential visitor to your website.

So what we are going to do is create a CNAME on your DNS provider to map your domain to the Azure Static Web App.

So first things first, we need to create a **custom domain** in the resource. We can do this by clicking the Custom domains button on the left hand side of your resource

![Configuring the resource - Custom Domain](/ASWA/azurestaticwebapp_16.png)

and then clicking the Add button.

Now we are asked to provide a custom domain, type in your domain (e.g. test.example.com)

![Adding the domain](/ASWA/azurestaticwebapp_17.png)

Once you have entered the domain, click the next button at the bottom of the shard, now we are given the CNAME host record we need to set on our provider.

![Getting the part 2 details](/ASWA/azurestaticwebapp_18.png)

For my domain, I'm using Google Domains (https://domains.google) but this will work on any provider.

![Creating a CNAME on your provider](/ASWA/azurestaticwebapp_19.png)

Once that is done, this may take a little while to propagate the DNS changes. But after 5 minutes, go back to the Azure page and then click Validate. If successful the validation will come back as an success and you can click Add. If it doesn't come back, give it another 5 minutes and then try again.

![Validating the CNAME on Azure](/ASWA/azurestaticwebapp_20.png)

After you click Add, this will create another deployment to your resource to provision the custom domain with SSL. Once deployed, you should be able to visit your site from the now more friendly CNAME.

## Adding some content to my web app

Now we have the hard configuration job, out of the way we can now add some content to our site.

For this, we can either edit the content directly on GitHub or we can clone the repository to our Computer so we can work on it in an IDE.

GitHub | VSCode
------ | ------
![GitHub Editor](/ASWA/azurestaticwebapp_21.png)   | ![VSCode](/ASWA/azurestaticwebapp_22.png)  

For this demo, we will be using Visual Studio Code.

I have made some changes to my HTML from the template, to personalise it more for me.

But now we have made some changes we can save them and then create a git commit which will prep them to be changed on the repository.

This can be done in the command line by running the following commands:

    | git add .
    | git commit -m "Made some changes"
    | git push

or we can do it all from Visual Studio Code.

After we have pushed the changes to GitHub, a GitHub action will run which will take the content of your repository and then bring it into the Azure Static Web App.

![GitHub Action](/ASWA/azurestaticwebapp_23.png)

After the workflow has run, your changes will be live.