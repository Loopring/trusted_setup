# Instructions for Participants

## Get Ready Before Your Participation

1. Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
1. Make sure you have [python 3](https://www.python.org/downloads/) installed
1. Make a [Keybase](https://keybase.io/) account and [link it to your twitter or github account](https://github.com/pstadler/keybase-gpg-github) so people can be sure it's actually you that participated. Install the [desktop software](https://keybase.io/download) which wil be used to sign the attestation.
1. Using your Keybase software, join a team called **loopringceremony**, this is the official communication channel, but we'll also collect another IM account of yours to inform you when your trun is coming up.
1. Install and config IPFS. If you do not have IPFS v0.4.22 already installed, please download it from [here](https://dist.ipfs.io/#go-ipfs), then run `ipfs init`. Now you need to config IPFS to support large files by running:
```console
ipfs config --json Datastore.StorageMax '"200GB"'
ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/9001
ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001
```

To use IPFS in the following steps, make sure you have IPFS daemon up running:

```console
ipfs daemon
```

## When Your Turn Comes

### Step#1

Download and compile the required source code:

```console
git clone https://github.com/LoopringSecondary/phase2-bn254 --branch master && \
cd phase2-bn254/phase2 && \
cargo build --release && \
cd ../loopring
```

**Stay in this folder from this point on.** `phase2-bn254/loopring` needs to be the directory you run all scripts in.

### Step#2

The coordinator will give you an IPFS CID, for example `Qme4u9HfFqYUhH4i34ZFBKi1ZsW7z4MYHtLxScQGndhgKE`, to download the contribution from the previous participant. For example, if you're participant 4 (sequence number `NNNN=0004`） you can download the output from participant 3 (sequence number is `0003`) from inside the `phase2-bn254/loopring` directory by running the following commands:

```console
ipfs get Qme4u9HfFqYUhH4i34ZFBKi1ZsW7z4MYHtLxScQGndhgKE
ipfs cat Qme4u9HfFqYUhH4i34ZFBKi1ZsW7z4MYHtLxScQGndhgKE > loopring_mpc_0003.zip
```
Note that the name of `loopring_mpc_0003.zip` must be accurate and the sequence number must be 1 smaller than your sequence number.

> If IPFS cannot find the file, please try again later. Sometimes it does take time for  files to become available to remote IPFS nodes.

### Setp#3

Run the computation:

```console
python3 contribute.py
```

You will see this prompt:

```
Contributing to NN circuits.
Type some random text and press [ENTER] to provide additional entropy...
```

Enter some random text as prompted. You should try to provide as much entropy as possible from sources which are truly hard to replicate. See below for examples derived from Zcash's own ceremony.

> Examples of Entropy Sources
>
> 1. `/dev/urandom` from one or more devices
> 3. The most recent Bitcoin block hash
> 2. Randomly mashing keys on the keyboard
> 5. Asking random people on the street for numbers
> 6. Geiger readings of radioactive material. e.g. a radioactive object, which can be anything from a [banana](https://en.wikipedia.org/wiki/Banana_equivalent_dose) to a [Chernobyl fragment](https://www.vice.com/en_us/article/gy8yn7/power-tau-zcash-radioactive-toxic-waste).
> 7. Environmental data (e.g. the weather, seismic activity, or readings from the sun)


After a short while you will see something like this:

```
Starting from contribution 3 with sha256 hash 0xdd8dd76af5af768bda1b407943b2e478d6da6d663657a376a8db0403c6424825 (please check if this is correct)
```

Make sure the hash that is shown is indeed the hash of the contribution you've downloaded and need to build upon (which is the hash of the contribution of the previous participant).

The computation will run for about 24 hours on a fast machine. Please try your best to avoid electronic surveillance or tampering during this time.

When the above step is done, you will see something like this:

```
Done! Thank you for contributing as participant 4!
Your contribution has sha256 hash 0xe9fe32323ec5192ec8c3491e883b7fec036a7b211491cd5fc3c06558b6bb0ea8
Please upload 'loopring_mpc_0004.zip'.
Also please fill out 'attestation.txt' and sign it by running 'python3 sign_attestation.py' and send us 'signed_attestation.txt'.
```

### Step#4

Reboot your computer.

### Step#5

Document the process you used and add it to `attestation.txt` (DO NOT CREATE THIS FILE, this file will have been auto-generated and will already contain important data!), following the template here: [./attestation_template.md](./attestation_template.md)

Sign it with your keybase GPG key by running

```console
python3 sign_attestation.py
```

This will generate the file `signed_attestation.txt`.

### Step#6
Now you will share your files with the coordinator using IPFS. 
Now run the following commands to share your contribution results (replacing `NNNN` with your sequence number):
```console
ipfs id >> NNNN_summary.txt
ipfs add --pin loopring_mpc_NNNN.zip >> NNNN_summary.txt
ipfs add --pin signed_attestation.txt >> NNNN_summary.txt
```
You need to share the `NNNN_summary.txt` file with the coordinator using the Keybase's chat, **while keep IPFS daemon up runing and also keep your computer from sleeping.** (It will to take the coordinator about 30 minutes to 1 hour to download all files.)

### Step#7
Wait patiently unitl the coordinator confirmed all files have been retrieved. Then you can run:
```console
ipfs repo gc
```
to release disk space and delete all code and files from disk.

