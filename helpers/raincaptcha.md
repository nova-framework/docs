This class can validate CAPTCHA images with RainCaptcha.

It can generate an URL to display a CAPTCHA validation image served by the RainCaptcha service.

The class can also send a request to RainCaptcha API to verify if the text that the user entered matches the text in the CAPTCHA image.

RainCaptcha is a CAPTCHA class that does not require any image processing extensions (GD, ImageMagick, etc). CAPTCHA was developed to be readable by humans and resistant to OCR software. It generates black-and-white images with 5 distorted letters on them and noise. Its checking algorithm is case-insensitive.

Create an alias:

````
use Helpers\RainCaptcha;
````

To use the class create a new instance, this can then be used to generate a captcha image using ->getImage().


````
$rainCaptcha = new RainCaptcha();

<img id="captchaImage" src="<?php echo $rainCaptcha->getImage(); ?>" />


<input name="captcha" type="text" />

<button type="button" class='btn btn-danger' onclick="document.getElementById('captchaImage').src = 

'<?php echo $rainCaptcha->getImage(); ?>&morerandom=' + Math.floor(Math.random() * 10000);"><span class="icon icon-refresh"></span></button>
````

To check if a user's input matches the captcha

````
$rainCaptcha = new RainCaptcha();

if (!$rainCaptcha->checkAnswer($_POST['captcha'])) {
    die('You have not passed the CAPTCHA test!');
}
````
