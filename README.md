# CyptoInfo
Simple bash script to get current prices of a list of cryptocurrencies from a config file.

```shell
#!/bin/bash

#defining basic urls
cburl='https://min-api.cryptocompare.com/data'
ccurrprice='/price?fsym='
dayavg='/dayAvg?fsym='
#search file to see if a fiat currency is defined.
while read i; do
  if [[ "${i,,}" == usd ]] ;
  then fiat='&tsym=USD' ;  fiats='&tsyms=USD'
  elif [[ "${i,,}" == eur ]] ;
  then fiat='&tsym=EUR' ; fiats='&tsyms=EUR'
  fi
done <$PWD/cryptoconfig

while read curr ; do
echo "${curr^^}"
echo "========="
echo "Current price" $(curl -s $cburl$ccurrprice"${curr^^}"$fiats)
echo "Today's average" $( curl -s $cburl$dayavg"${curr^^}"$fiat | awk -F, '{print $1"}"}')

done < <(sed -e '1,/starting/ d' $PWD/cryptoconfig)

```
Sample config file:
```text

#please define desired output currency for cryptocurrencies
usd

#list of cryptocurrencies that we would like information on.
#script will read starting on next line
btc
ETH
Ltc
```
