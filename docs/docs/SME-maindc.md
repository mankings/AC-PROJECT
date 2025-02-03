# MAINDC Server

### named.conf.local

```
view "ptpor" {
    match-clients { PTPOR; };
    recursion no;
    zone "cdn.cdn4all.com" {
        type master;
        file "/etc/bind/cdn-ptpor.db";
    };
}

view "ptlis" {
    match-clients { PTLIS; };
    recursion no;
    zone "cdn.cdn4all.com" {
        type master;
        file "/etc/bind/cdn-ptlis.db";
    };
}

view "esbar" {
    match-clients { ESBAR; };
    recursion no;
    zone "cdn.cdn4all.com" {
        type master;
        file "/etc/bind/cdn-esbar.db";
    };
}

view "other" {
    match-clients { any; };
    recursion no;
    zone "cdn.cdn4all.com" {
        type master;
        file "/etc/bind/cdn-other.db";
    };
}
```

### cdn-ptpor.db
```
$TTL 86400
@   IN  SOA     ns1.cdn4all.com. admin.cdn4all.com. (
            2021010101  ; Serial
            3600        ; Refresh
            1800        ; Retry
            604800      ; Expire
            86400 )     ; Minimum

@   IN  NS  ns1.cdn4all.com.
A   IN  NS  10.0.
```

### cdn-ptlis.db
```
```

### cdn-esbar.db
```
```

### cdn-other.db
```
```
