 # adnotium [![](https://img.icons8.com/ios-glyphs/30/000000/play--v1.png)](https://github.com/nikolio/adnotium/blob/main/adnotium.mp3)

A place to organize my thoughts about free self-hosted comments for static pages

---

It should be free as in both 
- free speech 
- free soda

---


 I love static websites. Especially when they don't require JS for their core functionality. I must be a weirdo right ? :alien:
 
 Here are some rants people love to hate :smiley:
 
 [You don't really need Javascript](http://youmightnotneedjs.com/) and it's daddy about [Jquery](https://youmightnotneedjquery.com/)

And the really great [Luke Smith's](https://lukesmith.xyz/) soy dev  video rants [here](https://odysee.com/@Luke:7/a-demonstration-of-modern-web-bloat:f) and [here](https://odysee.com/@Luke:7/the-war-against-web-bloat-continues...:a)

---
To my Idea: A portable and free way of managing comments for static sites without privacy compromising external plugins (ie discus) or need to pay and maintain server/vm's.

What's the catch? You (just) need an http form that is in your control!

The ideal system/utility should be in two parts.

The first part:

* an html form that users would submit their comments with, for a given static site article. It would use vanila js to make the http post json response using fetch, or by simple nojs http form post browser request.

The second part:

Or better called the script/utility, that should:
* query email creds(user/pass) from an open keepass instance or other secure place.
* login to an email box, search & download emails, containing the comments from http posted forms
* sanitize comment and post with hugo

The second part, should be further seperated into two "subsystems": 

* The fetching part that downloads the posted comments from an imap email account and puts them into a staging folder. You move the approved comments from the staging folder to folder called accepted.
 
* The site rebuilding & deploying/uploading: When you run the command to build your website, the comments from the acccepted folder, should get included and built in the relevant post. Additionally you could have sth like a cron job to rebuild the website with any new accepted comments automatically!

It can be done asynchronously. So you do not need to give up privacy, convenience, or money and attention making sure backend servers and subcriptions are up and running! yay! :smiley:

The great part in this concept, is that you can choose the email client of your choice. Setting up your comments this way could make yoube arch & os independent

My initial thoughts is to use:

* keepass for storing the email account username/password and use the KeePassRPC (the keepassrpc js client is heavily coupled with the kee extension, needs some work).
* curl to downlod the emails containing the comments (filter based on email sender/subject)
* a tool to sanitize comments with npm and save them to a staging folder as markdown files.


* npm to include the markdown comments into their relevant articles when building/deploying

My thoughts of doing this is with hugo & npm & ....

---

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


