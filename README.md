# PHP class for interaction with BeFrank Simplewallet API

Simple php class for interaction with [BeFrank Simplewallet JSON RPC API](https://github.com/befrank-project/befrank).

## Available methods ##
* getBalance
* getAddress
* getHeight
* getTransfers
* getPayments
* transfer
* store
* reset
* genPaymentId
* checkAddress
* checkPaymentId

### getBalance ###

Return wallet balance.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->getBalance();
?>
```
###### Output data: ######
```
{
	"status": true,
	"available_balance": 100,
	"locked_amount": 0
}
```

### getAddress ###

Returns wallet address.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->getAddress();
?>
```
###### Output data: ######
```
{
	"status": true,
	"address": "FctbUcra6R4DLHmDYbVvfrDBLQHxpjN2xVKs9EcmsDivXVWiu15uuF2YKgZfMPEYgT5dS4JGuJERmY46AwEVehwq35acL8f "
}
```

### getHeight ###

Returns the last top known block height for simplewallet. This method can be used to verify that simplewallet is correctly synchronized.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->getHeight();
?>
```
###### Output data: ######
```
{
	"status": true,
	"height": 201631
}
```

### getTransfers ###

Returns the list of all the wallet's incoming and outgoing transfers.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->getTransfers();
?>
```
###### Output data: ######
```
{
	"status": true,
	"transfers": [{
		"address": "",
		"amount": 1,
		"fee": 0.0001,
		"blockIndex": 200561,
		"output": false,
		"paymentId": "b661ca369901e91f51083bb131ad26c5c04d725bc0dfe3692e63fc4e093d331e",
		"time": 1518824088,
		"transactionHash": "5d67329ce94e8a127b1490b281feb13f74c18d0ac1dbe49338c1663c34a27738",
		"unlock_time": 0
	}, {
		"address": "FeRRSeu1z1PYQz9eTWy8W4efGTMB9ZCbxayX85rRr5w8E1gqDMujgtPLeGPmvcT1DPjoU7iCpbrn7KJxMpJhUTfrJjcKGpt",
		"amount": 1,
		"fee": 0.0001,
		"blockIndex": 201589,
		"output": true,
		"paymentId": "17d37b8d5a76da4e5e0d16459b386601e0a38eac80956f2d1abfeab4dda715a7",
		"time": 1519096397,
		"transactionHash": "db56c8f6bf45e37f4bfd1d0201d4fbe5e2514676e30018116b2491f8c71a9230",
		"unlock_time": 0
	}]
}
```

### getPayments ###

Receives all the payments with a corresponding payment_id that were sent to the wallet. This method is used to get the KRB payments for the 3rd party services. As Befrank uses only one address to receive KRB deposits, a unique payment_id should be assigned and shown to each user. The method will return all the payments for this user.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$paymentId = "b661ca369901e91f51083bb131ad26c5c04d725bc0dfe3692e63fc4e093d331e";
	$bfrWallet->getPayments($paymentId);
?>
```
###### Output data: ######
```
{
	"status": true,
	"payments": [{
		"amount": 1,
		"block_height": 200561,
		"tx_hash": "5d67329ce94e8a127b1490b281feb13f74c18d0ac1dbe49338c1663c34a27738",
		"unlock_time": 0
	}]
}
```

### transfer ###

Transfer KRB to several destinations with specified fee, mixin ambiguity degree, and unlock time.

Please note: fee param is a mandatory and should not be less than 0.0001 KRB.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$paymentID = $bfrWallet->genPaymentId();
	$fee = $bfrWallet->genPaymentId();
	$unlock_time = 0;
	$transData = [
		[
			"amount" => "100",
			"address" => "FeRRSeu1z1PYQz9eTWy8W4efGTMB9ZCbxayX85rRr5w8E1gqDMujgtPLeGPmvcT1DPjoU7iCpbrn7KJxMpJhUTfrJjcKGpt"
		]
	];
	$bfrWallet->transfer($transData, $paymentID, $fee, $unlock_time);
?>
```
###### Output data: ######
```
{
	"status": true,
	"payments": [{
		"amount": 1,
		"tx_hash": "5d67329ce94e8a127b1490b281feb13f74c18d0ac1dbe49338c1663c34a27738",
		"payment_id": "17d37b8d5a76da4e5e0d16459b386601e0a38eac80956f2d1abfeab4dda715a7"
	}]
}
```

### store ###

Store wallet data.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->store();
?>
```
###### Output data: ######
```
{
	"status": true
}
```

### reset ###

Erases simplewallet's internal state but keeps safe the wallet.bin. The method should be used to re-synchronize the wallet from scratch. The next refresh (which is automatically called each 20 seconds) will update the simplewallet state.

###### Example of usage: ######
```
<?php
	$bfrWallet = new Befrank();
	$balance = $bfrWallet->reset();
?>
```
###### Output data: ######
```
{
	"status": true
}
```

### genPaymentId ###

Generate payment id

###### Example of usage: ######
```
<?php
	Befrank::genPaymentId();
?>
```
###### Output data: ######
```
66897070238bc2c47bed4a327f78bed3ece3b87c30b1cb78c535597fd0c19456
```

### checkAddress ###

Check if address valid

###### Example of usage: ######
```
<?php
	Befrank::checkAddress("FeRRSeu1z1PYQz9eTWy8W4efGTMB9ZCbxayX85rRr5w8E1gqDMujgtPLeGPmvcT1DPjoU7iCpbrn7KJxMpJhUTfrJjcKGpt");
?>
```
###### Output data: ######
```
true
```

### checkPaymentId ###

Check if payment id valid

###### Example of usage: ######
```
<?php
	Befrank::checkPaymentId("66897070238bc2c47bed4a327f78bed3ece3b87c30b1cb78c535597fd0c19456");
?>
```
###### Output data: ######
```
true
```

Based on [Lastick/Karbo-api-php](https://github.com/seredat/Befrankwanec/wiki/Simplewallet-JSON-RPC-API) and [Simplewallet JSON RPC API wiki page](https://github.com/seredat/Befrankwanec/wiki/Simplewallet-JSON-RPC-API)