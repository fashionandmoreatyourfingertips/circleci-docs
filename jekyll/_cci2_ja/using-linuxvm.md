---
layout: classic-docs
title: "Linux VM 実行環境の使用"
description: "Learn how to configure a your jobs to run in the Linux VM execution environment using the machine executor."
version:
  - クラウド
  - Server v3.x
  - Server v2.x
---

You can run your jobs in the linux VM (virtual machine) execution environment by using the machine executor and specifying a Linux image. Using the machine executor runs your jobs in a dedicated, ephemeral VM.

Using the machine executor gives your application full access to OS resources and provides you with full control over the job environment. この制御は、ネットワークインターフェイスのリッスンなどの目的でネットワークスタックへのフルアクセスが必要な場合や、`sysctl` コマンドを使用してシステムを変更する必要がある場合に便利です。

To use the machine executor, use the [`machine` key]({{ site.baseurl }}/configuration-reference/#machine) in your job configuration and specify an image:

{:.tab.machineblock.Cloud}
```yaml
version: 2.1
jobs:
  my-job:
    machine:
      image: ubuntu-2004:current
    resource_class: large
```

{:.tab.machineblock.Server}
```yaml
version: 2.1
jobs:
  my-job:
    machine: true
    resource_class: large
```

You can view the list of available images [in the docs Configuration Reference]({{ site.baseurl }}/configuration-reference/#available-linux-machine-images), or on the [Developer Hub](https://circleci.com/developer/images?imageType=machine). If you are working on an installation of CircleCI server, you will notice in the example above the syntax is slightly different, and the available Linux images are managed by your system administrator.

## Available LinuxVM resource classes
{: #available-linuxvm-resource-classes }

{% include snippets/ja/machine-resource-table.md %}

## プリインストールされているソフトウェア
{: #pre-installed-software }

プリインストールされているソフトウェアの最新のリストは [Image Builder](https://raw.githubusercontent.com/circleci/image-builder/picard-vm-image/provision.sh) のページを参照してください。 詳細は、[Discuss](https://discuss.circleci.com/tag/machine-images) のページでもご確認いただけます。

`sudo apt-get install<package>` を使って追加パッケージをインストールできます。 パッケージが見つからない場合は、インストールの前に `sudo apt-get update` が必要な場合があります。

## Use machine with Docker
{:  #use-machine-with-docker }

Using the machine executor also means that you get full access to the Docker process. これにより、特権 Docker コンテナを実行し、新しい Docker イメージをビルドできます。

以下の例では、`image` キーを使用するとともに [Docker レイヤーキャッシュ]({{ site.baseurl }}/ja/docker-layer-caching) (DLC) を有効化しています。 DLC は、ジョブまたはワークフロー中に Docker イメージをビルドする場合に便利な機能です。

```yaml
machine:
  image: ubuntu-2004:202104-01
  docker_layer_caching: true    # default - false
```

## Using machine and IP ranges
{: #using-machine-and-ip-ranges }

IP アドレスの範囲 `192.168.53.0/24 `は、Machine Executor での社内使用のために CircleCI が予約しています。 この範囲はジョブ内でご使用にならないでください。

## 次のステップ
{: #next-steps }

プロジェクトで使用する Executor を Docker から `machine` に移行する方法については、[Docker Executor から Machine Executor への移行]({{ site.baseurl }}/ja/docker-to-machine)を参照してください。