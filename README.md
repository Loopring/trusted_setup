# Loopring ZKP Trusted Setup Multi-party Computation Ceremony

We use a very similar method of contributing as in [phase 1](https://github.com/weijiekoh/perpetualpowersoftau/) of the ceremony. As long as one party in the ceremony behaves honestly and is not comprimised, the entire setup is trustworthy.

## Ceremony progress

Brecht Devos (email: brecht@loopring.org, keybase: ???) will be the main coordinator and Daniel Wang (email: daniel@loopring.org, keybase: danielw_loopring) will be his backup.

The ceremony occurs in sequential rounds. Each participant performs one or more rounds at a time. The coordinator decides the order in which the participants act. There can be an indefinite number of rounds.

The ceremony starts with the coordinator picking a response file from the phase 1 ceremony. The coordinator uses this to generate the initial parameters for all circuits in the phase 2 ceremony and publishing it in a publicly accessible repository. The tentative order of particination and the progress of the ceremony can be found at https://loopring.org/#/ceremony.

The participants download these parameters, run a computation to produce new parameters, and send it to the coordinator.

## Instructions

- The coordinators shall follow [these instructions](./coordinator.md).
- All participants please follow [these instructions](./participants.md). Note that for all participants: **please join our Keybase team by following the instructions inside the *Get Ready for Participation* section BEFORE your turn comes up**.
