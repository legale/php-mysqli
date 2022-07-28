# php-mysqli
Simple php mysqli driver based mysql library. Based on colshrapnel/safemysql library.

## installation 
`composer require legale/mysqli`

## usage example
```php

 /*
  * Supported placeholders at the moment are:
  *  ?s ("string")  - strings (also DATE, FLOAT and DECIMAL)
  *  ?i ("integer") - the name says it all
  * ?n ("name")    - identifiers (table and field names)
  * ?a ("array")   - complex placeholder for IN() operator  (substituted with string of 'a','b','c' format, without parentesis)
  * ?u ("update")  - complex placeholder for SET operator (substituted with string of `field`='value',`field`='value' format)
  * ?p ("parsed") - special type placeholder, for inserting already parsed statements without any processing, to avoid double parsing.
  */
 
 //Connection:

 $db = new Db(); // with default settings

 $opts = array(
		'user'    => 'user',
		'pass'    => 'pass',
		'db'      => 'db',
		'charset' => 'latin1'
 );
 $db = new Db($opts); // with some of the default settings overwritten

/* Alternatively, you can just pass an existing mysqli instance that will be used to run queries
 instead of creating a new connection.
 Excellent choice for migration!*/

 $db = new Db(['mysqli' => $mysqli]);

 //Some examples:

 $name = $db->getOne('SELECT name FROM table WHERE id = ?i',$_GET['id']);
 $data = $db->getInd('id','SELECT * FROM ?n WHERE id IN ?a','table', array(1,2));
 $data = $db->getAll("SELECT * FROM ?n WHERE mod=?s LIMIT ?i",$table,$mod,$limit);

 $ids  = $db->getCol("SELECT id FROM tags WHERE tagname = ?s",$tag);
 $data = $db->getAll("SELECT * FROM table WHERE category IN (?a)",$ids);

 $data = array('offers_in' => $in, 'offers_out' => $out);
 $sql  = "INSERT INTO stats SET pid=?i,dt=CURDATE(),?u ON DUPLICATE KEY UPDATE ?u";
 $db->query($sql,$pid,$data,$data);

 if ($var === NULL) {
     $sqlpart = "field is NULL";
 } else {
     $sqlpart = $db->parse("field = ?s", $var);
 }
 $data = $db->getAll("SELECT * FROM table WHERE ?p", $bar, $sqlpart);
```
