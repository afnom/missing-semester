---
layout: lecture
title: "#13: Cloud Computing"
date: 2026-03-02
ready: false
hide: true
---

# Introduction
Welcome to our session today which is about Cloud Computing.  My goals for today are to

1. Explain what's possible with cloud computing
2. Give a little demo

## Before Cloud Computing
* As far back as the 60s, "time sharing" / client-server models show how computing advances are often costly / unevenly distributed
* Running servers/services "on prem"
* People / companies have to manage the infrastructure
* Only people/groups who could afford to buy "big iron" could perform/offer compute-heavy services

## What exactly is Cloud Computing?
According to International Organization for Standards (ISO) [there are 5 key factors](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-145.pdf):
1. **On-demand self service:** unilateral and without human interaction.
2. **Broad network access:** Available everywhere, not just corporate / home LANs.
3. **Resource pooling:** Provider's resources are pooled and shared.
4. **Rapid elasticity:** rapid provisioning and releasing; can appear unlimited.
5. **Measured service:** access to resources is monitored, logged and billed.

## Use Cases
Cloud Computing is often pitched at big companies but the key point of this talk is that there's no reason you can't use them for your own personal projects or assignments (though you should never NEED to use a paid service for your coursework!).  It's nice to know it's possible to access this power / hardware on a temporary basis.

* Hosting services eg:
	* Websites, databases
* Number crunching eg:
	* Rendering
	* Compiling
	* ML Training
* The whole [self hosting](https://github.com/awesome-selfhosted/awesome-selfhosted) universe
	* Make your own VPN
	* Git servers
	* Mastodon instance
* [Cloudy gaming](https://www.reddit.com/r/cloudygamer/)
* Testing software
	* ARM and Mac OS instances

---

# Amazon, AWS, and EC2
So how does this all work?

We're going to be exploring these concepts by via Amazon's Elastic Compute Cloud (EC2) but first a few notes:
1. This is *not* an advertisement for Amazon, I promise!  Many other cloud providers (Google, [Hetzner](https://www.hetzner.com/), [Linode](https://www.linode.com/), [Azure](https://portal.azure.com/)) exist.  Amazon's is just the oldest and the one I'm most comfortable with.
2. The interface for AWS is a bit hairy; there might be simpler solutions out there!
3. We're mostly going to be talking about rentable virtual computers with EC2.  However, EC2 is only a part of 'cloud computing' / Amazon Web Services (AWS); AWS has over 200 other services including EBS, S3, Lambda (serverless), Cloudfront, etc etc.
4. We're going to be working with 64-bit x86 linux instances but you can also create instances for other OSes / architectures including Windows.

Anyway, EC2 has a [few different types](https://aws.amazon.com/ec2/instance-types/) of computers to rent:
* **General Purpose:** A1, T3, T2, M5, M5a, M4, T3a
* **Compute Optimized:** C5, C5n, C4
* **Memory Optimized:** R5, R5a, R4, X1e, X1, High Memory, z1d
* **Accelerated Computing:** P3, P2, G3, F1
* **Storage Optimized:** H1, I3, D2

## Advantages
* Available everywhere, always on
* It's someone else's problem to make sure the infrastructure is working
* Flexible / Scalable: CPU / RAM / GPUs on demand etc etc
* Maintenance, power included
* Costs: renting a super computer for a short burst or renting a small instance for months/years
* [Scriptibility](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

## Disadvantages
* There is no cloud...
	![There is no cloud; it's just someone else's computer](/2024/files/no_cloud.jpeg)
* "Cost"
	* Did you shut it off?!
	* [Is cloud computing actually cheaper?](https://www.infoworld.com/article/2335678/why-exit-the-cloud-37signals-explains.html)
* Locality and Latency:
	![Map of AWS zones](/2024/files/aws_zones.png)
* More concerns around security
	* Must trust provider
    * Public IPs?  Firewall?
    * Sharing hardware with other people!
		* most 'instances' are a part of a larger computer!
* Lots more details [here](https://en.wikipedia.org/wiki/Cloud_computing_issues)

## Pricing on EC2
* On demand pricing
	* Remember: ~720 hours in a month
	* But also "bursts"
	* page 1 of 27:
	![EC2 Pricing; page 1 of 27](/2024/files/ec2_pricing.png)
* Savings plan -- probably not super relevant to us
* Spot Instances
* Free Tiers:
	![Free Tier w/ caveats](/2024/files/free_tier.png)

Remember: if something is persisting, you're probably paying for it.

---

# Case Study: Amazon EC2
Lets rent an amazon machine and crunch some numbers!

## Elements of creating an instance:

## Micro Instance example
* You can follow [this](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) to get a low-spec machine running on a free tier
* You'll need:
	* An Image (or AMI)
	* A desired instance type
	* A key pair
	* A network
	* A security group
	* An EBS volume
* If all goes well you'll be at a remote terminal now!
* Payoff for Missing Semester skills!

## Rendering example

![Blender splash screen White Lands](/2024/files/white_lands_small.jpg)

Lets try to render the Blender 3.2 splash screen image, White Lands, by Oksana Dobrovolska.

* A note about Quotas; might need to request access in advance!
	* Quotas are good actually

* Settings:
	* AMI: Ubuntu
	* Instance type: c5.4xlarge
	* Security group: make sure it has ssh / xrdp ports
	* 20GB+ drive
* Connect with something like: `ssh -i keypair.pem ubuntu@18.168.154.102`
* Then:
	```
	sudo apt update
	sudo apt install -y ubuntu-desktop xrdp
	sudo adduser xrdp ssl-cert
	sudo adduser jesse
	```
* reboot then
	```
	su - jesse
	wget https://ftp.nluug.nl/pub/graphics/blender/release/Blender4.3/blender-4.3.2-linux-x64.tar.xz
	wget https://download.blender.org/demo-files/splash-screens/blender-3-2/blender-splash-3-2-v2-640d7b753da44f1a8c9629209b46305a.blend
	```
* connect with rdp client!

---

# Final Thoughts
* PLEASE REMEMBER TO TURN OFF YOUR INSTANCES AND ANY EBS VOLUMES YOU'RE NOT USING!
	* Terminated instances will stick around in interface for a bit...
* Otherwise, have fun!

![Astral Projection](/2024/files/astral_ssh.jpg)
