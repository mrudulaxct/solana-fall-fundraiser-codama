## Versions

anchor-cli 1.1.2
solana-cli 4.2.1
node v24.15.0
@codama/cli 1.6.3

## TODO 3

Required: `fundraiser`, `vault`.

Optional: `contributorAccount`, `contributorAta`, `tokenProgram`, `systemProgram`.

Codama can derive `contributorAccount` because its seeds use the fundraiser address and contributor address that the async input already has. It can derive `contributorAta` from the contributor, mint, associated token program, and token program, and it can fill the token and system programs from constants.

Codama cannot derive `fundraiser` in `contribute` because that PDA is seeded with `fundraiser.maker`, a field inside the fundraiser account itself. That means the client would need the fundraiser account before it could find the fundraiser account. `initialize` is different because its fundraiser PDA is seeded from the explicit `maker` account, so the generated initialize builder can derive it.

`vault` is also required in `contribute` because it is the ATA for the fundraiser PDA, and the generated async builder does not resolve that vault account from the already-required fundraiser plus mint for this instruction.

## Bonus

Attempted and passing. The bonus test builds the contribution instruction with Codama, converts it to a web3.js instruction with `tests/helpers/kit-adapter.ts`, sends it through the Anchor provider, and checks that the vault grew by exactly one contribution.

## One thing that surprised me

The generated async builder can only resolve accounts from data already present in its input or from constants in the IDL. A PDA can still be impossible to derive automatically if one of its seeds lives inside the very account whose address you are trying to find.
