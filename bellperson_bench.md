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


|   primitive   |     size     | time  | gpu card         | PCIe bandwidth              |
|:-------------:|:------------:|-------|------------------|-----------------------------|
| bls12-381 msm | 2^20 scalars |       | GeForce RTX 3080 |
| bls12-381 fft | 2^20 scalars | 11ms  | GeForce RTX 3090 | H2D: 26.2GB/s, D2H:26.0GB/s |
| bls12-381 fft | 2^25 scalars | 584ms | GeForce RTX 3090 |

### Understand the bench result

> FFT of 2^25 scalars over the BLS12-381 curve took 584ms to finish on RTX 3090

First of all, `RTX 3090` has 10496 CUDA cores (82 SMs) and a warp has 32 threads.
We use radix-8 in the first round, which means the number of radix-8 butterfly operations
is `2^25 / 2^8 = 2^17`. Each SM needs to handle about 1596 ops, and it took about 15ms to finish.

This means that each radix-8 butterfly needs `15*10^6 / 1596 = 9386ns`.


>  MSM of 2^20 random scalars over the BLS12-381 curve took 

## Algorithms

FFT of `2^n` scalars is split into several radix-8 rounds.
For example, 
- n = 16, 2 radix-8 round.
- n = 17, 3 radix-8 round.
  - this is not optimal

### CUDA Programming Best Practices 

- warp
- stride

## Logs

1. FFT of bls12-381 on RTX 3090.

   ```text
   ============================
   Testing FFT for 65536 elements...
   fft radix round 0 -> 8 took 66.982µs
   fft radix round 8 -> 16 took 74.972µs
   GPU took 1ms.
   CPU (128 cores) took 18ms.
   Speedup: x18
   ============================
   Testing FFT for 131072 elements...
   fft radix round 0 -> 8 took 105.213µs
   fft radix round 8 -> 16 took 118.954µs
   fft radix round 16 -> 17 took 1.000704ms
   GPU took 3ms.
   CPU (128 cores) took 36ms.
   Speedup: x12
   ============================
   Testing FFT for 262144 elements...
   fft radix round 0 -> 8 took 178.046µs
   fft radix round 8 -> 16 took 192.367µs
   fft radix round 16 -> 18 took 1.325385ms
   GPU took 5ms.
   CPU (128 cores) took 62ms.
   Speedup: x12.4
   ============================
   Testing FFT for 524288 elements...
   fft radix round 0 -> 8 took 293.22µs
   fft radix round 8 -> 16 took 338.222µs
   fft radix round 16 -> 19 took 1.573814ms
   GPU took 8ms.
   CPU (128 cores) took 106ms.
   Speedup: x13.25
   ============================
   Testing FFT for 1048576 elements...
   fft radix round 0 -> 8 took 539.599µs
   fft radix round 8 -> 16 took 623.541µs
   fft radix round 16 -> 20 took 1.817142ms
   GPU took 11ms.
   CPU (128 cores) took 159ms.
   Speedup: x14.454545
   ============================
   Testing FFT for 2097152 elements...
   fft radix round 0 -> 8 took 1.033376ms
   fft radix round 8 -> 16 took 1.19845ms
   fft radix round 16 -> 21 took 2.07546ms
   GPU took 20ms.
   CPU (128 cores) took 323ms.
   Speedup: x16.15
   ============================
   Testing FFT for 4194304 elements...
   fft radix round 0 -> 8 took 2.028999ms
   fft radix round 8 -> 16 took 2.35808ms
   fft radix round 16 -> 22 took 3.178428ms
   GPU took 38ms.
   CPU (128 cores) took 664ms.
   Speedup: x17.473684
   ============================
   Testing FFT for 8388608 elements...
   fft radix round 0 -> 8 took 3.974506ms
   fft radix round 8 -> 16 took 4.6728ms
   fft radix round 16 -> 23 took 6.549423ms
   GPU took 73ms.
   CPU (128 cores) took 1364ms.
   Speedup: x18.68493
   ============================
   Testing FFT for 16777216 elements...
   fft radix round 0 -> 8 took 7.89872ms
   fft radix round 8 -> 16 took 9.294258ms
   fft radix round 16 -> 24 took 13.761629ms
   GPU took 144ms.
   CPU (128 cores) took 2789ms.
   Speedup: x19.368055
   ============================
   Testing FFT for 33554432 elements...
   fft radix round 0 -> 8 took 15.754057ms
   fft radix round 8 -> 16 took 18.553053ms
   fft radix round 16 -> 24 took 27.491648ms
   fft radix round 24 -> 25 took 301.149745ms
   GPU took 584ms.
   CPU (128 cores) took 5546ms.
   Speedup: x9.496575
   ============================
   ```