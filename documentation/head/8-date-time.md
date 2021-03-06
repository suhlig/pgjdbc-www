---
layout: default_docs
title: Using Java 8 Date and Time classes
header: Chapter 5. Using Java 8 Date and Time classes
resource: media
previoustitle: Creating and Modifying Database Objects
previous: ddl.html
nexttitle: Chapter 6. Calling Stored Functions
next: callproc.html
---

The PostgreSQL™ JDBC driver implements native support for the
[Java 8 Date and Time API](http://www.oracle.com/technetwork/articles/java/jf14-date-time-2125367.html)
(JSR-310) using JDBC 4.2.

<a name="8-date-time-supported-data-types"></a>
**Table 5.1. Supported escaped numeric functions**

<table summary="Supported data type" border="1">
  <tr>
    <th>PostgreSQL™</th>
    <th>Java SE 8</th>
  </tr>
  <tbody>
    <tr>
      <td>DATE</td>
      <td>LocalDate</td>
    </tr>
    <tr>
      <td>TIME [ WITHOUT TIMEZONE ]</td>
      <td>LocalTime</td>
    </tr>
    <tr>
      <td>TIMESTAMP [ WITHOUT TIMEZONE ]</td>
      <td>LocalDateTime</td>
    </tr>
    <tr>
      <td>TIMESTAMP WITH TIMEZONE</td>
      <td>OffsetTime</td>
    </tr>
  </tbody>
</table>

This is closely aligned with tables B-4 and B-5 of the JDBC 4.2 specification.
Note that `ZonedDateTime`, `Instant` and
`OffsetTime / TIME [ WITHOUT TIMEZONE ]` are not supported. Also note
that all `OffsetTime` will instances will have be in UTC (have offset 0).
This is because the backend stores them as UTC.

<a name="reading-example"></a>
**Example 5.5. Reading Java 8 Date and Time values using JDBC**

`Statement st = conn.createStatement();`  
`ResultSet rs = st.executeQuery("SELECT * FROM mytable WHERE columnfoo = 500");`  
`while (rs.next())`  
`{`  
&nbsp;&nbsp;&nbsp;`System.out.print("Column 1 returned ");`  
&nbsp;&nbsp;&nbsp;`LocalDate localDate = rs.getObject(1, LocalDate.class));`  
&nbsp;&nbsp;&nbsp;`System.out.println(localDate);`  
`}`
`rs.close();`  
`st.close();` 

For other data types simply pass other classes to `#getObject`.
Note that the Java data types needs to match the SQL data types in table 7.1.


<a name="writing-example"></a>
**Example 5.5. Writing Java 8 Date and Time values using JDBC**

`LocalDate localDate = LocalDate.now();`  
`PreparedStatement st = conn.prepareStatement("INSERT INTO mytable (columnfoo) VALUES (?)");`  
`st.setObject(1, localDate);`  
`st.executeUpdate();`
`st.close();`


