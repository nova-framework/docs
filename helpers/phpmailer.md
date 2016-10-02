PHPMailer is a third party class for sending emails, Full docs are available at https://github.com/Synchro/PHPMailer

Make an alias:

```php
use Helpers\Mail;
```

To use PHPMailer create a new instance of it:

```php
$mail = new Mail();
```

Once an instance has been created all the properties are available to you, a typical example:

```php
$mail = new Mail();
$mail->setFrom('noreply@domain.com');
$mail->addAddress('user@domain.com');
$mail->subject('Important Email');
$mail->body("<h1>Hey</h1><p>I like this <b>Bold</b> Text!</p>");
$mail->send();
```

To add attachments:

```php
$mail->addAttachment('relative/path/to/image');
```

To use gmail use at your own risk!! You must enable an option in order to allow to send email through gmail goto https://myaccount.google.com/security#connectedapps login and enable Allow access to less secure apps.

The class has the ability to send via SMTP in order to do so edit Helpers/PhpMailer/Mail.php and enter your SMTP settings you can also set a default email from address so you don't have to supply it each time:

```php
public $From     = 'noreply@domain.com';
public $FromName = SITETITLE;
public $Host     = 'smtp.gmail.com';
public $Mailer   = 'smtp';
public $SMTPAuth = true;                         
public $Username = 'email@domain.com';                         
public $Password = 'password';                         
public $SMTPSecure = 'tls';                         
public $WordWrap = 75;
```

You don't need to specify a plain text version of the email to be sent out, this is done automatically from the supplied body.