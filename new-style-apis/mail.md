- [Configuration](#mailer-configuration)
- [Basic Usage](#mailer-basic-usage)
- [Embedding Inline Attachments](#embedding-inline-attachments)
- [Mail & Local Development](#mailer-and-local-development)
- [Working along with the classic Mailer Helper](#mailer-and-classic-helper)

<a name="mailer-configuration"></a>
## Configuration

The new **Mailing API** provides a clean, simple API over the popular [SwiftMailer](http://swiftmailer.org) library. The mail configuration file is **app/Config/Mail.php**, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global `from` address for all messages delivered by the library. You may use any SMTP server you wish. If you wish to use the PHP `mail` function to send mail, you may change the `driver` to `mail` in the configuration file. A `sendmail` driver is also available.

<a name="mailer-basic-usage"></a>
## Basic Usage

The `Mailer::send` method may be used to send an e-mail message:

```php
Mailer::send('Emails/Welcome', $data, function($message)
{
   $message->to('foo@example.com', 'John Smith')->subject('Welcome!');
});
```
The first argument passed to the `send` method is the name of the view that should be used as the e-mail body. The second is the `$data` that should be passed to the view, and the third is a Closure allowing you to specify various options on the e-mail message.

> **Note:** A `$message` variable is always passed to e-mail views, and allows the inline embedding of attachments. So, it is best to avoid passing a `message` variable in your view payload.

You may also specify a plain text view to use in addition to an HTML view:

```php
Mailer::send(array('html.view', 'text.view'), $data, $callback);
```
Or, you may specify only one type of view using the `html` or `text` keys:

```php
Mailer::send(array('text' => 'view'), $data, $callback);
```
You may specify other options on the e-mail message such as any carbon copies or attachments as well:

```php
Mailer::send('Emails/Welcome', $data, function($message)
{
    $message->from('us@example.com', 'Nova Framework');

    $message->to('foo@example.com')->cc('bar@example.com');

    $message->attach($pathToFile);
});
```
When attaching files to a message, you may also specify a MIME type and / or a display name:

```php
$message->attach($pathToFile, array('as' => $display, 'mime' => $mime));
```
> **Note:** The message instance passed to a `Mail::send` Closure extends the SwiftMailer message class, allowing you to call any method on that class to build your e-mail messages.

<a name="embedding-inline-attachments"></a>
## Embedding Inline Attachments

Embedding inline images into your e-mails is typically cumbersome; however, Nova Framework provides a convenient way to attach images to your e-mails and retrieving the appropriate CID.

#### Embedding An Image In An E-Mail View
```php
<body>
    Here is an image:

    <img src="<?php echo $message->embed($pathToFile); ?>">
</body>
```

#### Embedding Raw Data In An E-Mail View
```php
<body>
    Here is an image from raw data:

    <img src="<?php echo $message->embedData($data, $name); ?>">
</body>
```

> Note that the `$message` variable is always passed to e-mail views by the `Mail` class.

<a name="mail-and-local-development"></a>
## Mail & Local Development

When developing an application that sends e-mail, it's usually desirable to disable the sending of messages from your local or development environment. To do so, you may either call the `Mailer::pretend` method, or set the pretend option in the `app/Config/Mail.php` configuration file to `true`. When the mailer is in `pretend` mode, messages will be written to your application's log files instead of being sent to the recipient.

#### Enabling Pretend Mail Mode
```php
Mailer::pretend();
```
<a name="mailer-and-classic-helper"></a>

## API preservation - Working along with the classic Mailer Helper

The Mailing API, being designed for the **New Style APIs**, will work along with the classic Mailer Helper, with **no conflict** between, they being absolutely independent. 

Also, it is possible to use the Mailing API while using also the Classic APIs.