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

---

- hit the ground running: to be ready to work immediately on a new activity
    - His previous experience will allow him to hit the ground running when he takes over the Commerce Department.


---

- lead the pack: to be first or best of a group
    -  For the second week in a row, the new Star Wars movie leads the pack at the box office.

---

- low-hanging fruit:  a course of action that can be undertaken quickly and easily as part of a wider range of changes or solutions to a problem
    - first pick the low-hanging fruit.
    - Reducing HTTP requests and optimizing database queries are low-hanging fruit that noticeably improve application performance and response time.

---

- infallible: a person or thing that is incapable of error or failure
    - HHVM is just fine for your production server, but it’s not infallible. I recommend you keep tabs on HHVM’s master process with Supervisord.

---

- HHVM: The first initiative is HHVM, or the Hip Hop Virtual Machine.
    - This alternative PHP engine was released in October 2013. Its just-in-time (JIT) compiler provides performance many times better than PHP-FPM.

---

- seamless: having no seams;
    - Hack is a server-side language that is similar to and seamless with PHP.

---

- dialect:
    - Hack’s developers even call Hack a dialect of PHP.

---

- Zend Engine:
    - HHVM is the first true competitor to the traditional Zend Engine PHP runtime.

---

- reap:To harvest
    - Both HHVM and the Zend Engine will improve, and PHP developers will reap the benefits.


---

- bee's knees: informal an excellent or ideally suitable person or thing

---

- it pays to:
    - static factory methods and public constructors both have their uses, and it pays to understand their relative merits


---

- in preference to: 优于，优先于
    - Avoid creating unnecessary objects by using static factory methods (Item 1) **in preference** to constructors on immutable classes that provide both.
    - In the example that follows, a static member class is used in preference to an anonymous class to allow the concrete strategy class to implement a second interface, Serializable

- be preferable to: 优于
    - the static factory method Boolean.valueOf(String) **is almost always preferable to** the constructor Boolean(String) .

---

- performance gains: 性能提升
    - **Using a static initializer** results in significant performance gains if the method is invoked frequently.

---

- puzzling over: 苦恼；冥思苦想
    - It is not uncommon for a programmer to write an equals method that looks like this, and then spend hours puzzling over why it doesn’t work properly:

---

- To avoid the duplication of effort: 避免不必要的
    - To avoid the duplication of effort and save memory, you can add common properties and methods to the prototype property of the constructor

---

- is padded with: 用... 填充
- leading zeros: 前置0
    - If this number is too small to fill up its field, the field is padded with leading zeros. For example, 123's representation will be 0123

---

- resorting to: 求助于；诉诸与；
    - Unfortunately, the Cloneable interface fails to serve this purpose (permit cloning). Its primary flaw is that it lacks a clone method, and Object ’s clone method is protected. You cannot, without resorting to reflection (Item 53), invoke the clone method on an object merely because it implements Cloneable.
    - Even a reflective invocation may fail

---

- it pays to:值得去花时间
    - Despite this flaw and others, the facility is in wide use so it pays to understand it.

---

- spell out: 讲清楚
- as of： 自从...起
    - The Cloneable interface does not, as of release 1.6, spell out in detail the responsibilities that a class takes on when it implements this interface.

---

- consist of: 由...构成
    - The hash table's internals consist of an array of buckets

---

- approach to doing: 做...的方法
- resulting: 结果的
    - A final approach to cloning complex objects is to call super.clone, set all of the fields in the resulting object to their virgin state

---

- risk-prone: 有风险的
    - The copy constructor approach and its static factory variant don’t rely on a risk-prone extralinguistic object creation mechanism

---

- associated with: about
    - Given all of the problems associated with Cloneable , it’s safe to say that other interfaces should not extend it

---

- pretty: 格式优雅
    - If you run the application now, you should see a list of notes. it's not particularly pretty or useful yet, but it's a start

---

- shoot yourself in the foot:  搬起石头砸自己的脚
- indiscriminately: adv. 不加选择地；任意地
    - You should not kill processes indiscriminately, especially if you don't know what they're doing. You may be shooting yourself in the foot.

---

- scratch the surface: 触及表面; 还停留在肤浅的表面
    - This discussion has barely scratched the surface of how to use disks and other storage devices on Linux System

- from scratch: 白手起家；从头做起
  - This page will outline the steps necessary to generate a full set of indexes from scratch based on consumption of a new database image in to an environment.

---

- put off: 打扰；使苦恼；使心烦; 干扰
    - Don’t be put off by the mathematical nature of this contract.

---

- go over: 复习; 查看; 研究;
    - Let’s go over the provisions of the compareTo contract.

---

- forgo: (vi, vt) 放弃；停止；对…断念
    - there is no way to extend an instantiable class with a new value component while preserving the compareTo contract, unless you are willing to forgo the benefits of object-oriented abstraction

---

- in place of: 代替
    - sorted collections use the equality test imposed by compareTo in place of equals .

---

- work your way down: 然后按您的方式往下做
    - You must start with the most significant field and work your way down.
    - An easy approach is to start at the top of the XML document and work your way down.

---

- feel pretty comfortable with:
- dive into
    - If you’ve read all those titles and you feel pretty comfortable with those topics, it’s time we dive into the evolution of JS to explore all the changes coming not only soon but farther over the horizon.

---

- wait around: 呆呆地等；空等
    - It’s not just waiting around for years for some official document to get a vote of approval, as many have done in the past.

---

- would recommend doing:
    - I’d recommend using just one let Declarations.

---

- enclose: To surround on all sides; close in
    - a valley that is enclosed by rugged peaks.
    - traditional var-declared variables are attached to the entire enclosing function scope

---

- old-school: 老派, 老式学院派
    - doing things in the old-school pre-ES6 way

---

- fuzzy: 模糊的
    - If that’s still a bit fuzzy, go back and read it again, and play with this yourself.

---

- be thought of as: 被认为是
    - super is typically thought of as being only related to classes.

---

- resolve to: To become separated or reduced to constituents.
    - super here would basically be Object.getPrototypeOf(o2), resolves to o1 of course

---

- dereferenced: 解除引用
    - "A" will remain in memory until the timeout is triggered and the function is dereferenced

                    // Memory leaks
                    (function outerFunction() {
                        var A = 'some variable';
                        setTimeout(function(){ alert('I have access to A whether I use it or not'); }, 5);
                    })();

---

- Meh[me]: Used to express indifference, apathy, or boredom.

---

- trained eye: 群众的眼睛是雪亮的
    - The average person would perhaps not notice, but to the trained eye and ear there is plenty happening.

---

- is inversely proportional to: 成反比的
    - I’d say that the readability gains from => arrow function conversion are inversely proportional to the length of the function being converted

---

performance hit:

---

- Some things to note:
- It’s also important to note that: 我们应该注意到
    - It is not insignificant to note that super behaves differently depending on where it appears.
    - Before going into the details of how a system call works, we note some general points

---

- a match: 一个匹配
    - The Regular Expression does not have a ^ start-of-input anchor, free to move ahead to look for a match.

---

- ad hoc: adj. 特别的 adv.特别地
    - JS developers have been ad hoc designing and implementing iterators in JS programs since before anyone can remember, so it’s not at all a new topic.

- on an ad hoc basis: 权宜之计
    - The meetings will be held on an ad hoc basis

---

- when considering: 当考虑
    - iterators can also be thought of as controlling behavior one step at a time. This can be illustrated quite clearly when considering generators

---

- go beyond: v. 超出；胜过
- **completed** signal: 完成信号
    - You have to call next() again, in essence going beyond the end of the array’s values, to get the completed signal done: true .

---

- be tempted to: 很想; 忍不住;
    - Since there can only be one default per module, you may be tempted to design your module with one default export of a plain object with all your API methods on it.

                    export default {
                        foo() { .. },
                        bar() { .. },
                        ..
                    };

---

- in exchange for: 作为…的交换
    - Many developers would be quick to point out that such approaches can be more tedious, requiring you to regularly revisit and update your import statement(s) each time you realize you need something else from a module. The tradeoff is in exchange for convenience.

---

- off the bat: immediately
    - Let me say right off the bat that I don't blame you for this problem. I know who you mean, but I can't think of his name right off the bat.

---

- syntactic sugar
- the strong tide of:
    - Even though JS’s prototype mechanism doesn’t work like traditional classes, that doesn’t stop the strong tide of demand on the language to extend the syntactic sugar so that expressing “classes” looks more like real classes.

---

- in the habit of: 有…习惯
    - It means that if you’re in the habit of taking a method from one “class” and “borrowing” it for another class by overriding its this , say with call(..) or apply(..) , that may very well create surprises if the method you’re borrowing has a super in it.

---

- This is an important detail to note: 这是一个重要的细节要注意

---

- deviation/limitation: 差别/限制
    - Another perhaps surprising deviation/limitation of ES6 subclass constructors
- It boils down to: 简单说来; 归结为
    - you cannot access "this" until super(..) has been called. it boils down to the fact that the parent constructor is actually the one creating/initializing your instance’s this .

---

- artifact: 工件
    - An artifact is a file, usually a JAR, that gets deployed to a Maven repository.
    - A Maven build produces one or more artifacts, such as a compiled JAR and a "sources" JAR
    - Each artifact has a group ID (usually a reversed domain name, like com.example.foo), an artifact ID (just a name), and a version string. The three together uniquely identify the artifact.
    - A project's dependencies are specified as artifacts.

---

- it is not permitted to: 不允许
    - If a method overrides a superclass method, it is not permitted to have a lower access level in the subclass than it does in the superclass

---

- if any, : 若有的话; 如果有的话
    - The doctor will follow up the patient in future for occurrence of disease, if any.

---

- Another way of thinking about
    - Another way of thinking about a promise is as an event listener, upon which you can register to listen for an event that lets you know when a task has completed.

- way of conceptualizing:
    - Yet another way of conceptualizing a Promise is that it’s a future value, a time-independent container wrapped around a value.

---

- goes for: (1)apply to
- The same goes for: 这同样适用于
    - Children are not allowed and that goes for senior citizens, too.
    - p1 and p2 will essentially identical behavior. The same goes for resolving with a promise

                    var p1 = Promise.resolve( 42 );
                    var p2 = new Promise( function pr(resolve){ resolve( 42 ); });

- goes for: (2)agree;  give an affirmative reply to; respond favorably to;
    - I go for this resolution

---

- be much more preferable: 即更为可取; 更好的;
    - However, there’s a much better option for expressing async flow control, and it will probably be much more preferable in terms of coding style than long promise chains.

---

- It’d be tempting to: 容易让人有同意的倾向
- assume: 想当然的认为
    - It’d be tempting to look at a feature named “typed array” and assume it means an array of a specific type of values, like an array of only strings.

---

- wherever possible:    
    - Immutable classes should take advantage of this by encouraging clients to reuse existing instances wherever possible.

---

- suppose that: 假如; 假设
    - For example, suppose that you have a million-bit BigInteger and you want to change its low-order bit

---

- is inversely proportional to: 成反比的
- proportional to: 与……成比例
    - The operation requires time and space proportional to the size of the BigInteger

---

- potential: n. 可能性
    - Immutable classes provide many advantages, and their only disadvantage is the potential for performance problems under certain circumstances.

---

- proceed with: 继续进行
    - Please proceed with what you are doing.
    - The mocks demonstrate that the tests can pass, serves as prototypes for the provider teams, and enables the coordinator teams to proceed with development

---

- leave sb with:
    - . If you use abstract classes to define types, you leave the programmer who wants to add functionality with no alternative but to use inheritance.

---

- irritate
- deficient

---

- identify: 识别
    - This numbering scheme is not normally visible to programs, which identify system calls by name.

- specify: 指定
- vice versa
    - Each system call may have a set of arguments that specify information to be transferred from user space to kernel space and vice versa.

---

- obsolete['ɒbsəliːt]: (adj.) 废弃的
    - cmr_id on opportunity is obsolete but still active in the code and ux, next sprint

---

- be jumbled together: 乱七八糟地混做一团浆糊
    - Readability is further harmed because multiple implementations are jumbled together in a single class. (one shortcoming of tagged classes)

---

- appropriate: [ə'prəʊprɪət] adj. 适当的；恰当的；合适的
    - tagged classes are seldom **appropriate**.

---

- be tempted to: 忍不住;
    - If you’re tempted to write a class with an explicit tag field, think about whether the tag could be eliminated and the class replaced by a hierarchy.

---

- more than sufficient: 绰绰有余
    - A decent drawing program and a word processor are more than sufficient for our purposes.
    - agriculture is now so mechanized that only about 2 percent of American workers make a living as farmers who can feed the whole country more than sufficient.

---

- Only after...was/were there: 从..之后 才有了
    - Only after the law of visual stay was discovered was there the art of cartoon and animation and only after the invention of projectors was made was there the boom of silver screen

---

- complement and facilitate: 补充和促进
    - art is as weighty as science and technology to human advancement and they actually complement and facilitate each other.

---

- enclosing:
    - A nested class should exist only to serve its enclosing class. (Item 22)

---

- All but the first: 除了第一个所有的
    - All but the first kind are known as inner classes.
    - (static member classes, nonstatic member classes, anonymous classes, and local classes)

---

- happen to be: 碰巧是；恰巧是
    - [A static member class] It is best thought of as an ordinary class that happens to be declared inside another class and has access to all of the enclosing class’s members, even those declared private.

---

- in the declaration:
    - the only difference between static and nonstatic member classes is that static member classes have the modifier static in their declarations.

---

- in isolation: 孤立地；绝缘
    - If an instance of a nested class can exist in isolation from an instance of its enclosing class, then the nested class must be a static member clas

---

- thereafter: 其后；从那时以后
    - The association between a nonstatic member class instance and its enclosing instance is established when the former is created; it cannot be modified thereafter.

---

- in question: 讨论中的；成问题的；考虑中的
    - It is doubly important to choose correctly between a static and a nonstatic member class if the class in question is a public or protected member of an exported class.

---

- in the midst of: 在...之中
    - Because anonymous classes occur in the midst of expressions, they must be kept short—about ten lines or fewer—or readability will suffer.

---

- on the fly: While activity is ongoing
    - One common use of anonymous classes is to create function objects (Item 21) on the fly.

- on the fly:
    - 1. In a hurry or between pressing activities: took lunch on the fly.
    - 2. While moving: The outfielder caught the ball on the fly.
    - 3. In the air; in flight: The ball carried 500 feet on the fly.
    - 4. While activity is ongoing: A coach can change players on the fly in hockey. This computer program compiles on the fly when a script is executed.

---

- in common with: 与...一样; 与...有共同之处
    - Local classes have attributes in common with each of the other kinds of nested classes.

- in common: 共同的
    - For example, suppose you want to write a method that takes two sets and returns the number of elements they have in common.

---

- leave something to be desired: 不够完美的
    - Admittedly this error message leaves something to be desired, but the compiler has done its job, preventing you from corrupting the collection’s type invariant.
- leave much to be desired: 有很大改进空间
- leave nothing to be desired: 完美无缺, 尽善尽美

---

- in place of: 替换
    - The use of unbounded wildcard types in place of raw types does not affect the behavior of the instanceof operator in any way.

---

- opt(s) out of: 决定(从...) 退出
    - Set is a raw type, which opts out of the generic type system

---

- familiarize yourself with: 使...熟悉
    - Before you start, read Studio basics to familiarize yourself with the Studio environment and the terminology used, then refer to the topics in the help to find out how to complete specific tasks.

---

- comment out: 注释掉
    - A common developer practice is to comment out a code snippet, meaning to add comment syntax causing that block of code to become a comment, so that it will not be executed in the final program.

- uncomment:取消备注
    - If you want to test your controllers directly in your programming environment, you can uncomment the debug.

---

- One, but not both, of: 一个而不是两个都
    - [Format] awk 'pattern + {action}'
    - One, but not both, of pattern {action} can be omitted.

---

- present: (后置) 存在的
    - we only have to remove it if it is present; otherwise, insert it.

- existing: (前置) 现存的, 已有的
    - A renderer that delivers the output of the existing function in a new format.

---

- desired: 期望的, 期待的
    - Since the desired behavior is to toggle it on and off, we only have to remove it if it is present; otherwise, insert it.

---

- leave out/off:
  - Experienced JavaScript programmers learn to look at the line following a statement whenever they want to leave out a semicolon.
  - Leaving off a var/let/const declarator

---

- ready: (vt) 使准备好
  - I will use this wiki as the outline of what steps are taken to ready a test environment for its next release assignment.

---

- goes to: 发布到  (= is successfully deployed to)
  - This will occur every time a major release goes to production.

- goes live: 上线
  -  Using the current releases as an example, when R3.6 goes live it means the test system used for R3.5.0.X now will be come the N+2 release test environment and as such needs to be readied for that role.
---

- Using .. as an example: 以..为例
  - Using the current releases as an example, when R3.6 goes live it means the test system used for R3.5.0.X now will be come the N+2 release test environment and as such needs to be readied for that role.

---

- is tasked with doing: 是负责
  - This means that what was N+1 is now N and the test environment used for that release now is tasked with doing all of the production support for the weekly fix deliveries and any SEV 1 deliveries.

---

- be rotated to: 被转换成
  - The environment previously used for production support is now rotated to be the N+2 test environment

---

- after a successful Release deployment: 一次成功的发布之后
  - On the Monday after a successful Release deployment you will stop cast iron, apache on all web servers, optimizer, etc so that the system is no longer available to those that may have been using it. (This is because its no longer the valid environment for matching production as well as the first step to ready it for its new DB.)

---

- make sb do: 让某人去做某事
  - Once the new masked production database from the release deployment is made available restore that database to the servers and get them up in PEER status.

- get up: To find within oneself
  - Once the new masked production database from the release deployment is made available restore that database to the servers and get them up in PEER status.

> 1. To arise from bed or rise to one's feet: She got up and opened the door.
> 2. To climb: How long will it take to get up the mountain?
> 3. To act as the creator or organizer of: got up a petition against rezoning.
> 4. To dress or adorn: She got herself up in a bizarre outfit.
> 5. To find within oneself; summon: got up the nerve to quit. 鼓起勇气出去

---

- HADR: 高可用性灾难恢复(High Availability Disaster Recovery)
  - This is for US HADR cluster, for EU database would be a simple restore

---

- take from: 从...抽取
- be unique to: 独一无二 ; 是唯一的
  - When ever taking a database from another system you will have to execute the set of scripts that define the EU database server as well as the passwords for all the users as they would be unique to each environment

---

- on a server: 在某台服务器上
  -  In addition to the database there is a copy of the web image provided from production to match the database.  You will restore this on the services web server.(svt<x>ws01)

---

- be in use: 在使用中
- in use: 使用中
  - Using the masked config, config_override and sfa.variables files as the starting point, update them to be appropriate for the test environment in use.

---

- with .adj: 随着..  
  - With the databases restored and web server up and functional you can now start to
  - 随着数据库被恢复了, web服务器开启并且可用了, 你可以开始生成

---

- matching: adj. 相匹配的
- go with: 与…相配
  - With the databases restored and web server up and functional you can now start to generate the set of elastic search indexes so that you have a matching set of indexes to go with the database

---

- complete: adj. 完整的；完全的；彻底的
- consume: vt. 使用, 消耗
  - Once indexes are complete, use the esgrabber system to create a backup for this starting point and so that it is available for other systems that will consume this database

---

- catch up to: 赶上
  - You are now ready to perform any necessary deployments to catch up to production as well as the N+1 release and finally the latest build for the N+2 release that this environment is intended to support

---

- regarding: = about
  - The RTC_2423 process is outlined in the page regarding creating input files and provisioning users.

---

- be deemed: 被认为
  - So long as the vast majority are successful its deemed a success and just wait to see if any users complain about access not working and address accordingly.

---

- provision:
  - (vt) 供给provision；提供provision; 以……供给provision
    - Similarly but only once cast iron is also up and running you can provision the latest set of BP test users.
  - (vi) To take preparatory action or measures 采取预备方案
    - A bank must provision against losses from bad loans.

---

- in greater detail: 更详细地
  - hat too is outlined in another page in greater detail and I will be uploading my latest set of xml files to that as part of my transition.

---

- on the same page: 意见一致;
  - just so everyone is on the same page on what days and time to expect deployments to SVT4/5/6

---

- next feat: 我的下一项工作
  - my next feat will be to try and get sugar to change their sev1 delivery time to better align with your schedules

---

- outline: 列举出
- regarding: 关于
  - This page is to outline the different types of deploys and general rules regarding what gets deployed during these types of deployments.

---

- deploy/patch: 一个部署, 补丁
- expedient: adj.权宜的
	- A severity 1 deploy/patch could occur at anytime and generally is needed the most **expedient path** to production as possible.

---

- downtime: 停机时间
	- they require either no downtime or minimal down time during the week. 

--- 

- is applicable to: 适用于
	- If the patch only effects the SC4IBM user server then only apply the patch to that server.
- apply sth to sth: apply the patch to 将这个补丁应用到
	- When QRR is involved then you have to apply the patch to ALL web servers.

---

- deprecated: 弃用
	- Since this code is deprecated it cannot support the new Roles that come available in the latest release and future releases so for now you have to manually update those after the input file is created.

---

- proceed: vi. 开始；继续进行；发生；行进

- But before we proceed, it is important to correct a common misapprehension that async is faster than multithreading

---

- be processed: 被处理
- hand control to: 把控制权移交给...
	- All event notifications are processed in the event loop when it calls select. Hence fetch must hand control to the event loop, so that the program knows when the socket has connected. 

---

- dive deeper: 更深入地了解
	- he subsequent articles in this series dive deeper and provide implementation examples.

---
- afterwards: 
	- you may have more questions afterwards

- beforehand: in advance - 提前; 事先，预先;

- back on track: **回到刚才的话题** ;重回正轨; 重上轨道; 改过自新
	- It may take some time to get the British economy back on track

- wouldn't use:
	- you wouldn't use it

---

-heads up:  as a warning to watch out for a potential source of danger, as at a construction site.
    - thanks for the heads up
