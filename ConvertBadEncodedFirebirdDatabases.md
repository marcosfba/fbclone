Convert an ISO8859\_1 Firebird 1.5 database to an UTF8 Firebird 2.x database.

# Introduction #

If you used to develop your apps with Delphi, some years ago, you take the habbit to create your Firebird 1.5 databases with an ISO8859\_1 charset. Today you want to switch to Delphi 2010 (Unicode) and Firebid 2.1 that ensure more strict control of input data and you can't access your data, or your backup/restore fails.

You can use fbclone 2.1 to convert your databases and fix these old charset problems.

# Details #

That's quite simple :

```
fbclone.exe -v -s... -t... -tc UTF8 -rc NONE -wc WIN1252
```

Some details :
  * Firebird 1.5 where usually created with ISO8859\_1 charset, your delphi app was generating WIN1252 data because your operating system charset is WIN1252 (to handle the € sign for example that don't exists in ISO8859\_1 charset)
  * So you fed your Firebird 1.5 database with WIN1252 data and metadata while database default charset and fields charsets were ISO8859\_1, some characters are incompatible because they don't exist in both charset.
  * Now you want to switch to Firebird 2.1 and gbak fails due to character transliteration.
  * The solution is to connect to source database using the NONE charset
  * The data read from source database is WIN1252 because that's what you feeded with your Delphi application (sorry...)
  * The new database has been created with UTF8 as default charset
  * We connect the clone database with WIN1252 charset and let Firebird transliterate data and metadata to UTF8.