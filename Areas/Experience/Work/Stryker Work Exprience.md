I did two projects with this client
1. This project was to decrypt the database used to search for blacklisted devices. The library used for encryption was sqlite3 **SEE SQLite Encryption Extension**. The database was decrypted using the method shown in [this blog](https://eighty-twenty.org/2018/06/13/mendeley-encrypted-db).
2. This second project was the extension of first project where the I had to reverse the decoding logic of the value obtained from sqlite3 database.


## MCU Reversing

- Stack on debugging another MCU firmware because of watchdog timer
	- [How does watch dog timer work](https://interrupt.memfault.com/blog/firmware-watchdog-best-practices)