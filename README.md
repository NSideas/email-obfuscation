# Email Obfuscation for Wordpress

## Javascript
```
// Email Obfuscation Handling
$('a.email-obf').each(function() {
  $(this).removeClass('hidden');
  $(this).click(function(e) {
    e.preventDefault();
    var email = $(this).data('un') + $(this).data('domain');
    window.location.href = 'mailto:' + email;
  });
});
```
