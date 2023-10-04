---
title: Virtualizing my API 
date: "2022-12-02T08:12:03.284Z"
categories:
    - blog
tags:
  - python
---
Hello there,
I've decided to accomplish most of my codecember goals by learning something new about Azure, a provider that I've never used.

Azure has a great free tier, allowing each user to create resources that won't scale over their credit balance. 
(Hopefully)

Running an application on Cloud means that another machine, not the one you are currently using, executes the algorithm you write. 
To avoid configuration mistakes, a developer can use Docker and virtualize the execution of the code.


So today, I'll create a Dockerfile and will push it to Azure Container Registry.

My Dockerfile:

```
FROM python:3.10.4-slim-buster

RUN pip install --upgrade pip

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY main.py main.py
EXPOSE 9090

ENTRYPOINT ["python"]
CMD ["main.py"]
```

To check if everything is ok, I type:
`docker build . --tag codecember-docker`
`docker images` 
to see if there is the image 
`docker run codecember-docker`

Everything works!
Let's install Azure Cli

`curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

```
az group create --name codecember --location francecentral
az acr create --resource-group codecember --name az acr create --resource-group codecember --name codecembercontainerregistry --sku Basic
az acr build --image codecember/days-to-xmas:v1 --registry codecembercontainerregistry --file Dockerfile .
```

So, now I have built my image and uploaded it to Azure Container Registry.

To run it, I'll just:

```
az acr run --registry codecembercontainerregistry \
  --cmd '$Registry/codecember/days-to-xmas:v1' /dev/null
```

It works!

See you next post

Girolamo


