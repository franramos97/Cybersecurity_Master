# Getting data from AlienVault OTX

AlienVault Open Threat Exchange (OTX) is "a crowd-sourced computer-security platform". Founded in 2012 OTX was created and is run by AlienVault (now AT&T Cybersecurity).
Registration is free here: https://otx.alienvault.com

Once inside the Dashboard, in the settings of the user profile, you can get a API Key to access data programmatically. 

There is a Python SDK for the API that can be installed as usual:

`
pip install OTXv2

`

#diccionarios: 

#a={}
#a["nombre"] = ["pepe"] 
#a["nombre"] #esto es la clave ="pepe" # esto es el valor
#a["telefono"]="91..."

#print(a["nombre"])
#a["gustos"]=["cine","teatro"]
#a["gustos"][1] ==teatro

## Getting a client 


```python
from OTXv2 import OTXv2
from OTXv2 import IndicatorTypes
otx = OTXv2("d48dbd3713054eba7e1cce4c36c23e11d4eab78ddb35bea5f88a33e0cee8ae94") #esto esta en alienvault, en settings arriba derecha y lo he metido aqui
```

## Getting info from a pulse


```python
pulse_details = otx.get_pulse_details("60146fd59c6b2bfdcd615572") #este código es simplemente la URL de algun pulso dentro de ALIENVAULTS
```


```python
pulse_details.keys() #pulse_details es un diccionario, y me devuelve todas las 'keys' q hay dentro del diccionario
```


```python
pulse_details['name']
```


```python
pulse_details['tags']
```


```python
pulse_details['malware_families'] #si me fijo, es una lista --> "[]" con 5 diccionarios --> "{}"
```

## Getting indicator info


```python
indicators = otx.get_pulse_indicators("59b00ff1bd02917779c07f9c") #'indicators' son los IOCs
```


```python
indicators[0], type(indicators[0])
```

## Getting data on a indicator of type file


```python
indicator_details = otx.get_indicator_details_full(IndicatorTypes.FILE_HASH_SHA256, "b4100e5cd5ebd42551e6bf0a2cce9eef033a033ba31fb90fafaf64f69c978522")
```


```python
indicator_details.keys()
```


```python
indicator_details['analysis']['analysis']['plugins']['msdefender']
```


```python

```
