```none{7, 8}
scenario.description = Hermes requests address from Conscience

include = common.properties

step1.adapter.toConscience.write = scenario01/hermesAddressRequest.xml
step2.stub.conscience.read = scenario01/conscienceAddressRequest.xml
step3.stub.conscience.write = scenario01/conscienceAddress.xml
step4.adapter.toConscience.read = scenario01/hermesAddress.xml
```
