# Instructions for Participants

## Get Ready Before Your Participation

1. Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
1. Make sure you have [python 3](https://www.python.org/downloads/) installed
1. Make a [Keybase](https://keybase.io/) account and [link it to your twitter or github account](https://github.com/pstadler/keybase-gpg-github) so people can be sure it's actually you that participated. Install the [desktop software](https://keybase.io/download) which wil be used to sign the attestation.
1. Using your Keybase software, join a team called **loopringceremony**, this is the official communication channel, but we'll also collect another IM account of yours to inform you when your trun is coming up.


## When Your Turn Comes

The coordinator will send you a `sftp.credential` file for accessing our SFTP server. He will also confirm with you your sequence number. In the instructions we assume you are the 4th participant with sequence number 0004, **please make sure you replace 0004 with your own sequence number in all commands.**

### Step#1

Download and compile the required source code:

```console
git clone https://github.com/LoopringSecondary/phase2-bn254 --branch master && \
cd phase2-bn254/phase2 && \
cargo build --release && \
cd ../loopring
```

Save the `sftp.credential` file shared by the coordinator in this directory. **Stay in this folder from this point on.** `phase2-bn254/loopring` needs to be the directory you run all scripts in.

### Step#2

Download the contribution zip file from the previous participant. 

```console
sftp -i sftp.credential loopring@sftp.loopring.org
sftp> reget loopring_mpc_0003.zip
sftp> exit
```
Note that the sequence number in `loopring_mpc_0003.zip` must 1 smaller than yours (0004)。

The file is very large (tens of Gigabytes),  it will take quite some time to download. If the download is interrupted, simply repeat the above commands so it will resume downloading.


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

Document the process you used and add it to `attestation.txt` (DO NOT CREATE THIS FILE, this file will have been auto-generated and will already contain important data!), following the template here: [./attestation_template.md](./attestation_template.md)

Sign it with your keybase GPG key by running

```console
python3 sign_attestation.py
```

This will generate the file `signed_attestation.txt`.

### Step#5
Now run the following commands to share your contribution results:
```console
sftp -i sftp.credential loopring@sftp.loopring.org
sftp> reput signed_attestation.txt signed_attestation_0004.txt
sftp> reput loopring_mpc_0004.zip
sftp> exit
```

The file is very large (tens of Gigabytes),  it will take quite some time to upload. If the download is interrupted, simply repeat the above commands so it will resume uploading.

### Step#6
**Reboot your computer**, then notify the coordinator that your have completed your contribution.

