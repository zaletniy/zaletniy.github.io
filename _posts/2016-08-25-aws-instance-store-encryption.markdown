---
layout: post
title:  "Encryption of rootfs in AWS EC2 Instance Store-Backed images"
date:   2016-08-25 19:20:00 +0700
categories: atricles aws ec2
---

## Problem
If you are using AWS and care about encryption your data on disk, you 
probably use [EBS encryption][ebs-encryption] feature provided by AWS.

But unfortunatelly it is not available for [Instance Store-Backed 
instances][instance-store-backed]. Using such type of instances could be
faster and cheaper (I mean you don't pay extra for your EBS and
corresponding snapshot).

## Solution
Encryption of rootfs on first boot at initramfs stage.
  
### Components

1. [dracut-encryptrootfs][dracut-encryptrootfs-github] module for
manipulate with disk before it is mounted
1. [AWS Key Management Service][aws-kms] for management key and not to 
store it on machine
1. [simple-cloud-encrypt][simple-cloud-encrypt] key management 
implementation what relies on AWS KMS

### All together

With [dracut-encryptrootfs][dracut-encryptrootfs-github] module we are 
creating `/boot` and LUKS partition to use it as rootfs.

The detailed description how it works could be found here
[README.md][dracut-encryptrootfs-readme].

Let us take a look what happens during *first boot*
![First boot diagram][first_boot_diagram]

During the *subsequent boots* we are just unlocking LUKS volume and mounting 
it.
![Second boot diagram][second_boot_diagram]

As you can see an actual LUKS key is stored *encrypted* at `/boot`
partition.

AWS credentials are provided with [IAM Roles][iam_roles] mechanism.

<!-- first boot sequence diagram
created with http://knsv.github.io/mermaid/live_editor/

sequenceDiagram
    dracut->>encryptrootfs:start on pre-mount hook
    encryptrootfs->>simple_cloud_encrypt:gererate key
    simple_cloud_encrypt->>encryptrootfs: random key
    encryptrootfs->>encryptrootfs:create LUKS volume
    encryptrootfs->>encryptrootfs:create /boot partition
    encryptrootfs->>simple_cloud_encrypt:encrypt key
    simple_cloud_encrypt->>AWS KMS:encrypt with KMS
    AWS KMS->>simple_cloud_encrypt: encrypted key
    simple_cloud_encrypt->>encryptrootfs:encrypted key
    encryptrootfs->>encryptrootfs: storing encrypted key at /boot partition
    encryptrootfs->>encryptrootfs: mount LUKS volume
    encryptrootfs->>dracut:return control
    dracut->>dracut:switch_root
-->

<!-- second boot sequence diagram
created with http://knsv.github.io/mermaid/live_editor/

sequenceDiagram
    dracut->>encryptrootfs:start on pre-mount hook
    encryptrootfs->>encryptrootfs:check if /boot partiton exists
    encryptrootfs->>simple_cloud_encrypt: decrypt LUKS key
    simple_cloud_encrypt->>AWS KMS:decrypt with KMS
    AWS KMS->>simple_cloud_encrypt: decrypted key
    simple_cloud_encrypt->>encryptrootfs:decrypted key
    encryptrootfs->>encryptrootfs: unlock LUKS volume
    encryptrootfs->>encryptrootfs: mount LUKS volume
    encryptrootfs->>dracut:return control
    dracut->>dracut:switch_root
-->


[ebs-encryption]: http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html
[instance-store-backed]:http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html#RootDeviceStorageConcepts
[dracut-encryptrootfs-github]:https://github.com/zaletniy/dracut-encryptrootfs
[dracut-encryptrootfs-readme]:https://github.com/zaletniy/dracut-encryptrootfs/blob/master/README.md
[aws-kms]:https://aws.amazon.com/kms
[simple-cloud-encrypt]:https://github.com/cviecco/simple-cloud-encrypt
[first_boot_diagram]: /assets/dracut_encryptrootfs_first_boot.svg "First boot diagram"
[second_boot_diagram]: /assets/dracut_encryptrootfs_second_boot.svg "Second boot diagram"
[iam_roles]:http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/