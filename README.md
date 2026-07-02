<p align="center">
<img src="https://i.imgur.com/fMUwhTM.png" height="45%" width="45%"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>

<h2>Summary</h2>
<p>This guide outlines how to deploy Active Directory in Azure. It also explains why each step is necessary and what it actually does.</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computers)
- Remote Desktop
- Active Directory Domain Services
- Dns

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Enterprise

<h2>Introduction to active directory</h2>
<p>I think it's best to begin this tutorial with a little explanation of what Active Directory, Domain Controller, and Clients are and how they work together. This will make it much easier to understand the steps coming.</p>
<p>One of the most important components of an Active Directory environment is also the DNS server. So I would start from there</p>

<h3>DNS</h3>
<p>Computers and servers are identified with an IP address, which is a unique number that's assigned to a server in specific.</p>
<p>If right now you browse to say google.com, what do you think your computer is doing? To you, you are simply typing a name on the browser, but on the backend, a lot of important things are happening</p>
<p>For starters, google.com is a server that is a computer somewhere out in the world that's running a specific code that generates the google.com webpage</p>
<p>Now that creates a problem, how can your computer know which server is actually the Google.com server?</p>
<p>Well, the google.com server must have a unique IP address that we can use to connect to it. But then how do we know which IP address is that of google.com?</p>
<p>That's where DNS servers enter. A DNS server is a server that contains pairings of human-readable names, say google.com, and IP addresses, say 172.217.160.142</p>

<br>
<p>Whenever you type google.com in your browser, what will happen is that your computer will query the DNS server assigned to it for the IP address associated with google.com. It will return to your computer the corresponding address in this case 172.217.160.142, and your web browser would browse to the 172.217.160.142 server</p>
<p>This relationship is better explained in the following graph</p>
<p>If you wanna test this manually, you can actually type 172.217.160.142 in your browser, and it should take you to the google.com webpage</p>
<p>DNS servers are used, namely, to facilitate user interaction with webpages due to the simple fact that it is, of course, easier to type google.com than to type 172.217.160.142 every time you need to go to that webapge</p>

<p>Active Directory (AD) is, in essence, a database that contains objects like users, passwords, policies, and other assets that a company may need.</p>
<p>A domain controller (DC) is a server/computer that serves two purposes: first, it stores the AD, and secondly, every time a client needs to access any objects within the AD, like user accounts or services, it authenticates them.</p>

> [!IMPORTANT]
>The domain controller is a virtual machine that stores Active Directory. This means that even though Active Directory and the domain controller are two different components, they both work inside a single virtual machine


<p>Clients are computers that are joined to the domain that can access its users, resources, and other objects.</p>

<h3>Example</h3>
<p>So let's assume you already have a functioning Active Directory system with one client, let's call him "client_1" joined to the domain. And client_1 is a computer used by Joe, who works for the company as an accountant.</p>
<p>As such, Joe will take his computer, meaning client_1, every morning and enter his username and password to log in. Henceforth, his username and password will be referred to as his credentials. And every morning when he does that, client_1 will request the domain controller to validate those credentials, meaning check in Active Directory whether that user exists and whether the entered password is correct. And if the credentials entered are indeed valid, it will tell client_1 to create a session for his user, and lastly, Joe will be able to log in</p>
<p>This process is showcased graphically in the graphic below</p>
<img src="https://i.imgur.com/ZJvMBn4.png" height="100%" width="100%"/>
<p>I'm outlining this example for 1 simple reason. In the following steps, I will be explaining every step we need to take to create an Active Directory system just like the one i explained. And admittedly enough, a lot of the steps may seem random at first. Nevertheless, I think that if, while I explain them, I refer back to this example, they will make a lot more sense</p>


<h2>Create a resource group</h2>
<p>Everything in Azure has to exist within a resource group. A resource group is basically a folder that we use to group different resources</p>
<p>Virtual machines are no exception. Before we create a single one, we need to create a resource group to store them</p>
<p>And importantly, resources of different resource groups can not communicate with each other, so we must ensure that all of the resources for our Active Directory environment are located in the same resource group.</p>
<br>
<p>To do that, we have to start by logging in to the Azure portal and then going to the resource group icon. We simply hover the icon with the cursor for a couple of seconds until the option "+ Create" appears, and we press it to create a new resource group.</p>
<img src="https://i.imgur.com/DWG1v40.png" height="25%" width="25%"/>
<p>Afterwards, a page where we can name our resource group will appear. We can name our resource group however we like, but for the case of this tutorial, I'm gonna name it "ADDC_Lab"</p>
<p>After we chose our name then we selec the "Review + Create" option to actually create our resource group</p>
<img src="https://i.imgur.com/gcMQds7.png" height="25%" width="25%"/>

<h2>Create a vnet</h2>
<p>A virtual network or vnet is basically a group of devices that communicate with each other</p>
<p>We need to join all of the virtual machines that are going to be part of our Active Directory environment together in a network to allow them to communicate with each other.</p>
<p>To explain why this is necessary, it's worth going back to the example I showed earlier. We need to make sure client_1 is in the same vnet that the domain controller so that client_1 can send its credentials to the dc and the dc can send an authorization to log in to client_1 </p>
<br>
<p>To create a vnet we simply need to hover the virtual network icon with our cursor, and when the option "+ Create" appears, simply click it to create a new vnet</p>
<img src="https://i.imgur.com/lfAHn4b.png" height="25%" width="25%"/>
<p>Then we simply have to set the resource group to the resource group we are going to use for our Active Directory environment. In my case that is the "ADDC_Lab" resource group</p>
<p>And name our vnet, in my case ill name it "ADDC_Lab_vnet"</p>
<img src="https://i.imgur.com/2AtFv1x.png" height="25%" width="25%"/>
<p>Finally we just create our vnet by selecting the "Review + Create" option</p>

<h2>Create a virtual machine to serve as our domain controller</h2>
<p>We then need to create a vm that we will be using as our domain controller.</p>
<br>
<p>To create one, we simply need to hover the "virtual machines" icon with our cursor and when the option "+ Create" appears, simply click it and select "Virtual Machines" to create a new virtual machine</p>
<img src="https://i.imgur.com/EPDovce.png" height="25%" width="25%"/>
<p>In the "Create a virtual machine" page that will appear afterwards, we need to do the following:</p>
<p>Set the resource group of the vm to "ADDC_Lab" or the group we are using for this project</p>
<p>Give the machine the "dc" name or any other appropriate name</p>
<img src="https://i.imgur.com/627hCtQ.png" height="25%" width="25%"/>
<p>Use "Windows Server 2022 Datacenter - x64 Gen2" as our image. We use that as our operating system because it is an operating system built specifically to be used in servers</p>
<p>Set the username and password to anyone of your choice and click next until you get to the networking tab</p>
<img src="https://i.imgur.com/0UGeUAI.png" height="25%" width="25%"/>
<p>There, just set our virtual network to the "ADDC_Lab_vnet" we created earlier and select "Review + Create"</p>
<img src="https://i.imgur.com/rDCaYRs.png" height="25%" width="25%"/>

<h2>Install the necessary attributes into our domain controller</h2>
<p>As stated above, a domain controller is simply a computer that serves the purpose of storing ad and authenticating requests</p>
<p>Right now, our "domain controller" computer doesn't have those capabilities, so we are going to install them one by one, starting from AD </p>
<br>
<p>To install AD, we have to go to the Server Manager app.</p>
<img src="https://i.imgur.com/xIrqmPz.png" height="25%" width="25%"/>
<p>Then, to the add roles and features option</p>
<img src="https://i.imgur.com/axxeIOb.png" height="25%" width="25%"/>
<p>and within that window, we just hit next until we get to this page, in which we check the active directory domain services option</p>
<img src="https://i.imgur.com/Jwa7Sc3.png" height="25%" width="25%"/>
<p>And then we just continue to click next until we get to this page, in which we simply click install</p>
<img src="https://i.imgur.com/7hdO3LK.png" height="25%" width="25%"/>

<br>
<p>So right now in our domain controller, we have the database installed. Nevertheless, our domain controller still isn't really a domain controller because it cannot handle and authenticate requests</p>
<p>Now, before we get into the practical steps, I will explain what our domain controller actually lacks</p>
<p>1. It lacks a DNS server</p>
<p>The internet is just a big network -> explain that</p>
<p>A Domain Name System or DNS server is basically a computer that hosts records containing IP addresses and human-readable name pairs</p>
<p>This is relevant because a machine doesn't understand what Google.com is; it just understands IP addresses. So if a computer wants to browse to Google.com, it needs to know what the Google.com IP address is. Now computers do have internal records that have human-readable names and IP address pairs, but those often don't contain web apps like Google. So what will happen when a user wants to use a computer to browse to google.com is that it will first try to look in its internal DNS and when it fails to find it there will try to look for a DNS server who has an entry for google.com</p>

<p>To do this, we have to go to the flag icon at the top and click it and then click promote to domain controller</p>
<img src="https://i.imgur.com/djfSlFc.png" height="25%" width="25%"/>


<P>And when a computer wants to connect to google.com the user will type google.com in the web browser, and the computer will look in its local records to see what the ip address of google.com is, and when it doesn't find it it will look in its assigned DNS server if there is an entry for google.com, where it will find that google.com has the 172.217.162.142 IP. Lastly, with that knowledge, it will redirect the user to </P>

<p>As I explained, the connection between clients and the domain controller is a core component of an Active Directory system. But a client doesn't know from the beginning what computer we are using as our dc and so </p>


<p>For this demonstration i will be using <code>domain.com</code> as the sample domain.</p>
<br>

<p>Next, I made sure the ip of my domain controller was static. This is important due to the fact that if this change is not made the ip of our domain controller will change every time we stop it, and thus it will make our clients unable to connect to it.</p>

<h2>Join computers to the domain</h2>
<p>add that this can be done with already existing vms or we can create new ones</p>

<p>To allow users to connect to our domain, I created a second vm with the Windows 10 Enterprise os.</p>
<img src="https://i.imgur.com//2l9mYT0.png" height="25%" width="25%"/>

<br>
<p>Afterwards i configured our client vm so that it had the same vnet as our domain controller.</p>
<img src="https://imgur.com/0NRjpAn.png" height="25%" width="25%"/>

<br>
<p>Once the client vm finished creating, i went inside its dns settings and ensured its dns server was pointing to our domain controller ip.</p>
<img src="https://imgur.com/nGhtotO.png" height="25%" width="25%"/>

<br>
<p>With both vms functioning, i remote desktop to my client vm, went into its settings and with my admin account i joined it into the domain.</p>
<img src="https://imgur.com/VzKDrue.png" height="25%" width="25%"/>

<br>
And finally our client was joined into our domain controller.
