# WordPress

## Information

***NOTE***: Update the values in the `.env` file before composing.  
Especially the DBPASS variable, which was randomly generated for this config.

This uses the latest version of WordPress, along with MySQL version 5.7.  
The site is ready to be setup by going to http://localhost:7080/  
Additionally, this will be placed into its own network, WPNet, on 10.0.0.0/29.


## Networking

This is intentionally a small network, as it only needs 2 IPs for containers.  
10.0.0.0/29 is 10.0.0.0-7 (8 IPs), used as follows:

**Gateway**: 10.0.0.1  
**WordPress**: 10.0.0.2  
**MySQL**: 10.0.0.3  

WordPress is open on port 80, and MySQL on port 3306.


## Volumes

### Content

The `content` volume is available to directly interact with the WordPress  
plug-ins, themes, and user-submitted uploads. This is particularly useful when  
having to upload numerous files into the WordPress site.

***However***, not using the WordPress Dashboard, the images will not go through  
the sizing and processing, and will only be available in their original size.

### Database

The `database` volume is not required, however, can be useful for creating a  
database export and import. I suggest a repair and optimize before an export.  

Run the following commands inside the MySQL container:  
*(this will prompt for the password; DBPASS value)*

```
# Repair and Optimize
mysqlcheck -u dbuser -p --auto-repair wpdb
mysqlcheck -u dbuser -p --optimize wpdb

# Export the database
mysqldump --add-drop-table -u dbuser -p wpdb > /var/lib/mysql/wpdb.sql
```

I advise that you ***DO NOT*** export the database into web-accessible directories  
as it can be easy for anyone to forget it's there, and it's available for anyone  
to download as well.

Both of the volumes contain the unique content that would be needed to backup  
and restore from one instance to another, be it between a self-hosted machine,  
hosting provider, or even another Docker instance.
