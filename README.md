 # :speech_balloon: :mailbox_with_mail: adnotium [![](https://img.icons8.com/ios-glyphs/30/000000/play--v1.png)](https://github.com/nikolio/adnotium/blob/main/adnotium.mp3)

:black_nib: A place to organize my thoughts about free self-hosted comments for static pages

---
:exclamation: Not production ready :x:


---

It should be free as in both 
- free speech 
- free soda

---


 I love static websites. Especially when they don't require JS for their core functionality. I must be a weirdo right :question: :alien:
 
 Here are some rants people love to hate :smiley:
 
 [You don't really need Javascript](http://youmightnotneedjs.com/) and it's daddy about [Jquery](https://youmightnotneedjquery.com/)

And the really great [Luke Smith's](https://lukesmith.xyz/) soy dev  video rants [here](https://odysee.com/@Luke:7/a-demonstration-of-modern-web-bloat:f) and [here](https://odysee.com/@Luke:7/the-war-against-web-bloat-continues...:a)

---

But I digress :expressionless:

## :sparkles: :boom: To my Idea: 
A portable and free way of managing comments for static sites without privacy compromising external plugins (ie disqus) or need to pay and maintain server/vm's, such as heroku or other (paid) ones.

What's the catch? You (just _might_) need an http form that is in your control!  Oh and a seperate email address! Both are pretty easy to find in this day and age!

My initial thoughts is to use:

* keepass for storing the email account username/password and use the KeePassRPC (the keepassrpc js client is heavily coupled with the kee extension, needs some work).
* curl to downlod the emails containing the comments (filter based on email sender/subject)
* a tool to sanitize comments with npm and save them to a staging folder as markdown files.
* npm to include the markdown comments into their relevant articles when building/deploying website.

My thoughts of doing this is with hugo & npm & ....

It could be offered as a docker image, as well.

## :thought_balloon:  System Architecture

The ideal system/utility should be in two parts.

The first part html/css/js comment ui form (as hugo partial)an html form that users would submit their comments with, for a given static site article

* It would use vanilla js to post the json response by making a simple http post using fetch, or by a nojs http form submit post from the browser. It would _require_ a form url to email service for such.
  
* Sending an email (without using any smtp server) from the browser to the email's destination smtp server. We don't really care if it lands in the spam folder. Use of fingerprinting ip/browser/device/etc to prevent abuse when receiving/filtering, that would be auto discarded if the received message is not abusive - This would require disclaimer and research on how to be Data Protection Regulation _Compliant_ - . This way we don't need the "cloud"/someone else computer for receiving the comments (Would adopt a syntax like [this](https://stackoverflow.com/a/54489746)).

The second part (sh script/nodejs):

Or better called the script/utility, that should:
* query email creds(user/pass) from an open keepass instance or other secure place.
* login to an email box, search & download emails, containing the comments from received email, or http posted forms
* sanitize comment and post with hugo or the static site builder of your choice

The second part, should be further seperated into two "subsystems": 

* The fetching part that downloads the posted comments from an imap email account and puts them into a staging folder. You move the approved comments from the staging folder to a folder called accepted.
 
* The site rebuilding & deploying/uploading: When you run the command to build your website, the comments from the acccepted folder, should get included and built in the relevant post. Additionally you could have sth like a cron job to rebuild the website with any new accepted comments automatically!

It can be done asynchronously. So you do not need to give up privacy, convenience, or money and attention making sure backend servers and subcriptions are up and running! yay! :smiley:

The great part in this concept, is that you can choose the email client of your choice. Setting up your comments this way could make you arch & os independent.

---
## :snowflake: Site & node app config options

Hugo/Whatever static builder site Config settings:

- email: email@somedomain.tld Use some privacy email forwarding service here such as 33mail
- method: form-post-plain, service-template (custom-template, [Cloudflare-Workers-MailChannels](https://blog.cloudflare.com/sending-email-from-workers-with-mailchannels/), wordpress, google-forms, wix, [hubspot](https://blog.hubspot.com/marketing/html-form-email), [formsubmit](https://formsubmit.co/), [mailthis](https://mailthis.to/), [JotForms](https://www.jotform.com/email-forms/), [Majestic Forms](https://www.majesticform.com/forms/contact-form-free), [Postmail-Invotes](https://postmail.invotes.com/), etc .... ), email-submit
- pgp-key: your pgp/gpg key to encrypt the commenter's email. If pgp-key is ommited the commenter's email address will be sha512sum hashed. Use of [OpenPGPJs](https://openpgpjs.org/)

Adnotium Yaml Config settings (interface):

runCondition: shell-async, node-async

email address: email@somedomain.tld

email/imap message handling setting: read, unread, comments, receive-purge

imap-server: imap.somedomain.tld The imap server domain.

This eventually will be dropped and resolved via https://autoconfig.thunderbird.net/v1.1/gmail.com The resulting domain could be saved to a file for the fetch shell script fetch_email_comments.sh


incoming-comments: the dir location to save the comments 

accepted-comments: the dir location you move the comments you approve. 

The accepted comment (by-default) should be deleted (both from staging area and from the receiver's email server) after it has been built into the site.

---

## :books: My dev notes

[yaml parser in sh](https://github.com/mrbaseman/parse_yaml)

[parse inis in sh](https://github.com/wallyhall/shini)

[parse toml in bash](https://github.com/hyperupcall/bash-toml)

[yaml parser with awk](https://unix.stackexchange.com/a/633740)

### Email comment from client side

[Send email in Javascript](https://stackoverflow.com/a/46415414)

[SMTP Server in TypeScript](https://github.com/typemail/smtp/blob/main/docs/server.md)

[Heredoc Bash smtp email send](https://stackoverflow.com/a/10002000)

[curl send email](https://stackoverflow.com/a/16069786)

[SMTP Server in Node Deprecated](https://nodemailer.com/extras/smtp-server/)

[Sending emails with netcat](https://szclsya.me/posts/net/send-email-with-netcat/)

[With VB.NET](https://afterlogic.com/docs/mailbee-net-tutorials/sending-e-mail-messages-smtp/send-a-message-without-smtp-server)

[Sending emails without server](https://stackoverflow.com/a/48852105) dig gmail.com MX

[Email with Netcat](https://szclsya.me/posts/net/send-email-with-netcat/)

[Ditto](http://www.allgoodbits.org/articles/view/51)

[Resolve domain in JS](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/dns/resolve)

[Ditto](https://getbutterfly.com/check-dns-records-in-bulk-using-javascript/)

[Resolve domain in Node](https://blog.adriaan.io/validate-email-addresses-with-mx-records-in-node-js.html)

[Microsoft Test SMTP Server with telnet](https://learn.microsoft.com/en-us/exchange/mail-flow/test-smtp-telnet?view=exchserver-2019)

[SMTP Telnet](https://www.febooti.com/products/automation-workshop/tutorials/test-smtp-connection-send-test-email.html)

[SMTP Send email](https://mailtrap.io/blog/telnet-send-email/)

[SMTP Abbreviated list of Commands](https://mailtrap.io/blog/smtp-commands-and-responses/)

[Ditto](https://serversmtp.com/smtp-commands/)

[SugarCRM SMTP troubleshooting](https://support.sugarcrm.com/knowledge_base/email/testing_outbound_email_using_command_line/)

[SMTP brief tutorial](https://postmarkapp.com/guides/everything-you-need-to-know-about-smtp)

[Mailgun SMTP Commands](https://www.mailgun.com/blog/deliverability/smtp-commands/)

[IBM Documentation SMTP](https://www.ibm.com/docs/en/zvm/7.3?topic=traces-smtp-commands)

[IBM Documentation SMTP command example](https://www.ibm.com/docs/en/zvm/7.2?topic=interfaces-smtp-command-example)

[IBM Documentation SMTP List of Commands](https://www.ibm.com/docs/en/zvm/7.2?topic=interfaces-smtp-commands)

[IMB Documentation exit command](https://www.ibm.com/docs/en/zvm/7.2?topic=interfaces-using-smtp-command-exit)

[Signing with gpg](https://www.phildev.net/pgp/gpgsigning.html)

### curl and imap

[Curl imap examples](https://busylog.net/test-imap-with-curl-imap-example/)

[Imap commands note by busylog](https://busylog.net/telnet-imap-commands-note/)

[imap commands](https://www.atmail.com/blog/imap-commands/)

[Advanced Imap](https://www.atmail.com/blog/advanced-imap/)

[imap with telnet](https://support.moonpoint.com/network/email/telnet-imap.php)

[telnet emails](https://mediatemple.net/community/products/dv/204404584/sending-or-viewing-emails-using-telnet)

[netcat](https://www.varonis.com/blog/netcat-commands)

[Imap 101 by @mail](https://www.atmail.com/blog/imap-101-manual-imap-sessions/)


## Ideas about retrieving (& filtering perhaps) e-mails

- [How Imap works! - by Nylas](https://www.nylas.com/blog/nylas-imap-therefore-i-am/)

- [imap with curl @debian-administration.org](http://web.archive.org/web/20161130134317/https://debian-administration.org/article/726/Performing_IMAP_queries_via_curl)

- [Performing IMAP queries via curl - Google search](https://google.com/search?q=Performing+IMAP+queries+via+curl)

- [base64 in awk](https://archive.ph/R0nas)

- [fetchmail](https://en.wikipedia.org/wiki/Fetchmail) [[2] - Config](https://gist.github.com/iharsuvorau/45a078ecb597eb916fdf) [[3] - Config](https://calomel.org/fetchmailrc.html) [[4] - Config](https://serverfault.com/questions/45081/is-there-a-way-to-filter-mails-in-remote-imap-account)

- [IMAPFilter](https://serverfault.com/a/45111) [[2] - Github repo](https://github.com/lefcha/imapfilter)

- [MailDrop](https://en.wikipedia.org/wiki/Maildrop)

- [fdm](https://en.wikipedia.org/wiki/Fdm_(software)) [manual](https://github.com/nicm/fdm/blob/master/MANUAL)

- [imap-filter (In ruby)](https://github.com/flajann2/imap-filter/blob/master/README.org)

- [OfflineImap](http://www.offlineimap.org/) [[2] - Config](https://elric80.wordpress.com/mutt-2/offlineimap-and-msmtp/)

- [Sieve](https://en.wikipedia.org/wiki/Sieve_(mail_filtering_language))

[Executing external commands  for searching specific e-mails (with awk/sed) in node](https://stackoverflow.com/questions/20643470/execute-a-command-line-binary-with-node-js)


## OpenWrt (receiving emails)

[Receiving mail in openwrt](https://forum.openwrt.org/t/router-having-an-e-mail-address-to-receive-emails-and-act-upon-it/3383)

[uw-imap in openwrt](https://openwrt.org/packages/pkgdata/uw-imap)

[X-Mail](https://openwrt.org/docs/guide-user/services/email/xmail)

## Managing email login credentials with keepass

[Keepass related projects](https://awesomeopensource.com/projects/keepass)

[KeepassRPC arch wiki part](https://wiki.archlinux.org/title/KeePass#KeePassRPC_and_Kee)

### Autotype

[Noris ssh autotype gui](https://devops.norris.hu/2016/12/27/start-ssh-session-in-linux-gui-from-keepass-by-auto-type/)

[Add new entry with autotype](https://devops.norris.hu/2016/11/10/keepass-add-new-string-field-use-it-with-auto-type-feature/)

## Input Sanitization

[Html Sanitizer](https://stackoverflow.com/questions/1637275/simple-html-sanitizer-in-javascript)

[Email Validation](https://haacked.com/archive/2007/08/21/i-knew-how-to-validate-an-email-address-until-i.aspx/)

[Compose RFC Compliant emails using JS/TS](https://github.com/muratgozel/MIMEText)


[Good Form Security - no CAPTCHA](https://stackoverflow.com/questions/2603363/good-form-security-no-captcha)
