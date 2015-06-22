# notmuch-response

notmuch-response is a notmuch post-new hook that will tag all
unreplied messages in your mailboxes.  The primary use is to get the
list of unreplied e-mails sent to other people (so that you can ping
them after some time).

## Installation

Follow the following steps:

* Make sure the new messages are tagged with `new`
(see [this page](http://notmuchmail.org/initial_tagging/)).
* Copy `notmuch-response` and `post-new` to your notmuch
hooks directory
* Customize `post-new` as it is only a template
* Run `notmuch-response reindex` to perform initial indexing

After the initial run, the tags will be updated by the hook
every time you run `notmuch new`.

## Problems & bugs

* Initial reindexing is memory-hungry and it could fail on
some humongous mailbox (but I doubt anybody has such a mailbox)
* notmuch commands do not export the 'In-Reply-To' header so
the implementation recreates it from thread structure;
it's actually quite pretty, but it would be *much* simpler
to just get the header
* if used with offlineimap, `notmuch-response index` makes mutt
folders read immediately; it is due to a subtle interplay between
offlineimap and notmuch, see [Debian Bug #789625](http://bugs.debian.org/789625)
(contains a quick fix)

## Internals

The way it works is rather simple: we look at each message in the
mailbox and check whether it responds to another message. Then we tag
all messages that are replied to (with `response` tag). All remaining
messages will be tagged with `noresponse` tag. Additional tagging can
be done, as it is shown in `post-new`.  For example, I put tag `noack`
on messages that were sent by me, but I got no response to.
