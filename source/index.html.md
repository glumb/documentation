---
title: PyOTA Reference

language_tabs:
  - python: Python

toc_footers:
  

search: true
---

# Introduction

This is PyOTA, the official Python library for the IOTA Core.
It implements both the [official API](https://iota.readme.io/), API wrapper functions as well as newly proposed functionality (such as signing, bundles, utilities and conversion).

<aside class="notice">
  IOTA is still in early development stages.
  There may be performance or stability issues.
  Please report any bugs using the <a href="https://github.com/iotaledger/iota.lib.py/issues">PyOTA Bug Tracker</a>.
</aside>

## Join the Discussion

If you want to get involved in the community, need help with getting setup, have any issues related with the library or just want to discuss Blockchain, Distributed Ledgers and IoT with other people, feel free to join our [Slack](http://slack.iotatoken.com/).
You can also ask questions on our [dedicated forum](https://forum.iotatoken.com/).

# Installation
PyOTA is compatible with Python 3.6, 3.5 and 2.7.

Install PyOTA using `pip`:

```bash
pip install pyota
```

# Getting Started
To connect to an IOTA node, initialize an `iota.Iota` object with the URI of the node, and optional seed:

```python
from iota import Iota

# Generate a random seed.
api = Iota('http://localhost:14265')

# Specify seed.
api = Iota('http://localhost:14265', b'SEED9GOES9HERE')
```
Test your connection to the server by sending a `getNodeInfo` command:

```python
print(api.get_node_info())
```

You are now ready to send commands to your IOTA node!
