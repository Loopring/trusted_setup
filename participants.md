# Instructions for Participants

## Get Ready Before Your Participation

1. Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
1. Make sure you have [python 3](https://www.python.org/downloads/) installed
1. Make a [Keybase](https://keybase.io/) account and [link it to your twitter/github/username](https://github.com/pstadler/keybase-gpg-github) so people can be sure it's actually you that participated. Install the [desktop software](https://keybase.io/download) which wil be used to sign the attestation.
1. Using your Keybase software, join a team called **loopringceremony**, this is the official communication channel, but we'll also collect another IM account of yours to inform you when your trun is coming up.

## When Your Turn Comes
Download and compile the required source code:

```bash
git clone https://github.com/LoopringSecondary/phase2-bn254 --branch master && \
cd phase2-bn254/phase2 && \
cargo build --release && \
cd ../loopring
```

Stay in this folder from this point on. `phase2-bn254/loopring` needs to be the directory you run all scripts in.

Download the `loopring_mpc_nnnn.zip` file from the server to `phase2-bn254/loopring` (the coordinator will send you the link). For example, if you're participant 4 you will need to download the file `loopring_mpc_0003.zip` (i.e. the contribution of the previous participant).

Run the computation:

```
python3 contribute.py
```

You will see this prompt:

```
Contributing to NN circuits.
Type some random text and press [ENTER] to provide additional entropy...
```

Enter some random text as prompted. You should try to provide as much entropy as possible from sources which are truly hard to replicate. See below for examples derived from Zcash's own ceremony.

After a short while you will see something like this:

```
Starting from contribution 1 with sha256 hash 0xdd8dd76af5af768bda1b407943b2e478d6da6d663657a376a8db0403c6424825 (please check if this is correct)
```

Make sure the hash that is shown is indeed the hash of the contribution you've downloaded and need to build upon (which is the hash of the contribution of the previous participant).

The computation will run for about 24 hours on a fast machine. Please try your best to avoid electronic surveillance or tampering during this time.

When it is done, you will see something like this:

```
Done! Thank you for contributing as participant 4!
Your contribution has sha256 hash 0xe9fe32323ec5192ec8c3491e883b7fec036a7b211491cd5fc3c06558b6bb0ea8
Please upload 'loopring_mpc_0004.zip'.
Also please fill out 'attestation.txt' and sign it by running 'python3 sign_attestation.py' and send us 'signed_attestation.txt'.
```

Upload the generated `loopring_mpc_nnnn.zip` file to the server (the coordinator will send instructions on how to do this). For example, if you're participant 4 you will need to upload the file `loopring_mpc_0004.zip`.

Document the process you used and add it to `attestation.txt` (DO NOT CREATE THIS FILE, this file will have been auto-generated and will already contain important data!), following the template here: https://github.com/weijiekoh/perpetualpowersoftau/tree/master/0001_weijie_response.

Sign it with your keybase GPG key by running

```
python3 sign_attestation.py
```

This will generate the file `signed_attestation.txt`. Send this file to the coordinator.

## Examples of Entropy Sources

1. `/dev/urandom` from one or more devices
3. The most recent Bitcoin block hash
2. Randomly mashing keys on the keyboard
5. Asking random people on the street for numbers
6. Geiger readings of radioactive material. e.g. a radioactive object, which can be anything from a [banana](https://en.wikipedia.org/wiki/Banana_equivalent_dose) to a [Chernobyl fragment](https://www.vice.com/en_us/article/gy8yn7/power-tau-zcash-radioactive-toxic-waste).
7. Environmental data (e.g. the weather, seismic activity, or readings from the sun)
