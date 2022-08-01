```xml
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:ext="http://www.liquibase.org/xml/ns/dbchangelog-ext"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.0.xsd
        http://www.liquibase.org/xml/ns/dbchangelog-ext http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-ext.xsd">
    <changeSet id="1" author="martijn">
       <sql>
           CREATE TABLE product (
               productId INT PRIMARY KEY NOT NULL,
               address VARCHAR(50) NOT NULL,
               description VARCHAR(100),
               price DECIMAL NOT NULL
           )
       </sql>
    </changeSet>
    <changeSet id="2" author="martijn">
        <sql>
            ALTER TABLE product ADD modificationDate DATETIME;
        </sql>
    </changeSet>
    <changeSet id="3" author="martijn">
        <sql>
            UPDATE product SET modificationDate=PARSEDATETIME('2020-05-01', 'yyyy-MM-dd') WHERE modificationDate IS NULL
        </sql>
    </changeSet>
    <changeSet id="4" author="martijn">
        <sql>
            ALTER TABLE product ALTER COLUMN modificationDate SET NOT NULL
        </sql>
    </changeSet>
</databaseChangeLog>
```
