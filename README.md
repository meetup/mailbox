# mailbox [![Build Status](https://travis-ci.com/meetup/mailbox.svg?branch=master)](https://travis-ci.com/meetup/mailbox)

> ✉️ 💌 ✉️ imap query client that speaks json

## 👩‍🏭 development

This is a [rustlang](https://www.rust-lang.org/en-US/) application.
Go grab yourself a copy with [rustup](https://rustup.rs/).

## 🤸 usage

The default imap domain is Google's.

It is recommended (and the default) to pass in your [Mail app password](https://support.google.com/accounts/answer/185833?hl=en) from stdin.

```sh
echo -n "apppassword" \
| cargo run -p mailbox-cli -- \
  -u "you@gmail.com" \
  subject:lotto since:1-jan-2019
```

A non-0 exit code is yielded when there are no results. You can use this to detect if your
search was successful or not.

### query filters

Query filters expected in the form of `{name}:{value}` arguments. Multi-word values should be quoted.

Some example imap search filters are as follows

```
BCC "string" - match messages with "string" in the Bcc: field
BEFORE "date" - match messages with Date: before "date"
BODY "string" - match messages with "string" in the body of the message
CC "string" - match messages with "string" in the Cc: field
FROM "string" - match messages with "string" in the From: field
KEYWORD "string" - match messages with "string" as a keyword
ON "date" - match messages with Date: matching "date"
SINCE "date" - match messages with Date: after "date"
SUBJECT "string" - match messages with "string" in the Subject:
TEXT "string" - match messages with text "string"
TO "string" - match messages with "string" in the To:
UNKEYWORD "string" - match messages that do not have the keyword "string"
```

### awaiting results

Sometimes email is worth waiting for, you can use the `-w` ( `--wait` ) option providing a
duration to wait until messages that meet your query filter critera is met

```sh
echo -n "apppassword" \
| cargo run -p mailbox-cli -- \
  -u "you@gmail.com" \
  -w 10s
  subject:lotto since:1-jan-2019
```

### template output

You can optionally template the array of returned json messages using [handlebars template syntax](https://handlebarsjs.com/).

```sh
echo -n "apppassword" \
 | cargo run -p mailbox-cli -- \
   -u "you@gmail.com" \
   -t '{{#each .}}{{subject}}
{{/each}}' \
   subject:lotto since:1-jan-2019
```

Meetup, Inc 2019