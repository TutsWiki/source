---
date: 2018-06-13
linktitle: Node.js on Google App Engine
title: How to deploy Node.js app on Google App Engine
weight: 10
url: /nodejs-google-app-engine
description: Learn how to deploy your Node.js app on the Google App Engine.
keywords:
  - nodejs
  - node
  - npm
  - google cloud
  - app engine
  - gcloud
  - deploy
---
<meta property="og:image" content="https://tutswiki.com/images/nodejs-google-cloud.png"/>
[Google App Engine](https://cloud.google.com/appengine/) is a part of the Google Cloud Suite which provides a cloud platform for developers where they can develop and host their apps. It provides easily configurable, fast and secure programming environments/tools with the help of which developers can setup a development environment in just a few minutes. Therefore developers don't need to worry about configuring the environment, they can just focus on writing code. It supports all the popular programming languages Java, PHP, Node.js, Python, C#, .Net, Ruby and Go.

{{< youtube id="2PRciDpqpko" >}}

Till now there was no support for Node.js, but Google has just [announced](https://cloudplatform.googleblog.com/2018/06/Now-you-can-deploy-your-Node-js-app-to-App-Engine-standard-environment.html) that developers will now be able to deploy their [Node.js apps to the App Engine](https://cloud.google.com/nodejs/).
![NodeJS on Google Cloud App Engine](/images/nodejs-google-cloud.png "NodeJs on Google Cloud")
## Steps to deploy Node.js web service on App Engine

### Prerequisites

   1. You must have a project on the Google Cloud Platform. If you don't, create one using the [GCP Console](https://console.cloud.google.com/projectselector/appengine/create?lang=nodejs&st=true&_ga=2.136419507.-245729952.1528854239).

   2. The development environment should be configured. You have 2 options here.
      * Configure it on the cloud using [Cloud Shell](https://console.cloud.google.com/appengine?cloudshell=true&_ga=2.72119440.-245729952.1528854239)
      * Setup your local machine
         * Download and install Node.js and Node Package Manager {{% button href="https://nodejs.org/en/download/" icon="fa fa-download" icon-position="right" %}}Get Node.js and npm{{% /button %}}
         {{% notice note %}}
Only Node.js version 8 or greater is supported on App Engine.
        {{% /notice %}}
        * Download and install Google Cloud SDK {{% button href="https://cloud.google.com/sdk/docs/#install_the_latest_cloud_tools_version_cloudsdk_current_version" icon="fa fa-download" icon-position="right" %}}Get Google Cloud SDK{{% /button %}}
         {{% notice note %}}
Google Cloud SDK also installs gcloud command line tool.
        {{% /notice %}}

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-9878675755379402"
     data-ad-slot="5842766387"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### Hello world!

   1. There's a sample Node.js repository on GitHub. Either clone it using [git](https://www.youtube.com/watch?v=kTXksoxWClw&list=PLndX_e9bdotV7vq2NoTwR0OPCNaA9jBEN&index=10) or [download the zip](https://github.com/GoogleCloudPlatform/nodejs-docs-samples/archive/master.zip).

    <!-- language: lang-sh -->
        git clone https://github.com/GoogleCloudPlatform/nodejs-docs-samples
   2. Navigate to the below directory

    <!-- language: lang-sh -->
        cd nodejs-docs-samples/appengine/hello-world/standard
   3. Install the required dependencies using npm utility
   
    <!-- language: lang-js -->
        npm install
   4. Start the web server
   
    <!-- language: lang-js -->
        npm start
   5. Check output
       * If you are using Cloud Shell then click on Web preview
       * If you are on Local Machine then navigate to http://localhost:8080/
        
### Deploy on App Engine

Since we have verified that our Hello World program is working fine, it's time to deploy it.

   1. If you had installed [Google Cloud SDK](https://cloud.google.com/sdk/) properly (as mentioned in [Prerequisites](#prerequisites)), you'll have access to the gcloud command. Run it as below

    <!-- language: lang-sh -->
        gcloud app deploy

   2. Your app should now be live at https://&lt;PROJECT_ID&gt;.appspot.com/
   
    <!-- language: lang-sh -->
        gcloud app browse   
   
That's it. You have successfully deployed your sample Node.js app on the Google App Engine.

{{% notice tip %}}
Refer Google's official guide on [Building a Node.js App on App Engine](https://cloud.google.com/appengine/docs/standard/nodejs/building-app/)
{{% /notice %}}
{{% notice info %}}
You can try out [Google App Engine's free tier](
https://cloud.google.com/free/docs/always-free-usage-limits) to deploy your Node.js app and see if you like it.
{{% /notice %}}

<style>
div.notices {
margin: 0.3rem 0;
</style>
