# Testing the trusted setup code

> You need to follow the [instructions](./coordinator.md) for the coordinator.

To test the code in a reasonable time we'll have to do some small changes to the general instructions provided [here](https://github.com/Loopring/trusted_setup#instructions-for-the-coordinator).

Before compiling the rust code, change the `REQUIRED_POWER` in `phase2-bn254/powersoftau/src/small_bn256/mod.rs` from 28 to 18:
```
const REQUIRED_POWER: usize = 18;
```
This will limit us to circuits of 2**18 constraints.

Don't forget to `cargo build --release` in `powersoftau` to compile the code again if you already compiled it before.

Instead of downloading a response file, create one like this:
```bash
../powersoftau/target/release/new_constrained &&
../powersoftau/target/release/compute_constrained
```

Now continue on starting from the random beacon steps as usual.

Make sure to only run the setup code for circuits < 2**18 constraints (so in general only do circuits with 1 request/block)!!! If not sufficient, increase `REQUIRED_POWER` but be prepared to wait even longer on some of the steps. But be warned, even at 18 some of the steps are pretty slow (10-30 minutes for some of the commands).
