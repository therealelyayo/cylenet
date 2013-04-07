# CyleNet

## What...?

CyleNet is an experiment, in which I build my own internet-like thing. It (currently) has two primary features:

- **WiNS**, AKA Winners Name Service, a DNS-like service that expects hostnames and responds with IP addresses.
- **CTP**, AKA Cyle's Transfer Protocol, an HTTP-like service that expects requests for files and responds with the contents of those files.

Both are meant to be top-to-bottom SSL-encrypted.

## Why...?

Because why not.

## Requirements

- node.js 0.10 or later (at least, that's what I built it with)

## How do I use this?

Right now it's all very, very simple. It's currently configured to work on your own computer only. On a Mac, open three Terminal windows.

First, using one of them, you need to make some SSL certificates:

1. in Terminal, go to the "cylenet" folder
1. in Terminal, run `openssl genrsa -out cylenet.key.pem 2048`
1. in Terminal, run `openssl req -new -key cylenet.key.pem -out cylenet.csr.pem`
1. in Terminal, run `openssl x509 -req -in cylenet.csr.pem -signkey cylenet.key.pem -out cylenet.crt.pem`

Yes, you just made a self-signed certificate, which isn't great for real authorized encrypted traffic, but for now it'll have to do until I come with a CyleNet SSL alternative.

Now that you have a cert, here's what you do with the project to test this out:

1. in one Terminal window, go to the "WiNS-SSL" folder, and run `node server.js`
1. in another Terminal window, go to the "CTP-SSL" folder, and run `node server.js`
1. in the final Terminal window, go to the "CTP-SSL" folder, and run `node client.js "cyle.lol" "GET /"`

That final step does the following...

1. uses my WiNS-SSL module to translate the desired server hostname "cyle.lol" to the IP "127.0.0.1"
1. sends the request "GET /" to the CTP server at that IP
1. renders out the response! which should be a simple text file, in Markdown format

## Technical crap

- WiNS-SSL runs on port 21335
- CTP-SSL runs on port 21337
- all of this is still TCP-based, which is inadequate for my hopes with WiNS
- the SSL certificate crap still relies on DNS, which I don't want
- WiNS and CTP are "standards" much like DNS and HTTP, I just need to write the standards specs. Most of it involves a simple request which expects a response that contains a status code and a result.

## Ideas

- I'd like WiNS to be much louder, broadcasting requests instead of relying on specific IPs to act as WiNS servers. I'd like to find a way to decentralize WiNS lookups.
- I'm not sure how I want to make a "browser" for CyleNet. I like the idea of having only text-based files served, and have them in Markdown format, which can be parsed and then presented in a standardized fashion. When I first thought of CyleNet, I thought of a network protocol that was *only* for text. But good-looking text.
- WiNS entries are currently stored in an array in the javascript file, but this should be abstracted further with a key-value database like Redis or something. The "top-level" domains I'd like to use are .lol, .wut, and .dicks. Basically, this is what I'd like the domain name standard to look like, in RegEx format: `/^[-a-z0-9]{3,100}\.[a-z]{2,5}$/i`
- Need to compress CTP traffic with GZIP or something.