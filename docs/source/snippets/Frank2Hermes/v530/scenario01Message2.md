```none{6}
scenario.description = Hermes requests address from Conscience

include = common.properties

step1.adapter.toConscience.write = scenario01/hermesAddressRequest.xml
step2.stub.conscience.read = scenario01/conscienceAddressRequest.xml
```
