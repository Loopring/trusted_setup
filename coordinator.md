# Instructions for the Coordinator

- Install Rust and Cargo following the instructions [here](https://www.rust-lang.org/tools/install)
- Make sure you have [python 3](https://www.python.org/downloads/) installed

Clone the following repo's in the same folder
- https://github.com/Loopring/protocols (master branch)
- https://github.com/LoopringSecondary/phase2-bn254 (loopring36 branch)

Go into `protocols/packages/loopring_v3` and follow the build instructions to build the circuits.

Next, go into `phase2-bn254` and build the rust programs:

```bash
cd powersoftau && cargo build --release && \
cd ../phase2 && cargo build --release && \
cd ../loopring
```

Stay in the folder `phase2-bn254/loopring` from now on to run all commands.

Download the response of the latest participant from [phase 1](https://github.com/weijiekoh/perpetualpowersoftau/) into `phase2-bn254/loopring`. Rename the file to `response`.

A final contribution is needed with a [beacon](https://github.com/ZcashFoundation/powersoftau-attestations/tree/master/0088). The beacon we use is the hash of Bitcoin block #602168: `00000000000000000013a0dab9d26be0353108f6eb5a2be6ac389986296607c7`, which can be found in `phase2-bn254/powersoftau/src/bin/beacon_constrained.rs`:

```
// Update the number of iterations: We use 2^37 iterations

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
// Update the number of iterations: We use 2^37 iterations

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
