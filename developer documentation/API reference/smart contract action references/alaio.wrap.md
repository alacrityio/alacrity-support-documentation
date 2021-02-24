# Wrap

**Type:** class

The `alaio.wrap` system contract permits block producers to go around authorization checks or run privileged actions with 15/21 producer approval and thus clarifies block producers superuser actions. It as well makes these actions simpler to audit.

No block producers are given any additional powers or privileges that do not already exist within the ALAIO based blockchains. As it is passed , in an ALAIO based blockchain, 15/21 block producers can alter an accounts permissions or change  an accounts contract code if they deemed it is beneficial for the blockchain and community. On the other hand, the current method is opaque and leaves undesirable side effects on specific system accounts, and thus the `alaio.wrap` contract solves this matter by providing an easier method of executing important governance actions.

The `alaio.wrap` system contract only has one action implemented, which is the `exec` action. This actions permits for an execution of a transaction, which is carried over to the exec method in the form of a loaded transaction in json format via the 'trx' parameter and the `executer` account that executes the transaction. The `executer` account will also be used to pay the RAM and CPU fees needed to execute the transaction.

## exec

**Type:** void

An action that implements a transaction while bypassing regular authorization checks.

Preconditions:

* Requires authorization of alaio.wrap which needs to be a privileged account.

Postconditions:

* Deferred transaction RAM usage is billed to 'executer'

Parameter Name | Description
--- | ---
executer | - account executing the transaction,
trx | - the transaction to be executed.
