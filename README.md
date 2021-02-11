# Eutelia
Mail server tester script, check your mailserver for common vulnerabilities.

## Why Eutelia

Eutelia is a cartoon character in a popular cuban, nosy and sincere girl who says all the things that adults think but do not say. The name came as a joke in the SysadmindeCuba Telegram group, and we agreed on it.

## What it does

It test for common vulnerabilities in mail servers, such as:

- Open Relay
- SPF validation
- Sender from the same domain
- Non real address as recipients
- Others...

## Usage

Just clone this repository in a folder and edit the eutelia.conf file with the settings, then run eutelia with the IP of the servers to test.

```sh
eutelia 123.234.132.1
```

You will get a resume in the console and a detailed log in a file called `eutelia.log`

# Donations

If this script is of any help to you please take into account that I make this for free and you can help me by donating some amount via QvaPay in this link

[Tip the developer via QvaPay](https://qvapay.com/payme/pavelmc)

