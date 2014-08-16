Bodeguero
===============
```
/bəˈdeɪgəːrəʊ/
	
	noun: bodeguero; plural noun: bodegueros
	A business person engaged in retail trade, but
	most commonly refered as the owner/manager of a bodega.
```

It does not matter what you are looking for, the `bodeguero` can and will get it for you.
Just ask politely and you shall receive.

The `bodeguero` currently supports `PostgreSQL` only but more engines are on the way.


#### Super simple usage:

Install your `bodeguero`:

```
npm install bodeguero
```

Require your `boguero` and put him to work:

```
var bodeguero = require('bodeguero').open(config);
```

The `config` object requires these properties:

* `user` - Your DB `username` (*string*).
* `password` - Your DB `password` (*string*).
* `database` - The `database` you are trying to connect to (*string*).

Optional properties:

* `host` - The `host` where your DB is to be found. (*string*, default: `localhost`).
* `port` - The `port` on which your DB is listening. (*int*, default: `5432`).
* `ssl` - Whether to use an SSL connection or not. (*boolean*, default: `false`). 



### Usage

##### Which table(s) to use

Tell the `bodeguero` from which table you are trying to get the data:

	bodeguero.in(<table>)

you can also tell him to join more tables to perform your query:

	var joinedTables = {
		'->': '<another table> on .<field> = .<field>',
		'<-': '<another table> on .<field> = .<field>',
		'><': '<another table> on .<field> = .<field>',
		'<>': '<another table> on .<field> = .<field>',
	};
	bodeguero.in('<table>', joinedTables)

The keys from the `joinedTables` are the logic that tells the `bodeguero` how to join the tables for the query:

* '->' : Right join
* '<-' : Left join
* '><' : Inner join
* '<>' : (Full) outter join

Notice the `on` condition; this tells `bodeguero` which fields to match when joining tables. The first field always corresponds to the table being joined. Here's an illustration:

	var join = {
		'<-': 'Brand on .id = .brand_id'
	};
	bodeguero.in('Wine', join)
	
will translate into:

	SELECT
		...
	FROM "Wine"
	LEFT JOIN "Brand" ON "Brand"."id" = "Wine"."brand_id"
		...



##### The field(s) to get

Now that you have defined the table(s) on which to perform the query, specify some fields:

	bodeguero.in(<table>).find('<one field>, <another field>')
	
of course, you also tell the `bodeguero` to simply get you everything:

	bodeguero.in(<table>).find('*')


##### The rules to apply

Finally, you need to tell the `bodeguero` how you want to filter the data:

	var filter = '<one field> = <condition> && <another field> != <condition>';
	bodeguero.in(<table>).find('<one field>, <another field>').where(filter);

you can write your filter just like a standard JavaScript conditional statement. For example `=`, `==` and `===` are all the same. Standard SQL keywords are also allowed; `LIKE` is perfectly acceptable.

You do not need to worry about SQL injection or escaping. The `bodeguero` will take care of everything for you. He uses nicely [prepared statements](http://en.wikipedia.org/wiki/Prepared_statement), in case you are wondering. 



### And here's a practical example

	var bottle = bodeguero.in('Wine').find('brand, color').where('year = 1980');

Found only one bottle? - The the response will be like this:

	{
	 brand: "Tempranillo de la Torre",
	 color: "red"
	}

found more than one?

	[
	 {
	  brand: "Tempranillo de la Torre",
	  color: "red"
	 },
     {
	  brand: "Monjas de Calavera",
	  color: "white"
	 }
	]




























