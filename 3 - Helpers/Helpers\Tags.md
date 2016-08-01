The tags helper is a collection of useful methods:

````php
Tags::clean($data)
```
Clean function to convert data into an array.
````php
Tags::get($string)
```
This method converts the $string depending on the condition:
````php
Tags::get('[year]')
```
Returns the year
````php
Tags::get('[sitetitle]')
```
Returns the site title constant (SITETITLE)
````php
Tags::get('[siteemail]')
```
Returns the site email constant (SITEEMAIL)
````php
echo Tags::get('[feedburner username]');
```
Generates a Feedburner subscriber form
````php
echo Tags::get('[googleplusbox username]');
```
Generates a Google+ box
````php
echo Tags::get('[twitterfollowbutton username]');
```
Generates a Twitter follow button for the given username
````php
echo Tags::get('[twittersharebutton username]');
```
Generates a Twitter share button for the given username
````php
echo Tags::get('[youtube id]');
```
Generates an embed iframe for youtube, pass the video id only
````php
echo Tags::get('[youtubesub username]');
```
Generates a youtube subscribe widget
````php
echo Tags::get('[vimeo id]');
```
Generates an embed iframe for vimeo, pass the video id only