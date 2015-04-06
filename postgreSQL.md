#### Create a new user with `createuser` command (interactively)
	createuser --interactive joe

#### Change user password
	`ALTER USER user_name WITH PASSWORD 'password';

#### Create database with `createdb` command
	createdb -U username -E utf8 db_name

#### Show table
	\dt 
	SELECT * FROM pg_catalog.pg_tables WHERE tableowner=db_name;

#### PostgreSQL auto increment
**SERIAL** is the equivalent of autoincrement in MySQL,

	CREATE TABLE "user" (
	    UserID SERIAL,  
	    Username varchar(50)  
	);

Or use custom **SEQUENCE** to do auto increment with any other *integer data type*

	CREATE SEQUENCE user_id_seq;
	CREATE TABLE user (
	    user_id smallint NOT NULL DEFAULT nextval('user_id_seq')
	);
	ALTER SEQUENCE user_id_seq OWNED BY user.user_id;

To escape keyword, wrap it into *double-quotes*

#### **psql** commands
<table cellspacing="0" cellpadding="4" border="1">
<tbody>
<tr><th>Listing</th><th>Command</th><th>Argument</th></tr>
<tr><td>Table, index, view, or sequence</td><td><code>\d</code></td><td><i>name</i></td></tr>
<tr><td>Tables</td><td><code>\dt</code></td><td><i>name</i></td></tr>
<tr><td>Indexes</td><td><code>\di</code></td><td><i>name</i></td></tr>
<tr><td>Sequences</td><td><code>\ds</code></td><td><i>name</i></td></tr>
<tr><td>Views</td><td><code>\dv</code></td><td><i>name</i></td></tr>
<tr><td>Permissions</td><td><code>\dp or \z</code></td><td><i>name</i></td></tr>
<tr><td>System tables</td><td><code>\dS</code></td><td><i>name</i></td></tr>
<tr><td>Large objects</td><td><code>\dl</code></td><td><i>name</i></td></tr>
<tr><td>Types</td><td><code>\dT</code></td><td><i>name</i></td></tr>
<tr><td>Functions</td><td><code>\df</code></td><td><i>name</i></td></tr>
<tr><td>Operators</td><td><code>\do</code></td><td><i>name</i></td></tr>
<tr><td>Aggregates</td><td><code>\da</code></td><td><i>name</i></td></tr>
<tr><td>Comments</td><td><code>\dd</code></td><td><i>name</i></td></tr>
<tr><td>Databases</td><td><code>\l</code></td><td><i>name</i></td></tr>
</tbody>
</table>

See more about [psql](http://www.postgresql.org/docs/current/interactive/app-psql.html).