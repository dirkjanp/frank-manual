```xml
<Module
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="../../../ibisdoc.xsd">
    <Adapter name="adapterProcessDestination">
        <Receiver name="receiverProcessDestination">
            <JavaListener name="listenerProcessDestination" serviceName="listenerProcessDestination"/>
        </Receiver>
        <Pipeline firstPipe="pipeCheckProductIdExists" transactionAttribute="RequiresNew">
            <SenderPipe name="pipeCheckProductIdExists">
                <FixedQuerySender
                    name="senderCheckProductIdExists"
                    queryType="select"
                    query="SELECT COUNT(*) AS cnt FROM product WHERE productId = ?"
                    datasourceName="jdbc/${instance.name.lc}"
                    maxRows="1"
                    includeFieldDefinition="false">
                    <Param name="id" xpathExpression="/apartment/productId"/>
                </FixedQuerySender>
                <Forward name="success" path="pipeChooseInsertOrUpdate"/>
            </SenderPipe>
            <XmlIfPipe
                name="pipeChooseInsertOrUpdate"
                xpathExpression="/result/rowset/row/field"
                expressionValue="0"
                thenForwardName="pipeDoInsert"
                elseForwardName="pipeDoUpdate">
            </XmlIfPipe>
            <SenderPipe
                name="pipeDoInsert"
                getInputFromSessionKey="originalMessage">
                <FixedQuerySender
                    name="senderDoInsert"
                    query="INSERT INTO product VALUES(?, ?, ?, ?, NOW())"
                    datasourceName="jdbc/${instance.name.lc}">
                    <Param name="id" xpathExpression="/apartment/productId"/>
                    <Param name="address" xpathExpression="/apartment/address"/>
                    <Param name="description" xpathExpression="/apartment/description"/>
                    <Param name="price" xpathExpression="/apartment/price"/>
                </FixedQuerySender>
                <Forward name="success" path="EXIT"/>
            </SenderPipe>
            <SenderPipe
                name="pipeDoUpdate"
                getInputFromSessionKey="originalMessage">
                <FixedQuerySender
                    name="senderDoUpdate"
                    query="UPDATE product SET address = ?, description = ?, price = ?, modificationDate = NOW() WHERE productId = ?"
                    datasourceName="jdbc/${instance.name.lc}">
                    <Param name="address" xpathExpression="/apartment/address"/>
                    <Param name="description" xpathExpression="/apartment/description"/>
                    <Param name="price" xpathExpression="/apartment/price"/>
                    <Param name="id" xpathExpression="/apartment/productId"/>
                </FixedQuerySender>
            </SenderPipe>
            <Exit state="success" path="EXIT" code="200"/>
        </Pipeline>
    </Adapter>
</Module>
```
