# bellperson 

## Features
1. OpenCL impls of common zkp primitives through [ec-gpu](https://crates.io/crates/ec-gpu) and [ec-gpu-gen](https://crates.io/crates/ec-gpu-gen).
    - [ ] bls12-381 field arithmetic
    - [ ] bls12-381 ec arithmetic
    - [ ] FFT over bls12-381 `Fr` scalar field
    - [ ] MSM over bls12-381 `Fr` and `Fq`
2. Easy to support new curves (opencl files are auto generated)

    a. bls12-381: has native support for SNARK-friendly hash function (pedersen hash)

    b. bn254: supported on ethereum

3. Groth16 proof system  


## Bench


|   primitive   |     size     | time | gpu card         |
|:-------------:|:------------:|------|------------------|
| bls12-381 msm | 2^20 scalars |      | GeForce RTX 3080 |