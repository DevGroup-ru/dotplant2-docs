# Реализация класса провайдера курсов валют

Класс провайдер должен реализовывать метод [[Swap\ProviderInterface::fetchRate()]]. Метод  `fetchRate` принимает в качестве параметра экземпляр класса [[Swap\Model\CurrencyPair]], который описывает пару валют. Провайдер должен вернуть экземпляр класса [[Swap\Model\Rate]], который описывает курс пары валют с привязкой ко времени.

Доступные методы класса [[Swap\Model\CurrencyPair]]:

* __construct(baseCurrency, quoteCurrency) - конструктор, параметры названия базовой и вторичной валют соответственно в формате ISO 4217
* createFromString(string) - статичный метод, создающий пару валют, принимает строку в формате EUR/USD
* getBaseCurrency() - метод возвращает код ISO 4217 основной валюты
* getQuoteCurrency() - метод возвращает код ISO 4217 вторичной валюты
* toString() - метод возвращает ISO 4217 коды валют в формате EUR/USD

Пример реализации класса провайдера, в качестве провайдера курса валют выступает Центральный Банк РФ:

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