---
layout: post
title:      "FormData Object"
date:       2018-03-02 22:04:14 +0000
permalink:  formdata_object
---


I made the mistake of making file upload one of the main features of my rails project. It was not terribly difficult to get the backend to work using the carrierwave gem but when it came time to create the same functionality with a single page web application I stumbled around quite a bit. I did not see a clear way to configure the XMLHttpRequest() to handle  **multipart/form-data**. After quite a bit of google bushwacking I found the answer: The HTML5 FormData Object.

From the MDN site:

> The FormData object lets you compile a set of key/value pairs to send using XMLHttpRequest. It is primarily intended for use in sending form data, but can be used independently from forms in order to transmit keyed data. The transmitted data is in the same format that the form's submit() method would use to send the data if the form's encoding type were set to multipart/form-data.
> 

Sweet. This turns out to be a simple and painless way to upload photos with either jQuery or vanilla JS. The trick that is instead of serializing the form elements you create a FormData object that you either send with XHR or use as data in jQuery ajax. Serialize in jQuery works great unless you have to upload a file. Unfortunately before HTML5 and FormData Object, javascript could not access local files.

```
	 myForm = document.getElementById("myForm");
	 var formData = new FormData(myForm);

	 $.ajax({
		  url: 'api.cfm',
		  data: formData,
```

Thanks! Happy Coding.

**Additional Resources**
https://developer.mozilla.org/en-US/docs/Web/API/FormData/Using_FormData_Objects
https://robots.thoughtbot.com/ridiculously-simple-ajax-uploads-with-formdata
https://www.sitepoint.com/easier-ajax-html5-formdata-interface/
