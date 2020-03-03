# Email Obfuscation for Wordpress

This is a summary of the email obfuscation implementation on the [Cleveland Foundation website](https://www.clevelandfoundation.org/). This implementation involves creating a shortcode that handles obfuscation of email addresses.

## The Code

Add the following PHP to your `functions.php` file.
```php
function email_obfuscation( $atts ) {

  $atts = shortcode_atts(
	  array(
      'link-text' => NULL,
		  'username'  => NULL,
		  'domain'    => NULL
	  ), $atts, 'email' );

  if (!empty($atts['username'])) {
    $un = $atts['username'];
  } else {
    $un = 'Hello';
  }

  if (!empty($atts['domain'])) {
    $dm = $atts['domain'];
  } else {
    $dm = '@CleveFdn.org';
  }

  $classList = 'email-obf hidden';

  if (!empty($atts['link-text'])) {
    $linkText = $atts['link-text'];
    $classList.= ' has-link-text';
  } elseif (!empty($atts['username'])) {
    $linkText = 'Email ' . $un;
  } else {
    $linkText = 'Email us';
  }

  return '<a class="'.$classList.'" data-domain="'.$dm.'" data-un="'.$un.'" href="mailto:nospam">'.$linkText.'<noscript> [Enable Javascript to send an email]</noscript></a>';

}
add_shortcode( 'email_nospam', 'email_obfuscation' );
```

Add the following javascript to your site (requires jQuery).

```javascript
$('a.email-obf').each(function() {
  $(this).removeClass('hidden');
  $(this).click(function(e) {
    e.preventDefault();
    var email = $(this).data('un') + $(this).data('domain');
    window.location.href = 'mailto:' + email;
  });
});
```

## Usage

After adding the above code to your website, you will be able to use the `[email_nospam]` shortcode in your Wordpress posts and pages.

The following optional attributes are supported in this shortcode:
- `username` - Everything _before_ the @ symbol in the email address
- `domain` - Everything _after_ the @ symbol in the email address
- `link-text` - Text to display for the link

### Default Usage
Shortcode: `[email_nospam]`  
Appears: [Email us](mailto:nospam)

### Username Attribute
Shortcode: `[email_nospam username="TCFscholarships"]`  
Appears: [Email TCFscholarships](mailto:nospam)

### All Attributes
Shortcode: `[email_nospam username="dev" domain="@nsideas.com" link-text="Send us your favorite photos"]`  
Appears: [Send us your favorite photos](mailto:nospam)

All of the above will render in the following way if there is no Javascript support:  
[Email us [Enable Javascript to send an email]](mailto:nospam)
