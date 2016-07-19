# The implementation class currency exchange provider

Class provider must implement the method of [[Swap\ProviderInterface::fetchRate()]]. `fetchRate` method takes as a parameter an instance of [[Swap\Model\CurrencyPair]], which describes the currency pair. The provider must return an instance of [[Swap\Model\Rate]], which describes the pair exchange rate with reference to the time.

Available methods of the class [[Swap\Model\CurrencyPair]]:

* __construct(baseCurrency, quoteCurrency) - конструктор, constructor parameters and name of the base currency of the secondary respectively in ISO 4217 format
* createFromString(string) - a static method, which creates a currency pair takes a string in the EUR/USD format
* getBaseCurrency() - method returns the main ISO 4217 currency code
* getQuoteCurrency() - method returns the ISO 4217 currency code of the secondary
* toString() - method returns the ISO 4217 currency codes in the EUR / USD format

An example of the implementation of the provider class, as a provider of currency exchange rate is the Central Bank of the Russian Federation:

```php
use Swap\Model\CurrencyPair;
use Swap\Model\Rate;
use Swap\Provider\AbstractProvider;

class CbrFinanceProvider extends AbstractProvider
{
    const URL = 'http://www.cbr.ru/scripts/XML_daily.asp?date_req=%s';

    public function fetchRate(CurrencyPair $currencyPair)
    {
        $date = date("d/m/Y");

        $url = sprintf(self::URL, $date);

        $content = $this->httpAdapter->get($url)->getBody()->getContents();
        $cbr = new \SimpleXMLElement($content);

        $res = $cbr->xpath('/ValCurs/Valute[CharCode="' . $currencyPair->getBaseCurrency() . '"]');
        if (array_key_exists(0, $res)) {
            return new Rate(str_replace(',', '.', $res[0]->Value), new \DateTime());
        } else {
            throw new \Swap\Exception\Exception('The currency is not supported');
        }
    }
}
```