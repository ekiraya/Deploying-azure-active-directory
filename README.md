<p align="center">
<img src="https://i.imgur.com/pU5A58S.png"/>
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

<h2>Create a windows vm that will serve as our domain controller and the vnet that will allow us to join our clients to the domain</h2>

- Create a vm in azure with the windows 2022 os
- fill up all the info required
- Go to the network page and create a vnet
<img src="https://i.imgur.com//PQ1nikh.png" height="40%" width="40%"/>

<h4>Convert that same vm into a domain controller</h4>
after doing the previous configuration you can just deploy the vm, nontheless it is not a domain controller yet
the following steps outline the process to go from vm to dc


