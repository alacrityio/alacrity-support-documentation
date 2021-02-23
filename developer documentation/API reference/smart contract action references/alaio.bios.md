# Bios

**Type:** class

Alaio.bios is the first example of system contract provided by Alacrity through the ALAIO platform. It only supplies the actions that are absolutely essential to boostrap a chain and nothing more thus making it a minimalist system contract. This allows for a chain agnostic approach to bootstrapping a chain.

Similar to the `alaio.system` sample contract implementation, There are a few actions which are not incorporated at the contract level (`newaccount`, `updateauth`, `deleteauth`, `linkauth`, `unlinkauth`, `canceldelay`, `onerror`, `setabi`, `setcode`), they are just declared in the contract so they will show in the contract's ABI and users are allowed to push those actions to the chain via the account holding the `alaio.system` contract, but the incorporation is a the ALAIO core level. ALAIO native actions is what they are referred to as.

## newaccount

**Type:** void

New account is an action that is called after a new account is created. This code enforces resource-limits rules for new accounts as well as new account naming conventions.

* accounts cannot contain '.' symbols which forces all acccounts to be 12 characters long without '.' until a future account auction process is implemented which prevents name squatting.
* new accounts must stake a minimal number of tokens (as set in system parameters) therefore, this method will execute an inline buyram from receiver for newacnt in an amount equal to the current new account creation fee.

## updateauth

**Type:** void

Update authorization is an action that updates permission for an account.

Parameter Name | Description
--- | ---
account | - the account for which the permission is updated,
pemission | - the permission name which is updated,
parem | - the parent of the permission which is updated,
aut | - the json describing the permission authorization.

## deleteauth

**Type:** void

Delete authorization is an action that removes the authorization for an account's permission.

Parameter Name | Description
account | - the account for which the permission authorization is deleted,
permission | - the permission name been deleted.

## linkauth

**Type:** void

Link authorization is an action that assigns a specific action from a contract to a permission you've created. Five system actions can not be linked `updateauth`, `deleteauth`, `linkauth`, `unlinkauth`, and `canceldelay`. When doing Authorization checks, the ALAIO based blockchain starts with the action needed to be approved (and the contract belonging to), and looks up which permission is needed to pass authorization validation, which makes this action useful. If a link is set, that permission is used for authorization validation otherwise then active is the default, with the exception of `alaio.any`. `alaio.any` is an implicit permission which exists on every account; you can link actions to `alaio.any` and that will make it so linked actions are available to any permission defined for the account.

Parameter Name | Description
--- | ---
account | - the permission's owner to be linked and the payer of the RAM needed to store this link,
code | - the owner of the action to be linked,
type | - the action to be linked,
requirement | - the permission to be linked.

## unlinkauth

**Type:** void

Unlink authorization action it's doing the reverse of linkauth action, by unlinking the given action.

Parameter Name | Description
--- | ---
account | - the owner of the permission to be unlinked and the receiver of the freed RAM,
code | - the owner of the action to be unlinked,
type | - the action to be unlinked.

## canceldelay

**Type:** void

Cancel delay action cancels a deferred transaction.

Parameter Name | Description
--- | ---
canceling_auth | - the permission that authorizes this action,
trx_id | - the deferred transaction id to be cancelled.

## setcode

**Type:** void

Set code is an action that sets the contract code for an account.

Parameter Name | Description
--- | ---
account | - the account for which to set the contract code.
vmtype | - reserved, set it to zero.
vmversion | - reserved, set it to zero.
code | - the code content to be set, in the form of a blob binary..

## setabi

**Type:** void

Set ABI is an action that sets the ABI for contract identified by `account` name. It generates an entry in the abi_has_table index, with `account` name as key, if it is not already present and defines its value with the ABI hash. Otherwise it is updating the current ABI hash value for the existing `account` key.

Parameter Name | Description
--- | ---
account | - the name of the account to set the abi for
abi | - the abi hash represented as a vector of characters

## onerror

**Type:** void

On error action, notification of this action is delivered to the sender of a deferred transaction when an objective error occurs while executing the deferred transaction. This action is not meant to be called directly.

Parameter Name | Description
--- | ---
sender_id | - the id for the deferred transaction chosen by the sender,
sent_trx | - the deferred transaction that failed.

## setpriv

**Type:** void

Set privilege is an action that give the ability to set privilige status for an account (turns on/off)

Parameter Name | Description
--- | ---
account | - the account to set the privileged status for.
is_priv | - 0 for false, > 0 for true.

## setalimits

**Type:** void

Sets the resource limits of an account

Parameter Name | Description
--- | ---
account | - name of the account whose resource limit to be set
ram_bytes | - ram limit in absolute bytes
net_weight | - fractionally proportionate net limit of available resources based on (weight / total_weight_of_all_accounts)
cpu_weight | - fractionally proportionate cpu limit of available resources based on (weight / total_weight_of_all_accounts)

## setprods

**Type:** void

Set producers is an action that sets a new list of enabled producers, by suggesting a schedule change, once the block that contains the proposal cannot change, the schedule is promoted to "pending" automatically. Once the block that promotes the schedule is irreversible, the schedule will become "active"

Parameter Name | Description
--- | ---
schedule | - New list of active producers to set

## setparams

**Type:** void

Set params is an action that sets the blockchain parameters. by defining these parameters, multiple degrees of customization can be achieved.

Parameter Name | Description
--- | ---
params | - New blockchain parameters to set

## reqauth

**Type:** void

Require authorization is an action that checks if the account name `from` passed in as param has authorization to access current action if it is listed in the actions allowed permissions vector.

Parameter Name | Description
--- | ---
from | - the account name to authorize

## activate

**Type:** void

Activate is an action that enables a protocol feature.

Parameter Name | Description
--- | ---
feature_digest | - hash of the protocol feature to activate.

## reqactivated

**Type:** void

Require activated is an action that declares that a protocol feature has been activated.

Parameter Name | Description
--- | ---
feature_digest | - hash of the protocol feature to check for activation.
