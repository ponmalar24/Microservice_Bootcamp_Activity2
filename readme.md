> ### __Dockerize both the microservices and upload in Google container registry or IBM registry or docker hub__
* docker build -t ponmalar/currencyconversionfactor .  
* docker push ponmalar/currencyconversionfactor
* docker run -p 8000:8080 -d ponmalar/currencyconversionfactor
 
* docker build -t ponmalar/convertcurrency . 
* docker push ponmalar/convertcurrency  
* docker run -p 8001:8081 -d ponmalar/convertcurrency

> ### __Invoke the MS-1 microservice from MS-2 using DNS discovery__
* C:\Users\vmware19\Downloads\K8S>kubectl apply -f conversionfactor.yaml
* C:\Users\vmware19\Downloads\K8S>kubectl apply -f convertcurrency.yaml 
* C:\Users\vmware19\Downloads\K8S>kubectl apply -f currencyconfig.yaml
 
* C:\Users\vmware19\Downloads\K8S>kubectl exec -it mynginx -- curl convertcurrency/ConvertedAmount/currency/AUD/amount/124315
![DNS Discovery](./DNSdiscovery.jpg)

>### __Set up 3 instances of MS-1 and load balance the call from MS-2 to MS-1__
Sample program to sending multiple request:
```shell script
PS C:\Users\vmware19\Downloads\K8S>kubectl exec -it mynginx /bin/sh
#i=1
#while [ "$i" -le 1000 ]; do       
>curl convertcurrency/ConvertedAmount/currency/AUD/amount/124315;
>i=$(( i + 1))
>done 
3107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.03107875.0 
```
> ### __Protect both microservices with a KONG Ingress rate limiting plugin of 20 Requests per minute__
* PS C:\Users\vmware19\Downloads\K8S>kubectl apply -f ./conversionfactor.yaml -f ./convertcurrency.yaml
* PS C:\Users\vmware19\Downloads\K8S>kubectl apply -f ./currencyingress.yaml
* PS C:\Users\vmware19\Downloads\K8S>kubectl apply -f ./currencyratelimit.yaml
![ingress Result](./ingress.jpg)

 
