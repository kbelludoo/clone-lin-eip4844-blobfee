# clone-lin-eip4844-blobfee

**EXPERIMENTAL** LIN clone of go-ethereum [`fakeExponential` / `CalcBlobFee`](https://github.com/ethereum/go-ethereum/blob/v1.14.12/consensus/misc/eip4844/eip4844.go) (LGPL-3.0 upstream).

This is **not** a Geth node, not Ethereum mainnet blob-market parity, and not uint256. The kernel is uint64-scale with a 128-bit product for the Taylor step `(accum * numerator) / denominator`. Class: EXPERIMENTAL.

## Provenance

| Field | Value |
|---|---|
| Upstream | https://github.com/ethereum/go-ethereum |
| Path | `consensus/misc/eip4844/eip4844.go` |
| Tag | `v1.14.12` |
| Commit | `293a300d64be3d9a1c2cc92c26fcff4089deadcd` |
| File sha256 | `23af9802a9d2acc3818dba2768bc81da37fb857dd67590a27e3e13edaae02868` |
| Git blob | `2dad9a0cd3de18ac8db0dde8ba7101300b11d6d9` |
| License (upstream) | LGPL-3.0 |
| Cancun constants | minBlobGasPrice=1, updateFraction=3338477, target=3*131072 |

Canonical vectors (recomputable from Geth `TestCalcBlobFee`): `CalcBlobFee(0) = 1`, `CalcBlobFee(10485760) = 23`.

## Files

- `src/lin_eip4844_blobfee.lin` — scalar LIN kernel (128-bit schoolbook Taylor muldiv)
- `test/eip4844_integer.c` — C11 extract used as the naive uint64 restatement
- `docs/PROVENANCE.rulel` — claims / non-claims

## Proofs live in lin-open

Results and the external harness stay in the LIN toolchain repo:

- Results: https://github.com/kbelludoo/lin-open/tree/cursor/linguagem-lin-e-valida-o-57bd/examples/eip4844_blobfee
- Harness: `python3 test/prove_eip4844_blobfee_external.py` (gcc == Python == `lin_c0` vm)
- Claim sheet: `docs/events/EVENT_EIP4844_BLOBFEE_CLONE_LIN.rulel`

```bash
make -C transpile/c c0
./transpile/c/bin/lin_c0 vm src/lin_eip4844_blobfee.lin bb_test_suite
# value=1
./transpile/c/bin/lin_c0 vm src/lin_eip4844_blobfee.lin bb_cancun_blob_fee 0
# value=1
./transpile/c/bin/lin_c0 vm src/lin_eip4844_blobfee.lin bb_cancun_blob_fee 10485760
# value=23
```
