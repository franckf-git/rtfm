# [RTFM](https://franckf.gitlab.io/rtfm)

Manual pages for GNU/linux and others tips.  

[Generate BDD](http://www.generatedata.com/)
[Online Regex Tester](https://regex101.com/)
[OpenData](https://www.data.gouv.fr/fr/datasets/)
[Python calculator](https://www.numworks.com/fr/simulateur/)
[QR Code](http://generator.code-qr.net/#vcard)
[Speed Test](https://speed.cloudflare.com/)

### Raspberry

[Config.txt](https://www.raspberrypi.org/documentation/configuration/config-txt/README.md)
[Gpiozero Library](https://gpiozero.readthedocs.io/en/stable/recipes.html)
[Guizero Library](https://lawsie.github.io/guizero/#on-raspberry-pi)
[Pinout](http://fr.pinout.xyz/)
[SenseHat Emulator](https://trinket.io/sense-hat)

### Send / Receive

[Receive Email](https://10minutemail.com/10MinuteMail/index.html?dswid=2971)
[Receive Email 2](http://www.yopmail.com/fr/)
[Receive Email 3](https://www.mailinator.com/)
[Receive SMS](http://www.receive-sms-online.info/)
[Receive SMS 2](http://receivefreesms.com/)
[Receive SMS 3](http://www.receive-sms-now.com/)
[Send Email](http://send-email.org/)
[Send Email 2](http://anonymouse.org/cgi-bin/anon-email.cgi)
[Send SMS](http://www.sms-anonyme.net/index.php)
[Send SMS 2](http://www.monsmsgratuit.com/)

**[`^ back to top ^`](#)**

## IP address

### inline

- `curl l2.io/ip`
- `curl echoip.de`
- `curl ifconfig.me`
- `curl ipecho.net/plain`
- `curl -L ident.me` #[API](http://api.ident.me)
- `curl -L canihazip.com/s`
- `curl -L tnx.nl/ip`
- `curl wgetip.com`
- `curl whatismyip.akamai.com`
- `curl ip.tyk.nu`
- `curl bot.whatismyipaddress.com`
- `curl curlmyip.net`

### new line

- `curl eth0.me`
- `curl ipaddr.site`
- `curl ifconfig.co`
- `curl ifconfig.pro`
- `curl curlmyip.net`
- `curl ipinfo.io/ip`
- `curl icanhazip.com`
- `curl checkip.amazonaws.com`

### JSON only

- `curl httpbin.org/ip`
- `curl wtfismyip.com/json`

## Geolocation

- `curl ipinfo.io/8.8.8.8` or `curl ipinfo.io/8.8.8.8/loc`
- `curl ifconfig.co/country` or `curl ifconfig.co/city` or `curl ifconfig.co/country-iso` or `http ifconfig.co/json`

## Text Sharing

- `echo "Hello world!" | curl -F 'f:1=<-' ix.io`
- `echo "Hello world!" | curl -F file=@- 0x0.st`
- `echo "Hello world!" | curl -F 'clbin=<-' https://clbin.com`
- `echo "Hello world!" | nc termbin.com 9999`

## URL shortener

- `curl -s http://tinyurl.com/api-create.php?url=http://www.google.com`
- `curl -X POST --data "http://google.com" http://iret.ml/shorten`

## File Transfer

- `curl --upload-file <file> transfer.sh/<filename>`

## Browser

- `ssh brow.sh`

## Tools

- `curl qrenco.de/STRING` or `echo STRING | curl -F-=\<- qrenco.de` — create QR-code for a string ([chubin/qrenco.de](https://github.com/chubin/qrenco.de))

## Monitoring

- `curl ping.gg`

## Weather

- `curl wttr.in` or `curl wttr.in/Berlin` - the right way to check the weather
- `finger oslo@graph.no`

## News

- `curl getnews.tech/world+cup` - fetch the latest news
- `gopher txtn.ws`

## Information boards

- `curl http://frcl.de/gulasch` - Gulaschprogrammiernacht 2019 Fahrplan

## Map

- `telnet mapscii.me` - show a zoomable world map

## Money

- `curl rate.sx` - get cryptocurrencies exchange rates
- `curl moneroj.org` - get Monero exchange rate
- `curl cmc.rjldev.com` - get coinmarketcap top 100 cryptocurrencies

## Documentation

- `curl cheat.sh` - UNIX/Linux commands cheat sheets using curl ([chubin/cheat.sh](https://github.com/chubin/cheat.sh))

## Dictionaries and translators

- `curl 'dict://dict.org/d:command line'`

## Generators

- `git commit -m $(curl -sk https://whatthecommit.com/index.txt)` — generate random commit message
- `curl -H 'Accept: text/plain' foaas.com/cool/:from` — fuck off as a service
- `curl pseudorandom.name` — generate a pseudo random (American?) name ([treyhunner/pseudorandom.name](https://github.com/treyhunner/pseudorandom.name))
- `curl -s https://uinames.com/api/?region=france\&amount=25 | jq '.[] | .name +" " + .surname'` — generate 25 random french names
- `curl https://icanhazdadjoke.com` — random jokes

## Entertainment

- `nc towel.blinkenlights.nl 23` — watch StarWars in terminal
- `ssh chat.shazow.net` — chat over SSH ([shazow/ssh-chat](https://github.com/shazow/ssh-chat))
- `curl parrot.live` — display an animated party parrot ([hugomd/parrot.live](https://github.com/hugomd/parrot.live))
- `curl byemck.atulr.com` — display animated goodbye message for colleagues ([master-atul/byemck](https://github.com/master-atul/byemck))

**[`^ back to top ^`](#)**

![rtfm, for real.](https://imgs.xkcd.com/comics/rtfm.png)
