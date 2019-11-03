# Loopring ZKP Trusted Setup Multi-party Computation Ceremony

We use a very similar method of contributing as in [phase 1](https://github.com/weijiekoh/perpetualpowersoftau/) of the ceremony. As long as one party in the ceremony behaves honestly and is not comprimised, the entire setup is trustworthy.

## Ceremony progress

There is a coordinator and multiple participants. The ceremony occurs in sequential rounds. Each participant performs one or more rounds at a time. The coordinator decides the order in which the participants act. There can be an indefinite number of rounds.

The ceremony starts with the coordinator picking a response file from the phase 1 ceremony. The coordinator uses this to generate the initial parameters for all circuits in the phase 2 ceremony and publishing it in a publicly accessible repository.

The participants download these parameters, run a computation to produce new parameters, and send it to the coordinator.

## Instructions for participants

- Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
- Make sure you have [python 3](https://www.python.org/downloads/) installed
- Make a [keybase](https://keybase.io/) account and [link it to your twitter/github/username](https://github.com/pstadler/keybase-gpg-github) so people can be sure it's actually you that participated. Install the [desktop software](https://keybase.io/download) which wil be used to sign the attestation.

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

## Instructions for the coordinator

- Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
- Make sure you have [python 3](https://www.python.org/downloads/) installed

Clone the following repo's in the same folder
- https://github.com/Loopring/protocol3-circuits (branch mpc)
- https://github.com/Loopring/protocols (branch mpc)
- https://github.com/LoopringSecondary/phase2-bn254

Go into `protocols/packages/loopring_v3` and follow the build instructions to build the circuits.

Next, go into `phase2-bn254` and build the rust programs:

```bash
cd powersoftau && cargo build --release && \
cd ../phase2 && cargo build --release && \
cd ../loopring
```

Stay in the folder `phase2-bn254/loopring` from now on to run all commands.

Download the response of the latest participant from [phase 1](https://github.com/weijiekoh/perpetualpowersoftau/) into `phase2-bn254/loopring`. Rename the file to `response`.

A final contribution is needed with a [beacon](https://github.com/ZcashFoundation/powersoftau-attestations/tree/master/0088). To do this a bitcoin hash is used at a certain block height. Publicly share the block height of the block you will use a couple of hours before the block is mined. Once the block is mined put the hash of the block in `phase2-bn254/powersoftau/src/bin/beacon_constrained.rs`:

```
// Update the number of iterations: Sapling MPC did 2^42 iterations

// Place block hash here (block number #564321)
let mut cur_hash: [u8; 32] = hex!("0000000000000000000a558a61ddc8ee4e488d647a747fe4dcc362fe2026c620");
```

Recompile with `cargo build --release` in `phase2-bn254/powersoftau` and now run this:

```bash
../powersoftau/target/release/verify_transform_constrained -skipverification && \
mv new_challenge challenge && mv response old_response && \
../powersoftau/target/release/beacon_constrained && \
../powersoftau/target/release/prepare_phase2
```

We're now ready to setup phase 2. Open `config.py` and modify `circuits` with all the circuits that are needed. Once you've done this, create a new branch for the `phase2-bn254` repo and commit your changes to that branch. Make sure all participants checkout that branch when participating.

We can now create the initial parameters:

```
python3 setup.py
```

Depending on the number of circuits this can take a very long time. Once finished the file `loopring_mpc_0000.zip` will have been created with the initial parameters for all circuits. Upload it to the server so the first participant can download it.

At this point an indefinite number of participants can contribute. For every participant the coordinator needs to do a couple of things:
- Check if the `loopring_mpc_NNNN.zip` file a participant has sent is correct. This can be done by executing `python3 verify_contribution.py X` where `X` is the index of the contribution to check. This check also needs contribution file `X-1`.
- Check if the signed attestation is indeed signed by the paricipant (can easily be checked on the website of keybase).
- Update the website/github with the attestation file and the sha256 hash of the contribution file. This allows the new participant to check if he is indeed building upon the correct contribution.

At any point we can use the parameters to create the verification and proving keys.

If you decide to use a certain contribution, you first have to do another contribution on top of it with [a beacon](https://lists.zfnd.org/pipermail/zapps-wg/2018/000380.html) (just like before with the phase 1 result). To do this a bitcoin hash is used at a certain block height. Publicly share the block height of the block you will use a couple of hours before the block is mined. Once the block is mined put the hash of the block in `phase2-bn254/phase2/src/bin/beacon.rs`:

```
// Update the number of iterations: Sapling MPC did 2^42 iterations

// Place block hash here (block number #564321)
let mut cur_hash: [u8; 32] = hex!("0000000000000000000a558a61ddc8ee4e488d647a747fe4dcc362fe2026c620");
```

Recompile with `cargo build --release` in `phase2-bn254/phase2` and now contribute like this:

```
python3 contribute.py beacon
```

This will generate the contribution that can finally be used to create the verification and proving keys!

```
python3 export_keys.py
```

This will use the latest `loopring_mpc_NNNN.zip` file it can find and write the keys to `protocols/packages/loopring_v3/keys`.

## Examples of entropy sources

1. `/dev/urandom` from one or more devices
3. The most recent Bitcoin block hash
2. Randomly mashing keys on the keyboard
5. Asking random people on the street for numbers
6. Geiger readings of radioactive material. e.g. a radioactive object, which can be anything from a [banana](https://en.wikipedia.org/wiki/Banana_equivalent_dose) to a [Chernobyl fragment](https://www.vice.com/en_us/article/gy8yn7/power-tau-zcash-radioactive-toxic-waste).
7. Environmental data (e.g. the weather, seismic activity, or readings from the sun)
