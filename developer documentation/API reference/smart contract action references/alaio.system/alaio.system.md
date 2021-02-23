# ALAIO.system

Alacrity provides the alaio.system smart contract as a sample system contract, this defines the structures and actions required for blockchain's core functinonality.

Similar to the alaio.bios sample contract implementation, a few actions are not implemented at the contract level (newaccount, updateauth, deletauth, linkauth, unlinkauth, canceldelay, onerror, setabi, setcode), they are simply proclaimed in the contract so they will show in the contract's ABI and users will be able to push those actions the the chain by the account holding the alaio.system contract, but the implementation is at the ALAIO core level. ALAIO native actions is what they are referred to as. 

* Users can stake tokens for CPU and Network bandwidth, and then vote for producers or delegate their vote to a proxy.
* Producers register in order to be voted for, and can claim per-block and per-vote rewards.
* Users can buy and sell RAM at a market-determined price.
* Users can bid on premium names.
* A resource exchange system (REX) allows token holders to lend their tokens, and users to rent CPU and Network resources in return for a market-determined fee.

## system

**Type:** `class`

The ALAIO system contract. It governs ram market, voters, producers, and global state.

## init

**Type:** `void`

The Init actions boots the system contract for a version and a symbol. It is only approved when:

* version is 0 and
* symbol is found and
* system token supply is greater than 0,
* and system contract wasn't already been initialized.

Parameter Name | Description
--- | ---
version | - the version, has to be 0,
core | - the system symbol.

## onblock

**Type:** void

On block action is a special action that is triggered when a block is applied by the given producer and can't be generated from any other source. It's used to pay producers and calculate missed blocks of other producers. Their pay is deposited into the producer's stake balance and over time can be withdrawn. If Blocknum is the start of a new round, this may update the active producer config from the producer votes.

Parameter Name | Description
--- | ---
header | - the block header produced.

## setalimits

**Type:** void

Set account limits action sets the resource limits of an account

Parameter Name | Description
--- | ---
account | - name of the account whose resource limit to be set,
ram_bytes | - ram limit in absolute bytes,
net_weight | - fractionally proportionate net limit of available resources based on (weight / total_weight_of_all_accounts),
cpu_weight | - fractionally proportionate cpu limit of available resources based on (weight / total_weight_of_all_accounts).

## setacctram

**Type:** void

Set account RAM limits action, which sets the RAM limits of an account

Parameter Name | Description
--- | ---
account | - name of the account whose resource limit to be set,
ram_bytes | - ram limit in absolute bytes.

## setacctnet

**Type:** void

Set account NET limits action, which sets the NET limits of an account

Parameter Name | Description
--- | ---
account | - name of the account whose resource limit to be set,
net_weight | - fractionally proportionate net limit of available resources based on (weight / total_weight_of_all_accounts).

### setacctcpu

**Type:** void

Set account CPU limits action, which sets the CPU limits of an account

Parameter Name | Description
--- | ---
account | - name of the account whose resource limit to be set,
cpu_weight | - fractionally proportionate cpu limit of available resources based on (weight / total_weight_of_all_accounts).

## activate

**Type:** void

The activate action, activates a protocol feature

Parameter Name | Description
--- | ---
feature_digest | - hash of the protocol feature to activate.

## delegatebw

**Type:** void

Delegate bandwidth and/or cpu action. Stakes SYS from the balance of from for the benefit of receiver.

Parameter Name | Description
--- | ---
from | - the account to delegate bandwidth from, that is, the account holding tokens to be staked,
receiver | - the account to delegate bandwith to, that is, the account to whose resources staked tokens are added
stake_net_quantity | - tokens staked for NET bandwidth,
stake_cpu_quantity | - tokens staked for CPU bandwidth,
transfer | - if true, ownership of staked tokens is transfered to receiver.

## setrex

**Type:** void

Setrex action, sets total_rent balance of REX pool to the passed value.

Parameter Name | Description
--- | ---
balance | - amount to set the REX pool balance.

## deposit

**Type:** void

Deposit to REX fund action. Deposits core tokens to user REX fund. Any proceeds and expenses in relation to REX are added to or Taken from this fund. An inline transfer from 'owner' liquid balance is initiated. All REX-related expenses and proceeds are deducted from and added to 'owner' REX fund, with one exception being buying REX using staked tokens. Storage change is billed to 'owner'.

Parameter Name | Description
--- | ---
owner | - REX fund owner account,
amount | - amount of tokens to be deposited.

## withdraw

**Type:** void

Withdraw from REX fund action, withdraws core tokens from user REX fund. An inline token transfer to user balance is executed.

Parameter Name | Description
--- | ---
owner | - REX fund owner account,
amount | - amount of tokens to be withdrawn.

## buyrex

**Type:** void

Buyrex is an action that buys REX in exchange for tokens taken out of user's REX fund by transferring core tokens from user REX fund and converts them to REX stake. By purchasing REX, the user is lending tokens in order to be loaned as CPU or NET resources. Storage change is charged to 'from' account. 

Parameter Name | Description
--- | ---
from | - owner account name,
amount | - amount of tokens taken out of 'from' REX fund.

## unstaketorex

**Type:** void

Unstaketorex action, uses staked core tokens to buy REX. Storage change is billed to 'owner' account.

Parameter Name | Description
--- | ---
owner | - owner of staked tokens,
receiver | - account name that tokens have previously been staked to,
from_net | - amount of tokens to be unstaked from NET bandwidth and used for REX purchase,
from_cpu | - amount of tokens to be unstaked from CPU bandwidth and used for REX purchase.

## sellrex

**Type:** void

Sellrex action, sells REX in exchange for core tokens by converting REX stake back into core tokens at current exchance rate. If the order is unable to be processed, it's queued until there is enough in the REX pool to complete an order, and will be processed for a maximum of 30 days. If it is successful, user votes are updated, that, proceeds are deducted from user's voting power. In case a sell order is queued, storage change is billed to 'from' account. 

Parameter Name | Description
--- | ---
from | - owner account of REX,
rex | - amount of REX to be sold.

## cnclrexorder

**Type:** void

Cnclrexorder action, cancels unfilled REX sell order by owner if one exists.

Parameter Name | Description
--- | ---
owner | - owner account name.
rentcpu

**Type:** void

Rentcpu is an action that uses payment to rent as many SYS tokens as possible as determind by market price and then stake them for CPU for the benefit of receiver, after 30 days the rented core delegation of CPU will expire. At expiration, if balance is greater than or equal to loan_payment is taken out of loan balance and used for renewing the loan. Or else, the loan is concluded and the user is refunded any balance remaining. The owner can fund or refund a loan any time before it expires. All loan expenses and refunds come out of or are added to the owners REX fund.

Parameter Name | Description
--- | ---
from | - account creating and paying for CPU loan, 'from' account can add tokens to loan balance using action fundcpuloan and withdraw from loan balance using defcpuloan
receiver | - account receiving rented CPU resources,
loan_payment | - tokens paid for the loan, it has to be greater than zero, amount of rented resources is calculated from loan_payment,
loan_fund | - additional tokens can be zero, and is added to loan balance. Loan balance represents a reserve that is used at expiration for automatic loan renewal.

## rentnet

**Type:** void

Rentnet is an action that uses payment to rent as many SYS tokens as possible as determind by market price and stakes them for NET for the benefit of reciever, after 30 days the rented core delegation of NET will expire. At expiration, if the balance is greater than or equal to loan_payment, loan_payment is taken out of loan balance and used to renew the loan. Otherwise, the loan is canceled and the user is refunded any remaining balance. Owner can fund or refund a loan at any time before it expires. All loan expenses and refundes come out of or are added to owner's REX fund.

Parameter Name | Description
--- | ---
from | - account creating and paying for NET loan, 'from' account can add tokens to loan balance using action fundnetloan and withdraw from loan balance using defnetloan,
receiver | - account receiving rented NET resources,
loan_payment | - tokens paid for the loan, it has to be greater than zero, amount of rented resources is calculated from loan_payment,
loan_fund | - additional tokens can be zero, and is added to loan balance. Loan balance represents a reserve that is used at expiration for automatic loan renewal.

## fundcpuloan

**Type:** void

Fundcpuloan action, transfers tokens from REX fund to the fund of a specific CPU loan in order to be used for loan renewal at expiry.

Parameter Name | Description
--- | ---
from | - loan creator account,
loan_num | - loan id,
payment | - tokens transfered from REX fund to loan fund.

## fundnetloan

**Type:** void

Fundnetloan action, transfers tokens from REX fund to the fund of a specific NET loan in order to be used for loan renewal at expiry.

Parameter Name | Description
--- | ---
from | - loan creator account,
loan_num | - loan id,
payment | - tokens transfered from REX fund to loan fund.

## defcpuloan

**Type:** void

Defcpuloan action, withdraws tokens from the fund of a specific CPU loan and adds them to REX fund.

Parameter Name | Description
--- | ---
from | - loan creator account,
loan_num | - loan id,
amount | - tokens transfered from CPU loan fund to REX fund.

## defnetloan

**Type:** void

Defnetloan action, withdraws tokens from the fund of a specific NET loan and adds them to REX fund.

Parameter Name | Description
--- | ---
from | - loan creator account,
loan_num | - loan id,
amount | - tokens transfered from NET loan fund to REX fund.

## updaterex

**Type:** void

Updaterex action, updates REX owner vote weight to current value of held REX tokens.

Parameter Name | Description
--- | ---
owner | - REX owner account.

## rexexec

**Type:** void

Rexexec is an action that processes max CPU loans, max NET loans, and max queued sellrex orders as well. The action does not execute anything related to a specific user.

Parameter Name | Description
--- | ---
user | - any account can execute this action,
max | - number of each of CPU loans, NET loans, and sell orders to be processed.

## consolidate

**Type:** void

Consolidate is an action that combine REX maturity buckets into one bucket that can be sold after four days starting from the end of the day.

Parameter Name | Description
--- | ---
owner | - REX owner account name.

## mvtosavings

**Type:** void

Mvtosavings is an action that moves a specic amount of REX into the savings bucket. REX savings bucket never matures. In order for it to be sold, it has to be moved decidely out of that bucket. Then the moved amount will have the regular maturity period of four days starting from the end of the day.

Parameter Name | Description
--- | ---
owner | - REX owner account name.
rex | - amount of REX to be moved.

## mvfrsavings

**Type:** void

Mvfrsavings is an action that moves a specific amount of REX out of savings bucket. The amount moved will have the regular REX maturity period of four days.

Parameter Name | Description
--- | ---
owner | - REX owner account name.
rex | - amount of REX to be moved.

## closerex

**Type:** void

Closerex is an action that removes owner records from REX tables and frees up used RAM. The owner is required to not have an outstanding REX balance.

owner REX balance entry is deleted. REX fund entry is deleted.

Parameter Name | Description
--- | ---
owner | - user account name.

## undelegatebw

**Type:** void

Undelegate bandwitdh action, decreases the total tokens delegated by from to receiver and/or frees the memory associated with the delegation if there is nothing left to delegate. This will cause an immediate reduction in net/cpu bandwidth of the receiver. A transaction is scheduled to send the tokens back to from after the staking period has passed. If existing transaction is scheduled, it will be canceled and a new transaction issued that has the combined undelegated amount. The from account loses voting power as a result of this call and all producer tallies are updated.

deferred transaction with a delay of 3 days. action, pending action is canceled and timer is reset.

Parameter Name | Description
--- | ---
from | - the account to undelegate bandwidth from, that is, the account whose tokens will be unstaked,
receiver | - the account to undelegate bandwith to, that is, the account to whose benefit tokens have been staked,
unstake_net_quantity | - tokens to be unstaked from NET bandwidth,
unstake_cpu_quantity | - tokens to be unstaked from CPU bandwidth,

## buyram

**Type:** void

Buy ram action, increases receiver's ram quota based upon current price and quantity of tokens provided. An inline transfer from receiver to system contract of tokens will be executed.

Parameter Name | Description
--- | ---
payer | - the ram buyer,
receiver | - the ram receiver,
quant | - the quntity of tokens to buy ram with.

## buyrambytes

**Type:** void

Buy a specific amount of ram bytes action. Increases receiver's ram in quantity of bytes provided. An inline transfer from receiver to system contract of tokens will be executed.

Parameter Name | Description
--- | ---
payer | - the ram buyer,
receiver | - the ram receiver,
bytes | - the quntity of ram to buy specified in bytes.

## sellram

**Type:** void

Sell ram is an action that reduces quota by bytes and then performs an inline transfer of tokens to reciever based up the average purchase price of the original quota.

Parameter Name | Description
--- | ---
account | - the ram seller account,
bytes | - the amount of ram to sell in bytes.

## refund

**Type:** void

Refund is an action that is called after the delegation-period to claim all pending unstaked tokens that belong to the owner.

Parameter Name | Description
--- | ---
owner | - the owner of the tokens claimed.

## regproducer

**Type:** void

Register producer is an action that indicates a particular account that wishes to become a producer, this action will generate a producer_config and a producer_info object for producer scope in producers tables.

Parameter Name | Description
--- | ---
producer | - account registering to be a producer candidate,
producer_key | - the public key of the block producer, this is the key used by block producer to sign blocks,
url | - the url of the block producer, normally the url of the block producer presentation website,
location | - is the country code as defined in the ISO 3166, https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes

## regproducer2

**Type:** void

Register producer action, indicates that a particular account wishes to become a producer, this action will create a producer_config and a producer_info object for producer scope in producers tables.

Parameter Name | Description
--- | ---
producer | - account registering to be a producer candidate,
producer_authority | - the weighted threshold multisig block signing authority of the block producer used to sign blocks,
url | - the url of the block producer, normally the url of the block producer presentation website,
location | - is the country code as defined in the ISO 3166, https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes

## unregprod

**Type:** void

Unregister producer action, deactivates the block producer with account name producer.

Deactivate the block producer with account name producer.

Parameter Name | Description
--- | ---
producer | - the block producer account to unregister.

## setram

**Type:** void

Set ram action sets the ram supply.

Parameter Name | Description
--- | ---
max_ram_size | - the amount of ram supply to set.

## setramrate

**Type:** void

Set ram rate is an action that sets the rate of increase of RAM in bytes per block. It's capped by the uint16_t to a maximum rate of 3 TB per year. If update_ram_supply has not been called for the most recent block, then new ram will be assigned at the old rate up to the present block before switching the rate

Parameter Name | Description
--- | ---
bytes_per_block | - the amount of bytes per block increase to set.

## voteproducer

**Type:** void

Vote producer is an action that votes for a set of producers. This action updates the list of producers voted for which is for voter account. If voting for a proxy, the producer votes will not change until the proxy updates their own vote. the voter can vote for a proxy or a list of at more thirty producers. Storage change is charged to the voter.

Parameter Name | Description
--- | ---
voter | - the account to change the voted producers for,
proxy | - the proxy to change the voted producers for,
producers | - the list of producers to vote for, a maximum of 30 producers is allowed.

## regproxy

**Type:** void

Register proxy is an action that sets 'proxy account' as proxy. An account marked as proxy can vote with the weight of other accounts which have selected it as a proxy. Other accounts are required to refresh their voteproducter to update the proxy's weight. Storage change is billed to proxy.

Parameter Name | Description
--- | ---
rpoxy | - the account registering as voter proxy (or unregistering),
isproxy | - if true, proxy is registered; if false, proxy is unregistered.

## setparams

**Type:** void

Set the blockchain parameters. with adjuting these parameters a degree of customization can be achieved.

Parameter Name | Description
--- | ---
params | - New blockchain parameters to set.

## claimrewards

**Type:** void

Claim rewards is an action that claims block producing and vote rewards.

Parameter Name | Description
--- | ---
owner | - producer account claiming per-block and per-vote rewards.

## setpriv

**Type:** void

Allows to set privilige status for an account (turn on/off).

Parameter Name | Description
--- | ---
account | - the account to set the privileged status for.
is_priv | - 0 for false, > 0 for true.

## rmvproducer

**Type:** void

Remove producer is an action that deactivates a producer by name if not found asserts.

Parameter Name | Description
--- | ---
producer | - the producer account to deactivate.

## updtrevision

**Type:** void

Update revision action, updates the current revision. than or equal 1 (“set upper bound to greatest revision supported in the code”).

Parameter Name | Description
--- | ---
revision | - it has to be incremented by 1 compared with current revision.

## bidname

**Type:** void

Bid name is an action that allows an account bidder to place a big for a name newname.

Parameter Name | Description
--- | ---
bidder | - the account placing the bid,
newname | - the name the bid is placed for,
bid | - the amount of system tokens payed for the bid.

## bidrefund

**Type:** void

Bid refund is an action that allows the account bidder to refund the amount it bid so far on a newname name.

Parameter Name | Description
--- | ---
bidder | - the account that gets refunded,
newname | - the name for which the bid was placed and now it gets refunded for.

## setinflation

**Type:** void

Changes the annual inflation rate of the core token supply and clarify how the new issues of tokens will be disbursed based on the following structure.

Parameter Name | Description
--- | ---
annual_rate | - Annual inflation rate of the core token supply. (eg. For 5% Annual inflation => annual_rate=500 For 1.5% Annual inflation => annual_rate=150
inflation_pay_factor | - Inverse of the fraction of the inflation used to reward block producers. The remaining inflation will be sent to the alaio.saving account. (eg. For 20% of inflation going to block producer rewards => inflation_pay_factor = 50000 For 100% of inflation going to block producer rewards => inflation_pay_factor = 10000).
votepay_factor | - Inverse of the fraction of the block producer rewards to be distributed proportional to blocks produced. The remaining rewards will be distributed proportional to votes received. (eg. For 25% of block producer rewards going towards block pay => votepay_factor = 40000 For 75% of block producer rewards going towards block pay => votepay_factor = 13333).
