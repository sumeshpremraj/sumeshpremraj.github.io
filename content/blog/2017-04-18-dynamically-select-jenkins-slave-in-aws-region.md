---
layout: post
title: "Dynamically pick Jenkins slave in the same AWS region"
date: 2017-04-18 12:40:30 +0530
tags: ["ops"]
---

As part of efforts to shore up security, I worked on moving from a single Jenkins master to a distributed setup with a slave in each AWS region. This might help if you use a cloud provider from Amazon, Google GCP or similar, and want to do the same.

Jenkins has a great plugin ecosystem, and we can use the [Groovy Label Assignment plugin](https://wiki.jenkins-ci.org/display/JENKINS/Groovy+Label+Assignment+plugin) to make Jenkins pick the local slave automatically. The only problem is that Jenkins is used by each individual/organization differently, so finding the right plugin takes some time. For my particular use case, it took longer to find a plugin than actually set it up :)

I have a Jenkins build parameter that picks the region for deployment, so I wrote some Groovy code to check the value of this parameter and pick the right slave.

* Install the plugin linked above
* Set up your slaves and add a descriptive label. I used `slave-[region]`
* Create/edit a job, and select the checkbox for `Groovy script to restrict where this project can be run`.
Check the value of your build parameter and set the Jenkins slave. For example:
```groovy
switch (REGION) {
  case 'us-east-2': 
    return 'slave-us-east-2';
  case 'eu-west-2': 
    return 'slave-eu-west-2';
}
```

You can also use if-else statements (click the help icon next to the code box for code samples).

Implementing this allowed me to move all tasks into private networking of the VPC, improving the security and reducing the management overhead (firewalls, SSL certs etc).