<p align="center">
<img src="https://i.imgur.com/fMUwhTM.png" height="60%" width="60%"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10
- Windows 10 Enterprise

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a resource group within azure
- Create a windows vm that will serve as our domain controller and the vnet that will allow us to join our clients to the domain
- Create a window vm that will serve as our client
- Make the client vms join the domain

<h2>Create a resource group within azure</h2>

<p>
this is likely the most simple step and the step you are likely already familiar with, you just go into azure and hit create then name the group and we are ready to go
</p>
<p>
<img src="https://i.imgur.com/LICvpdS.png" height="40%" width="40%"/>
</p>

<h2>On creating a resource group within azure</h2>
So, i think that a lot of documentation often lacks a deeper explanation on why we need to take x step on how x steps helps us get to our final goal, in my documentation i will use the on sections after every step to describe how each of the steps we did helps us further our goal

in the case of creating a resource group thats mostlly just busy work, azure is platforma that offers us options to create multiple resources, but the think is that it doesnt matter which resource we want to create azure will not let us create it unless we create a resource group to place it first

<h2>Create a windows vm that will serve as our domain controller and the vnet that will allow us to join our clients to the domain</h2>

- Create a vm in azure with the windows server 2022 os
- fill up all the info required
- Go to the network page and create a vnet
<img src="https://i.imgur.com//PQ1nikh.png" height="40%" width="40%"/>

<h4>Convert that same vm into a domain controller</h4>
after doing the previous configuration you can just deploy the vm, nontheless it is not a domain controller yet
to turn it into one we need to do some extra steps
first of all we need to download active directory domain services from the wizard
<img src="https://i.imgur.com//LpVjdrp.png" height="40%" width="40%"/>
and after that finishes intalling we need to promote our vm to dc, which we can do from the little flag on the corner
<img src="https://i.imgur.com//Z6xjiRz.png" height="40%" width="40%"/>
after that we will be asked to name our domain, this name is going to be the name of our root domain from which we can create subdomains if we want to
<img src="https://i.imgur.com//ErdIU8O.png" height="40%" width="40%"/>
then we are going to be asked to create a directory service restore mode password, which is basically an emergency password that we are likely to never use, so we can use literally any password we want, it really doesnt matter
<img src="https://i.imgur.com//NaRInvV.png" height="40%" width="40%"/>
after that we can just hit next a couple of times and then our computer is going to restart and when it finishes restarting we are ready

<h2>On creating a windows vm that will serve as our domain controller</h2>
so what is a domain controller? a domain controller can be at large described as a gate keeper coupled with a storage, it allows us to create a resource within itself and then defines we want it to be used
if our goal is to create a domain controller the first step we need to do is create a vm that we can configure to serv our goal, for that we need to use the windows server operating system, we must do this bcos thats the only operating system the can configure to serve as a domain controller
we also need to create a vnet, vnets can be thought as a group of trusted ips, so if our vm is in vnet with another vm they are going to trust each other and allow data packaged to pass between them, in our case we need to place our dc on a vnet so that its ip can be trusted by the clients and viceversa

lastlly we must complete the proces to promoting our vm to a domain controller, this process is the most important one bcos it will download into our vm all the required software to make it function as a vnet
first off we need to download active directory domain services





