# Nila — SCF #45 Tranche 1 Work Record

Nov 9, 2026 · @NilaDAO

Nila runs on Stellar mainnet since 6 November 2026. In the first week 13 users moved from Polygon; the 10 members among them each had an active loan, repaid in full (₹249,726 principal) and closed before the move.

## Summary

Nila is live on Stellar mainnet since 6 November 2026: nine Soroban contracts, the nIN asset, the trust's anchor, the node and agent, and the app. Mother Theresa Union is the first union on it. In the first week 13 users moved from Polygon to Stellar. The 10 members among them each had an active loan; all 10 repaid in full and closed (₹249,726 principal, ₹14,736 interest), withdrew their investment with their own signature, and had it booked on Stellar at the union desk.

**A — Anchor Platform on mainnet.** Run by the Nila Farmer Livelihoods Trust (Reg. Book-4/26/2026, PAN AAFTN4490C), which also issues nIN. stellar.toml at anchor.nila.land; SEP-10, SEP-12 and SEP-24 served by Anchor Platform 4.9.0 with Nila's business server. The ID check (K1) is done by the trust and recorded on-chain for two LPs (`fx_pool.set_id_checked`, `set_lp`). Cap: ₹10,000 per scan and per offer, held in `fx_pool.max_scan`. First capped withdrawal: an LP Collect of ₹9,299, offered on 8 November ([2d8dd233…](https://stellar.expert/explorer/public/tx/2d8dd2336d45fa6ae0f29cf3deffe089e0d95cf3183c567758f3d66faa3d53f5)), accepted by a union leader on 9 November ([5abd5e2b…](https://stellar.expert/explorer/public/tx/5abd5e2b5574e6669d3adae154ad3a897d60c186db8e4ca71e31df34e0cdda3c)), paid out in cash at the desk and scanned ([ce246ca1…](https://stellar.expert/explorer/public/tx/ce246ca103c7b860df42df984e109be01f18b6a5fcfe4eda92c993b03cf98041)).

**B — Privy in the existing app.** Members log in with an SMS code, not WhatsApp, so one channel also reaches keypad phones; Privy holds their Stellar key. 13 users are on Stellar mainnet, each with a smart account and a land title (10 members, 3 leaders).

**C — Nila's key backup.** No Nila-held copy of any Stellar member's key: the old login's encrypted keys of the 13 users on Stellar are deleted on November 9th, and its logs hold no plaintext key data. Privy holds their Stellar keys. Recovery after a lost number runs through `recover_by_sms`: a union leader plus the node's SMS key, after the union's 4-hour wait. Their land titles and FoodTokens stay on Polygon; their investment nIN is burned there, checkable on-chain. Members still on Polygon keep their encrypted key in the old login until they move.

**D — Mainnet deployment and hardening.** Contracts deployed from tag `mainnet-2026-11-06`, CI green on that commit (155 tests). nIN is a classic asset (auth required, revocable, clawback) with fx\_pool as SAC admin. USDC-backed mint into the union treasury is live (132 USDC so far). On-chain: ₹10,000 per cash scan and LP offer, rate 12.5 %, 48-hour upgrade delay on every contract. The ₹10,000 per-loan limit is kept by the crop plan engine and the union's agreement, and recorded as `max_loan` in the union config. Membership reads from `land_title` (13 titles). Union account: native 2-of-3, master key off. Trust admin: 3-of-5 since 9 November ([e291797c…](https://stellar.expert/explorer/public/tx/e291797c8a3479b6aceddaebaad58cc2f13d486dfc6641012578a900d6601799)). Runbooks in `docs/MAINNET_CHECKLIST.md`: deploy, verify, upgrade, caps, pause.

**Panel review follow-up.** Repayment: the 30 July audit sheet listed 9 closed loans as partially repaid because it summed only the closing repayment event; the contract records earlier instalments without an event. Counting every payment into the loan contract shows all 9 repaid in full, principal plus interest ("Repayment reconciliation" below); no loan has ever defaulted, and the 10 loans closed on 4–8 November add to that record. Circle: T1 runs on Circle's native USDC on Stellar (LP deposits and the union treasury). Direct work with Circle starts in T2, when senior USDC from EVM chains enters through CCTP.

**Not yet done.** SDF `anchor-tests` against the mainnet anchor; signed loan acceptances on Stellar (no loan drawn there yet); the key-rotation and key-loss runbook.

## Mainnet deployment (D)

Nine contracts and the nIN SAC went live on 6 November 2026 between 10:42 and 10:52 UTC, from tag `mainnet-2026-11-06` (commit `3eb0849`). The post-deploy check passed 81 of 81: each admin is the trust account, each contract has a 48-hour upgrade delay and no pending upgrade, and the code on-chain matches the uploaded build.

| Contract | Contract id | Wasm sha256 | Deploy tx |
| --- | --- | --- | --- |
| roles | [CCZOA…Q7SA](https://stellar.expert/explorer/public/contract/CCZOASMPMTKWLLKBYULQOCE4UGUDZ5AK4PFIO6R7KRPWIOCRK56MQ7SA) | `289ca94f…` | [89d88db2…](https://stellar.expert/explorer/public/tx/89d88db2239fee19eb37a9a0601b3e01ed8df9457d7fb0037167d416ac2bd496) |
| land\_title | [CDGXC…54TX](https://stellar.expert/explorer/public/contract/CDGXC3GRMNSSM4GTRTF7HB4GD47BKOB7F5QTZJV5YZ5GYRKS6QOW54TX) | `30d90deb…` | [6df6d3de…](https://stellar.expert/explorer/public/tx/6df6d3deac69d5c46d28816c8478ce065da05109c403a50967e9c7275ab8799b) |
| fund\_viewer | [CDXOP…GVII](https://stellar.expert/explorer/public/contract/CDXOPXECBXUNF5F4ETZVGTZ73MDHSG2JNNNSU2542OIMPFGQ7GJLGVII) | `45526f69…` | [69dec604…](https://stellar.expert/explorer/public/tx/69dec60416687525f616f4754aa11b2143945c3064010e7dfa9968667bd9f4f2) |
| fund\_core | [CAJ2J…QGZJ](https://stellar.expert/explorer/public/contract/CAJ2JJLZJ3QDV3BFYP5IH2SO3GYTIW7NLK2MK5YFUNXFDQ6T3ZOCQGZJ) | `8c50fa57…` | [9188215b…](https://stellar.expert/explorer/public/tx/9188215bfd660ba4b9aa3b5c998ddab535e318a47bd629f26eb8968a1398e2e7) |
| fx\_pool | [CDTG4…34XM](https://stellar.expert/explorer/public/contract/CDTG4XSKBFJQCSPXEAYXEOUW5HE7BMIHYIXT4X5HJMHGDEJC5ZPT34XM) | `1a458fe9…` | [f68342ae…](https://stellar.expert/explorer/public/tx/f68342ae48645c1eed56773ec42366440d63eed2274ccfc3a0624c9290ba5455) |
| machinery | [CDZ36…C4XJ](https://stellar.expert/explorer/public/contract/CDZ36IMSAEMIL54DDK2APEZPPKNEGMJ7ZWLES2TYXKYTYZKKQKKXC4XJ) | `49c32799…` | [b046d9e6…](https://stellar.expert/explorer/public/tx/b046d9e62261e1824c55950138cb378f2f675af1ba95292bc77780d3d393e8fd) |
| crop\_calendar | [CCSB2…SNDC](https://stellar.expert/explorer/public/contract/CCSB26TZKGMASFLVHRTJJNVJL3APP5NX5MZ7EJVQTRE7AQBBJLCESNDC) | `61ea6800…` | [fd3ae6ae…](https://stellar.expert/explorer/public/tx/fd3ae6aee4a5f6c519d9d640235b86070245008d3d1f77d9002ba59ee59a5e3b) |
| senior | [CD3T5…445S](https://stellar.expert/explorer/public/contract/CD3T5LXYQBNW4LE743RIMFUOHVKCX72QXBXFYXB6IJSTTX5LYSO4445S) | `f4570eba…` | [a5bf39b4…](https://stellar.expert/explorer/public/tx/a5bf39b4b331c3f222209ab563cae35b96b4dee67a3a06a1e9df011e83ad2599) |
| account\_deployer (member smart accounts, code `af510619…`) | [CDLG5…YSDU](https://stellar.expert/explorer/public/contract/CDLG5L7NYR56UJ7DPK6VP4X6OZPWXIGG7G3NPUBM762ELOWL2X4RYSDU) | `a1d7b282…` | [6949d6c9…](https://stellar.expert/explorer/public/tx/6949d6c98844635af8569b49b7bd171bc3cc0d694a64b0567974eee204d4ff01) |
| nIN SAC (`nIN:GABG…IG5A`) | [CAWW5…HODN](https://stellar.expert/explorer/public/contract/CAWW5N5OJ53CEBK2TSAZEIU6SWYHU3CDXSK6N7Q3RLKFVBX2MX5UHODN) | — | [6bdf5df9…](https://stellar.expert/explorer/public/tx/6bdf5df9e2bac0367db99e54c3a19de2420b0d9ac0087482bd632ca78e11de9c) |

| Account | Address | Signers |
| --- | --- | --- |
| Trust admin (admin of every contract) | [GC2V…5U3E](https://stellar.expert/explorer/public/account/GC2VBH7W2N2ZWEFZHWGB7DJ35HSD3DZKTNBMSCKJUYGJKUKESEHK5U3E) | 3-of-5 since 9 November 10:05 UTC: Carst (Ledger), Anand, Juliya, Ashadipta and Carmen at weight 1, thresholds 3/3/3 ([e291797c…](https://stellar.expert/explorer/public/tx/e291797c8a3479b6aceddaebaad58cc2f13d486dfc6641012578a900d6601799), five operations) |
| nIN issuer | [GABG…IG5A](https://stellar.expert/explorer/public/account/GABG43CGVHEJBZ24ABEULUCY6SZCETNKC6YNFXQNDFN5H4SKEXG6IG5A) | Ledger key; flags auth required, revocable, clawback; home domain anchor.nila.land; not locked yet |
| Mother Theresa Union | [GCDS…XEXW](https://stellar.expert/explorer/public/account/GCDSPDDG6H343Y3E24DOYJNC6CBSCC6GIPJXF6HED4DLLXKCEKKLXEXW) | native 2-of-3 of three leaders, master weight 0 ([handover tx](https://stellar.expert/explorer/public/tx/cdb30d0d6597a8f0102c0617fdaa8e6b5887c07f451d61cfff29f4576ca8db43)) |
| Anchor | [GB2O…ABG6](https://stellar.expert/explorer/public/account/GB2OFUYJCQRZJSIACYK4UNGLI3HPORL7FQK2L76ZHQDPM5VP2XWQABG6) | one key; set as `fx_pool.anchor` ([tx](https://stellar.expert/explorer/public/tx/1163033f6ca8c945f0e16a3c554456b8f2bed3c68175ffbf474a59a56c98da86)) |
| Node sponsor · attester · oracle · SMS key | [GDZS…EYQL](https://stellar.expert/explorer/public/account/GDZSAZ7XAXKCGNIWTRXP5OCAG5R3Y2PZRE7ZFGURMHXAUV536CGQEYQL) · [GDHZ…XUNH](https://stellar.expert/explorer/public/account/GDHZEOQID34M3GMYUN6P4TLKI2XHHGIL6WHTUF3ARG4VXYJTSYVKXUNH) · GDHJ…RKON · GCEG…SC3G | one key each, held by the node |

Limits set on-chain: ₹10,000 per cash scan and per LP offer (`fx_pool.max_scan`), 2,500 USDC a day, senior pool cap 0 until an audit. The ₹10,000 limit per loan in the first cycle is a rule, not a contract check. The crop plan engine (an LLM with code checks) sizes every loan within it, and the union, which decides how much cash it puts in or takes out, has agreed to keep to it. The union config records it as `max_loan` ₹10,000 next to the 12.5 % rate ([tx](https://stellar.expert/explorer/public/tx/c42aa5a3babc8c7520620f69ec447c874b4d82000c8e825c1297007abcc6ce32)), and every loan amount is readable on-chain.

## The move from Polygon

13 users of Mother Theresa Union moved to Stellar between 4 and 8 November: 10 members and 3 leaders. Each of the 10 members had an active Polygon loan. All 10 loans were repaid in full and closed on Polygon — ₹249,726 principal plus ₹14,736 interest, 9 of them over the weekend of 7–8 November — before her investment moved. The Polygon contracts confirm each loan as closed with nothing outstanding (`GenericFundViewer.getBorrowerInfo`).

A move takes four steps at the union desk:

1. She repays her Polygon loan in cash; the leader scans the notes and the loan closes (Polygon).
2. She withdraws her investment in the old app, signing it herself: a request, then a claim after the 2-minute wait (Polygon).
3. The node makes her Stellar smart account and the attester writes her land title (Stellar).
4. The leader books her investment by cash scan into the union fund (Stellar).

| Title | Polygon loan closed | Repaid (leader's cash scan) | Investment withdrawn (her signature) | Stellar account | Booked on Stellar |
| --- | --- | --- | --- | --- | --- |
| #11 | ₹28,742 + ₹62, 4 Nov | [0xe18048b2…](https://polygonscan.com/tx/0xe18048b2f4bf417d802f97f32679451de598d060aec35b78b2cc3412beb3ea2f) | [0xd5dff9e4…](https://polygonscan.com/tx/0xd5dff9e4f88414f560473419056e9cec13d7e5d2345397b1aad7c729c2598412) | [CDDUP…7SXQ](https://stellar.expert/explorer/public/contract/CDDUPAQCL77HD2CFN2IPMWC7BLQYM4QQHQ64G4G3TXRWP5HMXKWZ7SXQ) | ₹642 [445c9e97…](https://stellar.expert/explorer/public/tx/445c9e97afd2d0a6e4f6aced2f324278fb044da59d5c1c6f2933a16d0dedf9f7) |
| #48 | ₹20,016 + ₹1,316, 7 Nov | [0x07a5b8ce…](https://polygonscan.com/tx/0x07a5b8ced648a1d372d740fb5eb6d9409d00b4f81e553edabd6abe99513970a9) | [0x590ad414…](https://polygonscan.com/tx/0x590ad414e0c11d83048095784cc5cd83b9f4ce8d45ec1f3bedda2f3b85a5ed3b) | [CBXEE…4YDB](https://stellar.expert/explorer/public/contract/CBXEETRTUM2B23W5KKUQPXLNAXCFWJHCBF2VG4J5R2JZWGUOR7SU4YDB) | ₹1,056 [3c88c339…](https://stellar.expert/explorer/public/tx/3c88c33947e7985436e85427d85c897b5e45ea3888aab9eacd657a2866674a79) |
| #23 | ₹25,000 + ₹1,233, 7 Nov | [0x36752dc9…](https://polygonscan.com/tx/0x36752dc984b52dc3fe99a6c09277af3743c3de416f419d634ac6998ff88bd4b5) | [0x051dd643…](https://polygonscan.com/tx/0x051dd64378ddb5b90e94eb03d3b53421d054922256bd728d5efe1f6b2522122a) | [CA7PI…E5U7](https://stellar.expert/explorer/public/contract/CA7PIQ5OPY5LNWAKRKOOXSSPNR5YEXGSNJJ4SO2SN67IKCIQZYK7E5U7) | ₹627 [3fe799e8…](https://stellar.expert/explorer/public/tx/3fe799e8df339c56e3af725ed401e3ca00329fad850a6f90cb3258c7db14753c) |
| #47 | ₹17,145 + ₹1,128, 7 Nov | [0x7849a88e…](https://polygonscan.com/tx/0x7849a88e9c39e84ec86a750c7601fc3b80d9e64c1b2a410c76ca160dd549633f) | [0xfc93bf37…](https://polygonscan.com/tx/0xfc93bf374aa7ab3db309877aa2e02d65ac9132f2e207b47b1338fb5a8c178997) | [CABUK…B6S6](https://stellar.expert/explorer/public/contract/CABUK62UUOPABQLMSGFT2FW26ZSKDJPLIO7KIQLE777CDDDY34QCB6S6) | ₹1,066 [93f20285…](https://stellar.expert/explorer/public/tx/93f20285152d102c90802d98c2605cbfa5d3815063d4b262867686cdb035c173) |
| #50 | ₹23,823 + ₹1,247, 8 Nov | [0xb009ea6a…](https://polygonscan.com/tx/0xb009ea6a4084f2d291b0ec05e40469122897fd14293d33d6f6066018dfb15a63) | [0x3c6dfa1c…](https://polygonscan.com/tx/0x3c6dfa1c51ddceebbaf39e609bad82a662c324ac0cd3a4a62f5ff9205fd3d850) | [CD74S…G3HE](https://stellar.expert/explorer/public/contract/CD74SUYDYNW6IO7GWWNFQJNWKDWFMH2FNXHALJ6GANN5TQNJBJPMG3HE) | ₹631 [fefe5e83…](https://stellar.expert/explorer/public/tx/fefe5e8399534b255ab16fb8aa9aab9b640c72ffefd9361c26010643b782728d) |
| #42 | ₹25,000 + ₹1,861, 8 Nov | [0x1973f127…](https://polygonscan.com/tx/0x1973f127412abdbf3c8378d910f273bb9c7f5c0d9188cbd2350d6ce0ff519cdd) | [0x5580d147…](https://polygonscan.com/tx/0x5580d147fd57189068c6dce9c2cac865e0adc31c4f33574a6599ee32577b26da) | [CCU2U…ZG23](https://stellar.expert/explorer/public/contract/CCU2UHUCN636D7B6KVLMKNQHB3UUPQDR24BWJ6HZCGZKP6GRB7ZPZG23) | ₹640 [5e1074cb…](https://stellar.expert/explorer/public/tx/5e1074cb5bbc6b8cab7a2b2b758db0badf354c3eb135a7e2a0a216625b60634f) |
| #46 | ₹30,000 + ₹2,024, 8 Nov | [0x03f6b58a…](https://polygonscan.com/tx/0x03f6b58a0413e66aac765145461fab8ec9ab7995ca2ca771a994d99ba8b26b0c) | [0x37b52711…](https://polygonscan.com/tx/0x37b52711a1502b4d66b1435feb48f9880004aac0230c4c47e6243a954a6fa3fb) | [CDXCH…5IUN](https://stellar.expert/explorer/public/contract/CDXCHGJOOOISXRPXU4TGQF6XJZVMG3G36Q4NYBHELQ7JKKPYOEQA5IUN) | ₹1,054 [e9189937…](https://stellar.expert/explorer/public/tx/e91899375b6bab63d03f9afe845e170f12033a81219cc4426abb773654b4b332) |
| #51 | ₹25,000 + ₹1,275, 8 Nov | [0x658a1ea3…](https://polygonscan.com/tx/0x658a1ea31791f9e696472eb3d7e2feb2af4edc4e54e59553eefa189d81e99462) | [0x0a928fe9…](https://polygonscan.com/tx/0x0a928fe944b8ec2debe829e41498d6344a134b5d18b9dcd63f02a73b8041784b) | [CCC63…FIR3](https://stellar.expert/explorer/public/contract/CCC63R25WCCXBXZGQDVIVPV56GUFP4LC3A6UIHI73XVA3OV6WUP3FIR3) | ₹1,559 [0d7fb2f1…](https://stellar.expert/explorer/public/tx/0d7fb2f19d36628f6f9b537fed258247ec2591d6150536ccb7963f04f0663f5d) |
| #49 | ₹35,000 + ₹2,076, 8 Nov | [0x4c16f501…](https://polygonscan.com/tx/0x4c16f501753a2ea4170e6d6861ebc6850d2cdc72c981793b0372fba06dda5209) | [0x79605076…](https://polygonscan.com/tx/0x796050769c539730407bf68f87877c02d8f532a835e9539e831b5a26b1409a37) | [CB5BS…NSH4](https://stellar.expert/explorer/public/contract/CB5BSEBUEXIJMKZS5MGJ4JY4RCND7NVSVK2VFLWMZQZCWZLB3J67NSH4) | ₹741 [b19cbfbf…](https://stellar.expert/explorer/public/tx/b19cbfbf5687fabd8d9ad095edfab26e7efb74118abbb61a4c97f463d2234105) |
| #18 | ₹20,000 + ₹2,514, 8 Nov | [0xfbab71ea…](https://polygonscan.com/tx/0xfbab71ea335256bef39d6efd72aecda1219626a805ee5d4bd6d25282b80c805c) | [0x30e8ff37…](https://polygonscan.com/tx/0x30e8ff37afca7f3fd0d54b6566cd37e8755019f97b5a6357b08d6f4aa82a0351) | [CDIUQ…ONDH](https://stellar.expert/explorer/public/contract/CDIUQ75765LDZLACNFCOT6GZFAE76A6PNWJ4DI4CCXNVDETDEQNIONDH) | ₹641 [5a8298ac…](https://stellar.expert/explorer/public/tx/5a8298ac4778cd7ef525085930f2388fc03f87ccdb7d64acd13dff0d6bdb1018) |

The three leaders — Louisa (title #2), Anand (#8) and Carst (#40) — moved with their titles; Louisa also moved her investment (₹642, [Polygon claim](https://polygonscan.com/tx/0x59695224aebafda6c4f0f74aadcb6beac69ee6b8b260336c95924fdcc5aa7a94)). The leaders' accounts joined the union on Stellar with their own signatures (`set_union`). The Polygon loans were ₹17,145 to ₹35,000, sized under the Polygon rules; on Stellar the first cycle is capped at ₹10,000 per loan.

## Repayment reconciliation: the nine "partially repaid" loans

SCF's panel asked us to reconcile "0 defaults" with 9 loans that the 30 July 2026 audit sheet listed as partially repaid. That sheet summed `LoanRepaid` events, but the deployed contract emits `LoanRepaid` only when a loan closes, and that event carries only the final instalment; earlier instalments emit no event. Below, every token transfer into GenericFundCore (`0x4173…6756`) whose transaction carries the loan id. All nine were repaid in full, principal plus interest. No loan in the book has defaulted.

| Loan | Drawn | Principal ₹ | 30 Jul sheet: % repaid | Paid into Core ₹ | Interest ₹ | Payments |
| --- | --- | --- | --- | --- | --- | --- |
| `0x000c9168` | 13 Dec 2025 | 10,000 | 1.83 | 10,200.00 | 183.47 | [0x15eaf059…](https://polygonscan.com/tx/0x15eaf059e7fc0a70e13ec158a18b04b066eb609c49f5ab4d565b31719f233b42) 10,000 · [0x96c0b556…](https://polygonscan.com/tx/0x96c0b55665a5de4137859e4ce2165289a0ee844d41d31aafe1c0984085c04f0d) 200 |
| `0x00028a29` | 16 Dec 2025 | 10,000 | 32.59 | 10,261.00 | 259.04 | [0x6e5d4da7…](https://polygonscan.com/tx/0x6e5d4da7b336051a6c4ef6238e81052e89715af4531610a471417abd0bbb261c) 7,000 · [0xb90aeb36…](https://polygonscan.com/tx/0xb90aeb364577be0869eb3b31e5248f60226eb04c085e84f20ec0254470221bef) 3,261 |
| `0x0005ab81` | 16 Dec 2025 | 10,000 | 1.97 | 10,199.00 | 197.47 | [0xd40c541d…](https://polygonscan.com/tx/0xd40c541d29e2c54ed20625620502eb56c577d4ad143392306c0529e2ac71a43a) 10,000 · [0x792e90f0…](https://polygonscan.com/tx/0x792e90f0d86b36d72fc749358e431755518cb83a37b1010e67f45989e6afec80) 199 |
| `0x000a6e66` | 19 Dec 2025 | 11,000 | 10.85 | 11,300.00 | 193.40 | [0x9068f7c6…](https://polygonscan.com/tx/0x9068f7c695be55bd6d595494ae475ebdadf1e5d4194f21ce57dfcedbe8c054bc) 1,000 · [0xaedbc8f1…](https://polygonscan.com/tx/0xaedbc8f1e10bd0ffad87cda0365388336942c45ebc3326a785e9184fd4802d8f) 4,500 · [0x4b50ca7b…](https://polygonscan.com/tx/0x4b50ca7bf04b610a807a27ae166e23a40cdbd3ea1255dfaa65555e770da9e058) 4,500 · [0x353e6df3…](https://polygonscan.com/tx/0x353e6df321eb06b1dcbe26d05756bad8d39eed83235a6f36dd965ab65d79092c) 1,300 |
| `0x000d3865` | 24 Feb 2026 | 20,000 | 27.59 | 21,020.00 | 1,018.60 | [0xcbc66775…](https://polygonscan.com/tx/0xcbc66775c91bd0420148cce5eb25ac356575cfd2a9f928fdafcddf01fd6e3b88) 5,000 · [0x5d88b91f…](https://polygonscan.com/tx/0x5d88b91fa14a43ec8b4cb30086651c9819fcca523b36aadb2ecf6421ef414bb2) 10,500 · [0xe18b171b…](https://polygonscan.com/tx/0xe18b171b3a54a99074b689eaebe8621fa81fdedc3e9d18a3750a1bffd32fff40) 5,520 |
| `0x000a713d` | 9 Dec 2025 | 1,350 | 0.00 | 1,350.00 | 0.00 | [0x03d7294d…](https://polygonscan.com/tx/0x03d7294d52d603eb466aa1bcfe41b743dad0ec78645bb2dd207eec297ce50cd3) 1,350 |
| `0x000ec35e` | 9 Dec 2025 | 1,500 | 0.00 | 1,501.03 | 0.03 | six payments; largest [0x7a500d5c…](https://polygonscan.com/tx/0x7a500d5c674b5466bd72787a359ed0b4b0f8bcc7b640fe7c1f64dc263f19f804) 1,098.83 |
| `0x000572a6` | 9 Dec 2025 | 1,000 | 0.00 | 1,001.00 | 0.00 | four payments; largest [0x38014599…](https://polygonscan.com/tx/0x38014599c2bdde19ff3e4580cbe6e28b73879db19f6fa7756b9ce60b3a101c71) 998 |
| `0x00075d13` | 10 Dec 2025 | 1,000 | 0.00 | 1,000.00 | 0.00 | [0x9964fa3e…](https://polygonscan.com/tx/0x9964fa3e9ab2545145dcd47558d853b674c5128c312a8294df973683f69698dd) 1,000 |

The last four, of ₹1,000–1,500, were drawn and repaid by one address within days in December 2025; the farmer-sized record rests on the first five and on the ten loans closed on 4–8 November 2026. Members repay in cash at the union desk and a leader books it on-chain, so the paying address is usually a leader's. Re-run: `scripts/audit_site/dustcheck.js` in nila-integrations.

## Anchor and LPs (A)

The trust's anchor runs on mainnet since 6 November: Anchor Platform 4.9.0 plus Nila's business server, serving USDC to union LPs. An LP is a member who passed the trust's ID check (name, PAN, date of birth); she swaps rupees and USDC with her union at the desk. Members without the check hold neither USDC nor nIN.

| Endpoint | URL |
| --- | --- |
| SEP-1 stellar.toml | [anchor.nila.land/.well-known/stellar.toml](https://anchor.nila.land/.well-known/stellar.toml) |
| SEP-10 web auth | anchor-api.nila.land/auth |
| SEP-12 KYC | anchor.nila.land/sep12 |
| SEP-24 deposit and withdraw (USDC) | [anchor.nila.land/sep24/info](https://anchor.nila.land/sep24/info) |
| SEP-38 quotes | anchor.nila.land/sep38 |
| Union directory | [anchor-api.nila.land/unions](https://anchor-api.nila.land/unions) |

| UTC | Step | Tx |
| --- | --- | --- |
| 7 Nov 09:09 | Trust links the anchor and its swap key to fx\_pool | [1163033f…](https://stellar.expert/explorer/public/tx/1163033f6ca8c945f0e16a3c554456b8f2bed3c68175ffbf474a59a56c98da86), [15eb5167…](https://stellar.expert/explorer/public/tx/15eb51678d9348db2a3732ad9b7c7e00061469685b95ef6ce8aaf398c48dc123) |
| 7 Nov 11:57 | Trust sets the opening rate, ₹94.0468 per USD | [2b233b62…](https://stellar.expert/explorer/public/tx/2b233b623d2009e923e7c44e6e2fca41aeaa5bb7b3c4b3ef7244021d864a4ac1) |
| 7 Nov 12:03 | ID check passed and LP admitted: Carst | [76241e7d…](https://stellar.expert/explorer/public/tx/76241e7d7517a772f4e447899253f2d95a1cac6835e18c46e6af5ffdaa16d262), [ee973489…](https://stellar.expert/explorer/public/tx/ee9734899f71a0bb513dea94d4fc3bac5ef2a7531fd3ad5b54f75b4f9364662a) |
| 7 Nov 12:05 | ID check passed and LP admitted: Anand | [9bbb1977…](https://stellar.expert/explorer/public/tx/9bbb1977192c24899602b8ec232b5986ab94f99a9f930308d74adca7dd9592c6), [6a815a79…](https://stellar.expert/explorer/public/tx/6a815a7921524db4a46aa1e18ddacc374b1be2729ad12ff93d1228c7ec32507b) |
| 7 Nov 13:10–14:30 | USDC into the two LP accounts: 132.66 and 99.00 USDC | [854615b3…](https://stellar.expert/explorer/public/tx/854615b3c234e1f96a5f213bd51379e515495938ce724c951e120c19ffb1aaf3), [06138091…](https://stellar.expert/explorer/public/tx/061380916da945f933327e5ab336a9fc23ec4041597c89e5ec34b975d4789ea7) |
| 7 Nov 13:22–13:24 | 132 USDC into the union treasury, minted as nIN at the day's rate, each deposit ≤ ₹10,000 | [befccb4e…](https://stellar.expert/explorer/public/tx/befccb4e3be5a0e046066c70c904096316825959a8eb0d667df5fb94b1fe5fde), [709ad994…](https://stellar.expert/explorer/public/tx/709ad9949b802d96cf187be114417fc0b1f970ce6124f860422aad58014f9aee), [da74d510…](https://stellar.expert/explorer/public/tx/da74d5106ea224cb4127ac3bcb503a5e422e7d8cc13bc7dde9aaccfcdb35b678) |
| 8 Nov 03:03 | Attester's daily rate fix | [3f0e17f7…](https://stellar.expert/explorer/public/tx/3f0e17f736faa903584b2d64a67a4f08b8c18bf9611b24c0e4ca950a0e449aaf) |
| 8 Nov 10:11 | LP Collect offer posted: ₹9,299 for USDC | [2d8dd233…](https://stellar.expert/explorer/public/tx/2d8dd2336d45fa6ae0f29cf3deffe089e0d95cf3183c567758f3d66faa3d53f5) |
| 9 Nov 08:06–08:11 | Collect accepted by the union, rupees paid out at the desk and scanned; SEP-24 transaction completed | [5abd5e2b…](https://stellar.expert/explorer/public/tx/5abd5e2b5574e6669d3adae154ad3a897d60c186db8e4ca71e31df34e0cdda3c), [ce246ca1…](https://stellar.expert/explorer/public/tx/ce246ca103c7b860df42df984e109be01f18b6a5fcfe4eda92c993b03cf98041) |

SDF's `anchor-tests` ran against the testnet anchor on 30 October: 64 of 86 pass with the membership gate opened for the test, 22 of 86 with it on. Not yet run against mainnet; the mainnet /sep38 endpoints return 403.

## App, node and agent (B)

The app, the node and the agent run against Stellar mainnet since 6 November. The node publishes the network and every contract id at [node.nila.land/stellar/config](https://node.nila.land/stellar/config); the app reads them from there, so a redeploy needs no new app build.

| Part | Where | Version on 8 Nov | What it does on Stellar |
| --- | --- | --- | --- |
| App (PWA) | [pwa.nila.land](https://pwa.nila.land), AWS Amplify | 0.5.6, deployed 8 Nov 16:25 UTC; repo [NilaDAO/nila-pwa](https://github.com/NilaDAO/nila-pwa) | SMS code login; Privy holds the member's Stellar key; a moved member is routed to the Stellar app; leader desk tasks (cash scan, help QR, joins, treasury, LP offers); Nila lite for keypad phones |
| Node | [node.nila.land](https://node.nila.land/stellar/config), Akash | image `sensing_node_v0.5.0`, tag `mainnet-2026-11-06c` | makes member smart accounts; the attester writes land titles and the daily rate; tops up leaders and ID-checked LPs with XLM; collects the union's 2-of-3 signatures |
| Agent | Akash, beside the node | image `sensing_agent_v0.5.0`, same tag | satellite field borders and crop stage (sowing, harvest) per field; feeds crop plans and the harvest date that sets repayment |

Work done by each on mainnet so far: the node's sponsor made 12 of the 14 smart accounts and funded the leaders; the attester wrote 13 land titles and the rate fix. The leaders made 11 cash-scan bookings and 3 treasury deposits from the app.

## Key custody (C)

Nila holds no copy of any Stellar member's key: the 13 encrypted Polygon keys are deleted from the old login on 9 November. Members still on Polygon keep theirs there until they move.

|  | Member on Stellar (13 users) | Member still on Polygon |
| --- | --- | --- |
| Her signing key | her own key on her smart account: Privy for a smartphone, a P-256 device key for a keypad phone | generated by the old login, stored AES-encrypted in Cognito |
| Nila's copy | none: her encrypted Polygon key deleted from Cognito on 9 November; the login's logs hold no plaintext key data | the encrypted key and salt, until she moves |
| Lost phone, SMS still reaches her | Privy restores the key; nothing on-chain | the old login decrypts the stored key |
| Lost number, or keypad phone lost | `recover_by_sms`: a union leader plus the node's SMS key propose the new key; it takes effect after the union's 4-hour wait | — |
| Her Polygon assets | land title and FoodTokens stay at her Polygon address; her investment nIN is burned | unchanged |

The nIN burns, all to `0x…dEaD` on Polygon after the members' withdrawals:

| Title | nIN burned | Tx |
| --- | --- | --- |
| #2 | 604.66 | [0x81e1d914…](https://polygonscan.com/tx/0x81e1d9147b9b6106c432e200ceda8395b06ca7b414dbf65d8f18611af8194691) |
| #11 | 604.66 | [0x8d0fc4f1…](https://polygonscan.com/tx/0x8d0fc4f11382debc62b3196859ff020717d12cbfcc28797fb05b71ec1a8bdaca) |
| #18 | 612.33 | [0xcff24f2b…](https://polygonscan.com/tx/0xcff24f2bcb6a40e9c1dfe6ee17b7da287a98ad4f702c38bb18cc2356bb1b0eb2) |
| #23 | 602.66 | [0xe77c083d…](https://polygonscan.com/tx/0xe77c083d8d376a72b4e22c0db00896f552e6572fe50a82042f7bab1240da4e9b) |
| #40 | 12,021.12 + 514.97 | [0xd2fa9793…](https://polygonscan.com/tx/0xd2fa979366610c59fa772948860f6c17f727099630483b7641ff07077d332c28), [0x6e5ae507…](https://polygonscan.com/tx/0x6e5ae50797cb2ca4c2bd0d646c029433796d0fc066716966c098c3ce7d471287) |
| #42 | 606.40 | [0x3c71d352…](https://polygonscan.com/tx/0x3c71d35239c667b14227e85767ee6df73a6dbf5b10c08ac8cf893dd8b82b980c) |
| #46 | 5,923.74 | [0xa7ffdfa3…](https://polygonscan.com/tx/0xa7ffdfa39d4e075359d1359589345adce408538ffe6a2561b2ebdfb715de5558) |
| #47 | 1,007.33 | [0x0de6f887…](https://polygonscan.com/tx/0x0de6f8876f5c53b95389721cc417a2e3510f5946bf66da60c1e2505ad1fdec9b) |
| #48 | 1,005.23 | [0x1780d57a…](https://polygonscan.com/tx/0x1780d57a5faa48723d89c529e0035566864447b710559d70f149ccf25192a8a1) |
| #49 | 13,880.62 | [0x43fb81e3…](https://polygonscan.com/tx/0x43fb81e3bee3fdc585fbadce8baf34dbf7484fa6e0e3eef894a81314f6f9678c) |
| #50 | 604.51 | [0xb86c0283…](https://polygonscan.com/tx/0xb86c0283d437a6f5f2301d1956775f48553e54c88149d257666f5bb0ec1fbbb5) |
| #51 | 1,495.01 | [0x86148248…](https://polygonscan.com/tx/0x86148248732607014c0ced9bb85c52ee4e779e180ce174aa375c844fafa0496e) |

On 9 November 11 of the 13 addresses hold 0 nIN on Polygon. Two leaders still hold some: #40 (Carst) 483.54 nIN and #8 (Anand) 707.65 nIN.

## Changes from the approved plan

| Approved plan | As built | Why |
| --- | --- | --- |
| nIN issued by Aathava | issued by the Nila Farmer Livelihoods Trust, which also runs the anchor | the trust is registered: Nila Farmer Livelihoods Trust, Reg. Book-4/26/2026, PAN AAFTN4490C |
| Admin signers: Carst, Anand, Ashadipta operating; Rosalie, Juliya cold | trust admin 3-of-5: Carst, Anand, Ashadipta, Juliya, Carmen | one change: Carmen replaces Rosalie, for a balance of three signers in India and two in the Netherlands |
| `membership` contract, entries confirmed by leader and attester | membership = holding a land title in `land_title` | one record instead of two; the title already carries the leader's mapping |
| Members hold their own keys on plain accounts | every user has a member-owned Soroban smart account from one immutable deployer | recovery without a Nila-held member key |
| Union keys not specified | each union is a native Stellar 2-of-3 of three of its leaders, master key off | two leaders must agree on any union change |
| Privy signs for both chains | Privy holds the Stellar key; the Polygon key stays in the old login until the move is complete | moving Polygon keys into Privy means migrating assets that retire anyway |
| WhatsApp OTP | SMS code | SMS reaches every member's phone: basic keypad phones do not run WhatsApp, and keypad members get their Nila lite link by SMS. WhatsApp also needs Privy's paid plan. SMS sender registration under the trust (local route, late December at the earliest) addresses the delivery delays |
| Sponsored reserves | each account pays its own XLM; the node tops up leaders and LPs below 2 XLM | simpler, and no node signature on member calls |
| Members hold nIN and swap peer to peer | members hold neither nIN nor USDC and use cash at the union desk; the anchor serves ID-checked LPs | PMLA: a member holding crypto would need the full FIU-IND KYC stack (see Legal compliance) |
| SEP-12 wired to leader attestation; SEP-24 interactive | SEP-12 = the trust's ID check, recorded on-chain; the SEP-24 step completes inside the app | a leader can confirm membership but not identity; the FIU-IND KYC rules ask for a PAN check |
| Rate from Reflector inside a 2 % band | the attester's daily fix inside a 5 % band; Reflector in T2 | no INR feed wired yet |
| Disbursement on detected sowing | disbursement inside the on-chain crop calendar's sowing windows; repayment still follows the detected harvest | deterministic and checkable |
| ₹10,000 cap in code | ₹10,000 per loan kept by the crop plan engine and the union's agreement, recorded as max\_loan in the union config; cash scans and LP offers limited by the fx\_pool.max\_scan setting | the union decides how much cash it puts in or takes out, and has agreed to keep each loan within ₹10,000; the crop plan engine sizes every loan inside that rule, and each loan amount is readable on-chain |

## Legal compliance

Two Indian rules shaped the Stellar build more than any technical choice. Agricultural services are open to foreign investment and free of GST; that is where the machinery vouchers come from. A member holding crypto would fall under the full anti-money-laundering KYC stack; that is why only ID-checked LPs hold USDC, and why an LP position is a collateralised debt, not a ramp.

### Agri-services and the vouchers

**Tax.** Services relating to cultivation are exempt from GST under entry 54 of Notification 12/2017-Central Tax (Rate): agricultural operations (cultivation, harvesting, threshing, plant protection), supply of farm labour, and renting or leasing of agro machinery ([summary of entries 54–58](https://taxclue.in/blog/agriculture-exemption-entries-54-to-58-and-agricultural-produce)).

**Foreign investment.** India's FDI policy allows 100 % foreign investment under the automatic route in "services related to agro and allied sectors" ([Invest India](https://www.investindia.gov.in/team-india-blogs/fdi-opportunities-indian-agriculture)). The restriction on foreign borrowing for agriculture, FEMA Reg 3A(1)(d), excepts the same services.

**The voucher.** So the part of a loan that outside money pays for is a service, not cash. A member's crop share carries machinery vouchers counted in tractor acres. The union sets a price per acre for each machine type, the owner does the work and confirms the acres, and the senior side pays the owner. She repays the voucher at harvest. The cash part of a loan comes only from the members' own money at the union desk. On mainnet: 13 machine-type rates and one crop rate set by the union on 6 November (`machinery.set_rate`, `set_crop_rate`).

### LP positions: collateralised debt, not a ramp

Under FIU-IND's AML/CFT guidelines of 8 January 2026, whoever handles virtual digital assets for others needs KYC with PAN, a penny-drop bank check, a liveness selfie, the Travel Rule with no minimum amount, and five years of records. For a ₹4,000 loan that stack cannot be built. So members hold neither nIN nor USDC, and every rupee goes over the union desk by QR scan. Only members who pass the trust's ID check (name, PAN, date of birth) hold USDC: the LPs.

Every anchor transaction opens or closes a USDC-collateralised rupee position between an LP and her own union:

- **Collect.** The union pays the LP rupees in cash. Her own USDC moves into fx\_pool and stays locked there in her name. She owes the rupees; the USDC is her collateral and stays hers.
- **Bring.** The LP pays the union rupees. The union's own treasury USDC moves to her account, locked. The union owes the rupees; that USDC is the union's collateral. Senior USDC never backs a position.
- **Close.** Her opposite offer ends the position: the rupees go back, the USDC unlocks to its owner, and a 2 % fee is paid in rupees.
- **Settle.** If the INR/USD fix moves more than 5 % from the position's opening rate, anyone may settle it: the collateral goes to the side that is owed (a Collect's USDC to the union, a Bring's USDC to the LP), and each side keeps the cash it holds.

USDC changes owner only at a settle, against a debt already owed in rupees. Opening or closing a position moves collateral, not ownership. On the protocol side a Collect is still a SEP-24 withdrawal: USDC leaves the LP's wallet and she receives rupees at the desk, capped at ₹10,000.

## Work record

From award to mainnet took 50 days. Across six repositories that is 1,489 commits between 5 October and 8 November, peaking at 574 in the week of 19 October. Repository histories before 8 October were rewritten for a secret scrub, so earlier work shows as squashed commits.

| Date | Work | Where |
| --- | --- | --- |
| 9 Nov | Trust admin becomes a 3-of-5 multisig in one transaction: four signers added, thresholds 3/3/3. Every admin call on the contracts now needs three of five signatures; the Ledger alone can no longer sign | Stellar, tx e291797c… |
| 8 Nov | App 0.5.6 live: LP view for leaders, moved members' records loaded from IPFS by hash. First LP Collect offer on mainnet (₹9,299) | nila-pwa #39, #40; fx\_pool |
| 7–8 Nov | 9 members repay their Polygon loans at the desk, withdraw their investments and move to Stellar; leaders book them by cash scan | Polygon, Stellar |
| 7 Nov | Anchor linked to fx\_pool; 2 LPs pass the trust's ID check; 132 USDC into the union treasury; union account handed to its 2-of-3; app and node releases for the move (chain re-read, fee bid, join by help QR, treasury in USDC) | nila-pwa #33–#38, nila-sensing #33–#34, nila-admin #3–#6 |
| 6 Nov | Nine contracts and the nIN SAC deployed to mainnet; union admitted, leaders, settings and machine rates set; node and agent 0.5.0 on Akash; union account changed to 2-of-3 | nila-soroban `mainnet-2026-11-06`, nila-sensing `mainnet-2026-11-06c` |
| 5 Nov | Second contract review fixed: no `approve` ever, a per-account swap cap, session contracts need the core role. Polygon FoodToken upgraded by Ledger (leader mint, 15 crop families). Node keys made | nila-soroban `mainnet-2026-11-05`; Polygon |
| 4 Nov | One recovery path (`recover_by_sms`); every account pays its own XLM, no node fee-bump. First member (#11) repays and moves | nila-soroban, nila-pwa #26–#30 |
| 3 Nov | Member-owned accounts from one immutable deployer; unions admit themselves and are admitted by the trust, with a public directory; trust admin on Ledger, 3-of-5 rehearsed on testnet | decisions 007, 008; nila-anchor; nila-admin |
| 2 Nov | Senior pool in its own contract, cap 0 until audit; login stops logging keys | nila-soroban, nila-pwa |
| 30–31 Oct | LP model settled as a swap: USDC moves at the scan. Cash room on-chain. `anchor-tests` on testnet: 64 of 86 | nila-soroban #14, nila-anchor #5, nila-integrations #18 |
| 29 Oct | Trust registered (PAN). Last draw date and lapse from the crop calendar; desk tasks signed by the member | nila-soroban #13, nila-sensing #18, nila-pwa #21 |
| 26–27 Oct | Smart accounts for every user. Machinery vouchers counted in acres, not hours | nila-soroban, nila-sensing, nila-pwa |
| 22–23 Oct | Union cash desk, leader swaps, the trust's ID check (K1), keypad phones with a P-256 device key | nila-soroban #6–#7, nila-sensing #10–#11, nila-anchor #1 |
| 20 Oct | WhatsApp OTP dropped for SMS (Privy plan) | decision |
| 15–20 Oct | Contract steps C7–C10 (leader treasury deposits, members' cash claims); node steps O7–O16 (sponsored leader calls, loan list, field borders, keypad members); demo stack with checks E1–E7 | nila-soroban #2–#5, nila-sensing #1–#9, nila-integrations #1–#13 |
| 16 Oct | Login: SMS code plus Privy for the Stellar key; crop plan and the move in the app | nila-pwa P1–P4 |
| 10 Oct | Mainnet checklist; rate cap 12.5 % in code; 48-hour upgrade delay from day one | nila-soroban `docs/MAINNET_CHECKLIST.md` |
| 8–9 Oct | Step C1 on testnet: nIN asset, roles, union multisig, membership; the `membership` contract replaced by land titles | nila-soroban `scripts/c1-testnet.sh` |
| 21 Sep | Tranche 0 kickoff | — |
| 17 Sep | SCF #45 awarded, $95,000 | — |

## Code and tests

The contracts deployed to mainnet are tag `mainnet-2026-11-06` of [NilaDAO/nila-soroban](https://github.com/NilaDAO/nila-soroban) (MIT). CI passed on that exact commit: formatting, clippy, 155 tests with 25 `env.auths()` assertions, the wasm build and a 128 KiB size gate per contract ([run 42192602515](https://github.com/NilaDAO/nila-soroban/actions/runs/42192602515)). fund\_core is the tightest fit: 128,945 of 131,072 bytes.

| Repository | Holds | Visibility | Mainnet tag | Commits since award |
| --- | --- | --- | --- | --- |
| [nila-soroban](https://github.com/NilaDAO/nila-soroban) | Stellar contracts, mainnet checklist | public, MIT | `mainnet-2026-11-06` (`3eb0849`) | 148 |
| [nila-pwa](https://github.com/NilaDAO/nila-pwa) | member, leader and LP app | public | `mainnet-2026-11-07b`, `main` at #40 | 739 |
| nila-sensing | node and agent | private | `mainnet-2026-11-07b` | 301 |
| nila-anchor | the trust's anchor | private | `mainnet-2026-11-06` (`322c8a9`) | 33 |
| nila-integrations | demo stack, end-to-end checks, deploy scripts | private | `mainnet-2026-11-06` | 230 |
| nila-admin | trust console: signers, status, the move | private | — | 38 |
