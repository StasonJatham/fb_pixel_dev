# Facebook Pixel Fun

These are some example with Pixel, most of them are pretty standard and I will add more realistic use cases as I code them. Here are all the good resources Faacebook offers for their Pixel, what you can track with this is basically limitless. 
https://www.facebook.com/business/help/402791146561655?id=1205376682832142
https://developers.facebook.com/docs/facebook-pixel/reference
https://developers.facebook.com/docs/facebook-pixel/advanced
https://developers.facebook.com/docs/facebook-pixel/advanced/advanced-matching

Video Docs:
https://www.facebook.com/watch/?v=1002816166499449

### How to add a new Pixel
 - First you go to your Business Manager at: https://business.facebook.com/settings/pixels
 - You add a new Pixel
 - Set up manually and copy the html snipped
 - Follow instructions in Business Mnager to validate working Pixel
  
Here is the Pixel Code:
```HTML
<!-- Facebook Pixel Code -->
<script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
  n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)}(window, document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', '<GET YOUR OWN CODE>');
  fbq('track', 'PageView');
</script>
<noscript><img height="1" width="1" style="display:none"
  src="https://www.facebook.com/tr?id=<GET YOUR OWN CODE>&ev=PageView&noscript=1"
/></noscript>
<!-- End Facebook Pixel Code -->
  
```
 
- next adding "Advanced Matching", which in my case was already turned on in Business Manager
- Regardless, I added the Code manaully with the data supplied

This is an example of submitting advanced matching criteria to Facebook(manually):
```js
fbq('init', '<GET YOUR OWN CODE>', {
  em: 'johnsmith@example.com',        
  fn: 'john',    
  ln: 'smith',
  ct: 'menlopark',
  st: 'ca',
});
```

If doing it throgh the sneaky <img> Tag you have to hash the data yourself (SHA-256) <br>
YOu can use something like: https://github.com/bitwiseshiftleft/sjcl/ or whatever:
```html
    <noscript><img height="1" width="1" style="display:none"
    src="https://www.facebook.com/tr?id=<GET YOUR OWN CODE>&ev=PageView
      &ud[em]=77c15cba88f92469128047a9abfce151e12bba6aea47d36b7388c3540880e291
      &ud[fn]=96d9632f363564cc3032521409cf22a852f2032eec099ed5967c0d000cec607a
      &ud[ln]=6627835f988e2c5e50533d491163072d3f4f41f5c8b04630150debb3722ca2dd
      &ud[ct]=2b7b5dd4587e8545cc153c9c739bef276f32afe028ab2e46e73087d6a2c1eb32
      &ud[st]=7e8eea5cc60980270c9ceb75ce8c087d48d726110fd3d17921f774eefd8e18d8
      &noscript=1"
    /></noscript>
```

### For Dynamic Ads you have to have a Catalog
- detailed info here: https://www.facebook.com/business/help/1397294963910848
- add the Catalog through "Catalog Manager"
- connect the catalog to my Pixel under "Events Data Sources"

### Test our advanced tracking (add this to your product page)
- Send a ViewContent Event on page load including all 4 parameters to match our item
- i added this code right below the body tag including the required fields
- i then added "content_category" to add "men" to the event
- it is actually important that you use "10.10" and not "10,10" for pricing syntax 
```js
fbq('track', 'ViewContent', {
  value: 150.12,
  currency: 'EUR',
  content_ids: 'HWDUFH1231231',
  content_type: 'product',
  content_category: 'men',
});
```

### Track Purchase Events with Pixel Code
- Purchase Event is for confirmation Page (like a checkout page)
- Added all the required fields (i could have used "content" with "quantity" instead of content_ids)
- i added the "content_category" as it is used to indicate category

Here is my code (line 53-61 checkout.html):
```js
fbq('track', 'Purchase', {
  value: 150.12,
  currency: 'EUR',
  content_ids: 'HWDUFH1231231',
  content_type: 'product',
  content_category: 'men',
});
```
  
### Custom Button Click events
- add your code with your javascript (or include through .js file)
- just a plain JavaScript .addEventListner 
- when the "Subscribe" Button is clicked, the Pixel Event fires
- i just added status but there are other datapoints available: https://developers.facebook.com/docs/facebook-pixel/reference

```js
var subButton = document.querySelector('#subscribe_me');  
        subButton.addEventListener(
          'click', function() { 
            fbq('track', 'CompleteRegistration', {
              status: true,
            });          
          },
          false
        );
```
