+++
title = "oracledb_session resource"
draft = false

platform = "os"

[menu.resources]
    title = "oracledb_session"
    identifier = "resources/core/oracledb_session.md oracledb_session resource"
    parent = "resources/core"
+++

Use the `oracledb_session` Chef InSpec audit resource to test SQL commands run against an Oracle database.

## Availability

### Install

{{< readfile file="content/reusable/md/inspec_installation.md" >}}

### Version

This resource first became available in v1.0.0 of InSpec.

## Syntax

A `oracledb_session` resource block declares the username and PASSWORD to use for the session with an optional service to connect to, and then the command to be run:

```ruby
describe oracledb_session(user: 'username', PASSWORD: 'PASSWORD', service: 'ORCL.localdomain').query('QUERY').row(0).column('result') do
  its('value') { should eq('') }
end
```

where:

- `oracledb_session` declares a username and PASSWORD with permission to run the query (required), and optional parameters for:
  - `host` (default: `localhost`)
  - `port` (default: `1521`)
  - `service`: the Oracle service name or SID
  - `tns_alias`: TNS alias from tnsnames.ora (recommended for TCPS/SSL connections)
  - `env`: hash of environment variables (for example, TNS_ADMIN, LD_LIBRARY_PATH, ORACLE_HOME)
  - `as_db_role`: database role (for example, sysdba, sysoper)
  - `as_os_user`: OS user to switch to before running queries (Unix/Linux only)
  - `sqlplus_bin`: path to sqlplus binary (default: `sqlplus`)
  - `sqlcl_bin`: path to sqlcl binary (if using SQLcl instead of sqlplus)
- To run queries as sysdba/sysoper, use the `as_db_role` option. See examples.
- SQLcl can be used in place of sqlplus. Use the `sqlcl_bin` option to set the sqlcl binary path instead of `sqlplus_bin`.
- `query('QUERY')` contains the query to be run
- `its('value') { should eq('') }` compares the results of the query against the expected result in the test

## oracledb_session(...).query method Properties

- `rows` - the query result as an array of hashes
- `row(number)` - selected row from query result, where number is a row number in the query result
- `column(name)` - array with values from selected column

## Examples

The following examples show how to use this Chef InSpec audit resource.

### Test for matching databases

```ruby
sql = oracledb_session(user: 'USERNAME', pass: 'PASSWORD')

describe sql.query('SELECT NAME AS VALUE FROM v$database;').row(0).column('value') do
  its('value') { should cmp 'ORCL' }
end
```

### Test for matching databases with custom host, SID and sqlplus binary location

```ruby
sql = oracledb_session(user: 'USERNAME', pass: 'PASSWORD', host: 'ORACLE_HOST', sid: 'ORACLE_SID', sqlplus_bin: '/u01/app/oracle/product/12.1.0/dbhome_1/bin/sqlplus')

describe sql.query('SELECT NAME FROM v$database;').row(0).column('name') do
  its('value') { should cmp 'ORCL' }
end
```

### Test for table contains a specified value in any row for the given column name

```ruby
sql = oracledb_session(user: 'USERNAME', pass: 'PASSWORD', service: 'ORACLE_SID')

describe sql.query('SELECT * FROM my_table;').column('COLUMN') do
  it { should include 'my_value' }
end
```

### Test tablespace exists as sysdba

The check will change user (with su) to specified user and run 'sqlplus / as sysdba' (sysoper, sysasm)

```ruby
sql = oracledb_session(as_os_user: 'oracle', as_db_role: 'sysdba', service: 'ORACLE_SID')

describe sql.query('SELECT tablespace_name AS name FROM dba_tablespaces;').column('name') do
  it { should include 'TABLE_SPACE' }
end
```

The `as_os_user` option is available only on Unix-like systems and isn't supported on Windows. This option also requires that you're running InSpec as `root` or with `--sudo`.

### Test number of rows in the query result

```ruby
sql = oracledb_session(user: 'USERNAME', pass: 'PASSWORD')

describe sql.query('SELECT * FROM my_table;').rows do
  its('count') { should eq 20 }
end
```

### Use data out of (remote) DB query to build other tests

```ruby
sql = oracledb_session(user: 'USERNAME', pass: 'PASSWORD', host: 'my.remote.db', service: 'ORACLE_SID')

sql.query('SELECT * FROM files;').rows.each do |file_row|
  describe file(file_row['path']) do
    its('owner') { should eq file_row['owner']}
  end
end
```

### Test using TNS alias for TCPS/SSL connections

```ruby
sql = oracledb_session(
  user: 'system',
  password: 'Oracle123',
  tns_alias: 'XEPDB1_TCPS',
  env: {
    'TNS_ADMIN' => '/opt/oracle/network/admin',
    'LD_LIBRARY_PATH' => '/opt/oracle/instantclient',
    'ORACLE_HOME' => '/opt/oracle'
  }
)

describe sql.query('SELECT * FROM dual;').row(0).column('dummy') do
  its('value') { should eq 'X' }
end
```

**NOTE:** The `tns_alias` option is recommended for TCPS/SSL connections as it allows proper TNS name resolution from tnsnames.ora. When using `tns_alias`, the `env` hash can be used to set required environment variables like `TNS_ADMIN` (path to tnsnames.ora and wallet files). This enables secure connections with SSL/TLS certificate validation.

## Matchers

{{< readfile file="content/reusable/md/inspec_matchers_link.md" >}}
