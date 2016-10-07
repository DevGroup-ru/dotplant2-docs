# The development of payment systems for DotPlant2

As (PS) generally, modules payment systems are built on the same principle:

> On the side of the online store (MI) and the payment system server (CAS) is a secret key by which generates a verification code (PC).

When making a reservation the client store (IM), IM receives data, such as:

1. Order Id
2. The positions of the goods
3. The total amount of the order
4. Delivery Amount
5. Link to the page, if the payment is successful
6. Link to the page, if the payment is not passed
7. Page to check the payment status (CheckResult method)
8. Id transaction

(Mandatory data differ from the SPS)

By processing the data on the side of MI with a secret klyuem formed PC, which, together with the data transmitted to the ATP.

If the PCA processing data received from them to receive the same PC, the client is given the opportunity to pay for your order.
After taking actions KM (payment, refusal, etc.) SPS sends a new PC and payment data to MI way.

MI checks the correctness of the PC and changes the status of the order.

### DotPlant2 allows you to add new payment system in 2 steps:

1. Create a payment system class, which is a subclass of `app\components\payment\AbstractPayment` and overriding the 2 methods

    * content — called at the last stage of the basket, in this method you need to implement the ability to move the user to the SPS payment page. Most often at this stage it is necessary to form a PC and redirect pass it along with the data on the order page SPS handler.

    * CheckResult — here comes the data from the SPS on the result of the payment of the order of clients. Usually come payment data and a new PC. The method should be to implement checks come from the PC server and PC data generated from the payment. If the check came and the order must be paid in the transaction get the id of the data received from the ATP and change the data transaction.
    * 
```php
    $transaction->result_data = Json::encode($arrParams);
    $transaction->status = OrderTransaction::TRANSACTION_SUCCESS;
    $transaction->save(true, ['status', 'result_data']);
```

2. Create a migration to add a payment system.
    Example migration
```php
public function up()
{
    $this->insert(
        \app\models\PaymentType::tableName(),
        [
            'name' => 'Platron',
            'class' => 'app\components\payment\PlatronPayment',
            'params' => \yii\helpers\Json::encode(
                [
                    'merchantId' => '',
                    'secretKey' => '',
                    'strCurrency' => 'RUR',
                    'merchantUrl' => 'www.platron.ru',
                    'merchantScriptName' => 'payment.php'
                ]
            ),
        ]
    );
}
```
    
    where :
    * name - the name of the payment system
    * class - the class path of the payment system
    * params - the data in JSON, which will be assigned to the SS-class properties.