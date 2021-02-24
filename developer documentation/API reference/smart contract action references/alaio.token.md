# Token

**Type:** class

The `alaio.token` sample system contract defines the structures and actions that allow users to create, issue, and manage tokens for ALAIO based blockchains. It demonstrates one way to implement a smart contract which allows for creation and management of tokens. It is possible for one to create a similar contract which suits different needs. However, it is recommended that if one only needs a token with the below listed actions, that one uses the `alaio.token` contract instead of developing their own.

The `alaio.token` contract class also creates two useful public static methods: `get_supply` and `get_balance`. The first permits one to check the total supply of a specified token, created by an account and the second greants one to check the balance of a token for a defined account (the token creator account has to be defined as well).

The `alaio.token` contract manages the set of tokens, accounts and their corresponding balances, by using two internal multi-index structures: the `accounts` and `stats`. The `accounts` multi-index table holds, for each row, instances of account object and the account object holds information about the balance of one token. The accounts table is scoped to an alaio account, and it keeps the rows indexed based on the token's symbol. This means that when one queries the accounts multi-index table for an account name the result is all the tokens that account holds at the moment.

comparably, the stats multi-index table, controls instances of `currency_stats` objects for all rows, which holds information about both current and maximum supply as well as the creator account for a symbol token. The `stats` table is scroped to the token symbol, therefore, when one queries the `stats` table for a token symbol the outcome is a singular row corresponding to the queried symbol token if it was previously generated, or nothing, otherwise. 

## create

**Type:** void

Permits `issuer` account to create a token in supply of `maximum_supply`. If validation is granted a new entry in statstable for token symbol scope gets generated.

Parameter Name | Description
--- | ---
issuer | - the account that creates the token,
maximum_supply | - the maximum supply set for the token created.

## issue

**Type:** void

This action issues to `to` account a `quantity` of tokens.

Parameter Name | Description
--- | ---
to | - the account to issue tokens to, it must be the same as the issuer,
quantity | - the amount of tokens to be issued,
## retire

**Type:** void

The counter for create action, if every validation is approved, it debits the statstable.supply amount.

Parameter Name | Description
--- | ---
quantity | - the quantity of tokens to retire,
memo | - the memo string to accompany the transaction.

## transfer

**Type:** void

Permits from account to shift to an account the quantity tokens. The account is debited and the other is attributed with quantity tokens.

Parameter Name | Description
--- | ---
from | - the account to transfer from,
to | - the account to be transferred to,
quantity | - the quantity of tokens to be transferred,
memo | - the memo string to accompany the transaction.

## open

**Type:** void

Permits ram_player to generate an account owner with no balance for token symbol at the expense of ram_player.

Parameter Name | Description
--- | ---
owner | - the account to be created,
symbol | - the token to be payed with by ram_payer,
ram_payer | - the account that supports the cost of this action. More information can be read here and here.

## close

**Type:** void

Close is an action that is opposite for open, it closes the account owner for token symbol.

Parameter Name | Description
--- | ---
owner | - the owner account to execute the close action for,
symbol | - the symbol of the token to execute the close action for.
