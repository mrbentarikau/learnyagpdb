---
description: >-
  Various Limits in YAGPDB custom commands for smooth functioning of the bot and
  misuse prevention.
---

# Custom Command Limits

## OVERALL

* **Max amount of CCs:** 100/250 \(free/prem\)
* **Max CCs that can be triggered by a single action:** 3/5 \(free/prem\)
* **Character limit:** 10k \(5k for join/leave msg, warn dm, etc...\)
* **Limit writer:** 25kb
* **Max operations:** 1/2.5kk \(free/prem\)
* **Response Character Limit:** 2k
* **Generic API based Action call limit :** 100 per CC
* **State Lock based Actions :** 500 per CC \(mentionRoleName/ID ; hasRoleName ; targetHasRoleName/ID\)

## CALLING A CC

### execCC

* **Calls per CC:** 1/10 \(free/prem\) -&gt; counter key "runcc"
* **StackDepth limit:** 2 \(executing with 0 delay\)
* **Delay limit:** int64 limit \(292 years\)

### scheduleUniqueCC

* **Calls per CC:** 1/10 \(free/prem\) -&gt; counter key "runcc"
* **Delay limit:** int64 limit \(292 years\)
* There can only be 1 per server per key

### cancelScheduledUniqueCC

* **Calls per CC:** 10/10 \(free/prem\) -&gt; counter key "cancelcc"

## DATABASE

### Overall Limits

* **Max amount of DBs:** Membercount \*50\*1/10\(free/prem\)
* **Key length limit:** 256
* **Expire limit:** int64 limit \(292 years\)
* **Value size limit:** 100kb

### Database Interactions

* **Calls per CC:** 10/50 \(free/prem\) -&gt; counter key "db\_interactions"
* Valid for all database commands -&gt;  
  * dbSet/dbSetExpire
  * dbGet
  * dbDel/dbDelByID
  * dbIncr
  * dbGetPattern/dbTopEntries
  * dbCount

### Database Multiple Entry Interactions

* **Calls per CC:** 2/10 \(free/prem\) -&gt; counter key "db\_multiple"
* Valid for all database multiple entry related commands -&gt;
  * dbGetPattern/dbTopEntries
  * dbCount

## CONTEXT

* **Max file size \(complexMessage\):** 100kb
* **joinStr max string length:** 1000kb
* **sendDM:** 1 call per CC -&gt; counter key "send\_dm"
* **sendTemplate/sendTemplateDM:** 3 calls per CC -&gt; counter key "exec\_child"
* **addReactions:** 20 calls per CC -&gt; counter key "add\_reaction\_trigger". Each reaction added counts towards the limit.
* **addResponseReactions:** 20 calls per CC -&gt; counter key "add\_reaction\_response". Each reaction added counts towards the limit.
* **addMessageReactions:** 20 calls per CC -&gt; counter key "add\_reaction\_message". Each reaction added counts towards the limit.
* **delMessageReaction: 1**0 calls per CC -&gt; counter key "del\_reaction\_message". Each removed added counts towards the limit.
* **editChannelName/Topic:** 10 calls per CC -&gt; counter key "edit\_channel"
* **regex cache limit:** 10 \(this means you cant have more than 10 different regexes on a CC\)
* **onlineCount:** 1 call per cc -&gt; counter key "online\_users"
* **onlineCountBots:** 1 call per cc -&gt; counter key "online\_bots"
* **editNickname:** 2 calls per cc -&gt; counter key "edit\_nick"
* **Append/AppendSlice limit:** 10k size limit of resulting slice
* **userArg:** 5 calls per cc -&gt; counter key "commands\_user\_arg"
* **exec/execAdmin:** 5 calls per cc -&gt; no key
* **deleteResponse/deleteMessage/deleteTrigger max delay:** 86400s
* **take/removeRoleID/Name max delay:** int64 limit \(292 years\)
* **sleep:** 60 seconds

