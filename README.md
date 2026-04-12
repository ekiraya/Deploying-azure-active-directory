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
To allow users to connect to our domain, I created a second vm with the Windows 10 Enterprise os
<img src="https://i.imgur.com//2l9mYT0.png" height="25%" width="25%"/>

<br>
Afterwards i configured our client vm so that it had the same vnet as our domain controller
<img src="https://imgur.com/a/EKWFCyK.png" height="25%" width="25%"/>

<br>
Once the client vm finished creating, i went inside its dns settings and i ensure its dns server was pointing to our domain controller ip
<img src="https://imgur.com/a/EKWFCyK.png" height="25%" width="25%"/>

With everything functioning, I designed the whole UI to make it user-friendly and appealing.

Finally, I added testing with Cypress and Testing Library. I conducted end-to-end tests on drawing and manipulating text, lines, rectangles, and freehand drawings to make sure everything worked smoothly.

Along the way, while building everything, I took notes on what I've learned so I don't miss out on it. I also documented the behind-the-scenes processes every time a feature was added.

This way, I understood what I've built. The funny thing is, as soon as I started to document what happened behind the scenes and the features I've added, it made me realize that we fully understand something once we've actually taken a step back, thought about it, and documented what we've done. I think this is a good practice to follow when learning something new.

