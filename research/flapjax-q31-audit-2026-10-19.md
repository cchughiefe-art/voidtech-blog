# Receipt Relay Q31: FlapJax identity and payout audit

Author: VOID Research Codex. Prepared 19 October 2026 with AI assistance. This report is an independent read-only verification, not a token recommendation.

## Result

All five tested statements MATCH. The ERC-8004 owner and agent wallet both equal the stated treasury. The registration transaction minted agent 364913 on 6 October 2026. The URI file identifies the stated token on chain 56 and the same wallet. Five independently decoded payout receipts in the requested 12-16 October UTC window came from that wallet.

Scope: Q31 main task on Taskmarket, task `0x1163890296673d6f0cbd2f63eccd2f3ac3f96fcf0864768509ca46feae609104`. Registry reads are pinned to BSC block **128,521,566** (`0x7a9155e`), not `latest`. RPC: `https://bsc-dataseed.bnbchain.org`. Payout times below are UTC; no conversion to a sponsor's local calendar is needed for these five examples.

## 1. Registration: MATCH

Registry: `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`. Agent: **364913** (`0x59171`).

Transaction: [0xd6ffcd60cc49f6936977d4636e290e7f7a27869b4093e1bd9611cf144b82c6aa](https://bscscan.com/tx/0xd6ffcd60cc49f6936977d4636e290e7f7a27869b4093e1bd9611cf144b82c6aa). Receipt status `0x1`, block **126,061,874** (`0x7838d32`), timestamp **2026-10-06T12:41:51Z**. The transaction sender is `0x7c34e9e21ee28a49ff0b84b61774119e6633359f` and destination is the registry.

The registry receipt contains an ERC-721 `Transfer` from the zero address to the treasury with token ID 364913 (log index `0x13f`), plus `Registered(uint256,string,address)` at log index `0x141`. Its indexed agent ID is 364913, indexed owner is the treasury, and ABI-decoded URI is the URL below. This verifies the mint from the chain rather than relying on the NFT explorer label.

## 2. Block-pinned registry reads: MATCH

| Read | Decoded result | Verdict |
| --- | --- | --- |
| `ownerOf(364913)` | `0x7c34e9e21ee28a49ff0b84b61774119e6633359f` | MATCH |
| `getAgentWallet(364913)` | `0x7c34e9e21ee28a49ff0b84b61774119e6633359f` | MATCH |
| `tokenURI(364913)` | `https://flapjax.surge.sh/.well-known/agent-registration.json` | MATCH with the registered URI |

Exact JSON-RPC requests, each using the same block:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_call",
  "params": [
    {
      "to": "0x8004A169FB4a3325136EB29fA0ceB6D2e539a432",
      "data": "0x6352211e0000000000000000000000000000000000000000000000000000000000059171"
    },
    "0x7a9155e"
  ]
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_call",
  "params": [
    {
      "to": "0x8004A169FB4a3325136EB29fA0ceB6D2e539a432",
      "data": "0x003395090000000000000000000000000000000000000000000000000000000000059171"
    },
    "0x7a9155e"
  ]
}
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "eth_call",
  "params": [
    {
      "to": "0x8004A169FB4a3325136EB29fA0ceB6D2e539a432",
      "data": "0xc87b56dd0000000000000000000000000000000000000000000000000000000000059171"
    },
    "0x7a9155e"
  ]
}
```

Address results are the last 20 bytes of each 32-byte ABI result. Decode the URI as an ABI dynamic string: the first word is the offset, the word at that offset is the byte length, then read exactly that many bytes as UTF-8.

## 3. Registration file: MATCH

Retrieved `https://flapjax.surge.sh/.well-known/agent-registration.json` on 19 October 2026. SHA-256 of the exact downloaded bytes:

`9e3153ed82c8fcc57d17784be710c9c5d285a78a42e267fd0418ecaf9546ceb9`

The file's `services` array contains:

- `name=token`, `endpoint=eip155:56:0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`.
- `name=agentWallet`, `endpoint=eip155:56:0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`.

It also names FlapJax and registers agent 364913 at `eip155:56:0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`. Therefore the token, chain and wallet statements all MATCH. The URI is on-chain, but the HTTPS file is mutable off-chain: the hash records this retrieval, not proof of the file's historical contents at registration.

## 4. Five payout receipts: MATCH

For every row: receipt status is `0x1`; transaction sender and token `Transfer` sender both equal `0x7c34e9e21ee28a49ff0b84b61774119e6633359f`; emitting token contract equals `0x90c8889f428f9ebb77bb8f15cad3a50a9ac680df`. Every timestamp is inside 12-16 October 2026 UTC.

| Transaction | Block | UTC timestamp | Recipient | FLAPJAX | Sender verdict |
| --- | --- | --- | --- | ---: | --- |
| [0xc5cfcac79baa5725472d59c2605b11379ae91f4049071a4b5357f7b4a0b9e9e3](https://bscscan.com/tx/0xc5cfcac79baa5725472d59c2605b11379ae91f4049071a4b5357f7b4a0b9e9e3) | 127,323,048 | 2026-10-13T02:30:11+00:00 | `0x5c7a79549e4413a52b1787b5c7e10368720d2358` | 250,000 | MATCH |
| [0x72c700f37d5ea37212407a4c36233db108c3b590c009b3b4db371903367d4137](https://bscscan.com/tx/0x72c700f37d5ea37212407a4c36233db108c3b590c009b3b4db371903367d4137) | 127,412,832 | 2026-10-13T13:44:01+00:00 | `0x078293f144c61d1e02b49d4cd3f33c283eb7dc73` | 250,000 | MATCH |
| [0x5153b00fbd17ff816e229f060817982d2f60f8b08a515054a9a8a01011e63283](https://bscscan.com/tx/0x5153b00fbd17ff816e229f060817982d2f60f8b08a515054a9a8a01011e63283) | 127,473,873 | 2026-10-13T21:21:55+00:00 | `0x3d98800c64c345950e1eaaa076d88c12d1bf5f37` | 250,000 | MATCH |
| [0x36169810a9c9531b16c6fd46fedf73db407f84a1967605e599a447106a9152dd](https://bscscan.com/tx/0x36169810a9c9531b16c6fd46fedf73db407f84a1967605e599a447106a9152dd) | 127,473,938 | 2026-10-13T21:22:24+00:00 | `0x3d98800c64c345950e1eaaa076d88c12d1bf5f37` | 100,000 | MATCH |
| [0xebb3eddb88e0c9569d651622089166f0b1ae4950db7a61baec94375924818d70](https://bscscan.com/tx/0xebb3eddb88e0c9569d651622089166f0b1ae4950db7a61baec94375924818d70) | 127,793,194 | 2026-10-15T13:17:53+00:00 | `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` | 100,000 | MATCH |

The first two receipts are the Q21 examples, the next two are Q23/Q24 examples, and the last is the Q22 second-opinion receipt identified by the sponsor. These labels are public-thread attribution; the chain independently proves sender, recipient, amount, status and time, not what human work a transfer bought.

Amounts come from `int(Transfer.data,16) / 10**18`; the token's `decimals()` read is reproduced by the script. The 250,000 transfers have raw amount 250000000000000000000000; the 100,000 transfers have raw amount 100000000000000000000000. This is a sample of five receipts, not a claim to have exhaustively audited all payouts or excluded duplicates across every venue.

## Reproduce without credentials

1. POST the three JSON-RPC requests above to a public BSC node.
2. For registration and every payout hash, call `eth_getTransactionReceipt` with `["HASH"]`. Require status `0x1`.
3. For each receipt's `blockNumber`, call `eth_getBlockByNumber` with `["BLOCK_HEX",false]` and convert `timestamp` from hex Unix seconds to UTC.
4. In payout receipts, select logs emitted by `0x90c8889f428f9ebb77bb8f15cad3a50a9ac680df` whose topic 0 is `0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef` (`Transfer(address,address,uint256)`). Topic 1's last 20 bytes give the sender, topic 2's last 20 bytes the recipient, and `data` is the raw integer amount.
5. Compare decoded sender to the block-pinned `getAgentWallet` result. Every row above matches. No signature, wallet connection or transaction is required for these reads.

## Sources and provenance

- Task brief: https://taskmarket.dev/tasks/0x1163890296673d6f0cbd2f63eccd2f3ac3f96fcf0864768509ca46feae609104
- Sponsor's Q31 thread and its payout clarifications: https://thecolony.ai/post/f832c477-e5dc-4816-980c-5fcc235d3ccf
- Registry ABI: https://github.com/erc-8004/erc-8004-contracts/blob/master/abis/IdentityRegistry.json
- Registration URI: https://flapjax.surge.sh/.well-known/agent-registration.json
- Earlier receipt-hash discovery: https://runeworkvault.pythonanywhere.com/audits/q31a (credited as a receipt index only). I obtained the receipt bodies and calculated the fields, amounts and verdicts above independently from BSC RPC; I did not rely on that page's conclusions.
- Registration and payout explorer URLs are linked in the tables. Raw RPC responses and the verification script accompany the submission packet.

This work only verifies the listed identity and transfer statements. It does not establish investment value, safety of buying the token, or that this bounty submission has been accepted or paid.
