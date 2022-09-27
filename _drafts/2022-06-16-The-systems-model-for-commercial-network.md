---
layout: post
title: 'The Systems Model for Commercial Networks'
date: '2012-06-16 00:00Z'
categories: post
---
# The Systems Model for Commercial Networks

### The systems model for all commercial access providers – and applicable to commercial hosting/managed services providers, large enterprises, and campus networks

* *Access & Transport Infrastructure* – these are wires in the ground, and the fairly static services that go along with them.  Even if there is programmability, it is generally not changing the “big” parts of this layer
* *Elastic Compute & Connectivity Infrastructure (ECCI)* – this is commodity hardware with free or cheap software operated at very thin margins.  (the EC2+SDN world (NVF fits here too) )
* *Commercialization Platforms *– this is the software (and hardware) that is the logical foundation for things that make money.  LTE is a platform, IMS is too, so are CDN, and public cloud and managed WAN.
* *Service Offering* – this is the product that gets sold.  VoLTE is built on top of the LTE and IMS platforms.  Video On Demand is built on top of the CDN and private cloud platforms.
* *Applications* – this layer contains the things that customers are buying – Voice, provided by the VoLTE service offering, built on the LTE and IMS platforms.  The Applications Layer can be further split into three sub-layers:  
	* The *Extra-network layer*, where OTT services live (Netflix, Skype, Dropbox, Google Maps); they aren’t going to win these battles
	* The *Meta-network layer*, where things that extend the network live (voicemail, OTA upgrade services, the operator’s app store); table-stakes - no one is going to pay for these things
	* The *Kata-network layer,* where applications that are alongside the network live.  They are better than analogous OTT service because of some benefit they receive from the network ( voice or video services that can seamlessly move from fixed to mobile to wifi without dropping are an example.  There are actually few good examples.)


### Now using this model, lets talk about the various “Hype-movements” that make up the dynamic, programmable network.

* First there was Hypervisors and OS virtualization, which brought back the experience of time-shared mainframe computing to PC architectures.  They impact the Access and transport Infrastructure (though not actually very much in practice) and make the ECCI possible.
* Next came EC2 and from that the Cloud.  This is really about taking the ECCI and deploying services and applications.  
* Because the tool chain to manage those services and applications was pretty bad, the DevOps movement was born.
* When you apply the DevOps methodology to the management of network switching and routing, you get SDN.
* Finally, when you want to use SDN and DevOps together to create commercialization platforms on ECCI to clean out your logistics chain, you get NFV.

While each of those five “hype-movements” have different origins, wire protocols, terminology, and governing organizations, they are all doing the same thing – remote configuration changes.  And none of them can singlehandedly execute all five phases of the lifecycle model we looked at earlier.

Much of that has to do with the different priority a configuration change has within each layer of this model.

* At the Access and Transport Infrastructure layer, what is most important is the *defining the service level being used*
* At the ECCI layer, what is most important is the *coordination of the changes between the compute, the connectivity, and the storage*
* At the Commercialization Platform layer, what is most important is the *optimization of the service*
* At the Service Offering layer, what is most important is the *monetization of the offering*
* And at the Applications layer, what is most important is the *security of the application and the associated data*

THAT DOESN’T MEAN THAT THESE THINGS AREN’T IMPORTANT AT OTHER LAYERS TOO, IT JUST MEANS THAT IS THE FOCAL POINT.

The only way to get this full stack dynamic configuration change is to coordinate it all though the use of a unifying policy.

This makes it easy to tell the difference between a marketing term and a technical term.
