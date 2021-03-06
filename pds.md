# PDS

The PDS object is a class wrapper around the Nimbus::API PDS functions. Reference the PDS.pm module for the functions included.

## Examples 

```perl
use Nimbus::PDS; 

my $PDS = Nimbus::PDS->new(); 
$PDS->put("name","robot_name",PDS_PCH);
my ($RC,$RES) = nimNamedRequest("/DOMAIN/HUB-NAME/ROBOT-NAME/hub","getrobots",$PDS->data);

if($RC == NIME_OK) {
    my $ROBOTS_PDS = Nimbus::PDS->new($RES);
    for( my $count = 0; my $_T = $ROBOTS_PDS->getTable("robotlist",PDS_PDS,$count); $count++) {
        # Do action here...
    }
}
```

## PDS Types 

| Constant | Type |
| --- | --- |
| PDS_PCH | STRING |
| PDS_INT | INT | 
| PDS_PDS | HASH OF PDS | 
| PDS_F | FLOAT | 

## PDS Error 

We have two constants for PDS error, `PDS_ERR` and `PDS_ERR_NONE`.

## API 

### new() -> pdsHandle

Create a new Nimbus::PDS

```perl
my $PDS = Nimbus::PDS->new(); 
```

### dump() 

Dump a Nimbus PDS Object

### data() 

Return the original PDS (C Way) data !

### asHash() -> HASH

Transform PDS to hash. Useful when a callback return a flat PDS structure 

```perl
my $PDS = Nimbus::PDS->new();
my ($RC,$RES) = nimNamedRequest("/DOMAIN/HUB-NAME/ROBOT-NAME/controller","get_info",$PDS->data);

if($RC == NIME_OK) {
    my $Info = Nimbus::PDS->new($RES)->asHash();
    print "$Info->{origin}\n";
}
```

### put(szName,anyValue,pds_type) -> BOOLEAN

Put a new value in your PDS

```perl
my $PDS = Nimbus::PDS->new(); 
$PDS->put("A",5,PDS_INT);
$PDS->put("B","hello world",PDS_PCH);
```

### float(szName,iValue) -> BOOLEAN

Wrapper of put method for FLOAT Type

### number(szName,iValue)

Wrapper of put method for INT Type.

### string(szName,szValue) 

Wrapper of put method for STRING Type.

### get(szName,pds_type) -> <TYPE>VALUE

Get a value with the given type 

```perl
my $A_Value = $PDS->get("A",PDS_INT);
```

### getTable(szName,pds_type) -> PDS_PDS | ARRAY

Returns the contents of a table in the PDS.

### putTable($name, $value[, $type])

Adds the key/value pair as a table element in the PDS

### reset() 

This function re‑initializes the PDS object to an empty buffer with initial pointer settings. If you want to reset the get pointer, use the pdsRewind() function. This function deletes data. The buffer is not reallocated.

### rewind()

This function re-initializes the get pointer within the PDS object.

**Note**: You can put a block of data and perform multiple gets separated with a pdsRewind(pds) call.

### remove(szName) 

Remove a key 

```perl
$PDS->remove("A");
$PDS->remove("B");
```

