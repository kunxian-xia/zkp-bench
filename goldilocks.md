# Goldilocks

prime field $F_p = 2^{64} - 2^{32} + 1$.

## Fp arithmetic


```text
2023-09-18T03:43:08+00:00
Running ./bench
Run on (64 X 922.621 MHz CPU s)
CPU Caches:
  L1 Data 48 KiB (x32)
  L1 Instruction 32 KiB (x32)
  L2 Unified 1280 KiB (x32)
  L3 Unified 55296 KiB (x1)
Load Average: 7.72, 1.93, 0.95
0.931738ns / op
----------------------------------------------------------------------
Benchmark                            Time             CPU   Iterations
----------------------------------------------------------------------
ADD_AVX2_BENCH/32/real_time       1.86 s          1.86 s             1
0.931685ns / op
ADD_AVX2_BENCH/64/real_time       1.86 s          1.86 s             1
2023-09-18T03:43:12+00:00
Running ./bench
Run on (64 X 861.694 MHz CPU s)
CPU Caches:
  L1 Data 48 KiB (x32)
  L1 Instruction 32 KiB (x32)
  L2 Unified 1280 KiB (x32)
  L3 Unified 55296 KiB (x1)
Load Average: 7.18, 1.92, 0.95
2.11913ns / op
----------------------------------------------------------------------
Benchmark                            Time             CPU   Iterations
----------------------------------------------------------------------
MUL_AVX2_BENCH/32/real_time       4.24 s          4.24 s             1
2.11918ns / op
MUL_AVX2_BENCH/64/real_time       4.24 s          4.24 s             1
```
## NTT

Benchmarked on aws `c6i.16xlarge`. The `NTT_BENCH` bench case has 100 NTT of size $2^23$.

That is, 
- 1 NTT of size $2^23$ takes about 38.4ms.
- As a comparison, NTT over bn254 scalar field takes about .



```text
Running ./build/bench
Run on (64 X 871.959 MHz CPU s)
CPU Caches:
  L1 Data 48 KiB (x32)
  L1 Instruction 32 KiB (x32)
  L2 Unified 1280 KiB (x32)
  L3 Unified 55296 KiB (x1)
Load Average: 0.00, 0.00, 0.00
-----------------------------------------------------------------------
Benchmark                             Time             CPU   Iterations
-----------------------------------------------------------------------
NTT_BENCH/32/real_time             5.61 s          5.39 s             1
NTT_BENCH/64/real_time             4.27 s          3.84 s             1
NTT_BLOCK_BENCH/32/real_time       1.80 s          1.80 s             1
NTT_BLOCK_BENCH/64/real_time       2.26 s          2.26 s             1

```
## Poseidon

## Merkle Tree