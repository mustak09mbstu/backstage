# References
initial server setup
['https://www.digitalocean.com/community/tutorials/initial-server-setup-with-rocky-linux-9']

installation
['https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-rocky-linux-9']



# FATAL: Ident authentication failed for user "postgres"
Typically occurs because PostgreSQL is trying to authenticate the user using the ident method, which relies on the system's user accounts rather than a password.
To resolve this, you need to change the authentication method for the postgres user in PostgreSQL to md5 or password, which allows password-based authentication.

## Steps to fix the issue:
1. Edit the pg_hba.conf file:
   - The pg_hba.conf file controls how PostgreSQL users are authenticated
   - The file is usually located in /var/lib/pgsql/data/ or /etc/postgresql/ (depending on your setup).
   - Find the line in pg_hba.conf that looks like this:
```bash
host    all             all             127.0.0.1/32            ident
```
Change ident to md5 or password to enable password-based authentication:
vi /var/lib/pgsql/data/pg_hba.conf
```bash
host    all             all             127.0.0.1/32            md5
```
2. Restart PostgreSQL: After editing pg_hba.conf, restart the PostgreSQL service to apply the changes.
```bash
sudo systemctl restart postgresql
```
3. Verify password for the postgres user: Ensure that the postgres user has a password set. You can change the password by connecting to the database as the system user:
```bash
sudo -i -u postgres
psql
```
4. Then set a new password for the postgres user:
```bash
ALTER USER postgres PASSWORD 'your_password';
ALTER USER postgres PASSWORD 'postgres';
```
5. Reconnect with password: Once the authentication method is set to md5 and the password is configured, you should be able to log in using:
```bash
PGPASSWORD='your_password' psql -h 127.0.0.1 -U postgres -d postgres
```
```bash
psql -h 127.0.0.1 -U postgres -d postgres
```