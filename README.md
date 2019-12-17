# NEXUSe2e-Justid
NEXUSe2e messaging with Justid / CIOT

Documentation of the messaging with Justid in The Netherlands. The messaging protocol is ebms/ebXML and NEXUSe2e can be used to implement the messaging server. This documentation describes the installation and configuration of NEXUSe2e.

There is now (formal) relationship with [direkt-gruppe.de](https://www.direkt-gruppe.de/), the developers of NEXUSe2e. More info about NEXUSe2e on their website [nexuse2e.org](https://nexuse2e.org).

The source of the documentation is in the `docs/` directory in [ReStructuredText](https://en.wikipedia.org/wiki/ReStructuredText) format. This source can be built with [Sphinx](http://www.sphinx-doc.org).

## Quickstart

Install Sphinx and related software:

    pip install sphinx
    pin install sphinx_rtd_theme

Optional

    pip install rst2pdf

Then in the `docs` directory run `make` for options and run, for example, `make singlhtml` to build the documentation.
