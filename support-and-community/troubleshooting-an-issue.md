# Troubleshooting an issue

Facing a problem is never fun. This document explains the most common workflow and steps you should take to **identify the issue** you're having more easily and hopefully solve it yourself or with community help.

Identifying the problem is crucial.

## 1. Replicating the issue

First and foremost, try to determine when the issue happens. Try to replicate the problem. Try to update and restart your server to verify you can reproduce your issue. If you think it will describe your issue better, take a screenshot.

### 1.1 Updating the server

Check your version of Bitcart. If it is much older than the [latest version](https://github.com/bitcart/bitcart/releases/latest), [updating your server](../guides/server-management-settings.md#update-the-server) may resolve the issue.

### 1.2 Restarting the server

Restarting your server is an easy way to solve many of the most common Bitcart issues. You may need to SSH into your server to restart it.

### 1.3 Restarting a service

Some issues you may only need to restart a particular service in your deployment. Such as restarting the lets encrypt container to renew the SSL certificate.

```text
sudo su -
cd bitcart-docker
docker restart letsencrypt-nginx-proxy-companion
```

Use `docker ps` to find the name of a different service you would like to restart.

### 2. Looking through the logs

Logs can provide an essential piece of information. In the next few paragraphs, we will describe how to get the **log information for various parts of Bitcart**.

### 2.1 **Bitcart** Logs

You can easily access Bitcart logs from the front-end. If you are a server admin, go to **Server Management &gt; Server logs** \([guide](../guides/server-management-settings.md#server-logs)\) and open the logs file. If you don't know what a particular error in the logs means, make sure to mention it when troubleshooting.

If you would like more detailed logs and you're using a Docker deployment, you can view logs of specific Docker containers using the command line.

Use `docker ps` to get the name of container you need. Usually it is the [docker component ](https://github.com/bitcart/bitcart-docker/blob/master/generator/docker-components)name with [deployment name](../guides/multiple-deployments-on-one-server.md) prepended to it.

Run the commands below to print logs by container name. Replace the container name to view other container logs.

```text
sudo su -
cd bitcart-docker
docker ps
docker logs --tail 100 compose-worker-1
```

## 3. Finding a solution yourself \(Google, FAQ, GitHub issues\)

Even though setups differ, the chances that someone else experienced the same issue as yours are pretty high. Take a few moments, Google around and see if you can solve it yourself.

### 3.1 Bitcart FAQ

We try to document the most common issues on the [Frequently Asked Questions page](faq/). Take a look there and see if your question is recorded.

### 3.2 GitHub

When there's an advanced technical issue, users usually open an issue on GitHub. Take a look at the Bitcart GitHub repository and browse [search the closed issues](https://github.com/bitcart/bitcart/issues?q=is%3Aissue+is%3Aclosed)

### 3.3 Telegram

Our Telegram group is great for similar issues, other users experienced before you. Search messages for your question

## 4. Asking for help

If you're unable to solve the problem yourself, do not worry. There's an amid community ready to help you.

The better you describe the problem, the higher are the chances of getting a timely fix. Be concise and provide as much relevant information as possible. Be sure to include the version you're using and describe your deployment setup. Try to explain what you're trying to do and what's the issue. If you can provide the logs. If you think it's relevant, feel free to include a screenshot.

Here's a good example of how to ask a question.

> I'm having a problem with XYZ. I can replicate the problem. My Bitcart version is A.B.C.D, and I deployed my server on Digital Ocean by following Docker deployment guide. I've searched through the FAQ and closed GitHub issues, but there's no solution to my problem. My Bitcart Setup is XYZ, and the issue is occurring when I do XYZ. Here are the logs I was able to get from my Bitcart instance. You can see the error in the image I attached.

WARNING

The community will not provide extensive support for custom deployments. I.e. Variations of [Manual Deployments](../deployment/manual.md) are expected to be used only for development purposes and by users with technical literacy with the ability to **resolve deployment and maintenance issues on their own**.

### 4.1 Asking the community \(general problems\)

For quick answers to fundamental problems, it's best to post a question in one of our [communities](https://bitcart.ai/#community).

### 4.2 Opening an Issue on GitHub \(advanced problems\)

If you have a custom build setup and are facing a complex problem, [open an issue on GitHub](https://github.com/bitcart/bitcart/issues) so that developers can help you out.

### 4.3 Premium Support

Some community members provide paid support. If you want a quicker help, check out the list of [members providing premium support](support.md#paid-support).
