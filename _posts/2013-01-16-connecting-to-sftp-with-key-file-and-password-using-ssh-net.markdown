---
author: jokecamp
comments: true
date: 2013-01-16 20:06:46+00:00
layout: post
slug: connecting-to-sftp-with-key-file-and-password-using-ssh-net
title: Connecting to SFTP with key file and password using SSH.NET
desc: "Learn how to write code to connect to an SFTP server with a key file and password using the C# library SSH.NET"
wordpress_id: 667
categories:
- Code
tags:
- C#
- SFTP
- SSH.NET
---

[SSH.NET](http://sshnet.codeplex.com/) is an open source library codeplex for SSH and SFTP features. I was able to pull the latest code and get a working client in about 15 minutes. The library is great and the code rather straight forward.

By creating my own ConnectionInfo instance with two authentication methods I was able to connect with a password and a key file.

I was getting a lot of "Invalid private key file" exceptions in the PrivateKeyFile constructor. I needed to [convert my private key file](http://sshnet.codeplex.com/discussions/395583) from "PuTTY-User-Key-File-2: ssh-rsa" to an [OpenSSH format](http://www.openssh.com/faq.html#1.1) using puttygen. The key should begin with BEGIN RSA PRIVATE KEY. After switching my key file to the supported format I was good to go.

Below is a simple example of connecting to an SFTP site with username/password credentials along with a (RSA or DSA) key file. This example connects to an specific directory and downloads all the listed files.

```csharp
var localPath = @"C:\Projects\SshProject\CopyTarget";
var keyFile = new PrivateKeyFile(@"C:\Projects\SshProject\OpenSsh-RSA-key.ppk");
var keyFiles = new[] {keyFile};
var username = "username";

var methods = new List<AuthenticationMethod>();
methods.Add(new PasswordAuthenticationMethod(username, "password"));
methods.Add(new PrivateKeyAuthenticationMethod(username, keyFiles));

var con = new ConnectionInfo("example.host.com", 22, username, methods.ToArray());
using (var client = new SftpClient(con))
{
    client.Connect();

    var files =  client.ListDirectory("/ftpfolder/files/");
    foreach (var file in files)
    {
        Console.WriteLine(file);
        using (var fs = new FileStream(localPath + file.Name, FileMode.Create))
        {
            client.DownloadFile(file.FullName, fs);
        }
    }
}
```
