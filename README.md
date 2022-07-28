<div style="text-align: left"><img src="docs/img/logo.png" height="120px">



# BirdMessenger

[![NuGet](https://img.shields.io/nuget/v/BirdMessenger.svg?color=blue&style=popout-square)](https://www.nuget.org/packages/BirdMessenger)[![NuGet](https://img.shields.io/nuget/dt/BirdMessenger.svg)](https://www.nuget.org/packages/BirdMessenger)
>"Our aim is to solve the problem of unreliable file uploads once and for all. tus is a new open protocol for resumable uploads built on HTTP. It offers simple, cheap and reusable stacks for clients and servers. It supports any language, any platform and any network." - https://tus.io


BirdMessenger 中文名为：青鸟——相传为西王母的信使。
BirdMessnger 是一个基于.NET Standard 的 Tus协议的实现客户端。

## Features

### Protocol implementation

* Create
* HEAD
* PATCH
* OPTIONS
* DELETE

## Install

Package manager

``Install-Package BirdMessenger -Version 2.2.1``

.NET CLI

``dotnet add package BirdMessenger --version 2.2.1``

## Getting Started

```C#
// file to be uploaded
FileInfo fileInfo = new FileInfo("test.txt");

// remote tus service
var hostUri = new Uri(@"http://localhost:5000/files");

// build a standalone tus client instance
var tusClient = TusBuild.DefaultTusClientBuild(hostUri)
                .Configure((options, httpClientBuilder) =>
                {
                    //customize http client
                    httpClientBuilder.ConfigureHttpClient(httpClient =>
                    {
                        httpClient.DefaultRequestHeaders.Authorization =
                            new AuthenticationHeaderValue("Bearer", "ACCESS_TOKEN");

                    });
                   /* httpClientBuilder.ConfigurePrimaryHttpMessageHandler(() => new HttpClientHandler()
                    {
                        UseCookies = false,
                    });*/
                })
                .Build();

//hook up events
tusClient.UploadProgress += printUploadProcess;
tusClient.UploadFinish += uploadFinish;

 //define additional file metadata 
 MetadataCollection metadata = new MetadataCollection();
metadata["filename"] = fileInfo.FullName;
            
TusRequestOption requestOption = new TusRequestOption();
requestOption.HttpHeader["myHttpheader"] = "hello";
            
 //create upload url
 var fileUrl = await tusClient.Create(fileInfo,null,requestOption);

 var uploadOpt = new TusRequestOption()
 {
      UploadWithStreaming = true //enable streaming Upload
 };

 //upload file
 var uploadResult = await tusClient.Upload(fileUrl, fileInfo, null,uploadOpt);
```

* You can see more examples in unit tests

## Document

[Wiki](https://github.com/bluetianx/BirdMessenger/wiki)

## Development

Development is done on the 'master' branch. 

## Who is using the library

* [China National Petroleum Corporation](https://www.cnpc.com.cn/cnpc/index.shtml)
* [BSS-ONE](https://www.bss-one.ro)

## Support and Sponsorship

<a href="https://www.jetbrains.com" target="_blank">
    <img src="./docs/img/jetbrains_logo.png" title="JetBrains" width="100" />
</a>
