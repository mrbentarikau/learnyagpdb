# Custom Command Database

Databases are used for storing persistent data that you want to keep between custom command executions. You access and manipulate them with functions which we are going to elaborate on in this guide.

## Overall

This section covers the structure of a database entry, the database's size limits, as well as the entry's size limit and lastly the interaction limit per custom command execution.

### Structure of an Entry

A database entry has the following structure:

| Field | Description |
| :--- | :--- |
| .ID | The ID of this entry. Not to be confused with the User ID. |
| .GuildID | The server ID. |
| .UserID | ID of the associated user. |
| .User | The associated user object. |
| .CreatedAt | When this entry was firstly created. |
| .UpdatedAt | When this entry was lastly updated. |
| .ExpiresAt | When this entry will expire. |
| .Key | The key of this entry. |
| .Value | The value of this entry. |

{% hint style="info" %}
This can also be found in the [documentation](https://docs.yagpdb.xyz/reference/templates#dbentry).
{% endhint %}

It is to note that the user ID doesn't actually have to point to a valid user - it can be any integer. If you for example used `2000` as ID, this will be `.UserID`. If you used a non-user ID, the `.User` object will be invalid.  
 The fields `.CreatedAt`, `.UpdatedAt`, and `.ExpiresAt` all evaluate to the `time.Time` structure and can therefore be used with `time.Time` related methods, which can be found [here](https://docs.yagpdb.xyz/reference/templates#time).

### Size Limitations

As everything that has to do with computers and data, there are limitations. Our YAGPDB database is no exception. However, we tried to make them as large as possible, whilst still remaining somewhat conservative and not going completely overboard.

In general, you can have `50 * Members` values \(entries\) in your server's database. If your server has premium activated, this increases to `500 * Members`. Note that this will not immediately change as this value is cached, should your members leave or newly join. If you go above your maximum entries, all new write functions will fail. Honestly though, already `50 * Members` is a lot to deal with, so you are safe for a while.  
 Please note though that this is not 50 \(or 500 with premium\) entries _per user_, but rather for everything. This of course means that one user could take up every entry there is. So, be careful with your database!

Database keys are strings and are limited to 256 bytes in length \(aka 256 characters\). If used with `dbSet` or `dbSetExpire`, the key argument will be internally converted to a string, so you can actually pass whatever you want.

Lastly, each value can hold up to 100kB, which is, considering we only deal with characters \(mostly\), a lot. Try writing a `.txt` document that is 100kB large, then you know how much this seemingly small space can actually hold.

### Interaction Limits

To prevent spam and therefore possible lag across YAGPDB, we put interaction limits in place. First of all, you can't use more than 10 overall interactions, so every function that starts with `db` counts towards that limit. This limit increases to 50 if you have premium active. Moreover, there are so-called multiple interactions, namely `dbCount`, `dbGetPattern`, `dbGetPatternReverse`, `dbTopEntries`, and `dbBottomEntries`. These are limited at two total calls per custom command execution, 10 with premium. You don't have to understand how they work right now, we'll get later into that. Just keep in mind that they are counting towards a second limit.

That concludes the overview, now let's get into basic interactions!

## Basic Interactions

These interactions are your constant companion when dealing with the database. You are most likely to use them more often that the upcoming functions, so really get them into your muscle memory, then the rest will be super easy!

### dbSet

To start, let's take a look at the syntax:

```go
{{dbSet <UserID> <Key> <Value>}}
```

We've already covered `UserID` and `Key` further up. As a small refresher: `UserID` can be any integer, and `Key` is a string, the name of the entry so to say.  
 Now, what exactly is that `Value`? You surely know how variables work in YAGPDB-CC, and the value is nothing else. Just that it is persistent between custom command executions, and variables are not.

### dbGet

Okay, we can now set a value into the database, but how on earth are we going to retrieve it? Well, this is where `dbGet` comes in handy. As its name already suggests, it gets an entry \(not the value!\) from the database with the given Key and ID.

```go
{{dbGet <UserID> <Key>}}
```

As you can see, we don't have a value as above, just simply `ID` and `Key`. The call of this function returns an entry object, as described above. So, before you continue reading, take a moment to think about how to get the value.

### dbDel

Now we know how to set and get stuff with the database, but a good program also frees up unused memory, and custom commands are no exemption to that. These cases are where we use `dbDel`.

```go
{{dbDel <UserID> <Key>}}
```

## Task

Write a custom command that sets an entry `"Database is cool!"` under the key `yagpdbDatabase` for the triggering user. Then you should get the entry, print out the time when this entry was updated, its key and its value. The format of the time does not matter. You should access those fields using the database entry object. Afterwards, delete the entry again.

## Advanced Interactions

Now, you might want to become a little more special with your database - That's why we have a few more functions, `dbIncr` and `dbSetExpire`.  
 With these functions, you are able to add a whole new layer to your custom commands, so let's get started!

### dbIncr

`dbIncr` is quite a handy function, as it increases the value inside the entry by the given increment and returns the increased value in the same moment, allowing you to save it to a variable.  
 You might ask yourself, what exactly is the increment? This argument can be any valid number, so integers and floats. Please note however that the return type of `dbIncr` is always a float. So if you are using integers as increment and plan to use them as such, please don't forget to convert them. Now let us take a quick look at the syntax.

```go
{{dbIncr <UserID> <Key> <Increment>}}
```

What's also noteworthy is the fact that `dbIncr` sets the value to the given increment, shouldn't the entry exist already.  
 Try thinking about how you would implement a custom command that increases a given entry by a set amount, gets the value, but also sets a new entry if it doesn't already exist. I think you can see how helpful `dbIncr` really is, considering that this function can condense the following code into one function call.

```go
{{$db := dbGet .User.ID "someKey"}}
{{$add := add (toFloat $db.Value) $x}}
{{dbSet .User.ID "someKey" (str $add)}}
{{$add}}
```

As you see, using only basic functions essentially requires you to waste a database function call you can probably use elsewhere. And as we discussed in the beginning, those are limited at 10 \(50 with premium\), so quite a precious resource that should not be wasted.

### dbSetExpire

Now you might want to set entries which get deleted after a while. You could of course use a delay using `execCC`, or use `dbSetExpire`. Why is it worth to use `dbSetExpire`? You don't have to waste an `execCC` to delete the entry later on. As we recall from the beginning, the `.ExpiresAt` field is of type `time.Time`, so the related functions are available to us. Let's take a glimpse at the syntax:

```go
{{dbSetExpire <UserID> <Key> <Value> <Expires in>}}
```

The `Expires in` is given in seconds, the rest is standard stuff as explained under [Basic Interactions]().

A common use case for this function is a cooldown: As long as the entry exists, the command is still on cooldown.

```go
{{if $db := (dbGet 2000 "cooldown")}}
    Command is on cooldown :(
    Cooldown will be over at {{$db.ExpiresAt.Format "Mon 02 Jan 15:04:05"}}
{{else}}
    Command is not on cooldown :)
    {{dbSetExpire 2000 "cooldown" "random value" 60}}
{{end}}
```

What's really interesting to note here though, is that once the entry expires, it is still in the system, but will be considered gone.

## Multiple Interactions

Lastly there are special functions which allow you to get multiple entries. We call those coincidentally multiple entry interactions. Every function except one returns a slice of entries. Depending on what function you use, this slice is sorted by certain criteria.  


### dbCount

This is the only function interacting with multiple entries not returning a slice. Since this function is fairly easy to understand, we'll start with that. Let's take a quick look at the syntax:

```go
{{dbCount <userID>}}
{{/* or */}}
{{dbCount <key>}}
```

This should be pretty clear: Count the entries either for the given user ID or the given Key.   
 Please note that this function returns the amount of entries, so to avoid random spam, you'd have to store it into a variable. Apart from that, this function is very simple to use and might come in handy for a few things.

### dbTopEntries and dbBottomEntries

These functions return a slice of entry objects, in case of `dbTopEntries` ordered by descending value, in case of `dbBottomEntries` in ascending order. You cannot retrieve more than 100 elements in one call - honestly though, 100 entries in one go is definitely a lot, probably more you would have to actually deal with.

```go
{{dbTopEntries <pattern> <amount> <nSkip>}}
{{dbBottomEntries <pattern> <amount> <nSkip>}}
```

What's new here are three things: `pattern`, `amount`, and `nSkip.` Let's walk through them one by one. For `pattern`, we use basic PostgreSQL patterns, you can read on them further down. The `amount` specifies how many entries we want to retrieve. Lastly, you tell YAGPDB how many entries it should skip using the `nSkip` argument.  
 Now to retrieve the value of each entry, we range over the given slice and access the `.Value` field:

```go
{{$entries := dbTopEntries "someKey" 10 0}}
{{range $entries}}
    Current Entry Value: {{.Value}}
{{end}}
```

In analogy to the above code example, you can access any other field as well.  
 These functions might come in handy when you are for example trying to compose a leaderboard.

### dbGetPattern and dbGetPatternReverse

These two functions allow you to get multiple entries under one user ID with matching keys, again using patterns. They return a slice of entries sorted by value, just as the above functions. The only difference here is only the limitation to one user/ID instead of all IDs.

```go
{{dbGetPattern <userID> <pattern> <amount> <nSkip>}}
{{/* or */}}
{{dbGetPatternReverse <userID> <pattern> <amount> <nSkip>}}
```

Just as above, we range over the given slice to access fields of the entry object. For simplicity's sake however, no code example, as it should be pretty clear how to do this.  


## Appendix

### Patterns

As mentioned earlier, we use patterns for a set of functions. Now, you rightfully ask yourself "what the hell are patterns?". Allow me to lift some confusion: We're lucky, it's not RegEx. Instead, there are only two special characters: `%` and `_`. More are not used, just those two. In case you need to escape those, prepend them with a backslash `\`.

#### What do those characters do?

Quite easy, actually:

* The percent sign `%` matches any sequence of zero or more characters
* The underscore `_` matches any single character

Okay, with that in mind, let's take a look at an example. The following pattern will match anything that starts with the letter `l` and ends in `n`.

```text
l%n
```

The following example showcases the usage of `_`.

```text
hel_o
```

This pattern matches words such as `hello`, `helgo`, `heloo`. I think it is pretty clear now what `%` and `_`, so let's get onto some tasks!

#### Task

1. Write a pattern that matches any string ending in `er`. 
2. Write a pattern that matches string having `h` as the second character, any at the start and lastly ending in `l`. Tip: between `h` and `l` you need to match any character sequence.

### Serialization

As you might have noticed from time to time, people have some "weird" code, such as the following:

```go
{{$sdict_received := sdict (dbGet 2000 "someKey").Value}}
```

This is because custom types, namely `sdict` and `cslice`, are being converted to their underlying data types `map[string]interface{}` and `[]interface{}` respectively.

They can be easily converted back to their original type by using their constructor method `sdict` or `cslice` when receiving them. For a more elaborate explanation, visit the [documentation](https://docs.yagpdb.xyz/reference/templates#custom-types).

