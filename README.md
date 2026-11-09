# Nila — SCF #45 tranche records

Nila finances smallholder farmers through their own unions: crop loans and machinery vouchers, booked on Stellar.
Stellar Community Fund #45 award: $95,000, paid in tranches T0–T3.
Tranche 1 work record: [tranche-1/](tranche-1/Nila_SCF45_T1_Work_Record.md) ([PDF](tranche-1/Nila_SCF45_T1_Work_Record.pdf)); this page as [PDF](SCF45_Panel_Feedback.pdf).
Code: [NilaDAO/nila-soroban](https://github.com/NilaDAO/nila-soroban) (contracts and tests, mainnet tag [`mainnet-2026-11-06`](https://github.com/NilaDAO/nila-soroban/tree/mainnet-2026-11-06)) · [NilaDAO/nila-pwa](https://github.com/NilaDAO/nila-pwa) (app).
Live: [pwa.nila.land](https://pwa.nila.land) · [anchor.nila.land/.well-known/stellar.toml](https://anchor.nila.land/.well-known/stellar.toml).

## SCF #45 panel feedback

1. **Repayment story ("0 defaults" vs 9 partial loans):** [tranche-1/Repayment_Reconciliation_9_loans.md](tranche-1/Repayment_Reconciliation_9_loans.md) — all 9 repaid in full on-chain; the 30 Jul sheet summed only final instalments. Also the 10 loans repaid and closed on 4–8 Nov 2026 (work record, "The move from Polygon").
2. **Privy and Circle:** Privy is the Stellar key layer in the live app ([pwa.nila.land](https://pwa.nila.land), work record "App, node and agent"). USDC on Stellar is Circle's native asset (issuer GA5Z…KZVN); LP deposits and the union treasury already run on it (work record "Anchor and LPs").
3. **Indian regulatory position:** work record, section "Legal compliance" — public here.
4. **Soroban work:** [github.com/NilaDAO/nila-soroban](https://github.com/NilaDAO/nila-soroban), tag `mainnet-2026-11-06`, [CI run 42192602515](https://github.com/NilaDAO/nila-soroban/actions/runs/42192602515). Soroban contracts: Ashadipta Pati, Rust/Soroban engineer (Mumbai), built and verified all nine contracts deployed to mainnet, starting from his pre-award port of fund_core, fund_viewer, roles and membership to testnet (24 tests); Carst Abma on architecture. nila-contracts is the Polygon code only.
5. **Public proof:** every transaction in the work record links to stellar.expert or Polygonscan. Team: Carst Abma (Amsterdam, architecture and engineering), Anand Pandurangan (Chennai, India operations), Ashadipta Pati (Mumbai, Soroban contracts). All code is committed from the shared NilaDAO account: the team builds with Claude Code agents, so commits are not split per person.

Text licensed [CC BY 4.0](LICENSE).
