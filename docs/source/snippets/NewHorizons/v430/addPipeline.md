```xml{11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21}
<Configuration
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="../FrankConfig.xsd">
  <Adapter name="IngestBooking">
    <Receiver name="input">
      <ApiListener
          name="inputListener"
          uriPattern="booking"
          method="POST"/>
    </Receiver>
    <Pipeline firstPipe="checkInput">
      <Exit path="Exit" state="SUCCESS" code="201" />
      <Exit path="BadRequest" state="ERROR" code="400" />
      <XmlValidatorPipe
          name="checkInput"
          root="booking"
          schema="booking.xsd">
        <Forward name="success" path="Exit" />
        <Forward name="failure" path="BadRequest" />
      </XmlValidatorPipe>
    </Pipeline>
  </Adapter>
</Configuration>
```
