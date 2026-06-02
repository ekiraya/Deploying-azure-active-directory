<p align="center">
<img src="https://i.imgur.com/fMUwhTM.png" height="45%" width="45%"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>

<h2>Summary</h2>
<p>This tutorial/guide outlines how to deploy Active Directory in Azure. This tutorial also explains why every step is necessary and what each step actually does,</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computers)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Enterprise

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Windows VM that will serve as our domain controller
- Configure our domain controllers' IP
- Promote our domain controller VM into an actual domain controller
- Create a Windows VM that will serve as our client.
- Join the client VM to the domain

<h2>Introduction to active directory</h2>
<p>I think its best to beging this tutorial with a little explanation of what active directory is and how it works, this will make it much easier to understand the steps coming</p>
<p>Active directory(AD) is in a nusthsell a database that contains objects like users, passwords, policies and other assets that a company may need. Domain controller (DC) is a server/computer that serves two propuses first it stored the AD and secondlly everytime a client needs to access any resources within the AD it authentificates it. Lastlly clients are computers that are joined to the domain that can acces its users and resources</p>
<p>Even though this tutorial doesnt outline settings such as gpos or user configurations i think it is a good idea to showcase how this system would handle such requests, yeah</p>


<h2>Create a resource group</h2>
<p>To create anything in azure we need to first create a resource group where that resource will be created</p>
<p>To do that we have to start by loging in to azure portal and then going to the resource group icon. We simply hover the icon with the coursor for a couple of seconds until the option "+ Create" appeares then, presse it to create a new resouce group.</p>
<img src="https://i.imgur.com/DWG1v40.png" height="25%" width="25%"/>
<p>We named the resource group "ADDC_Lab" and then just select the "Review + Create" option</p>
<img src="https://i.imgur.com/gcMQds7.png" height="25%" width="25%"/>

<h2>Create a vnet</h2>
<p>For the clients, that is third party computers, to be able to access our domain controller and join the domain we have to first ensure the client can comunicate with the dc. To do that we need to create a vnet or virtual network. Vnets are groups of trusted ips, that means that if we put our domain controller on a vnet any other virtual machine that joins the vnet will be able to connect securely to it</p>
<p>To create a vnet we simply need to hover the virtual network icon with our coursor and when the option "+ Create" appears simply click it to create a new vnet</p>
<img src="https://i.imgur.com/lfAHn4b.png" height="25%" width="25%"/>
<p>Then we simply have to set the resource group to our already created "ADDC_Lab" resource group</p>
<p>And name our vnet, in my case ill name it "ADDC_Lab_vnet"</p>
<img src="https://i.imgur.com/2AtFv1x.png" height="25%" width="25%"/>
<p>Finally we just create our vnet by selecting the "Review + Create" option</p>

<h2>Create a virtual machine to serve as our domain controller</h2>
<p>We then need to create a vm that we will be using as our domain controller.</p>
<p>To create one we simply need to hover the "virtual machines" icon with our coursor and when the option "+ Create" appears simply click it and select "Virtual Machines" to create a new virtual machine</p>
<img src="https://i.imgur.com/EPDovce.png" height="25%" width="25%"/>
<p>In the "Create a virtual machine" page that will apear afterwards we need to do the following:</p>
<p>Set the resource group of the vm to "ADDC_Lab"</p>
<p>Give the machine the "dc" name</p>
<img src="https://i.imgur.com/627hCtQ.png" height="25%" width="25%"/>
<p>Use "Windows Server 2022 Datacenter - x64 Gen2" as our image, we use that as our operating system because it is an operating system build specifically to be use in servers</p>
<p>Set the username and password to anyone of your choice and click next until you get to the networking tab</p>
<img src="https://i.imgur.com/0UGeUAI.png" height="25%" width="25%"/>
<p>There just set our vnets virtual network to the "ADDC_Lab_vnet" we created earlier and select "Review + Create"</p>
<img src="https://i.imgur.com/rDCaYRs.png" height="25%" width="25%"/>

<h2>Turn our virtual machine into an actuall domain controller</h2>
<p>Okay, so what is a domain controller to begin with? a domain controller can be thought of as a computer to store and manage other computers atributes, for instance users all the users of our domain are going to be stored on our domain controller and from our domain controller we will define policy on how those users act</p>

<p>To make sure my virtual machine can be used as a domain controller, i had to download active directory domain services from the wizard.</p>
<img src="https://i.imgur.com//LpVjdrp.png" height="25%" width="25%"/>

<br>
<p>After that finished installing, i promoted my vm to a domain controller.</p>
<img src="https://i.imgur.com//Z6xjiRz.png" height="25%" width="25%"/>
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
