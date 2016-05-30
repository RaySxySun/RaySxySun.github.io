---
layout: post
title: 11. Useful & professional english in workplace
date: 2016-01-18
categories: en
tags: [en, workplace]
description:  
---

- delta 增量变量

    $ git push -f origin HEAD
    Counting objects: 15, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 804 bytes | 0 bytes/s, done.
    Total 6 (delta 5), reused 0 (delta 0)
    To git@github.com:RaySxySun/xxxxxx
     + a695c74...cfb90cf HEAD -> master (forced update)

---

- pass something on (to somebody)
	
    In C#, arguments can be passed to parameters either by value or by reference. 
	Much of the discount is pocketed(私吞了) by retailers instead of being passed on to customers.

---

- cancel out 抵消

    The pros and cons cancel out.

---

- -laden(used to form adjectives showing that something is full of, or loaded with, the thing mentioned)
    - feature-laden (multifunction)
        - Avoids feature-laden classes high up in the hierarchy
    - value-laden (influenced by personal opinions)
        - 'Freedom fighter' is a value-laden word.

---

- degenerate (adj.退化的,变质的 vt/vi. 使退化, 退化)
    - A decorator can be viewed as a degenerate(adj.) composite with only one component.

---

- Snippet: a small piece of information or news

    // Snippet of response
    
    "id":"43027995-e84d-d4df-15bf-5238fccf83ee"
    "name":"OXTON KVETON RIZAL"
    "primary_cmr":[]
    "cmr_number":""
    "default_cmr":""

---

- CMR(customer-managed relationship)
    - customer-managed relationship (CMR) is a relationship in which a business uses a methodology, software, and perhaps Internet capability to encourage the customer to control access to information and ordering. CMR can be viewed as an alternative to or as a possible approach to include in CRM (customer relationship management).

---

- walk sb through
    -to slowly and carefully explain something to someone or show someone how to do something
    - Hi folks, we will hold the Release R3.4 knowledge sharing session tomorrow between 7:30 PM and 8:00 PM at the training on the floor 3. Each team can walk us through changes/stories/even defects that your team has achieved during R3.4 from the business perspective or technical perspective. So all of us can get on the same page to learn from each other to avoid identical mistakes and gain knowledge. Let’s see which team is the best to do this for the first time.

---

- postpone
    - [meaning]to arrange for an event, etc. to take place at a later time or date 
    - [diff] delay: 
        - (n.) a period of time when sb/sth **has to** wait because of a problem that makes something slow or late
        - (v.)to not do something until a later time or to make something happen at a later time
            - delay doing something
            - delay something
            - delay somebody
    - [synonym] **put off**
        - to cancel a meeting or an arrangement that you have made with somebody
            - She put him off with **the excuse** (借口) that she had too much work to do.
        - to make somebody dislike somebody/something or not trust them/it
            - She's very clever but her manner does tend to put people off.
        - to disturb somebody who is trying to give all their attention to something that they are doing
            - Don't put me off when I'm trying to concentrate.
    - [example]We'll have to postpone the meeting until next week.

---

- undoable: can revert
    - Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.[Design Pattern: Command]
---

- prior to: before
    - during the week prior to the meeting

---

- exhaustive: full, detailed
    - Here is an exhaustive list of rules

- non-exhaustive:
    - Here is a non-exhaustive list of (optional) rule

---

- For the sake of: in order to get or keep something
    - For the sake of comprehension, I’ll assume that the data in the buffer are not locked by latches 

---

- end up: to find yourself in a place or situation that you did not intend or expect to be in
    - **end up doing something**: I ended up doing all the work myself.
    -  If you go on like this you'll end up in prison.
    -  If he carries on driving like that, he'll end up dead.

---

- end up with: Get as a result of something
    - He tried hard but ENDED UP WITH a poor grade.
---

- disappear, vanish, etc. into thin air: to disappear suddenly in a mysterious way
    - Durability ensures that Transaction 1 won’t disappear into thin air if the database crashes just after T1 is committed.

--- 

- ACID: Atomicity[ˌætəmˈɪsɪti], Isolation, Durability, Consistency

---

- granularity: is the extent to which a material or system is composed of distinguishable pieces or grains.
    - Of course a real database uses a more sophisticated system involving more types of locks (like intention locks) and more granularities (locks on a row, on a page, on a partition, on a table, on a tablespace) but the idea remains the same.

---

- overhead: regular costs that you have when you are running a business or an organization, such as rent, electricity, wages, etc.
    - there is no overhead from the “fat and slow” lock manager
    - High overheads mean small profit margins.

---

- Serializable: The SQL norm defines 4 levels of isolation
    - Serializable, Repeatable read, Read committed, Read uncommitted 

---

- Data versioning and locking are two different visions: optimistic locking vs pessimistic locking. 

---

- integrity: the state of being whole and not divided
    - Because the transaction has broken the integrity of the database (for example you have a UNIQUE constraint on a column and the transaction adds a duplicate)

---

- Database Log Record: Each operation (add/remove/modify) during a transaction produces a log. This log record is composed of
    - LSN(A unique Log Sequence Number), TransID, PageID, PrevLSN, UNDO, REDO

---

- mess: a condition in which things are dirty or not neat.
    - ARIES uses only logical UNDO because it’s a real mess to deal with physical UNDO.

---

- chronological order: chronological order
    - Each log has a unique LSN. The logs that are linked belong to the same transaction. The logs are linked in a chronological order (the last log of the linked list is the log of the last operation).

---

- clustered:
    - how to manage clustered databases and global transactions

- snapshot:
    - how to take a snapshot when the database is still running
- compress:
    - how to efficiently store (and compress) data

---

- rock-solid:
    - think twice when you have to choose between a **buggy** NoSQL database and a **rock-solid** relational database. 
    
