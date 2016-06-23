---
layout: post
title: PHP Good Practice
date: 2016-07-19
weather: cloudy
categories: php
tags: [php]
description:
---

# Good Practice


## 1.Sanitize, Validate, and Escape

> Trust no one: Never trust any data that originates from a source not under your direct control.

> [$\_GET, $\_POST, $\_REQUEST, $\_COOKIE, $argv, php://stdin, php://input, file_get_contents(), Remote databases, Remote APIs, Data from your clients]


| Number    | Practice     | Do | Don't
| :------------- | :------------- |:------------- |
| 1       | Sanitize Input HTML   | (1) htmlentities($input, ENT_QUOTES, 'UTF-8');|
|         |                       | (2) use the HTML Purifierlibrary|
| 2       | Sanitize Input SQL    | PDO prepared statement|
|3|Sanitize User profile information|filter_var($email, FILTER_SANITIZE_EMAIL);|
|4|Validate Data|(1)aura/filter (2)respect/validation (3)symfony/validator|
|5|Escape Output|echo htmlentities($output, ENT_QUOTES, 'UTF-8');|

note: twig/twig or smarty/smarty escape output automatically.

##2.Passwords

- Never Know User Passwords
- Never Restrict User Passwords
  - If you must restrict user passwords, I recommend you only require a minimum length
- Never Email User Passwords (If you send)
  - you know my password
  - you are storing my password in plain text or in a decryptable format;
  - you have no qualms sending my password over the Internet in plain text
- Hash User Passwords with bcrypt(future-proof)
  - should hash (one-way) user passwords. Do not encrypt(two-way) user passwords
  - Many hashing algorithms are available (e.g., MD5, SHA1, bcrypt, scrypt).
- Password Hashing API (password hashing API available in PHP 5.5.0)
  - hashing
    - URL-encoded HTTP POST request
    - $password = filter_input(INPUT_POST, 'password')
    - if(!$password \|\| mb_strlen($password) < 8) {throw new Exception('Password must contain 8+ characters')}
    - $passwordHash = password_hash($password,PASSWORD_DEFAULT,['cost' => 12]);
  - verification
    - if (password_verify($password, $user->password_hash) === false){throw new Exception('Invalid password');}
    - (optional) $passwordNeedsRehash = password_needs_rehash($user->password_hash,PASSWORD_DEFAULT, ['cost' => 15]);
  - Password Hashing API for PHP < 5.5.0
    - use Anthony Ferrara’s ircmax ell/password-compat component.
    - password_hash(), password_get_info(), password_needs_rehash(), password_verify()

## 3.Dates, Times, and Time Zones

- Set a Default Time Zone
  - (php.ini)date.timezone = 'America/New_York';
  - (php)date_default_timezone_set('America/New_York');
  - [Time-zone identifier](http://php.net/manual/timezones.php)
- The DateTime Class
  - $datetime = new DateTime();
  - $datetime = new DateTime('2014-04-27 5:03 AM');
  - $datetime = DateTime::createFromFormat('M j, Y H:i:s', 'Jan 2, 2014 23:04:12');
- The DateInterval Class
  - $datetime = new DateTime('2014-01-01 14:00:00');
  - $interval = new DateInterval('P2DT5H2M'); //two days, five hours, and two minutes.
  - $datetime->add($interval);
  - echo $datetime->format('Y-m-d H:i:s');
  - $datePeriod = new \\DatePeriod($dateStart, $dateInterval, 3);
  - [Brian Nesbitt’s nesbot/carbon PHP component.](https://github.com/briannesbitt/Carbon)

## 4.Databases

> MySQL, PostgreSQL, SQLite, MSSQL, and Oracle.
> eg. mysqli extension which adds various mysqli_*() functions to the PHP language

- The PDO Extension (PHP data objects)
  - downside to PDO (it provides a single interface to different databases,we still must write our own SQL statements.)
- Database Connections and DSNs
  - (Must) [1]Hostname or IP address [2]Port number [3]Database name [4]Character set

            <?php
              try {
                $pdo = new PDO(
                    'mysql:host=127.0.0.1;dbname=books;port=3306;charset=utf8',
                    'USERNAME',
                    'PASSWORD'
                    );            
                  } catch (PDOException $e) {
                    // Database connection failed
                    echo "Database connection failed";
                    exit;
                  }

- Prepared Statements [PDO::PARAM_BOOL, PDO::PARAM_NULL, PDO::PARAM_INT, PDO::PARAM_STR (default)]
  - $sql = 'SELECT id FROM users WHERE email = :email';
  - $statement = $pdo->prepare($sql);
  - $email = filter_input(INPUT_GET, 'email');
  - $statement->bindValue(':email', $email); // == $statement->bindValue(':email', $email, PDO::PARAM_STR);

- Query Results  

            <?php
            // Build and execute SQL query
              $sql = 'SELECT id, email FROM users WHERE email = :email';
              $statement = $pdo->prepare($sql);
              $email = filter_input(INPUT_GET, 'email');
              $statement->bindValue(':email', $email, PDO::PARAM_INT);
              $statement->execute();
              // Iterate results
              while (($result = $statement->fetch(PDO::FETCH_ASSOC)) !== false) {
                  echo $result['email'];
              }

- Transactions
  - $pdo->beginTransaction()
  - $pdo->commit()

## 5. Multibyte Strings

> an 8-bit character that occupies a single byte of memory. (128 characters ASCII set)

> However you may work with non-English characters

- Character Encoding
  - Use UTF-8.
  - Output UTF-8 Data
    - [php.ini] default_charset = "UTF-8"
    - [php] header('Content-Type: application/json;charset=utf-8');
    - [html] <meta charset="UTF-8"/>

## 6. Streams

> introduced in PHP 4.3.0

- Stream Wrappers
  - (1) Open communication. (2) Read data (3) Write data (4) Close communication
  - <scheme>://<target>
  - [HTTP stream wrapper]

          $json = file_get_contents( 'http://api.flickr.com/services/feeds/photos_public.gne?format=json' )

  - [File stream wrapper]  file:// stream wrapper
    - Implicit file:// stream wrapper
    - Explicit file:// stream wrapper

          $handle = fopen('file:///etc/hosts', 'r');
          while (feof($handle) !== true) {
            echo fgets($handle);
          }
          fclose($handle);

   - php://stdin, php://stdout, php://memory, php://temp
   - fopen() , fgets() , fputs() , feof() , fclose() , and other PHP filesystem functions are NOT for filesystem files only.

  - Stream Context
    - $context = stream_context_create($options)
    - $response = file_get_contents('https://example/users/rest/v10/oauth-token', false, $context)

  - Stream Filters
    - So far we’ve talked about opening, reading from, and writing to PHP streams. However, the true power of PHP streams is filtering, transforming, adding, or removing stream data in transit.
    - [separately]stream_filter_append($handle, 'string.toupper')
    - [with wrapper]$handle = fopen('php://filter/read=string.toupper/resource=data.txt', 'rb')
      - filter/read=<filter_name>/resource=<scheme>://<target>

              <?php
              $dateStart = new \DateTime();
              $dateInterval = \DateInterval::createFromDateString('-1 day');
              $datePeriod = new \DatePeriod($dateStart, $dateInterval, 30);
              foreach ($datePeriod as $date) {
                $file = 'sftp://USER:PASS@rsync.net/' . $date->format('Y-m-d') . '.log.bz2';
                if (file_exists($file)) {
                    $handle = fopen($file, 'rb');
                    stream_filter_append($handle, 'bzip2.decompress');
                    while (feof($handle) !== true) {
                      $line = fgets($handle);
                      if (strpos($line, 'www.example.com') !== false) {
                        fwrite(STDOUT, $line);
                      }
                    }
                    fclose($handle);
                }
              }

  - Custom Stream Filters
    - Custom stream filters are PHP classes that extend the php_user_filter built-in class. The custom stream class must implement the filter() , onCreate() , and onClose() methods
    - A PHP stream subdivides data into sequential buckets, and each bucket contains a fixed amount of stream data (e.g., 4,096 bytes)

                class DirtyWordsFilter extends php_user_filter
                {
                  /**
                  * @param resource $in       Incoming bucket brigade
                  * @param resource $out      Outgoing bucket brigade
                  * @param int                $consumed Number of bytes consumed
                  * @param bool               $closing Last bucket brigade in stream?
                  */

                  public function filter($in, $out, &$consumed, $closing)
                  {
                    $words = array('grime', 'dirt', 'grease');
                    $wordData = array();
                    foreach ($words as $word) {
                      $replacement = array_fill(0, mb_strlen($word), '*');
                      $wordData[$word] = implode('', $replacement);
                    }
                    $bad = array_keys($wordData);
                    $good = array_values($wordData);
                    // Iterate each bucket from incoming bucket brigade
                    while ($bucket = stream_bucket_make_writeable($in)) {
                      // Censor dirty words in bucket data
                      $bucket->data = str_replace($bad, $good, $bucket->data);
                      // Increment total data consumed
                      $consumed += $bucket->datalen;
                      // Send bucket to downstream brigade
                      stream_bucket_append($out, $bucket);
                    }
                    return PSFS_PASS_ON;
                  }
                }

                // ADD CUSTOM STREAM FILTER
                stream_filter_register('dirty_words_filter', 'DirtyWordsFilter');

                // EXCUTE
                <?php
                $handle = fopen('data.txt', 'rb');
                stream_filter_append($handle, 'dirty_words_filter');
                while (feof($handle) !== true) {
                  echo fgets($handle); // <-- Outputs censored text
                }
                fclose($handle);
