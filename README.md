<p align="center">
<img src="https://i.imgur.com/fMUwhTM.png" height="45%" width="45%"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computers)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Enterprise

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a windows vm that will serve as our domain controller and a vnet.
- Create a windows vm that will serve as our client.
- Join the client vm to the domain

<h2>Process</h2>
I started by creating a vm in azure with the Windows Server 2022 operating system. Then, within the same window i created a vnet, that will allow us to join our client machines to the domain.
<br>
<img src="https://i.imgur.com//PQ1nikh.png" height="25%" width="25%"/>

Next, I made sure the ip of my domain controller was static. This is important due to the fact that if this change is not made the ip of our domain controller will change every time we stop it, and thus it will make our clients unable to connect to it.

<br>
To make sure my virtual machine can be used as a domain controller, i had to download active directory domain services from the wizard.
<img src="https://i.imgur.com//LpVjdrp.png" height="25%" width="25%"/>

<br>
After that finished installing, i promoted my vm to a domain controller.
<img src="https://i.imgur.com//Z6xjiRz.png" height="25%" width="25%"/>

<br>
To allow users to connect to our domain, I created a second vm with the Windows 10 Enterprise os.
<img src="https://i.imgur.com//2l9mYT0.png" height="25%" width="25%"/>

<br>
Afterwards i configured our client vm so that it had the same vnet as our domain controller.
<img src="https://imgur.com/0NRjpAn.png" height="25%" width="25%"/>

<br>
Once the client vm finished creating, i went inside its dns settings and i ensure its dns server was pointing to our domain controller ip.
<img src="https://imgur.com/nGhtotO.png" height="25%" width="25%"/>

To join my client vm to our domain i remote desktop to it, went into its settings and with my admin account 

And finally our client was joined into our domain controller
