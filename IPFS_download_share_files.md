# Downlaod/ Large Files using IPFS

## Install IPFS

If you have IPFS already installed, please run:
```
ipfs config --json Datastore.StorageMax '"200GB"'
```
This is to make sure IPFS supports large files.

Otherwise,  [download IPFS](https://dist.ipfs.io/#go-ipfs) then install it on your OS, then run:
```
ipfs init
ipfs config --json Datastore.StorageMax '"200GB"'
```

### File Downloading
To Download a file using its content identifier (CID), for example `QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc`, run:

```
ipfs get QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc
```

To export the file as `FOO.data`, run:

```
ipfs cat QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc > FOO.data
```

### File Sharing
To allow another person to grab a file you have through IPFS, you need to add it to IPFS first:

```
ipfs add --pin BAR.data
```
The command will output something like the following:

```
added QmbFMke1KXqnYyBBWxB74N4c5SBnJMVAiMNRcGu6x1AwQH BAR.data
```
where `QmbFMke1KXqnYyBBWxB74N4c5SBnJMVAiMNRcGu6x1AwQH` is the CID of `BAR.data`.

Then you can share the IPFS files and their origin file names to the other party:

```json
{
"BAR.data": "QmbFMke1KXqnYyBBWxB74N4c5SBnJMVAiMNRcGu6x1AwQH",
"FOO.data": "QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc"
}
```

Please make sure:
- Your IPFS deamon is up running (`ipfs daemon`) until the other party confirms the file has been retrived.
- Your computer is not sleeping or is shutdown

### Release Disk Space
When a large file is no longer needed and you want to get it "deleted" from IPFS, do the following:

```
ipfs pin rm QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc
ipfs block rm QmY7hamBERCe9aAE2UoJQre9BJDug7r4Dcomjpu9ybkacc
ipfs repo gc
```
