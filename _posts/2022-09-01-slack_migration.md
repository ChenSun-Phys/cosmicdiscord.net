---
layout: post
title: Migrate from Slack to Mattermost
subtitle: 
tags: [meta-physics]
comments: true
<!-- cover-img: /assets/img/fermilab.jpg -->
<!-- thumbnail-img: /assets/img/fermilab.jpg -->
<!-- share-img: /assets/img/fermilab.jpg -->
---

So recently there was a change of policy from Slack. If your team is on a free plan, starting from Sep 1, 2022, you'll only have access to messages in a channel within 90 days. Previously, it allows access of 10k messages regardless of the date they were sent. In a nutshell, it puts more restrictions on small teams on a free plan while potentially benefit big teams that can exhaust the 10k quota in less than three months. While I can understand the profit driven decision, a lot of my collaborators and I in high energy physics use Slack on a daily basis and will be hit by this decision. To make it worse, Slack works as a project discussion history archive as much as an asynchronous communication channel. 

Seeing how hard these days to get/renew a grant in HEP, every buck counts. This post is to show how to migrate Slack messages to a free alternative. Although it's not related to physics, it might help someone improve their workflow of doing physics (in that sense, it's meta-physics.:-)) Plus, I always feel a twitch in my stomach when I think of the idea that someone from a big coorp has the full control of my data and can kidnap it any time they want (tongue-in-cheek: "Slack!"). So I finally took a weekend and bit the bullet in setting up everything and migrated a few workspaces of mine and my friends'. In retrospect I took a few detours. Through documenting (almost) everything I did, I hope it might save you some time for your weekend project. 

### Alternative choice
<!-- I managed to migrate a few workspaces of mine and my friends' to --> My choice here is Mattermost, an open source alternative. The reason of choosing Mattermost is mosly based on 

1. the GUI being similar to Slack with similar user experience, 
1. stability of the code base,
1. good documentation, 
1. ease of the migration process, 
1. some of my colleagues have already used it before. 

I was also eyeing those [Matrix] implementations but it seems to be an overkill for what I need. Mattermost has the business model that's a bit like Fedora/Redhat, Proxmox/Proxmox subscription. Its revenue is from providing paid support and/or hosting the server for you. The whole implementation is fully open source under MIT for binary/ AGPLv3 for source (with commercial licenses available). This means that you can host your own Mattermost server out-of-box with pretty good community support. 

 This post focuses on self-hosting a Mattermost server. That means you'll be the admin and solely responsible for your server. The initial time investment is a about 1-5 hours depending on how familiar you are with the basic linux stuff. The benefit of this over using the cloud service provided the company behind Mattermost is that the latter will also has restrictions (10k messages access, again, as of writing.) In addition, the migration from Slack to Mattermost can be done more easily with self-hosted Mattermost now that you have the full control of your database. (It's no clear to me if the migration can be done for Mattermost cloud service.)

### Set up the Mattermost server
I will mostly focus on the migration as setting up Mattermost is relatively straightforward with their amazing documentation. I will sketch the installation here with a few broad strokes. 

Popular choices of deployment method include [docker](https://mattermost.com/deploy/) for quick setup, [Turnkey Linux](https://www.turnkeylinux.org/mattermost) (but be sure to upgrade from 5.x to 6 then to 7.2) if you can run LXC containers, or manual installation (e.g. [on Ubuntu](https://docs.mattermost.com/install/installing-ubuntu-2004-LTS.html?src=dl)). For other methods, see [here](https://docs.mattermost.com/guides/deployment.html). 

One of the standard places is those dirt cheap VPS (say from Digital Ocean) for a few bucks a month. You can also host it on bare metal (not recommended) or a hypervisor (e.g. Proxmox or VMware ESXi,) which is what I use. Pay attention to the minimum spec requirement though, which is fairly tolerant (e.g. 1-1,000 users - 1 vCPU/cores, 2 GB RAM.) You can run it with a domain name or without one. For the latter, you'll be using the IP address of your server directly. If you do choose to host it on a spare computer of your own, I highly recommend doing so behind a NAT and use Cloudflare's free reverse proxy. It not only increase security, but also takes care of the TLS certificate for https connections. Otherwise, Let's Encrypt is recommended. 

If you also need the email notification when you @ someone, the easiest way is to get the free service from sendgrid.com and add their smtp to the Mattermost configuration under System Console. 

### Migration
There are quite a lot of tools for migrating the database from Slack to Mattermost. You can use the official export from Slack but that will only include public channels. (The private channels can exported if you contact the support but I saw cases where the request was rejected upfront.)
After some research, [Slackdump](https://github.com/rusq/slackdump) suits the purpose the best and is currently under active maintenance. It can export all channels and conversations you have access to, including the attachments. In addition, you don't need to generate a token for the emails or downloading the attachments, unlike many other solutions. After some chats with the maintainer Rupert, he kindly added the export format specifically suited for Mattermost import. See [this issue](https://github.com/rusq/slackdump/issues/107) for details. If you have questions, you can join their Instagram where Rupert usually answers very fast.

First export Slack with the following:

```
./slackdump -export export.zip -export-type mattermost -download 
```

To log into another team, do `./slackdump -auth-reset`. Then use ```mmetl```, the official tool from Mattermost to transform a Slack export into the Mattermost import format:

```
./mmetl transform slack --team yourteamname --file ./export.zip --output mattermost_import.jsonl
```

Note that there's a bug in ```mmetl``` reported [here](https://github.com/mattermost/mattermost-server/issues/19747). You need to do the following before uploading to your Mattermost server.

```
mkdir data
mv bulk-export-attachments/ data/
zip -r import.zip data/ mattermost_import.jsonl
```

Now you can use the tool `mmctl` to import it to the server. First log into your server with `mmctl`

```
./mmctl auth login https://your_url
./mmctl team list
./mmctl team create team-name
```

Then the actual import:
```
./mmctl import upload ./import.zip
./mmctl import list available
./mmctl import process name_from_last_output.zip
./mmctl import job show ID_from_last_output --json
```
Make sure the output shows "success".
Then, that's it. The passwords are automatically reset. You can check the log or reset it again and send it to your collaborators. 

```
./mmctl user change-password username --password yourpassword
```

Feel free to ping me if you need some help. 
