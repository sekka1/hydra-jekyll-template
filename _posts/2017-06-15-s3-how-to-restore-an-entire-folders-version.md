---
date: 2018-06-16
title: S3 - How to restore an entire folders version
categories:
  - AWS
  - s3
  - troubleshooting
  - versioning
author_staff_member: Garland Kan
---
This tutorial explains how to use a tool that will help you to restore an entire
S3 folder's files.

You must have had versioning enabled on your S3 bucket (https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html)

The problem is that the AWS CLI or console (GUI) does not help you restore an
entire bucket or a folder's version.  You have to restore each object individually.
This is a pain and error prone.

There is a tool called `s3-pit-restore` (https://github.com/madisoft/s3-pit-restore)
that helps you out with automating restoring.  It will read all of the files and
download the version of the file you want to your local computer.  At that point,
you can upload it back to S3 or use it in another way.

You will first have to install that project per their documentation (https://github.com/madisoft/s3-pit-restore#installing)

Then there are various options for restore.  The restore is more of it downloading
that version's file onto your local computer instead of reverting what is in S3.  That
confused me a bit when I first used this tool.  So from now on, I am going to say
download the version instead of restoring the version.

You first have to get the version that you want by going into the console and select
a file/object.  You will see a bunch of dates.  These are the versions.  You will have to convert them to a number format.

![s3 version]({{ "/images/blog/s3-version.png" }})

As an example, you can run this to download all of the buckets object from a particular
time:

```
s3-pit-restore -b my-bucket -d my-restored-bucket -t "06-17-2016 23:59:50 +2"
```

* `-b` is the bucket name.
* `-d` is the local directory where it will download the files to
* `-t` is the time stamp of the version you want

You can also just downlowd a sub folder of a bucket:

```
s3-pit-restore -b my-bucket -d my-restored-subfolder -p mysubfolder -t "06-17-2016 23:59:50 +2"```
```

Same command as above with one additional flag:
* `-d` is the sub folder that you want only
