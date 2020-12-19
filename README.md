# Part 2 - Integration Red Hat Single Sign-On with OpenShift v4.x Container Platform
*Written by Kerem ÇELİKER*
- Linkedin: **`linkedin.com/in/keremceliker`**
- Twitter: **`@CloudRss`**
- Blog: **`www.keremceliker.com`**




  <img src="Pics/Overview.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>



In this article you will learn how Red Hat Sign-On Provides the ability to federate user accounts from multiple different user stores specifically from Ldap Server and External MySql user database. Many Organizations and Customers often have existing user stores that holds the information about the users and their credentials. 

Most often enterprise applications will have multiple different personals and user types using it with each user type coming from different user stores or even the more common scenario is that different applications would have user bases and the underlying user account management systems might be different for each of the applications.  

That's why to handle such scenarios typically each application layer will have to build a suitable authentication module and to handle the authorization against these different user account management systems.  

So most often results in a kind of redundant authentication modules getting built and it gets closely tied with each of these applications so RedHat Single Sign-On user Federation capabilities exactly addresses these concern and provides a unified way to federate different user account systems. 


- I have got Red Hat Openshift deployed inside of VMware vSphere Bare-Metal.  

- Im going to go ahead and create a sample project for SSO.

 <img src="Pics/1.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>
 
 - Let's go ahead and browse the catalog and locate the single sign-on template from Developer/Topology. 

 
 <img src="Pics/2.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>


 

- You will notice there's four different ones we're going to use the one with mysql with persistent storage. 

 <img src="Pics/3.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/> 


- I'm going to go ahead and instantiate the template but before we do that we will go ahead enter a username + password for the RH-SSO Administrator 

 

 <img src="Pics/4.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/> 

 <img src="Pics/5.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

 

- We will switch back to administrator and will monitor the pod progress. We have got four pods and when two of them go into completed you know it's finished. 

 

 <img src="Pics/6.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 <img src="Pics/7.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>
 

 

- Now we can go ahead and look at the routes in the route location you will see exposed a url that I can go ahead by click. 

 

 <img src="Pics/11.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

- Open the SSO Application on that url link from another tab within the browser we can click on the administration console. Now we can login in as the username and password that we supplied to the template. 

 

 <img src="Pics/12.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

 

- We're going to go ahead to add a Realm. We will add a display name for it  and ll go ahead to add a user. For example; you ll call it test user or  whatever u want. 

 

 <img src="Pics/15.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>
 <img src="Pics/16.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

  

- Now we can go ahead the create a client. We are going to set it for using the client protocol of OpenID Connect. One of the things we ll do is change it to confidential from public and we're going to give it a valid redirect url. 

 

 <img src="Pics/19.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 <img src="Pics/20.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

- Now you will see a secret is applied we going to go ahead to use that later.  

 

- Now let's switch back to the openshift platform. Let's go to secrets and list "all-projects" because we re going to try and find the router. To go ahead copy the certificate value and to save it to a text file so we can use it later. 

 

 <img src="Pics/21.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 <img src="Pics/22.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

- Let's go ahead create a IDP user and set the identity provider to open id connect. 

- We will specify the client id as openshift name and then we are gonna use the secret that was supplied in the sso application. 

 

 

 <img src="Pics/23.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/> 

 

- Just paste that in Client Secret from OpenID Connect Provider and give the issuer url to the sso apps. 

 

 <img src="Pics/24.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/> 
 <img src="Pics/25.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

- We re going to set the CA file to that router certificate file that we copied. 

 

 <img src="Pics/26.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

- At this point we can go ahead to got all the logins created we can go log-out from Openshift Container Platform. 

 

- Now we see to have another option to log-in the openshift with as the OpenID Connect  

 

 

 <img src="Pics/27.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 <img src="Pics/28.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/> 

 

- Finally, Voila ! You're logged in using the Single Sign-On to Openshift Container Platform !  

 

 <img src="Pics/10.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

 

**Hope you Learned and Enjoyed !**

 
***Benefical Notes*** 

 You may get some errors when creating Pods for SSO. These will mostly be as follows: 

```
* Insufficient Memory 
* PullBackOff Error 
* Failed Scheduling 
* Read/Liveness Probe Issues
```

You can utterly resolve the above errors primarily by "Pod Debug" and by investigating the relevant circle-ish application links.

You may be getting these errors while pods wait too long in the "Pending" or "Creating Container" state. That's why it's important to run the following command to check and find out if you've received an error. 

**To Run under in SSO Project** if you are not using the **Multi-Tenant Container** structure. 
```
Oc describe pod "pod-name" 
```
**Sample Issue Image:**

 <img src="Pics/7-1.png" alt="Kerem's CloudNative a Sample Code" style="width: 500px;"/>

