# Eutelia

Script for security tests on mail servers

**WARNING**: this software is on active development, some errors can slip or be there... or incomplete docs, etc...

## Why Eutelia

Eutelia is a popular Cuban cartoon character. A nosy and sincere Cuban girl who says all the things that adults think but don't say. The name came as a joke in the SysadmindeCuba Telegram group, and we agreed on it.

## What it does

It test for common vulnerabilities in mail servers, such as:

- Open Relay
- SPF validation
- Id spoofing (auth != sender)
- Mails from domain users without authentication
- Non existen recipients for your domain
- Attachment size restrictions
- National user restrictions
- Local users restrictions
- Among others

## Mail best practices

The operation of this software assume a group of best practices in mail handling, so far you need to know that:

- No plain modes are allowed to users of the mail server (aka: your domain users), you must axe the possibility of users to use *no encryption by default* protocols, see below:
  - Use SUBMISSION (587) instead of SMTP (25), as it uses STARTTLS by default.
  - Use SUBMISSION (587) instead of SSMTP (465), as SSMTP can be tricked to turn into plain text or weak encryption.
  - Use POP3S (995) instead of POP3 (110) as this has a SSL layer by default (even so, use IMAPS instead when possible)
  - Use IMAPS (993) instead of IMAP (143) as this has a SSL layer by default
- For external users (not domain members, aka mail inbound mail for the domain):
  - Plain text SMTP (25) is allowed, but you must allow optional STARTTLS over SMTP
- For any mail with a sender address matching the local domain:
  - No relay allowed if the user is not in the local users table.
  - No relay allowed if not authenticated, no matter if the mail is for a local recipient.
  - No relay allowed if the sender is different from the authenticated user.
- SPF must be enabled with at least the `~all` parameter (`-all` is ideal)
- DNSBL must be enabled with spamhaus as the best RBLDNS list.

## Pre-requisites

As with any tool you need to match some requisites, to run this script you need:

- Create a test email account with international access and node the email and password (dont use a real user! create one for testing)
- [OPTIONAL] Create a National only access email account and note the email and password.
- [OPTIONAL] Create a Local only access email account and note the email and password.
- Make sure you have swaks and curl installed on your test machine.
- Make sure your machine IN NOT on the `mynetworks` net segment of the server.
- Setup `git` and clone this repository to the test machine.
- Edit the eutelia.conf file with the data it ask (the test users and passwords you created for the test, your domain, etc.)
- You nedd no root power to run eutelia, it works with any user.

## Usage

You can take a peek on the eutelia options by just runnig it.

```sh
~$ ./eutelia 
Eutelia: Script for security tests on mail servers by @stdevPavelmc
https://github.com/stdevPavelmc/eutelia

Usage: ./eutelia [mode] [options]

  [mode] is one of this two:
      -i, --internal          [Default] Internal mode, you are a valid client
                              of the domain
      -e, --external          External mode, you are a enternal server
                              delivering emails to the domian [or an attacker]

  [options] some of this:
      -s, --server [IP]       [Required] the IP or Hostname of the server to test
      -l, --local             Run local users test
      -n, --national          Run national user test
      -a, --attachment [size] Run attachments size restrictions, size in MB

Enjoy.
```

As per the instructions you will find that there is two modes [internal] and [external]

### Internal mode

This mode is used for the clients point of view, as if you are a client of the server. No matter if you are in the LAN a VPN or the external IP, if you are able to send and receive emails with the server then this is the internal mode.

You can activate the internal mode with the option `-i`

By default this mode just check for some basic tests, there are other optionals:

- Test local users restrictions `-l` switch
- Test national users restrictions `-n` switch
- Test attachments size restrictions `-a` switch

### External mode

This mode is to test some common misconfigurations from the external point of view, you need to setup your PC as a external IP (ask a fellow sysadmin to make the test for you?)

The basic test covers this cases:

- Open relay
- No recipient verification (send as an unknown user from the domain)
- ... [complete...]


### Exmaples

- Check server (192.168.1.23) from inside: `./eutelia -i -s 192.168.1.23`
- Check server (mail.co7wt.cu) from inside for a friend: `./eutelia -e -s mail.co7wt.cu`
- Same as above but checking attachment size: `./eutelia -e -s mail.co7wt.cu -a 2`
- etc.

## Interpreting results

As in life, you need to receive and analyze all the data *in your environment and with your setup in mind* some times an error report can be a feature in some cases.

## Donations

If this script is of any help to you please take into account that I make this for free and you can help me by donating some amount via QvaPay in this link

[Tip the developer via QvaPay](https://qvapay.com/payme/pavelmc)

