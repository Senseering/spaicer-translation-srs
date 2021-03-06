

<p align="center" >
  <img  href="https://www.spaicer.de/" src="assets/logo_spaicer_test.png" width="70%">
</p>

<p align="center">
   Scalable adaptive production systems through AI-based resilience optimization. <br>
    Powered by <a href="https://www.mydataeconomy.com/#/search">MyDataEconomy</a>
</p>
<p align="center">
  <a href="https://discord.gg/qDF38JDR3D" style="text-decoration:none;"><img src="https://img.shields.io/badge/Discord-9cf.svg?logo=discord" alt="Discord"></a>
</p>

## Motivation
### Testing scenario
The goal is to test and evaluate the requirements created in previous work packages as examples. 
Among other aspects, the execution of external services, the roles of data producers, and central architectural elements such as the catalog and vocabulary provider are to be tested.

In order to focus on testing the architecture, we have decided to use a translation service. However, this is an arbitrarily replaceable service. This scenario can also be performed with object recognition, annomaly detection, machine learning algorithms or artificial intelligence. 

### Spaicer
In a globalized and interconnected business world, production interruptions including supply chain disruption have been the leading business risk for many years.

The ability of a company to permanently adapt to internal and external changes and disruptions is the "quest for resilience". Reinforced by a significant increase in complexity in production due to Industry 4.0, resilience management thus becomes an indispensable success factor for manufacturing companies.

The SPAICER project is developing a data-driven ecosystem based on lifelong, collaborative and low-threshold Smarter Resilience services by leveraging leading AI technologies and Industrie 4.0 standards with the goal of anticipating disruptions (anticipation) and optimally adapting production plans to active disruptions (reaction) at any time.



## Architecture
It is a decentralized architecture in which participating parties exchange data in a sovereign way. The federated catalog serves as a search engine for data sources and data that was created, based on services.
<p align="end" >
  <img  href="https://www.spaicer.de/" src="assets/legend.png" width="100px">
</p>
<p align="center" >
  <img  href="https://www.spaicer.de/" src="assets/architecture.png" width="100%">
</p>


## Scenario

We have a data producer who regularly publishes texts. This can be done every minute or every second. This data producer passes on the data to participating partners within SPAICER, which may be used according to SPAICER guidelines. 
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/1.png" width="100%">
</p>
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/2.png" width="100%">
</p>


A service provider would like to use this data to build up its own database to improve its translation algorithms, and also give the data producer the opportunity to use its service. However, his USP (the translation algorithm) should not to be published. The service provider obtains the authorization to the different texts of the data producer ( Company Y) via the catalog. This is only possible because both parties are within the same project (SPAICER). 
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/3.png" width="100%">
</p>
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/4.png" width="100%">
</p>
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/5.png" width="100%">
</p>

The actual transfer of the data then takes place with a peer-to-peer connection.
This can be done with the help of batch downloads. In our case, we decide to use an event-based data stream (websockets).  

<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/6.png" width="100%">
</p>

```js
    //initially connect service to node of company-x 
    await worker.connect(config)

    //initialize a websocket connection to the data providing node (Company Y)
    const socket = new WebSocket("wss://<your-receipt-id>:<your-permission-key>@<your-provider-url>/uploader/")
    socket.on('open', function () {
        //waits for incoming data. This is called every time company Y stores data at its node
        socket.on('message', async function (message) {
            const fmessage = JSON.parse(message)
            let body = fmessage.message.data

            //execute service 
            let res = await translateText(body.data.text);

            //upload results to node of Company X (Service provider)
            await worker.publish({ text: res.text })
        })
        //...
    })
```

The translated texts are stored on the service provider's node and Company Y is released. 
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/7.png" width="100%">
</p>

<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/8.png" width="100%">
</p>

<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/9.png" width="100%">
</p>

Company Y can now use the catalog to find the data and either transfer the data to its own network or use it freely via open APIs (event-based or batch-based). 

<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/10.png" width="100%">
</p>
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/11.png" width="100%">
</p>
<p align="center" >
  <img  href="https://www.senseering.de/" src="assets/workflow/12.png" width="100%">
</p>






## Install
### Prerequisits
You need to install <a href="https://nodejs.org/en/">node.js</a>

### Create azure translate service
You can simply use  <a href="https://azure.microsoft.com/de-de/services/cognitive-services/translator/"> the instructions</a> and create a free service. You will then need the url and permissionkeys.

### Installation

Clone this repository by
```
git clone hgit@github.com:Senseering/spaicer-translation-srs.git
cd spaicer-translation-srs
```
install node modules by

```
cd textpublisher
npm install
cd ..
cd service
npm install
```


### Configuration 
To configure the two workers you need to register a new datasource at the providing Manager. This datasource publishes raw texts that need to be translated.
....
