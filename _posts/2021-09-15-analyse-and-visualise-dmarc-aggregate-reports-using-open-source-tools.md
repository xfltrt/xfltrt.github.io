---
layout: post
published: true
category: security
title: Analyse and Visualise DMARC Aggregate Reports Using Open Source Tools
---

So you're tasked to analyse your organisation's DMARC report. It's an urgent task and there's no time for budget request. You scour the internet for open source tools and came across a few on GitHub. Ultimately, you've decided to give [this particular GitHub repo](https://github.com/debricked/dmarc-visualizer) a try. You followed the what-could-barely-be-called-a-documentation [here](https://debricked.com/blog/analyze-and-visualize-dmarc-report/). The dashboard came out blank. You searched online on how to resolve the issue with minimal result. What do you do now?

I came across this problem when running `docker-compose up` (Step 2) on my clone (`git clone https://github.com/debricked/dmarc-visualizer.git`, Step 1). I have already installed all the neccesary dependencies, including the not so optional GeoIP.

Upon checking the outputs from the docker containers (which I coincidentally know little about), I notice that the exit code of one of the container (parsedmarc_1) turned "0" instead of its usual "1". I assume this is because the container has successfully run. Except it didn't. I later found out that *parsedmarc* requires *elastisearch* to be up before it could successfull execute.

Logically, I wanted the *parsedmarc* container to do what is supposed to do. I waited for all services to boot up properly and injected a command onto the container. After a quick search, I learned that I could do that with the following command:
``` bash
docker exec -it [container name] [command]
```

But what command to run specifically? To do this, I looked into the "docker-compose.yml" file and found the command to be run by the *parsedmarc container*. Running the following command, I could finally get the tools to work properly.

``` bash
sudo docker restart parsedmarc_1
sudo docker exec -it parsedmarc_1 parsedmarc -c /parsedmarc.ini /input/*
```

Hope this is helpful for someone.
