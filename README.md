<p align="center">
<img src="https://i.imgur.com/fMUwhTM.png" height="45%" width="45%"/>
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

- Create a windows vm that will serve as our domain controller and the vnet that will allow us to join our clients to the domain
- Create a window vm that will serve as our client
- Make the client vms join the domain

<h2>Process</h2>

I started by creating a vm in azure with the Windows Server 2022 operating system. Then, within the same window i created a vnet, that will allow us to join our client machines to the domain.
<br>
<img src="https://i.imgur.com//PQ1nikh.png" height="25%" width="25%"/>

Next, I made sure the ip of my domain contrller was static. This is important due to the fact that if this change is not made the ip of our domain controller will change every time we stop it, and thus making our clients unable to connect to it.

<img src="https://i.imgur.com//LDglemv.png" height="25%" width="25%"/>

To make sure mistakes could be fixed, I implemented undo and redo features. I also added freehand drawing for a more natural sketching experience and a text tool to label or note on the canvas.

To navigate larger drawings, I put in pan and zoom tools. With everything functioning, I designed the whole UI to make it user-friendly and appealing.

Finally, I added testing with Cypress and Testing Library. I conducted end-to-end tests on drawing and manipulating text, lines, rectangles, and freehand drawings to make sure everything worked smoothly.

Along the way, while building everything, I took notes on what I've learned so I don't miss out on it. I also documented the behind-the-scenes processes every time a feature was added.

This way, I understood what I've built. The funny thing is, as soon as I started to document what happened behind the scenes and the features I've added, it made me realize that we fully understand something once we've actually taken a step back, thought about it, and documented what we've done. I think this is a good practice to follow when learning something new.




<h2>Create a windows vm that will serve as our domain controller and the vnet that will allow us to join our clients to the domain</h2>

- Create a vm in azure with the windows server 2022 os
- fill up all the info required
- Go to the network page and create a vnet


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





